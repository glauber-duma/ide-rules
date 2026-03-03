# ide-rules

Conjunto de regras para uso no VS Code e Cursor, com guias completos e prompts para padronizar boas praticas em C# e Python (com foco em FastAPI).

## Visao geral

Este repositorio organiza rules (`.mdc`), prompts (`.md`) e **skills** (`.mdc`) para:

- Padroes de codigo e estilo.
- Arquitetura e organizacao de projetos.
- Boas praticas especificas (API, middleware, validacao, seguranca, observabilidade, Docker, testes, etc.).
- Templates de referencia prontos para equipes usando **.NET 9 / C# 12** com Clean Architecture, CQRS (MediatR), Result Pattern, FluentValidation, EF Core 9, Serilog, OpenTelemetry e xUnit.

## Estrutura

```
cursor_rules/
    csharp/
        AGENTS.MD                       ← catálogo mestre (alwaysApply: true)
        general/
            csharp-style-guide.mdc
            entity-validator.mdc
            efcore-repository.mdc
            dapper-repository.mdc
            fluentvalidation.mdc
            use-cases.mdc
            api-controllers.mdc
            observability.mdc
            docker-containers.mdc
            unit-testing.mdc
        inso/
            app-batch.mdc
        prompts/
            prompt-api-mvc-auth-observability.md
            prompt-api-mvc-simple-observability.md
            prompt-console-batch-complete-observability.md
            prompt-console-batch-simple-observability.md
            prompt-desktop-blazor-complete-auth-observability.md
            prompt-desktop-blazor-complete-observability.md
            prompt-desktop-blazor-simple-auth-observability.md
            prompt-desktop-blazor-simple-observability.md
            prompt-microservices-complete-observability.md
            prompt-microservices-dapr-complete-observability.md
            prompt-microservices-dapr-simple-observability.md
            prompt-microservices-simple-observability.md
            prompt-web-mvc-auth-observability.md
            prompt-web-mvc-simple-observability.md
        rules/
            api-mvc-auth-observability.mdc
            api-mvc-simple-observability.mdc
            console-batch-complete-observability.mdc
            console-batch-simple-observability.mdc
            desktop-blazor-complete-auth-observability.mdc
            desktop-blazor-complete-observability.mdc
            desktop-blazor-simple-auth-observability.mdc
            desktop-blazor-simple-observability.mdc
            microservices-complete-observability.mdc
            microservices-dapr-complete-observability.mdc
            microservices-dapr-simple-observability.mdc
            microservices-simple-observability.mdc
            web-mvc-auth-observability.mdc
            web-mvc-simple-observability.mdc
        skills/
            entity-domain.mdc
            repository-efcore.mdc
            repository-dapper.mdc
            use-case-cqrs.mdc
            api-controller.mdc
            observability-setup.mdc
            docker-setup.mdc
            unit-test-scaffold.mdc
            scaffold-api-project.mdc
    python/
        api/
            fastapi-project.mdc
            fastapi-endpoints.mdc
            fastapi-validation.mdc
            fastapi-middleware.mdc
            fastapi-exception.mdc
            fastapi-security.mdc
        general/
            python-style-guide.mdc
            python-cache.mdc
            python-database.mdc
            python-dependencies.mdc
            python-environments.mdc
            python-messaging.mdc
        skills/
            clean-architecture.mdc
            dapr-integration.mdc
            dependency-injection.mdc
```

## O que sao os arquivos

| Tipo | Descricao |
|---|---|
| `AGENTS.MD` | Catalogo mestre com `alwaysApply: true` — o agente carrega automaticamente em toda sessao C# |
| `.mdc` (rules) | Regras declarativas de arquitetura e estilo — o agente consulta ao raciocinar |
| `.mdc` (skills) | Guias procedurais com codigo pronto para gerar — o agente executa ao criar artefatos |
| `.md` (prompts) | Prompts de apoio para gerar ou expandir rules existentes |

## Como usar

1. O `AGENTS.MD` e carregado automaticamente em toda sessao — nao e necessario referenciar manualmente.
2. Para gerar um artefato especifico (entidade, repositorio, controller, etc.), invoque a **skill** correspondente.
3. Para scaffolding completo de projeto, use a skill `scaffold-api-project` — ela orquestra todas as demais na ordem correta.
4. Para expandir ou criar novas rules, use os prompts em `prompts/`.

## Rules disponiveis (resumo)

### C# / General

