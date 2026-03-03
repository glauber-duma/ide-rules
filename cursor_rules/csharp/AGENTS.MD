---
Rule: AGENTS.MD
Description: Rule principal do agente C# — cataloga e descreve todas as rules de general e rules, orientando o agente sobre propósito, comportamento e situações de uso de cada rule disponível.
alwaysApply: true
tags: ["csharp", "dotnet", "agent", "index", "rules-catalog"]
Author: Glauber Duma
Created: 2026-03
Version: 1.0.0
---

# AGENTS.MD — Catálogo Geral de Rules C# / .NET

Este arquivo é a **rule principal** do agente de desenvolvimento C#. Ele deve ser **sempre aplicado** (`alwaysApply: true`) e serve como ponto de entrada para o agente compreender o conjunto completo de rules disponíveis, seu propósito, comportamento e condições de ativação.

> **Convenção:** todas as rules utilizam `.NET 9` e `C# 12` como baseline tecnológico. O padrão arquitetural adotado é **Clean Architecture** com **CQRS**, **MediatR** e **Result Pattern**.

---

## 📁 Estrutura das Rules

As rules estão divididas em dois grupos:

| Grupo | Diretório | Finalidade |
|---|---|---|
| **General** | `cursor_rules/csharp/general/` | Padrões técnicos reutilizáveis e componentes base |
| **Rules** | `cursor_rules/csharp/rules/` | Templates completos de arquiteturas e tipos de aplicação |

---

## 🔧 Rules Gerais (`/general`)

As rules gerais definem **padrões técnicos transversais** que são referenciados por outras rules e devem ser seguidos em qualquer projeto C#/.NET.

---

### 1. `csharp-style-guide.mdc`

**Arquivo:** `cursor_rules/csharp/general/csharp-style-guide.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`

#### Propósito
Estabelece os padrões de escrita de código C# modernos para toda a base de código. É a **rule de mais alta prioridade** entre as gerais — todas as outras rules a referenciam.

#### Comportamento
- **Nomenclatura:** PascalCase para classes/interfaces/métodos, camelCase para variáveis locais, `_camelCase` para campos privados, prefixo `I` para interfaces.
- **Namespaces:** obrigatoriamente file-scoped (`namespace Company.Product.Feature;`).
- **Global Usings:** centralizados em `GlobalUsings.cs`.
- **DTOs e VMs:** devem ser `record` imutáveis sempre que possível.
- **Features modernas obrigatórias:** primary constructors, collection expressions, pattern matching, nullable reference types, required members.
- **Estrutura de projeto:** camadas bem definidas (`Domain`, `Application`, `Infrastructure`, `Presentation`).
- **Imports:** nunca usar `using` implícito sem `global using`; agrupar e ordenar usings.

#### Quando usar
Deve ser ativada **automaticamente** para qualquer arquivo `.cs`. É a base que dá consistência a todas as outras rules do ecossistema.

---

### 2. `entity-validator.mdc`

**Arquivo:** `cursor_rules/csharp/general/entity-validator.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@csharp-style-guide`

#### Propósito
Centralizar o padrão de entidades de domínio com **setters privados** e **auto-validação via FluentValidation** embutida no construtor e em métodos de mutação.

#### Comportamento
- Entidades com **construtor privado** (para EF Core) e **construtor público completo** que invoca `ValidateAndThrow`.
- Todas as propriedades possuem **setters privados**.
- Cada **método público que altera estado** invoca re-validação.
- `Guid.NewGuid()` é gerado automaticamente quando nenhum `Id` é informado.
- Uso de `#region` para organizar `Properties`, `Constructors` e `Public Methods`.
- O validador (`AbstractValidator<T>`) é declarado no mesmo arquivo, logo abaixo da entidade, dentro de `#region Validators`.

#### Quando usar
Sempre que criar ou modificar uma **entidade de domínio** (EF Core ou Dapper). Garante que uma entidade jamais seja persistida em estado inválido.

---

### 3. `efcore-repository.mdc`

**Arquivo:** `cursor_rules/csharp/general/efcore-repository.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@csharp-style-guide`, `@entity-validator`

