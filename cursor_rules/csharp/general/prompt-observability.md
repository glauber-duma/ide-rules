# Prompt: Observability

Para implementar observabilidade completa (logs, traces, metrics, health checks), siga **rigorosamente** o padrão definido em `@observability`.

## 🎯 Três Pilares OBRIGATÓRIOS

### 1. Logs Estruturados (Serilog)
- **SEMPRE** use Serilog com `CompactJsonFormatter` para produção
- **SEMPRE** use `UseSerilogRequestLogging()` para capturar requisições HTTP
- **SEMPRE** inclua CorrelationId e TraceId em todos os logs
- **SEMPRE** inclua UserId/TenantId quando autenticado
- **NUNCA** logue dados sensíveis (senhas, tokens, CPF, cartão)

### 2. Traces e Metrics (OpenTelemetry)
- **SEMPRE** configure OpenTelemetry com OTLP exporter
- **SEMPRE** habilite instrumentação: ASP.NET Core, HttpClient, EF Core
- **SEMPRE** adicione CorrelationId como tag nos spans
- **SEMPRE** crie custom spans para operações importantes de negócio

### 3. Health Checks
- **SEMPRE** exponha `/healthz/live` (liveness → sempre 200)
- **SEMPRE** exponha `/healthz/ready` (readiness → verifica dependências)
- **SEMPRE** adicione health checks para: DB, Redis, APIs externas

---

## 📝 Templates de Implementação

### Program.cs - Configuração Completa

```csharp
using Serilog;
using OpenTelemetry.Metrics;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;

var builder = WebApplication.CreateBuilder(args);

// ========== SERILOG ==========
builder.Host.UseSerilog((context, services, config) => config
    .ReadFrom.Configuration(context.Configuration)
    .ReadFrom.Services(services)
    .Enrich.FromLogContext()
    .Enrich.WithMachineName()
    .Enrich.WithEnvironmentName()
    .WriteTo.Console(new Serilog.Formatting.Compact.CompactJsonFormatter())
);

// ========== OPENTELEMETRY ==========
var serviceName = builder.Configuration["ApplicationName"] ?? "MyApi";

builder.Services.AddOpenTelemetry()
    .ConfigureResource(r => r.AddService(
        serviceName: serviceName,
        serviceVersion: "1.0.0",
        serviceInstanceId: Environment.MachineName))
    .WithTracing(tracing => tracing
        .AddAspNetCoreInstrumentation(opt =>
        {
            opt.RecordException = true;
            opt.EnrichWithHttpRequest = (activity, request) =>
            {
                if (request.HttpContext.Items.TryGetValue("CorrelationId", out var correlationId))
                    activity.SetTag("correlation.id", correlationId);
            };
        })
        .AddHttpClientInstrumentation(opt => opt.RecordException = true)
        .AddEntityFrameworkCoreInstrumentation(opt => opt.SetDbStatementForText = true)
        .AddSource(serviceName)
        .AddOtlpExporter(opt => opt.Endpoint = new Uri(
            builder.Configuration["OpenTelemetry:Endpoint"] ?? "http://localhost:4317"
        ))
    )
    .WithMetrics(metrics => metrics
        .AddAspNetCoreInstrumentation()
        .AddHttpClientInstrumentation()
        .AddRuntimeInstrumentation()
        .AddMeter(serviceName)
        .AddOtlpExporter(opt => opt.Endpoint = new Uri(
            builder.Configuration["OpenTelemetry:Endpoint"] ?? "http://localhost:4317"
        ))
    );

// ========== HEALTH CHECKS ==========
builder.Services.AddHealthChecks()
    .AddCheck("self", () => HealthCheckResult.Healthy(), tags: new[] { "ready" })
    .AddDbContextCheck<AppDbContext>("database", tags: new[] { "ready" });

var app = builder.Build();

// ========== MIDDLEWARES ==========
app.UseMiddleware<CorrelationIdMiddleware>(); // Primeiro!
app.UseSerilogRequestLogging(opt =>
{
    opt.EnrichDiagnosticContext = (ctx, httpContext) =>
    {
        if (httpContext.Items.TryGetValue("CorrelationId", out var cid))
            ctx.Set("CorrelationId", cid);
        if (httpContext.User.Identity?.IsAuthenticated == true)
        {
            var userId = httpContext.User.FindFirst("sub")?.Value;
            if (userId != null) ctx.Set("UserId", userId);
        }
    };
});

// ========== HEALTH CHECK ENDPOINTS ==========
app.MapHealthChecks("/healthz/live", new HealthCheckOptions
{
    Predicate = _ => false // Não verifica dependências
});

app.MapHealthChecks("/healthz/ready", new HealthCheckOptions
{
    Predicate = check => check.Tags.Contains("ready"),
    ResponseWriter = async (context, report) =>
    {
        context.Response.ContentType = "application/json";
        var result = System.Text.Json.JsonSerializer.Serialize(new
        {
            status = report.Status.ToString(),
            checks = report.Entries.Select(e => new
            {
                name = e.Key,
                status = e.Value.Status.ToString(),
                duration = e.Value.Duration.TotalMilliseconds
            })
        });
        await context.Response.WriteAsync(result);
    }
});

app.Run();
```

