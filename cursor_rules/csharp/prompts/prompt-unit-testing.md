# Prompt: Unit Testing com xUnit, Moq e Bogus

## Contexto
Você está escrevendo testes unitários para uma aplicação .NET 9 usando xUnit como framework de testes, Moq para mocking, Bogus para geração de dados fake e FluentAssertions para assertions legíveis.

## Objetivos
1. Criar testes unitários seguindo o padrão AAA (Arrange, Act, Assert)
2. Usar xUnit Facts e Theories apropriadamente
3. Mockar dependências com Moq
4. Gerar dados de teste com Bogus
5. Usar FluentAssertions para assertions expressivas
6. Seguir princípios FIRST (Fast, Independent, Repeatable, Self-validating, Timely)
7. Manter code coverage acima de 80%

## Regras de Implementação

### Nomenclatura
- Classe de teste: `{ClasseTestada}Tests`
- Método de teste: `MethodName_Scenario_ExpectedBehavior`
- Exemplo: `GetUserAsync_ValidId_ReturnsUser`
- Alternativo: `Should_Return_User_When_Id_Is_Valid`

### Estrutura AAA
```csharp
[Fact]
public async Task GetUserAsync_ValidId_ReturnsUser()
{
    // Arrange - Setup de mocks, dados, dependências
    var userId = 1;
    var expectedUser = new User { Id = userId, Name = "John" };
    var repositoryMock = new Mock<IUserRepository>();
    repositoryMock.Setup(r => r.GetByIdAsync(userId, It.IsAny<CancellationToken>()))
        .ReturnsAsync(expectedUser);
    var service = new UserService(repositoryMock.Object);

    // Act - Chamada ao método testado
    var result = await service.GetUserAsync(userId);

    // Assert - Verificações
    result.Should().NotBeNull();
    result.Should().Be(expectedUser);
}
```

### xUnit - Facts e Theories
```csharp
// Use [Fact] para testes simples
[Fact]
public void Add_TwoNumbers_ReturnsSum()
{
    var calculator = new Calculator();
    var result = calculator.Add(2, 3);
    result.Should().Be(5);
}

// Use [Theory] com [InlineData] para testes parametrizados
[Theory]
[InlineData(1, 2, 3)]
[InlineData(10, 20, 30)]
[InlineData(-5, 5, 0)]
public void Add_MultipleScenarios_ReturnsSum(int a, int b, int expected)
{
    var calculator = new Calculator();
    var result = calculator.Add(a, b);
    result.Should().Be(expected);
}

// Use [MemberData] para cenários complexos
public static IEnumerable<object[]> GetTestData()
{
    yield return new object[] { new User { Id = 1 }, true };
    yield return new object[] { new User { Id = 0 }, false };
}

[Theory]
[MemberData(nameof(GetTestData))]
public void ValidateUser_VariousUsers_ReturnsExpected(User user, bool expected)
{
    var result = new UserValidator().IsValid(user);
    result.Should().Be(expected);
}
```

### Moq - Mocking
```csharp
// Setup básico
var repositoryMock = new Mock<IUserRepository>();
repositoryMock
    .Setup(r => r.GetByIdAsync(1, It.IsAny<CancellationToken>()))
    .ReturnsAsync(new User { Id = 1 });

// Setup com exceção
repositoryMock
    .Setup(r => r.AddAsync(It.IsAny<User>(), It.IsAny<CancellationToken>()))
    .ThrowsAsync(new DatabaseException("Connection failed"));

// Verify chamadas
repositoryMock.Verify(
    r => r.AddAsync(It.IsAny<User>(), It.IsAny<CancellationToken>()),
    Times.Once);

// Verify com condições específicas
repositoryMock.Verify(
    r => r.AddAsync(
        It.Is<User>(u => u.Email == "john@example.com"),
        It.IsAny<CancellationToken>()),
    Times.Once);

// Callback para capturar argumentos
User? capturedUser = null;
repositoryMock
    .Setup(r => r.AddAsync(It.IsAny<User>(), It.IsAny<CancellationToken>()))
    .Callback<User, CancellationToken>((user, ct) => capturedUser = user)
    .Returns(Task.CompletedTask);
```