| Rule | Descricao |
|---|---|
| `csharp-style-guide` | Guia de estilo C# 12/.NET 9: namespaces file-scoped, primary constructors, collection expressions, etc. |
| `entity-validator` | Entidade de dominio com setters privados e validacao FluentValidation embutida |
| `efcore-repository` | Repositorio generico EF Core (`IRepository<TClass, TType>` + paginacao + `IEntityTypeConfiguration`) |
| `dapper-repository` | Repositorio Dapper para modelos simples sem tracking/lazy loading |
| `fluentvalidation` | Padroes de validators com AbstractValidator, RuleFor e mensagens padronizadas |
| `use-cases` | CQRS com MediatR: Commands, Queries, Handlers e Pipeline Behaviors |
| `api-controllers` | BaseApiController, Result Pattern (RFC 7807), versionamento de API |
| `observability` | Serilog, OpenTelemetry, CorrelationId Middleware e Health Checks |
| `docker-containers` | Dockerfile multi-stage non-root e docker-compose para .NET |
| `unit-testing` | xUnit + Moq + Bogus + FluentAssertions, padrao AAA e Builder Pattern |

### C# / Rules de Arquitetura (14 combinacoes)

| Rule | Descricao |
|---|---|
| `api-mvc-simple-observability` | API REST sem autenticacao, com observabilidade |
| `api-mvc-auth-observability` | API REST com JWT, com observabilidade |
| `web-mvc-simple-observability` | MVC server-side sem autenticacao |
| `web-mvc-auth-observability` | MVC server-side com Identity |
| `console-batch-simple-observability` | Batch/ETL simples |
| `console-batch-complete-observability` | Batch/ETL com resiliencia e mensageria |
| `desktop-blazor-simple-observability` | Blazor Hybrid sem auth |
| `desktop-blazor-complete-observability` | Blazor Hybrid com suporte offline |
| `desktop-blazor-simple-auth-observability` | Blazor Hybrid com auth |
| `desktop-blazor-complete-auth-observability` | Blazor Hybrid completo com auth e offline |
| `microservices-simple-observability` | Microsservicos sem Dapr |
| `microservices-complete-observability` | Microsservicos com Polly, Kafka e Docker |
| `microservices-dapr-simple-observability` | Microsservicos com Dapr (sidecar, pub/sub, state) |
| `microservices-dapr-complete-observability` | Microsservicos Dapr completos com resiliencia |

### C# / Skills (codigo gerado pelo agente)

| Skill | O que gera |
|---|---|
| `entity-domain` | EntityBase, entidade com FluentValidation, Value Objects e Domain Events |
| `repository-efcore` | IRepository, EfRepository, AppDbContext, IEntityTypeConfiguration e DI |
| `repository-dapper` | IDapperRepository, DapperRepository, repositorio especifico e DI |
| `use-case-cqrs` | Command/Query/Handler, ValidationBehavior, LoggingBehavior e DI |
| `api-controller` | Result/Error primitivos, BaseApiController, ApiProblemDetails e CRUD controller |
| `observability-setup` | Serilog, OpenTelemetry, CorrelationIdMiddleware, Health Checks e Program.cs |
| `docker-setup` | Dockerfile (multi-stage, non-root), docker-compose, .dockerignore e .env |
| `unit-test-scaffold` | Projeto xUnit, UserBuilder (Bogus), testes de handler/entidade/validator |
| `scaffold-api-project` | Arvore de decisao, estrutura completa, CLI commands e checklist de prontidao |

### Python / FastAPI

| Rule | Descricao |
|---|---|
| `fastapi-project` | Estrutura de projeto e configuracao base |
| `fastapi-endpoints` | Padroes de endpoints REST |
| `fastapi-validation` | Validacao com Pydantic |
| `fastapi-middleware` | Middlewares e interceptadores |
| `fastapi-exception` | Tratamento de excecoes e handlers |
| `fastapi-security` | Seguranca, autenticacao e autorizacao |

### Python / General

| Rule | Descricao |
|---|---|
| `python-style-guide` | Convencoes de estilo e type hints |
| `python-cache` | Estrategias de cache |
| `python-database` | Integracao com banco de dados e migrations |
| `python-dependencies` | Gerenciamento de dependencias |
| `python-environments` | Ambientes e setups |
| `python-messaging` | Mensageria e eventos |

### Python / Skills

| Skill | Descricao |
|---|---|
| `clean-architecture` | Principios e estrutura de Clean Architecture |
| `dependency-injection` | DI com dependency-injector |
| `dapr-integration` | Padroes com Dapr |

## Contribuicao

- Mantenha consistencia com as rules existentes.
- Prefira adicionar exemplos completos e prontos para uso.
- Skills devem seguir o padrao: Visao Geral → Estrutura de Arquivos → Passo a Passo → Exemplos → Checklist → NuGet → Rules Relacionadas.
- Evite duplicar definicoes entre skills (ex.: `PagedResult<T>` e definido apenas em `repository-efcore`).

## Licenca

Uso interno.