#### Propósito
Fornecer o padrão canônico de **repositório genérico com EF Core**, cobrindo CRUD, filtro por expressão lambda e paginação. É o padrão **preferencial** de acesso a dados.

#### Comportamento
- Interface `IRepository<TClass, TType>` com operações: `GetByIdAsync`, `ListAsync`, `PagedAsync`, `AddAsync`, `UpdateAsync`, `DeleteAsync`.
- Implementação `EfRepository<TClass, TType>` usando `DbSet<TClass>` injetado via `AppDbContext`.
- `PagedResult<T>` como `record` imutável contendo `Items`, `Total`, `Page` e `PageSize`.
- `AppDbContext` com `ApplyConfigurationsFromAssembly` — **nunca mapeamento inline**.
- Cada entidade deve ter seu `IEntityTypeConfiguration<TEntity>` em **arquivo separado**.
- Registro em DI com `AddScoped(typeof(IRepository<,>), typeof(EfRepository<,>))`.

#### Quando usar
Em todo projeto que use EF Core como camada de dados. Deve ser preferido ao Dapper, exceto em aplicações simples com modelo pequeno e sem relacionamentos complexos. Combine sempre com `@entity-validator`.

---

### 4. `dapper-repository.mdc`

**Arquivo:** `cursor_rules/csharp/general/dapper-repository.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@csharp-style-guide`, `@entity-validator`

#### Propósito
Fornecer o padrão de **repositório genérico com Dapper** para aplicações simples — modelo pequeno, poucos relacionamentos, sem tracking nem lazy loading.

#### Comportamento
- Interface `IDapperRepository<TClass, TType>` com as mesmas operações que o EF Core Repository.
- Implementação `DapperRepository<TClass, TType>` recebe `IDbConnection` e nome de tabela via construtor.
- Queries SQL explícitas com parâmetros nomeados (`@Id`, `@Skip`, `@Take`).
- Paginação com `OFFSET ... FETCH NEXT` (SQL Server).
- Mesmo `PagedResult<T>` do EF Core Repository — deve ser compartilhado.
- **Explicitamente proibido** em aplicações com modelo rico ou relacionamentos complexos.

#### Quando usar
Somente em **aplicações simples** onde EF Core seria excessivo. Mantenha o mesmo modelo de entidade/validator — apenas o acesso a dados muda. Combine com `@entity-validator` para consistência.

---

### 5. `fluentvalidation.mdc`

**Arquivo:** `cursor_rules/csharp/general/fluentvalidation.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@csharp-style-guide`, `@entity-validator`

#### Propósito
Padronizar a criação de **validadores FluentValidation** para DTOs, requests e entidades, com registro correto no DI.

#### Comportamento
- Validadores herdam de `AbstractValidator<T>`.
- Cada regra usa `.WithMessage()` com mensagem amigável e clara.
- Validadores compostos utilizam `RuleForEach` + `SetValidator` para coleções.
- Registro centralizado via `AddValidatorsFromAssemblyContaining<T>` em extension method próprio.
- Uso com injeção de dependência: `IValidator<T>` injetado no serviço ou handler.
- Integra com MediatR via `ValidationBehavior<TRequest, TResponse>` no pipeline.

#### Quando usar
Sempre que houver DTOs de request, commands, queries ou entidades que precisam de validação explícita. Complementa `@entity-validator` (que valida a entidade em si) ao validar os **contratos de entrada de dados**.

---

### 6. `use-cases.mdc`

**Arquivo:** `cursor_rules/csharp/general/use-cases.mdc`
**Globs:** `**/*.cs`
**Depende de:** `@csharp-style-guide`, `@fluentvalidation`

#### Propósito
Padronizar a implementação de **Use Cases** com CQRS, MediatR e Result Pattern, seguindo Clean Architecture.