### CorrelationIdMiddleware

```csharp
namespace Api.Middleware;

public class CorrelationIdMiddleware
{
    private readonly RequestDelegate _next;
    private const string CorrelationIdHeader = "X-Correlation-Id";

    public CorrelationIdMiddleware(RequestDelegate next) => _next = next;

    public async Task InvokeAsync(HttpContext context)
    {
        var correlationId = context.Request.Headers[CorrelationIdHeader].FirstOrDefault()
            ?? Guid.NewGuid().ToString();

        context.Items["CorrelationId"] = correlationId;
        context.Response.Headers.TryAdd(CorrelationIdHeader, correlationId);

        using (Serilog.Context.LogContext.PushProperty("CorrelationId", correlationId))
        {
            await _next(context);
        }
    }
}
```

### Logging em Classes (Structured Logging)

```csharp
public class OrderService
{
    private readonly ILogger<OrderService> _logger;

    public OrderService(ILogger<OrderService> logger) => _logger = logger;

    public async Task<Result<OrderDto>> CreateOrder(CreateOrderCommand command)
    {
        // ✅ CORRETO: Template + properties estruturadas
        _logger.LogInformation(
            "Creating order for customer {CustomerId} with {ItemCount} items",
            command.CustomerId,
            command.Items.Count
        );

        try
        {
            var order = new Order(command.CustomerId);
            
            _logger.LogInformation(
                "Order {OrderId} created successfully",
                order.Id
            );
            
            return Result.Success(MapToDto(order));
        }
        catch (Exception ex)
        {
            _logger.LogError(
                ex,
                "Failed to create order for customer {CustomerId}",
                command.CustomerId
            );
            return Result.Failure<OrderDto>(Error.Failure(
                "Order.CreateFailed",
                "Failed to create order"
            ));
        }
    }
}
```

### Custom Spans (Traces Manuais)

```csharp
using System.Diagnostics;

public class PaymentService
{
    private static readonly ActivitySource ActivitySource = new("MyApi");
    private readonly ILogger<PaymentService> _logger;

    public async Task<Result> ProcessPayment(Guid orderId, decimal amount)
    {
        using var activity = ActivitySource.StartActivity("ProcessPayment");
        
        activity?.SetTag("order.id", orderId);
        activity?.SetTag("payment.amount", amount);
        
        try
        {
            // Lógica de processamento
            await Task.Delay(100);
            
            activity?.SetTag("payment.status", "success");
            return Result.Success();
        }
        catch (Exception ex)
        {
            activity?.SetTag("payment.status", "failed");
            activity?.RecordException(ex);
            _logger.LogError(ex, "Payment failed for order {OrderId}", orderId);
            return Result.Failure(Error.Failure("Payment.Failed", ex.Message));
        }
    }
}
```

### Custom Metrics

```csharp
using System.Diagnostics.Metrics;

public class OrderMetrics
{
    private readonly Counter<int> _ordersCreatedCounter;
    private readonly Histogram<double> _orderProcessingDuration;
    
    public OrderMetrics(IMeterFactory meterFactory)
    {
        var meter = meterFactory.Create("MyApi");
        
        _ordersCreatedCounter = meter.CreateCounter<int>(
            "orders.created",
            unit: "order"
        );
        
        _orderProcessingDuration = meter.CreateHistogram<double>(
            "order.processing.duration",
            unit: "ms"
        );
    }

    public void RecordOrderCreated(string status)
    {
        _ordersCreatedCounter.Add(1, 
            new KeyValuePair<string, object?>("status", status));
    }

    public void RecordProcessingDuration(double durationMs)
    {
        _orderProcessingDuration.Record(durationMs);
    }
}
```

---

## ❌ Anti-Patterns (O que NÃO fazer)

### ❌ NUNCA use string interpolation em logs

