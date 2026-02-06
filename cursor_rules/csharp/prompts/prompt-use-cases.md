# Prompt: Use Cases com CQRS e MediatR

## Contexto
Você está desenvolvendo use cases seguindo CQRS (Command Query Responsibility Segregation) com MediatR e Result Pattern para uma aplicação .NET seguindo Clean Architecture.

## Objetivos
1. Implementar Commands para operações que modificam estado (Create, Update, Delete)
2. Implementar Queries para operações de leitura (Get, List, Search)
3. Usar Result Pattern para comunicação sucesso/falha tipada
4. Integrar FluentValidation no pipeline de validação
5. Implementar Pipeline Behaviors para cross-cutting concerns
6. Manter controllers finos delegando lógica para handlers

## Regras de Implementação

### Commands
- Use `record` imutável com `init` para imutabilidade
- Implemente `IRequest<Result<T>>` ou `IRequest<Result<Unit>>`
- Crie validator correspondente com FluentValidation
- Handler deve:
  - Validar regras de negócio
  - Persistir usando repositório + Unit of Work
  - Retornar Result<T> (Success ou Failure)
  - Logar operações importantes

### Queries
- Use `record` imutável para query parameters
- Implemente `IRequest<Result<T>>`
- Retorne DTOs/ViewModels (nunca entidades de domínio)
- Não modifique estado
- Use paginação para listas grandes
- Use PredicateBuilder para filtros complexos

### Result Pattern
- Use `Result<T>` para operações que podem falhar
- Use `Result.Success(value)` para sucesso
- Use `Result.Failure(error)` para falhas de negócio
- Não lance exceções para falhas esperadas
- Controllers mapeiam Result para HTTP status codes

### Pipeline Behaviors
- ValidationBehavior: validação automática antes do handler
- LoggingBehavior: logging estruturado de entrada/saída
- TransactionBehavior: transação automática para Commands
- Registre behaviors na ordem correta (validação primeiro)

### Controllers
- Injete apenas IMediator
- Envie command/query via `_mediator.Send()`
- Mapeie Result.IsSuccess/IsFailure para HTTP codes
- Use CancellationToken do request
- Adicione ProducesResponseType para documentação

## Estrutura de Pastas
```
Application/
  Users/
    Commands/
      CreateUser/
        CreateUserCommand.cs          // record + IRequest<Result<T>>
        CreateUserCommandValidator.cs // AbstractValidator
        CreateUserCommandHandler.cs   // IRequestHandler
      UpdateUser/
      DeleteUser/
    Queries/
      GetUser/
        GetUserQuery.cs
        GetUserQueryValidator.cs
        GetUserQueryHandler.cs
      ListUsers/
      SearchUsers/
  Common/
    Models/
      Result.cs                       // Result<T> class
      PagedResult.cs
    Behaviors/
      ValidationBehavior.cs
      LoggingBehavior.cs
      TransactionBehavior.cs
```

## Exemplos

### Command Completo
```csharp
// Command
public record CreateUserCommand : IRequest<Result<UserResponse>>
{
    public string FirstName { get; init; } = string.Empty;
    public string LastName { get; init; } = string.Empty;
    public string Email { get; init; } = string.Empty;
}

// Validator
public class CreateUserCommandValidator : AbstractValidator<CreateUserCommand>
{
    public CreateUserCommandValidator()
    {
        RuleFor(x => x.FirstName).NotEmpty().MaximumLength(100);
        RuleFor(x => x.Email).NotEmpty().EmailAddress();
    }
}

// Handler
public class CreateUserCommandHandler : IRequestHandler<CreateUserCommand, Result<UserResponse>>
{
    private readonly IUserRepository _repository;
    private readonly IUnitOfWork _unitOfWork;
    private readonly IMapper _mapper;

    public async Task<Result<UserResponse>> Handle(CreateUserCommand request, CancellationToken ct)
    {
        if (await _repository.ExistsAsync(u => u.Email == request.Email, ct))
            return Result<UserResponse>.Failure("Email already exists");

        var user = new User(Guid.NewGuid(), request.FirstName, request.LastName, request.Email);
        await _repository.AddAsync(user, ct);
        await _unitOfWork.SaveChangesAsync(ct);

        var response = _mapper.Map<UserResponse>(user);
        return Result<UserResponse>.Success(response);
    }
}
```

### Query Completo
```csharp
// Query
public record GetUserQuery(Guid Id) : IRequest<Result<UserResponse>>;

// Handler
public class GetUserQueryHandler : IRequestHandler<GetUserQuery, Result<UserResponse>>
{
    private readonly IUserRepository _repository;
    private readonly IMapper _mapper;

    public async Task<Result<UserResponse>> Handle(GetUserQuery request, CancellationToken ct)
    {
        var user = await _repository.GetByIdAsync(request.Id, ct);
        if (user == null)
            return Result<UserResponse>.Failure($"User {request.Id} not found");

        var response = _mapper.Map<UserResponse>(user);
        return Result<UserResponse>.Success(response);
    }
}
```

### Controller
```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly IMediator _mediator;

    [HttpPost]
    public async Task<IActionResult> Create([FromBody] CreateUserCommand command, CancellationToken ct)
    {
        var result = await _mediator.Send(command, ct);
        return result.IsSuccess
            ? CreatedAtAction(nameof(GetById), new { id = result.Value!.Id }, result.Value)
            : BadRequest(new { error = result.Error });
    }

    [HttpGet("{id:guid}")]
    public async Task<IActionResult> GetById(Guid id, CancellationToken ct)
    {
        var query = new GetUserQuery(id);
        var result = await _mediator.Send(query, ct);
        return result.IsSuccess ? Ok(result.Value) : NotFound(new { error = result.Error });
    }
}
```

## Checklist de Implementação
- [ ] Command/Query é record imutável
- [ ] Implementa IRequest<Result<T>>
- [ ] Validador FluentValidation criado
- [ ] Handler com injeção de dependências
- [ ] Handler retorna Result<T>
- [ ] Regras de negócio validadas antes de persistir
- [ ] Unit of Work usado para transações (Commands)
- [ ] Logging estruturado implementado
- [ ] Controller usa apenas IMediator
- [ ] Result mapeado para HTTP status codes
- [ ] Testes unitários do handler
- [ ] Testes do validator

## Diretrizes de Qualidade
- Controllers devem ter menos de 10 linhas por ação
- Handlers devem ter uma única responsabilidade
- Validadores devem ter regras auto-explicativas
- Result.Error deve ter mensagens claras para o usuário
- Logs devem incluir contexto (IDs, timestamps)
- Testes devem cobrir sucesso e falhas esperadas
- CancellationToken deve ser propagado em todas as chamadas async

## Observações
- Prefira Result Pattern a exceções para falhas de negócio
- Queries podem usar cache (Commands não)
- Behaviors executam na ordem de registro
- Commands são auditáveis (use behaviors para auditoria)
- DTOs são imutáveis (use records)