### Bogus - Geração de Dados
```csharp
// Faker simples
var faker = new Faker();
var name = faker.Name.FullName();
var email = faker.Internet.Email();

// Faker tipado
var userFaker = new Faker<User>()
    .RuleFor(u => u.Id, f => f.Random.Int(1, 1000))
    .RuleFor(u => u.FirstName, f => f.Name.FirstName())
    .RuleFor(u => u.LastName, f => f.Name.LastName())
    .RuleFor(u => u.Email, (f, u) => f.Internet.Email(u.FirstName, u.LastName))
    .RuleFor(u => u.BirthDate, f => f.Date.Past(50, DateTime.Now.AddYears(-18)))
    .RuleFor(u => u.IsActive, f => f.Random.Bool());

var users = userFaker.Generate(10);

// Builder com Faker
public class UserBuilder
{
    private static readonly Faker<User> _faker = new Faker<User>()
        .RuleFor(u => u.Id, f => f.Random.Int(1, 1000))
        .RuleFor(u => u.FirstName, f => f.Name.FirstName());
    
    private User _user;
    
    public UserBuilder()
    {
        _user = _faker.Generate();
    }
    
    public UserBuilder WithEmail(string email)
    {
        _user.Email = email;
        return this;
    }
    
    public User Build() => _user;
}
```

### FluentAssertions
```csharp
// Assertions básicas
result.Should().NotBeNull();
result.Should().Be(expected);
result.Should().BeGreaterThan(0);

// Strings
result.Name.Should().Contain("John");
result.Email.Should().EndWith("@example.com");

// Collections
users.Should().HaveCount(3);
users.Should().Contain(u => u.Name == "John");
users.Should().OnlyContain(u => u.IsActive);
users.Should().BeInAscendingOrder(u => u.Id);

// Objetos
result.Should().BeEquivalentTo(expected);
result.Should().BeEquivalentTo(new { Name = "John", Email = "john@example.com" });

// Exceções
await service.Invoking(s => s.GetUserAsync(999))
    .Should().ThrowAsync<NotFoundException>()
    .WithMessage("*not found*");

// Datas
user.CreatedAt.Should().BeCloseTo(DateTime.UtcNow, TimeSpan.FromSeconds(1));
subscription.ExpiryDate.Should().BeAfter(DateTime.UtcNow);
```

### Fixtures
```csharp
// Constructor fixture
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _repositoryMock;
    private readonly UserService _service;

    public UserServiceTests()
    {
        _repositoryMock = new Mock<IUserRepository>();
        _service = new UserService(_repositoryMock.Object);
    }

    [Fact]
    public async Task Test1() { /* usa _service e _repositoryMock */ }
}

// IClassFixture para fixture compartilhada
public class DatabaseFixture : IDisposable
{
    public ApplicationDbContext DbContext { get; }
    
    public DatabaseFixture()
    {
        var options = new DbContextOptionsBuilder<ApplicationDbContext>()
            .UseInMemoryDatabase($"TestDb_{Guid.NewGuid()}")
            .Options;
        DbContext = new ApplicationDbContext(options);
    }
    
    public void Dispose() => DbContext.Dispose();
}

public class UserRepositoryTests : IClassFixture<DatabaseFixture>
{
    private readonly DatabaseFixture _fixture;
    
    public UserRepositoryTests(DatabaseFixture fixture)
    {
        _fixture = fixture;
    }
}
```

## Exemplos Completos

### Teste de Service com Moq
```csharp
public class UserServiceTests
{
    [Fact]
    public async Task CreateUserAsync_ValidUser_SavesAndReturnsUser()
    {
        // Arrange
        var userRequest = new UserRequest 
        { 
            FirstName = "John", 
            LastName = "Doe",
            Email = "john@example.com" 
        };
        
        var repositoryMock = new Mock<IUserRepository>();
        var mapperMock = new Mock<IMapper>();
        
        var mappedUser = new User 
        { 
            Id = 1, 
            FirstName = "John", 
            LastName = "Doe",
            Email = "john@example.com" 
        };
        
        mapperMock
            .Setup(m => m.Map<User>(userRequest))
            .Returns(mappedUser);
        
        repositoryMock
            .Setup(r => r.AddAsync(It.IsAny<User>(), It.IsAny<CancellationToken>()))
            .Returns(Task.CompletedTask);
        
        var service = new UserService(repositoryMock.Object, mapperMock.Object);

        // Act
        var result = await service.CreateUserAsync(userRequest);

        // Assert
        result.Should().NotBeNull();
        result.Id.Should().Be(1);
        result.Email.Should().Be("john@example.com");
        
        repositoryMock.Verify(
            r => r.AddAsync(It.IsAny<User>(), It.IsAny<CancellationToken>()),
            Times.Once);
    }

    [Fact]
    public async Task GetUserAsync_UserNotFound_ThrowsNotFoundException()
    {
        // Arrange
        var repositoryMock = new Mock<IUserRepository>();
        repositoryMock
            .Setup(r => r.GetByIdAsync(It.IsAny<int>(), It.IsAny<CancellationToken>()))
            .ReturnsAsync((User?)null);
        
        var service = new UserService(repositoryMock.Object, null!);

        // Act & Assert
        await service.Invoking(s => s.GetUserAsync(999))
            .Should().ThrowAsync<NotFoundException>()
            .WithMessage("User*not found");
    }
}
```