```csharp
// ERRADO! Perde estrutura
_logger.LogInformation($"User {userId} placed order {orderId}");

// CORRETO! Structured logging
_logger.LogInformation("User {UserId} placed order {OrderId}", userId, orderId);
```

### ❌ NUNCA logue dados sensíveis

```csharp
// ERRADO!
_logger.LogInformation("User {Email} logged in with password {Password}", email, password);

// CORRETO!
_logger.LogInformation("User {Email} logged in", MaskEmail(email));
```

### ❌ NUNCA ignore exceptions

```csharp
// ERRADO!
catch (Exception ex) { /* sem log */ }

// CORRETO!
catch (Exception ex)
{
    _logger.LogError(ex, "Operation failed for {OrderId}", orderId);
}
```

### ❌ NUNCA crie health checks que sempre retornam 200

```csharp
// ERRADO! Health check inútil
.AddCheck("database", () => HealthCheckResult.Healthy());

// CORRETO! Verifica de verdade
.AddDbContextCheck<AppDbContext>("database");
```

---

## 📋 Checklist de Implementação

Ao implementar observabilidade, verifique:

### Logs
- [ ] Serilog configurado com `UseSerilog()`
- [ ] `UseSerilogRequestLogging()` habilitado
- [ ] Enrichers: `FromLogContext`, `WithMachineName`, `WithEnvironmentName`
- [ ] CorrelationId incluído em todos os logs
- [ ] UserId incluído quando autenticado
- [ ] NUNCA loga senhas, tokens ou PII
- [ ] Usa structured logging (template + properties)
- [ ] appsettings.json com níveis de log apropriados

### Traces
- [ ] OpenTelemetry configurado com `AddOpenTelemetry()`
- [ ] `AddAspNetCoreInstrumentation()` habilitado
- [ ] `AddHttpClientInstrumentation()` habilitado
- [ ] `AddEntityFrameworkCoreInstrumentation()` se usar EF Core
- [ ] CorrelationId adicionado como tag nos spans
- [ ] OTLP exporter configurado
- [ ] ServiceName e ServiceVersion configurados

### Metrics
- [ ] `AddAspNetCoreInstrumentation()` habilitado
- [ ] `AddRuntimeInstrumentation()` habilitado
- [ ] Custom metrics para KPIs de negócio
- [ ] OTLP exporter configurado

### Health Checks
- [ ] `/healthz/live` (liveness)
- [ ] `/healthz/ready` (readiness)
- [ ] Health checks para DB configurados
- [ ] Health checks para Redis/Cache (se usar)
- [ ] Health checks para APIs externas (se depender)
- [ ] Response writer com JSON detalhado

### CorrelationId
- [ ] `CorrelationIdMiddleware` implementado e registrado
- [ ] Header `X-Correlation-Id` aceito no request
- [ ] Header `X-Correlation-Id` incluído no response
- [ ] CorrelationId adicionado ao Serilog LogContext

---

## 🐳 Docker Compose Local (Desenvolvimento)

Use esta stack para testar observabilidade localmente:

```yaml
version: "3.9"
services:
  otel-collector:
    image: otel/opentelemetry-collector-contrib:0.91.0
    ports:
      - "4317:4317"   # OTLP gRPC
      - "8888:8888"   # Prometheus metrics

  jaeger:
    image: jaegertracing/all-in-one:1.52
    ports:
      - "16686:16686" # UI → http://localhost:16686

  prometheus:
    image: prom/prometheus:v2.48.0
    ports:
      - "9090:9090"   # UI → http://localhost:9090

  grafana:
    image: grafana/grafana:10.2.0
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    ports:
      - "3000:3000"   # UI → http://localhost:3000
```

**Acesse:**
- Jaeger (traces): http://localhost:16686
- Prometheus (metrics): http://localhost:9090
- Grafana (dashboards): http://localhost:3000

---

## 📚 Consulte `@observability` para

- **Serilog completo**: appsettings.json, enrichers, structured logging
- **OpenTelemetry completo**: configuração detalhada, custom spans, custom metrics
- **Health Checks avançados**: customizados, JSON response, autenticação
- **CorrelationIdMiddleware**: implementação completa
- **docker-compose.yml**: stack completa (OTel Collector, Jaeger, Prometheus, Grafana)
- **Anti-patterns**: Exemplos do que NÃO fazer

---

**Lembre-se**: 
- **Logs** para debugging e auditoria
- **Traces** para entender fluxo distribuído
- **Metrics** para monitoramento em tempo real
- **Health Checks** para disponibilidade e readiness
