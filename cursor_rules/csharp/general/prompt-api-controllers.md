# Prompt: API Controllers

Para implementar controllers de API REST, siga **rigorosamente** o padrão definido em `@api-controllers`.

## 🎯 Princípios Obrigatórios

### 1. Controllers THIN
- **SEMPRE** herde de `BaseApiController`
- **NUNCA** coloque lógica de negócio em controllers
- Controllers apenas orquestram: recebem requests → delegam para MediatR → retornam responses

### 2. Result Pattern
- **SEMPRE** use `HandleResult<T>(result)` para converter `Result<T>` em `IActionResult`
- **NUNCA** use exceptions para flow control
- Use cases devem retornar `Result<T>` ou `Result`

### 3. Problem Details (RFC 7807)
- **SEMPRE** retorne `ApiProblemDetails` padronizado para erros
- **SEMPRE** inclua `TraceId` para correlação com logs
- Erros são automaticamente convertidos para status codes corretos

---

## 📝 Templates de Implementação

### GET - Listagem com Paginação

```csharp
/// <summary>
/// Lista produtos com paginação
/// </summary>
[HttpGet]
[ProducesResponseType(typeof(PagedResult<ProductDto>), StatusCodes.Status200OK)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status400BadRequest)]
public async Task<IActionResult> GetProducts([FromQuery] GetProductsQuery query)
{
    var result = await _mediator.Send(query);
    return HandleResult(result);
}
```

### GET - Busca por ID

```csharp
[HttpGet("{id:guid}", Name = nameof(GetProductById))]
[ProducesResponseType(typeof(ProductDto), StatusCodes.Status200OK)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status404NotFound)]
public async Task<IActionResult> GetProductById(Guid id)
{
    var result = await _mediator.Send(new GetProductByIdQuery(id));
    return HandleResult(result);
}
```

### POST - Criação com Location Header

```csharp
/// <summary>
/// Cria um novo produto
/// </summary>
[HttpPost]
[ProducesResponseType(typeof(ProductDto), StatusCodes.Status201Created)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status400BadRequest)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status409Conflict)]
public async Task<IActionResult> CreateProduct([FromBody] CreateProductCommand command)
{
    var result = await _mediator.Send(command);
    
    if (result.IsFailure)
        return HandleResult(result);
    
    return CreatedAtActionResult(
        nameof(GetProductById),
        new { id = result.Value.Id },
        result.Value
    );
}
```

### PUT - Atualização Idempotente

```csharp
[HttpPut("{id:guid}")]
[ProducesResponseType(typeof(ProductDto), StatusCodes.Status200OK)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status400BadRequest)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status404NotFound)]
public async Task<IActionResult> UpdateProduct(Guid id, [FromBody] UpdateProductCommand command)
{
    if (id != command.Id)
        return BadRequest(new ApiProblemDetails
        {
            Title = "Invalid request",
            Detail = "ID in URL does not match ID in body",
            Status = StatusCodes.Status400BadRequest
        });

    var result = await _mediator.Send(command);
    return HandleResult(result);
}
```

### DELETE - Remoção

```csharp
[HttpDelete("{id:guid}")]
[ProducesResponseType(StatusCodes.Status204NoContent)]
[ProducesResponseType(typeof(ApiProblemDetails), StatusCodes.Status404NotFound)]
public async Task<IActionResult> DeleteProduct(Guid id)
{
    var result = await _mediator.Send(new DeleteProductCommand(id));
    return HandleResult(result);
}
```

---

## ❌ Anti-Patterns (O que NÃO fazer)

### ❌ NUNCA retorne entidades de domínio
```csharp
// ERRADO!
return Ok(await _context.Products.ToListAsync());

// CORRETO!
var result = await _mediator.Send(new GetProductsQuery());
return HandleResult(result); // Retorna ProductDto
```

### ❌ NUNCA coloque lógica de negócio no controller
```csharp
// ERRADO!
if (dto.Price < 0) return BadRequest();
var product = new Product { Price = dto.Price * 1.1m };

// CORRETO!
var result = await _mediator.Send(command); // Lógica no handler
return HandleResult(result);
```

### ❌ NUNCA use exceptions para flow control
```csharp
// ERRADO!
try { var product = await _repo.GetAsync(id); }
catch (NotFoundException) { return NotFound(); }

// CORRETO!
var result = await _mediator.Send(new GetProductByIdQuery(id));
return HandleResult(result); // Result Pattern
```

### ❌ NUNCA use status codes inconsistentes
```csharp
// ERRADO!
return Ok(new { error = "Not found" }); // 200 para erro!
return Ok(result.Value); // 200 para criação!

// CORRETO!
return HandleResult(result); // 400/404/409 para erros
return CreatedAtActionResult(...); // 201 para criação
```

---

## ✅ Status Codes Corretos

- **200 OK** → Sucesso com corpo (GET, PUT com retorno)
- **201 Created** → Criação com Location header (POST)
- **204 No Content** → Sucesso sem corpo (DELETE, PUT sem retorno)
- **400 Bad Request** → Validação falhou
- **401 Unauthorized** → Não autenticado
- **403 Forbidden** → Não autorizado
- **404 Not Found** → Recurso não encontrado
- **409 Conflict** → Conflito (ex: unique constraint)
- **500 Internal Server Error** → Erro não tratado

---

## 📋 Checklist de Implementação

Ao criar um controller de API, verifique:

- [ ] Controller herda de `BaseApiController`
- [ ] Usa construtor com `IMediator mediator`
- [ ] Todos os métodos delegam para MediatR com `_mediator.Send()`
- [ ] Usa `HandleResult()` para converter `Result<T>` em `IActionResult`
- [ ] POST usa `CreatedAtActionResult()` com Location header
- [ ] Todos os endpoints têm XML comments (`<summary>`, `<param>`, `<returns>`, `<response>`)
- [ ] `[ProducesResponseType]` para cada status code possível
- [ ] NUNCA retorna entidades de domínio (sempre DTOs)
- [ ] ZERO lógica de negócio no controller
- [ ] Status codes semânticos (201 para criação, 204 para deleção, etc)

---

## 📚 Consulte `@api-controllers` para

- **BaseApiController** completo com `HandleResult<T>()`
- **ApiProblemDetails** (RFC 7807)
- **PagedResult<T>** para paginação
- **API Versioning** (URL e header)
- **Validação automática** com `ApiBehaviorOptions`
- **Exemplos CRUD completos** (GET, POST, PUT, PATCH, DELETE, Bulk)
- **Health Checks** avançados
- **Swagger/OpenAPI** com versionamento

---

**Lembre-se**: Controllers devem ser **THIN** (magros). Toda lógica de negócio fica nos **Handlers** (use cases). Controllers apenas **orquestram**.