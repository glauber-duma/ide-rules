
Você é um engenheiro de software senior muito experiente. 
É especilista em C#, inteligência artificial, engenharia de prompts, etc.

Foi solicitado que você desenvolva `prompts` para geração de regras de IDEs (Visual Studio, JetBrains Rider, etc) para projetos em C#.
O objetivo é criar regras que ajudem a manter a consistência, qualidade e boas práticas de codificação em projetos C#.
Você deve gerar regras para os tipos de projetos listados abaixo, considerando as premissas fornecidas, restrições e boas práticas de desenvolvimento de software.

# Tipos de projetos 

- aplicatição web simples mvc sem autenticação e com observabilidade
- aplicatição web simples mvc com autenticação, autorização e com observabilidade
- API simples mvc sem autenticação e com observabilidade
- API simples mvc com autenticação, autorização e com observabilidade
- Microserviços simples usando Dapr e com observabilidade
- Microserviços completo usando Dapr e com observabilidade
- Microserviços simples sem uso de Dapr e com observabilidade
- Microserviços completo sem uso de Dapr e com observabilidade
- Aplicação console simples com observabilidade com foco em integração de sistemas legados (processos batch)
- Aplicação console completa com observabilidade com foco em integração de sistemas legados (processos batch)
- Aplicação desktop simples com observabilidade 
- Aplicação desktop completa com observabilidade
- Aplicação desktop simples com autenticação, autorização e com observabilidade
- Aplicação desktop completa com autenticação, autorização e com observabilidade

# **Premissas**
- Use a versão 9 do .net framework
- Use C# como linguagem de programação
- Organize o código em pastas de acorodo com o tipo de projeto
- Todos tipos de projetos devem conter regras de testes unitários e de integração(quando aplicável) 
- A pasta raiz é `/Users/glauberduma/ide-rules/cursor_rules/csharp``


# **Restrições**
- Não use bibliotecas ou frameworks proprietários ou pagos
- Use apenas bibliotecas ou frameworks open source ou nativos do .net framework
- Arquivos de prompts devem ser gerados com a extensão `.md`

# **Boas práticas**

É desejavel que os prompts gerem regras que sigam as boas práticas abaixo:
- Use clean code e clean architecture;
- Siga os princípios SOLID, DRY (Don't Repeat Yourself) e KISS (Keep It Simple, Stupid)
- Use o padrão de projeto repository para acesso a dados
- Use o padrão de projeto unit of work para transações
- Use injeção de dependência para gerenciar as dependências entre as classes
- Use o padrão de projeto mediator para comunicação entre os componentes
- Use o padrão de projeto factory para criação de objetos complexos
- Use o padrão de projeto singleton para classes que devem ter apenas uma instância
- Use o padrão de projeto decorator para adicionar funcionalidades a classes existentes
- Use o padrão de projeto strategy para implementar algoritmos intercambiáveis
- Use o padrão de projeto observer para notificação de eventos
- Use o padrão de projeto command para encapsular solicitações como objetos
- Use o padrão de projeto adapter para compatibilidade entre interfaces incompatíveis


**Analise o prjeto existente para que você entenda o contexto do que estou solcitando.
Primeiro gere todos os prompts necessários para criar regras de IDEs (Visual Studio, JetBrains Rider, etc) para todos os tipos de projetos listados acima.
Depois execute cada prompt individualmente e gere os arquivos de regras de IDEs.
Por ultimo valide se os arquivos gerados estão corretos e completos.
Se estiverem corretos e completos, atualize o arquivo README.md.

Qualquer dúvida que você tiver sobre o contexto, me pergunte antes de gerar os prompts.

**

Algumas observações importantes:

- Não encontrei nada sobre entidades que refletem a estrutura da tabela do banco de dados.
-  Elas devem ter todos seus sets private, um construtor simples e um construtor sobrecarregado que recebe todos os atributos.
-  Elas devem implementar validações simples no construtor, utilizando FluentValidation.
- Sempre que possível utilize records para representar entidades imutáveis.
- Utilize #region/endregion para organizar o código em seções lógicas.
- Utilize comentários para documentar classes, métodos e propriedades públicas.
- Não encontrei nenhum exemplo e nada falando sobre repositórios, quanto a isso eu desejo:
- Crie um repositório genérico base com as operações CRUD básicas, um método de filtro e um método de filtro com paginação.
- Crie interfaces para todos os repositórios.
- Crie implementações concretas para todos os repositórios.
- Utilize EF Core como ORM padrão, exceto para aplicações simples que devem utilizar Dapper/ Dappercontrib como ORM para acesso.

Ainda acho que as regras estão muito pobres em exemplos de código, gostaria que você incluísse mais exemplos de código prontos para uso, para que conseguimos padronizar de acordo como eu gosto de desenvolver o código.


Desejo que o item 5 seja desta forma:

## 5) Entidades (EF Core) com validacao
```csharp
public class Product
{
    #region Properties
    public Guid Id { get; private set; }
    public string Name { get; private set; }
    public decimal Price { get; private set; }
    #endregion