#### Comportamento
- **Commands** (escrita): herdam de `IRequest<Result<T>>`, imutáveis como `record`, com validator FluentValidation no mesmo namespace.
- **Queries** (leitura): herdam de `IRequest<T>` retornando DTOs/ViewModels, nunca modificam estado.
- **Handlers**: implementam `IRequestHandler<TRequest, TResponse>`, recebem repositórios/serviços via construtor, retornam `Result<T>` ou `Result`.
- **Result Pattern obrigatório**: `Result.Success(value)`, `Result.Failure(error)`, nunca `throw` para controle de fluxo.
- **Pipeline Behaviors**: `ValidationBehavior`, `LoggingBehavior`, `TransactionBehavior` — registrados por ordem.
- Cada Use Case em **pasta própria**: `Application/Users/Commands/CreateUser/`.

#### Quando usar
Em qualquer projeto que adote Clean Architecture ou Vertical Slice. Commands e Queries devem ser o único ponto de entrada para lógica de negócio — controllers e endpoints apenas delegam para eles.

---

### 7. `api-controllers.mdc`

**Arquivo:** `cursor_rules/csharp/general/api-controllers.mdc`
**Globs:** `**/*Controller.cs`, `**/Controllers/**/*.cs`, `**/Program.cs`
**Depende de:** `@csharp-style-guide`, `@use-cases`

#### Propósito
Estabelecer o padrão de **controllers API REST** com `BaseApiController`, Result Pattern, Problem Details (RFC 7807), paginação e versionamento.

#### Comportamento
- Controllers herdam de `BaseApiController` — **nunca** implementam lógica de negócio.
- `BaseApiController.HandleResult<T>()` converte `Result<T>` em `IActionResult` com status code correto (200, 204, 400, 404, 409, 401, 403, 500).
- Erros retornam `ProblemDetails` com `type`, `title`, `status`, `detail` e `traceId` automático.
- `ModelState` validado automaticamente via `[ApiController]`.
- Versionamento via `Asp.Versioning` com URL segment (`/api/v1/`).
- Paginação com `PagedResponse<T>` incluindo metadados (`page`, `pageSize`, `totalCount`, `totalPages`).
- `[ProducesResponseType]` declarados em todos os endpoints.

#### Quando usar
Em todo projeto com API REST. Deve ser ativada em conjunto com `@use-cases` e garante que a camada de apresentação seja **thin** e padronizada.

---

### 8. `observability.mdc`

**Arquivo:** `cursor_rules/csharp/general/observability.mdc`
**Globs:** `**/Program.cs`, `**/*Filter.cs`, `**/*Middleware.cs`, `**/appsettings*.json`
**Depende de:** `@csharp-style-guide`

#### Propósito
Garantir **observabilidade completa** com logs estruturados (Serilog), traces distribuídos e métricas (OpenTelemetry) e health checks padronizados.

#### Comportamento
- **Serilog:** configurado via `builder.Host.UseSerilog(...)`, lendo `appsettings.json`, enriquecendo com `MachineName`, `EnvironmentName`, `Application`. Sinks: Console (JSON compacto) + File (rolling diário, 7 dias).
- **Request Logging:** `UseSerilogRequestLogging` com template customizado incluindo `TraceId`, `CorrelationId`, `UserId`.
- **OpenTelemetry:** traces com `AddAspNetCoreInstrumentation`, `AddHttpClientInstrumentation`, `AddEntityFrameworkCoreInstrumentation`; métricas com `AddAspNetCoreInstrumentation`, `AddRuntimeInstrumentation`; exporters para OTLP/Jaeger.
- **CorrelationId Middleware:** propaga `X-Correlation-Id` entre requisições e adiciona ao contexto de log.
- **Health Checks:** endpoints `/health/live` e `/health/ready`, verificando DB e dependências externas.
- **Dados sensíveis:** **NUNCA** logar senhas, tokens, CPF, cartão — usar mascaramento.

#### Quando usar
Em **todos** os projetos de produção, independente do tipo. A observabilidade é requisito não-negociável. Deve ser configurada no `Program.cs` como uma das primeiras ações.

---

### 9. `docker-containers.mdc`