### Teste com Bogus
```csharp
public class OrderServiceTests
{
    private readonly Faker<Order> _orderFaker;
    
    public OrderServiceTests()
    {
        _orderFaker = new Faker<Order>()
            .RuleFor(o => o.Id, f => f.Random.Guid())
            .RuleFor(o => o.OrderNumber, f => f.Random.AlphaNumeric(10))
            .RuleFor(o => o.TotalAmount, f => f.Finance.Amount(10, 1000))
            .RuleFor(o => o.Status, f => f.PickRandom<OrderStatus>());
    }

    [Fact]
    public void CalculateTotal_MultipleOrders_ReturnsCorrectSum()
    {
        // Arrange
        var orders = _orderFaker.Generate(10);
        var service = new OrderService();

        // Act
        var total = service.CalculateTotal(orders);

        // Assert
        total.Should().Be(orders.Sum(o => o.TotalAmount));
    }
}
```

### Theory com InlineData
```csharp
[Theory]
[InlineData("john@example.com", true)]
[InlineData("invalid-email", false)]
[InlineData("", false)]
[InlineData(null, false)]
[InlineData("@example.com", false)]
public void IsValidEmail_VariousInputs_ReturnsExpected(string email, bool expected)
{
    // Arrange
    var validator = new EmailValidator();

    // Act
    var result = validator.IsValidEmail(email);

    // Assert
    result.Should().Be(expected);
}
```

## Checklist de Implementação
- [ ] Classe de teste nomeada como `{Classe}Tests`
- [ ] Método usa padrão `Method_Scenario_Expected`
- [ ] Estrutura AAA clara (Arrange, Act, Assert)
- [ ] `[Fact]` para testes simples
- [ ] `[Theory]` + `[InlineData]` para parametrizados
- [ ] Mocks criados para todas as dependências
- [ ] `Setup` com `ReturnsAsync` para métodos async
- [ ] `Verify` para garantir chamadas esperadas
- [ ] FluentAssertions para todas as verificações
- [ ] Testes independentes (podem rodar em qualquer ordem)
- [ ] Testes rápidos (< 100ms)
- [ ] Bogus para gerar dados variados
- [ ] Fixtures quando há setup compartilhado
- [ ] `ThrowAsync` para testar exceções async
- [ ] Code coverage acima de 80%

## Diretrizes de Qualidade
- Um assert por conceito (testes focados)
- Evite lógica condicional nos testes
- Não teste detalhes de implementação
- Teste comportamento, não métodos privados
- Use nomes descritivos e auto-explicativos
- Mantenha testes pequenos e legíveis
- Evite duplicação com helper methods
- Mock apenas dependências externas
- Use InMemoryDatabase apenas para testes de integração
- Prefira testes unitários a testes de integração

## Pacotes NuGet
```xml
<PackageReference Include="xunit" Version="2.6.6" />
<PackageReference Include="xunit.runner.visualstudio" Version="2.5.6" />
<PackageReference Include="Moq" Version="4.20.70" />
<PackageReference Include="FluentAssertions" Version="6.12.0" />
<PackageReference Include="Bogus" Version="35.3.0" />
<PackageReference Include="coverlet.collector" Version="6.0.0" />
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.9.0" />
```

## Observações
- Sempre use async/await para métodos async
- CancellationToken deve ser passado com `It.IsAny<CancellationToken>()`
- Use `Should()` do FluentAssertions ao invés de `Assert` do xUnit
- Fixtures são compartilhadas, cuidado com estado mutável
- InlineData não suporta objetos complexos, use MemberData
- MockBehavior.Strict força setup de todos os métodos (útil para garantir nada extra é chamado)