    #region Constructors
    private Product() { }

    public Product(Guid id, string name, decimal price)
    {
        Id = id == Guid.Empty ? Guid.NewGuid() : id;
        Name = name;
        Price = price;
        new ProductValidator().ValidateAndThrow(this);
    }
    #endregion

    #region Public Methods
    public void UpdatePrice(decimal price)
    {
        Price = price;
        new ProductValidator().ValidateAndThrow(this);
    }
    #endregion
}

public class ProductValidator : AbstractValidator<Product>
{
    public ProductValidator()
    {
        RuleFor(x => x.Name).NotEmpty().MaximumLength(200);
        RuleFor(x => x.Price).GreaterThan(0);
    }
}
```

Na parte do repositório genérico desejo que seja assim:

public interface IRepository<TClass, TType> where TClass : class
{
    Task<TClass?> GetByIdAsync(TType id, CancellationToken ct);
    Task<IReadOnlyList<TClass>> ListAsync(Expression<Func<TClass, bool>>? filter, CancellationToken ct);
    Task<PagedResult<TClass>> PagedAsync(Expression<Func<TClass, bool>>? filter, int page, int pageSize, CancellationToken ct);
    Task AddAsync(TClass entity, CancellationToken ct);
    Task UpdateAsync(TClass entity, CancellationToken ct);
    Task DeleteAsync(TClass entity, CancellationToken ct);
}

Inclua exemplo de repositório genérico Dapper para aplicações simples com as mesmas operações (CRUD, filtro, filtro paginado). Aplicação simples: modelo de dados pequeno, poucos relacionamentos, sem necessidade de recursos avançados de ORM (tracking de mudanças, lazy loading, etc.).


Reforços obrigatórios para todas as regras geradas:
- Sempre usar o item 5 com o modelo de entidade acima (regiões, sets privados, validação FluentValidation no construtor e nos métodos públicos).
- Repositório genérico EF Core deve seguir a assinatura `IRepository<TClass, TType>` (CRUD, filtro, paginação) com implementação `EfRepository<TClass, TType>`.
- Incluir repositório genérico Dapper para aplicações simples com a mesma superfície de operações; reafirme a definição de “aplicação simples” (modelo pequeno, poucos relacionamentos, sem tracking/lazy loading).
- Exemplos de código devem ser completos/prontos para uso (ex.: console-batch-simple-observability.mdc com jobs/handlers/use cases prontos).
- Controllers que usam Mediator devem seguir abordagem de use cases (controller chama use case/handler; use case chama repositórios/serviços); melhore os exemplos de controllers para refletir isso.
- Sempre atualizar o README.md ao final, registrando EF Core como padrão, Dapper para cenários simples, e que os prompts já contemplam os padrões de entidades/repos genéricos e uso de Mediator com use cases.
- Centralize o snippet de entidade/validator em csharp/general/entity-validator.mdc e mantenha o prompt correspondente (csharp/general/prompt_entity_validator.md) alinhado para reuso em todas as rules.
- Centralize o snippet de repositorio EF Core em csharp/general/efcore-repository.mdc e o prompt correspondente (csharp/general/prompt_efcore_repository.md) para reuso nas regras que exigem `IRepository<TClass, TType>` / `EfRepository<TClass, TType>`.
- Centralize o snippet de repositorio Dapper em csharp/general/dapper-repository.mdc e o prompt correspondente (csharp/general/prompt_dapper_repository.md) para reuso nas regras que exigem `IDapperRepository<TClass, TType>` / `DapperRepository<TClass, TType>` em aplicacoes simples.

Analise o que estou pedindo e providencie todos os ajustes necessários.