**Arquivo:** `cursor_rules/csharp/general/docker-containers.mdc`
**Globs:** `**/Dockerfile`, `**/docker-compose*.yml`, `**/.dockerignore`
**Depende de:** `@csharp-style-guide`

#### Propósito
Estabelecer padrões de **Dockerfile e docker-compose** com foco em **segurança**, **non-root users**, **multi-stage builds** e boas práticas de produção.

#### Comportamento
- **Multi-stage build obrigatório**: stage `build` (SDK alpine) → stage `runtime` (aspnet alpine).
- **Non-root user obrigatório**: criação de `appgroup`/`appuser` com UID/GID 1000, nunca rodar como root.
- **Cache de layers**: copiar `.csproj` e fazer `dotnet restore` antes de copiar o código-fonte.
- **Alpine como base**: `mcr.microsoft.com/dotnet/aspnet:9.0-alpine` para menor superfície de ataque.
- **Labels de metadados**: `maintainer`, `description`, `version`.
- **Health check** no Dockerfile com `HEALTHCHECK` instruction.
- **docker-compose**: redes nomeadas, volumes declarados, depends_on com `condition: service_healthy`, variáveis de ambiente via `.env`.
- **`.dockerignore`**: excluir `**/.git`, `**/bin`, `**/obj`, `**/*.user`, `**/node_modules`.
- **Secrets**: nunca hardcoded — usar `secrets` do compose ou variáveis de ambiente em runtime.

#### Quando usar
Sempre que o projeto precisar ser **containerizado**. Ativada automaticamente para arquivos `Dockerfile`, `docker-compose*.yml` e `.dockerignore`.

---

### 10. `unit-testing.mdc`

**Arquivo:** `cursor_rules/csharp/general/unit-testing.mdc`
**Globs:** `**/*Tests.cs`, `**/*Test.cs`, `**/Tests/**/*.cs`
**Depende de:** `@csharp-style-guide`

#### Propósito
Padronizar a escrita de **testes unitários** com xUnit, Moq, Bogus e FluentAssertions.

#### Comportamento
- **Nomenclatura de métodos:** padrão `MethodName_Scenario_ExpectedBehavior` ou `Should_ExpectedBehavior_When_Scenario`.
- **Estrutura AAA obrigatória:** `// Arrange`, `// Act`, `// Assert` com separação visual clara.
- **Bogus para dados fake:** `Faker<T>` gerando dados realistas em vez de strings estáticas.
- **Moq para mocks:** `Mock<IInterface>`, configurar com `.Setup()`, verificar com `.Verify()`.
- **FluentAssertions:** `result.Should().Be(expected)`, `result.Should().BeEquivalentTo(...)`.
- **Builder pattern** para objetos complexos de teste (`UserBuilder`, `OrderBuilder`).
- **Fixtures compartilhadas** para setup custoso (`DatabaseFixture`, `UserFixture`).
- **Theory com InlineData/MemberData** para testes parametrizados.
- **Organização:** `Tests/Unit/`, `Tests/Integration/`, `Tests/Fixtures/`, `Tests/Builders/`.
- **Cobertura mínima esperada:** handlers, validators, domain logic, helpers.

#### Quando usar
Para qualquer arquivo de teste (sufixo `Tests.cs` ou `Test.cs`). Deve ser combinada com `@use-cases` para testar handlers e com `@fluentvalidation` para testar validadores.

---

## 🏗️ Rules de Arquitetura (`/rules`)

As rules de arquitetura são **templates completos** que combinam múltiplas rules gerais para descrever um tipo específico de aplicação. Cada rule de arquitetura define **estrutura de pastas, dependências, configurações e checklist** para um determinado cenário.

> Ao receber uma instrução de scaffold ou geração de projeto, o agente deve identificar qual rule de arquitetura se aplica e utilizá-la como guia principal, incorporando automaticamente todas as rules gerais referenciadas.

---

### 11. `api-mvc-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/api-mvc-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **APIs ASP.NET Core MVC sem autenticação**, com observabilidade, entidades validadas, repositórios e containerização.

#### Comportamento
Estrutura de pastas padrão com `src/Api`, `src/Application`, `src/Domain`, `src/Infrastructure` e `tests/`. Aplica todas as regras de estilo, acesso a dados, validação, observabilidade e Docker. Não inclui camada de autenticação/autorização.

#### Quando usar
Para APIs internas, serviços de dados, integrações ou proof-of-concepts onde **autenticação não é requisito**. Quando a API for exposta publicamente com controle de acesso, usar `api-mvc-auth-observability`.

---

### 12. `api-mvc-auth-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/api-mvc-auth-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **APIs ASP.NET Core MVC com autenticação JWT Bearer e autorização**, além de observabilidade completa.

#### Comportamento
Igual ao `api-mvc-simple-observability` com adição de: middleware de autenticação JWT Bearer, configuração de `AddAuthentication` + `AddJwtBearer`, policies de autorização, decorators `[Authorize]` nos controllers sensíveis, middleware de extração de `UserId`/`TenantId` para enriquecimento de logs.

#### Quando usar
Para **APIs públicas ou semi-públicas** que requerem controle de acesso via token JWT. Cenários típicos: backends para frontends (BFF), APIs expostas via gateway, microsserviços com autenticação federated.

---

### 13. `web-mvc-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/web-mvc-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@observability`

#### Propósito
Template para **aplicações web ASP.NET Core MVC (server-side rendering) simples**, sem autenticação.

#### Comportamento
Estrutura com `src/Presentation` (Controllers, Views, ViewModels, Filters), `src/Application`, `src/Domain`, `src/Infrastructure`. Views Razor com layouts padrão. Sem camada de identidade ou autenticação.

#### Quando usar
Para **portais internos**, dashboards administrativos ou aplicações web onde autenticação é gerenciada externamente (ex: SSO via proxy reverso). Para apps web com login próprio, usar `web-mvc-auth-observability`.

---

### 14. `web-mvc-auth-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/web-mvc-auth-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@observability`

#### Propósito
Template para **aplicações web ASP.NET Core MVC com autenticação por cookie** (ASP.NET Core Identity) e autorização baseada em claims/roles.

#### Comportamento
Igual ao `web-mvc-simple-observability` com adição de: ASP.NET Core Identity configurado, páginas de login/logout/registro, `[Authorize]` nas áreas protegidas, roles e policies. Cookies HttpOnly e SameSite configurados para segurança.

#### Quando usar
Para **sistemas web tradicionais** com login próprio baseado em formulários e sessão — ERPs, portais de clientes, sistemas de gestão com formulário de login local.

---

### 15. `console-batch-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/console-batch-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@observability`

#### Propósito
Template para **aplicações console batch simples** — processamento periódico de dados com logging estruturado e tracing.

#### Comportamento
Estrutura com `src/App` (Program.cs, Jobs/), `src/Application/Services`, `src/Infrastructure/Observability`. GenericHost com `IHostedService`. Jobs simples sem retry/resiliência avançada. Conteinerizado com Dockerfile.

#### Quando usar
Para **integrações ETL simples**, importações de arquivos, sincronizações pontuais onde **resiliência avançada não é necessária**. Ideal para tarefas agendadas (CronJob Kubernetes) de baixa criticidade.

---

### 16. `console-batch-complete-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/console-batch-complete-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@observability`

#### Propósito
Template para **aplicações console batch completas** com resiliência (Polly), mensageria opcional e observabilidade de ponta a ponta.

#### Comportamento
Igual ao `console-batch-simple-observability` com adição de: Polly para retry/circuit breaker em chamadas externas, suporte a mensageria (Kafka/RabbitMQ) como origem/destino de dados, pipelines de processamento em estágios (`src/App/Pipelines/`), health checks para sondagem de dependências.

#### Quando usar
Para **integrações críticas ou de alto volume** — processamento de mensagens em fila, integrações com sistemas externos sujeitos a instabilidade, pipelines de dados com garantia de entrega.

---

### 17. `desktop-blazor-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/desktop-blazor-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@observability`

#### Propósito
Template para **aplicações desktop Blazor Hybrid simples** COM observabilidade e suporte básico a offline/cache, SEM autenticação.

#### Comportamento
Estrutura com `src/App` (Pages, Components, Services, State, Program.cs) e `src/Infrastructure` (Observability, Offline). Blazor Hybrid com MAUI ou WPF. Cache local simples. Serilog para logging local em arquivo.

#### Quando usar
Para **ferramentas desktop internas** sem necessidade de autenticação — utilities para equipes técnicas, apps de gestão local, ferramentas de configuração.

---

### 18. `desktop-blazor-simple-auth-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/desktop-blazor-simple-auth-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@observability`

#### Propósito
Template para **aplicações desktop Blazor Hybrid simples com autenticação OIDC/PKCE**.

#### Comportamento
Igual ao `desktop-blazor-simple-observability` com adição de: fluxo OIDC + PKCE integrando com Identity Provider (Keycloak, Azure AD, etc.), módulo de autenticação em `src/App/Auth/`, token refresh automático, autorização por claim/role nas rotas Blazor.

#### Quando usar
Para **aplicações desktop corporativas** que precisam autenticar usuários contra um Identity Provider existente, sem recursos offline avançados.

---

### 19. `desktop-blazor-complete-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/desktop-blazor-complete-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@observability`

#### Propósito
Template para **aplicações desktop Blazor Hybrid completas** com observabilidade, resiliência em chamadas externas e **sincronização offline**, SEM autenticação.

#### Comportamento
Igual ao `desktop-blazor-simple-observability` com adição de: SQLite local para persistência offline, estratégia de sincronização com backend (periodic sync, conflict resolution), Polly para resiliência em chamadas de API, OpenTelemetry trace exportado para arquivo local.

#### Quando usar
Para **aplicações desktop de campo** que precisam funcionar sem conectividade — apps de auditoria, coleta de dados, gestão de ativos em ambientes sem rede confiável.

---

### 20. `desktop-blazor-complete-auth-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/desktop-blazor-complete-auth-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@observability`

#### Propósito
Template para **aplicações desktop Blazor Hybrid completas com autenticação OIDC/PKCE, observabilidade e sincronização offline resiliente**.

#### Comportamento
Combina todos os recursos de `desktop-blazor-complete-observability` e `desktop-blazor-simple-auth-observability`. Inclui: autenticação OIDC/PKCE, SQLite local, sincronização offline, resiliência com Polly, observabilidade completa.

#### Quando usar
Para **aplicações desktop corporativas de campo** com usuários autenticados que precisam trabalhar offline — inspetores de qualidade, técnicos de manutenção, vendedores externos.

---

### 21. `microservices-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/microservices-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **microsserviços simples sem Dapr**, com observabilidade, camadas bem definidas e empacotamento Docker.

#### Comportamento
Estrutura com `src/Api`, `src/Application/Services`, `src/Domain`, `src/Infrastructure`, `deploy/` (Dockerfile, docker-compose). Sem mensageria ou resiliência avançada. Comunicação via HTTP REST entre serviços.

#### Quando usar
Para **microsserviços que ainda não precisam de Dapr ou mensageria** — serviços CRUD simples, serviços de suporte, microsserviços em fase inicial de decomposição do monolito.

---

### 22. `microservices-complete-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/microservices-complete-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **microsserviços completos sem Dapr**, com resiliência (Polly), mensageria (Kafka ou RabbitMQ) e observabilidade distribuída.

#### Comportamento
Igual ao `microservices-simple-observability` com adição de: CQRS completo com Commands/Queries/Handlers/Events em `src/Application/`, Domain Events em `src/Domain/Events/`, publicação de eventos via Kafka/RabbitMQ, Polly para circuit breaker e retry, traces distribuídos propagados entre serviços.

#### Quando usar
Para **microsserviços de domínio críticos** que publicam/consomem eventos, precisam de resiliência avançada e participam de fluxos distribuídos complexos.

---

### 23. `microservices-dapr-simple-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/microservices-dapr-simple-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **microsserviços simples usando Dapr** — pub/sub, state store e observabilidade essencial.

#### Comportamento
Estrutura similar ao `microservices-simple-observability` com adição de: `src/Infrastructure/Dapr/`, configurações Dapr em `dapr/components/` (pubsub, statestore), anotações Dapr no docker-compose, `DaprClient` para invocar serviços e publicar eventos, `StateStore` para cache distribuído.

#### Quando usar
Para **microsserviços que adotam Dapr como sidecar** em ambientes Kubernetes — quando o time quer abstrair mensageria, service invocation e state management sem lock-in de provider específico.

---

### 24. `microservices-dapr-complete-observability.mdc`

**Arquivo:** `cursor_rules/csharp/rules/microservices-dapr-complete-observability.mdc`
**Depende de:** `@csharp-style-guide`, `@entity-validator`, `@efcore-repository`, `@dapper-repository`, `@fluentvalidation`, `@use-cases`, `@unit-testing`, `@docker-containers`, `@api-controllers`, `@observability`

#### Propósito
Template para **microsserviços completos com Dapr**, resiliência, observabilidade ponta a ponta e entrega via containers.

#### Comportamento
Combina todos os recursos do `microservices-dapr-simple-observability` com: CQRS completo, Domain Events, resiliência configurada via Dapr resiliency policies (retry, circuit breaker declarativo em YAML), bindings para integração com sistemas externos, observabilidade distribuída com traces propagados pelo Dapr sidecar.

#### Quando usar
Para **microsserviços Dapr de produção** em plataformas Kubernetes — quando toda a arquitetura é baseada em Dapr e o serviço precisa de observabilidade completa, resiliência declarativa e integração com múltiplos sistemas externos.

---

## 🗺️ Guia de Seleção Rápida de Rules

Use este guia para identificar rapidamente qual rule de arquitetura ativar:

| Cenário | Rule a usar |
|---|---|
| API REST pública com JWT | `api-mvc-auth-observability` |
| API REST interna sem auth | `api-mvc-simple-observability` |
| Portal web com login | `web-mvc-auth-observability` |
| Portal web interno sem login | `web-mvc-simple-observability` |
| Batch simples / ETL leve | `console-batch-simple-observability` |
| Batch crítico / alta resiliência | `console-batch-complete-observability` |
| Desktop sem auth, offline básico | `desktop-blazor-simple-observability` |
| Desktop com auth OIDC | `desktop-blazor-simple-auth-observability` |
| Desktop offline resiliente sem auth | `desktop-blazor-complete-observability` |
| Desktop offline resiliente com auth | `desktop-blazor-complete-auth-observability` |
| Microsserviço simples HTTP | `microservices-simple-observability` |
| Microsserviço completo com mensageria | `microservices-complete-observability` |
| Microsserviço Dapr básico | `microservices-dapr-simple-observability` |
| Microsserviço Dapr completo | `microservices-dapr-complete-observability` |

---

## ⚙️ Regras de Comportamento do Agente

1. **Esta rule é sempre carregada** — serve como ponto de orientação para todas as interações C#/.NET.
2. Ao receber um pedido de **scaffold ou geração de projeto**, identificar qual rule de arquitetura se aplica pela tabela acima e carregá-la.
3. Todas as rules gerais referenciadas por uma rule de arquitetura **devem ser respeitadas simultaneamente**.
4. **`csharp-style-guide`** é a regra de maior precedência — qualquer conflito de estilo deve ser resolvido em seu favor.
5. Ao criar entidades, seguir **sempre** `entity-validator` — nunca criar entidades com setters públicos sem validação.
6. Ao criar controllers, seguir **sempre** `api-controllers` — nunca colocar lógica de negócio em controllers.
7. Ao gerar código com acesso a dados, **perguntar** se o projeto usa EF Core ou Dapper simples — usar a rule correspondente.
8. Segurança em containers é **obrigatória** — nunca gerar Dockerfiles com user root ou sem multi-stage.
9. Observabilidade é **não-negociável** — todo projeto de produção deve ter Serilog + OpenTelemetry.
10. Testes devem seguir padrão AAA, nomenclatura descritiva e uso de Bogus para dados fake.
