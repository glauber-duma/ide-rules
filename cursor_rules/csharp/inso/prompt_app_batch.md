você é um engenheiro de software senior especialista no desenvolvimento C#.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso do C# cobrindo boas práticas com projetos .net.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: prompt_app_batch.mdc
Description: Guia para padrões de código C# para aplicações de processamento em lote, do tipo console application.
globs: ["**/*.cs"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "console application", "best pratices"]
Author: Glauber Duma
Created: 2026-01
---

# C# Batch Console Application - Padrões e boas práticas

Este guia estabelece padrões consistentes para escrita de código C#, garantindo:

**Eficiencia no uso de recursos** sempre utilizando uma unica conexão com o banco de dados por requisição, reutilizando-a durante todo o ciclo de vida da requisição.
**Tratamento robusto de erros** com logs detalhados e mecanismos de retry para operações críticas.
**Facilidade de manutenção** com código modular e bem documentado.
**Desempenho otimizado** para processamento em lote, minimizando o tempo de execução e o consumo de recursos, utilizando operações assíncronas (async/await) quando possível.
**Informação** sempre crie uma tela de apresentação informando o nome do aplicativo, versão, autor e data de compilação ao iniciar a aplicação.
**Padronização** utilizando a biblioteca System.CommandLine para o desenvolvimento de aplicações de linha de comando (CLI).
**Segurança** usando variáveis de ambiente para gerenciar configurações sensíveis, como strings de conexão e chaves de API, evitando hardcoding no código-fonte.
**Compatibilidade**sempre utilize a versão 8.0.0 da bibliotoca Npgsql.EntityFrameworkCore.PostgreSQL para integração com PostgreSQL em projetos Entity Framework Core e Dapper.
**Testabilidade** com a criação de testes unitários e de integração para garantir a qualidade do código e facilitar futuras alterações sempre utilizando Xuni e Moq.
**Escalabilidade** sempre gere o Dockerfile com as melhores práticas.
**CD/CD** sempre crie scripts de CI/CD para automatizar o build, testes e deploy da aplicação, utilizando gitactions.
**Sempre utlize @csharp-style-guide.mdc como base para desenvolvimento das regras de estilo C#.**
**Versão do .net SDK** sempre utilize a versão 9.0 do .net SDK para desenvolvimento de novas aplicações.
```

## 📂 **Estrutura de Projeto Batch**

### 1.1 Estrutura de Diretórios / Clean Architecture

```csharp
{projetct-root}/
|--- {project-name}.sln
|--- {project-name}.csproj
|--- Program.cs
|--- appsettings.json
|--- appsettings.Development.json
|--- README.md
|--- /src
|    |--- /Config                             // Configurações da aplicação
|    |      |--- AppSettings.cs
|    |      |--- DependencyInjection.cs
|    |--- /Core                               // Lógica de negócio e entidades
|    |      |--- /Entities
|    |      |      |--- {Feature}Entity.cs
|    |      |--- /Interfaces
|    |      |      |--- I{Feature}Service.cs
|    |--- /Infrastructure                     // Implementações de acesso a dados e serviços externos
|    |--- /Data                               // Acesso a dados
|    |      |--- /Migrations
|    |      |      |--- {timestamp}_InitialCreate.cs
|    |      |--- AppDbContext.cs
|    |      |--- IBaseRepository.cs          // Repositório base genérico
|    |      |--- IBaseRepository.cs          // Implementação do repositório base
|    |      |--- /Repositories               // Repositórios específicos
|    |      |      |--- {Feature}Repository.cs
|    |      |--- /Configurations
|    |      |      |--- {Feature}Configuration.cs // Quando utilizado Entity Framework Core aqui ficam as classe de mapeamento IEntityTypeConfiguration<T> sendo uma classe por entidade/tabela. Quando utilizado Dapper aqui ficam as classes de mapeamento manual.
|    |--- /Services
|    |      |--- I{Feature}Service.cs.       // Serviços de negócio
|    |      |--- {Feature}Service.cs
|    |--- /Models                           // Modelos de dados
|    |      |--- {Feature}Model.cs
|    |--- /Utils                            // Classes utilitárias
|           |--- {Feature}Helper.cs
|--- /scripts                              // Scripts de automação
|    |--- build.sh
|--- /docker                               // Arquivos Docker
|    |--- Dockerfile
|    |--- docker-compose.yml
|--- /docs                                 // Documentação do projeto
|    |--- architecture.md
|    |--- design-patterns.md
|--- /tests                                // Testes automatizados
     |--- /UnitTests
     |      |--- {Feature}ServiceTests.cs
     |--- /IntegrationTests
           |--- {Feature}RepositoryTests.cs


```

## ⚙️​ **Configuração de Aplicação Batch**

### 2.1 Configuração de appsettings.json e injeção de dependências

```csharp
// src/config/appsettings.cs
/*

// O arquivo AppSettings.cs deve conter a classe de configuração e métodos que carregam as configurações do appsettings.json usando o padrão Options do .NET.

*/
<gerar aqui o código csharp>

```

### 2.1 Configuração de appsettings.json e injeção de dependências

```csharp
// src/config/AppSettings.cs
/*

// O arquivo AppSettings.cs deve conter a classe de configuração e métodos que carregam as configurações do appsettings.json usando o padrão Options do .NET.

*/
<gerar aqui o código csharp>

```


```csharp
// src/Config/DependencyInjection.cs
/*
O arquivo DependencyInjection.cs deve conter métodos de extensão para IServiceCollection que configurem:
- Serviços da aplicação (AddApplicationServices)
- Infraestrutura de dados (AddInfrastructure) incluindo DbContext do EF Core com Npgsql 8.0.0
- Logging com Serilog (AddLogging)
- Utilitários (AddUtilities)
*/
<gerar aqui o código csharp>

```

### 2.2 Program.cs - Ponto de Entrada com CLI

```csharp
// Program.cs
/*
O arquivo Program.cs deve conter:
- Método Main com System.CommandLine configurado
- Exibição de banner com nome da aplicação, versão, autor e data de build
- Configuração do Host com IHostBuilder
- ConfigureAppConfiguration com appsettings.json, appsettings.{Environment}.json e variáveis de ambiente
- ConfigureServices chamando os métodos de DependencyInjection
- Comandos CLI (ex: process, migrate, seed, etc.)
- Opções CLI (ex: --batch-size, --dry-run)
- Tratamento de erros com logging
*/
<gerar aqui o código csharp>

```

### 2.3 appsettings.json

```json
/*
Configurações da aplicação incluindo:
- AppSettings (ApplicationName, Version, BatchSize, MaxRetryAttempts, RetryDelaySeconds)
- ConnectionStrings (DefaultConnection para PostgreSQL)
- Serilog (MinimumLevel, WriteTo)
*/
<gerar aqui o arquivo json>

```

### 2.4 appsettings.Development.json

```json
/*
Configurações específicas de desenvolvimento com valores menores para batch-size e logging mais verboso
*/
<gerar aqui o arquivo json>

```

## 🏗️ **Core - Entidades e Interfaces**

### 3.1 Core/Entities

```csharp
// src/Core/Entities/{Feature}Entity.cs
/*
Classes de entidades que representam as tabelas do banco de dados.
Exemplo: UserEntity, OrderEntity
Propriedades:
- Id (int)
- Campos de negócio
- CreatedAt, UpdatedAt, DeletedAt (DateTime?)
- Navigation properties (ICollection<T>)
*/
<gerar aqui o código csharp para UserEntity e OrderEntity como exemplos>

```

### 3.2 Core/Interfaces

```csharp
// src/Core/Interfaces/I{Feature}Service.cs
/*
Interfaces de serviços de negócio.
Exemplo: IUserService, IOrderService
Métodos assíncronos para:
- GetByIdAsync
- GetAllAsync / GetPagedAsync
- CreateAsync
- UpdateAsync
- DeleteAsync
- Métodos de negócio específicos (ex: ProcessOrdersAsync)
*/
<gerar aqui o código csharp para IUserService e IOrderService como exemplos>

```

## 🗄️ **Data - Acesso a Dados**

### 4.1 Data/AppDbContext.cs

```csharp
// src/Data/AppDbContext.cs
/*
Contexto do Entity Framework Core que:
- Herda de DbContext
- Define DbSet<T> para cada entidade
- Sobrescreve OnModelCreating para aplicar configurações
- Sobrescreve SaveChangesAsync para atualizar timestamps automaticamente
*/
<gerar aqui o código csharp>

```

### 4.2 Data/IBaseRepository.cs (Interface)

```csharp
// src/Data/IBaseRepository.cs
/*
Interface genérica de repositório com operações CRUD básicas:
- Read: GetByIdAsync, GetAllAsync, FindAsync, FirstOrDefaultAsync, CountAsync, ExistsAsync
- Write: AddAsync, AddRangeAsync, UpdateAsync, UpdateRangeAsync, DeleteAsync, DeleteRangeAsync
- Batch: GetPagedAsync
Todos os métodos devem aceitar CancellationToken
*/
<gerar aqui o código csharp>

```

### 4.3 Data/BaseRepository.cs (Implementação)

```csharp
// src/Data/BaseRepository.cs
/*
Implementação genérica do IBaseRepository<T> que:
- Recebe AppDbContext via construtor
- Implementa todos os métodos da interface usando DbSet<T>
- Usa async/await para todas as operações
- Salva mudanças após operações de escrita
*/
<gerar aqui o código csharp>

```

### 4.4 Data/Repositories - Repositórios Específicos

```csharp
// src/Data/Repositories/I{Feature}Repository.cs
/*
Interfaces de repositórios específicos que herdam de IBaseRepository<T>
e adicionam métodos customizados.
Exemplo: IUserRepository, IOrderRepository
*/
<gerar aqui o código csharp para IUserRepository e IOrderRepository>

```

```csharp
// src/Data/Repositories/{Feature}Repository.cs
/*
Implementações de repositórios específicos que herdam de BaseRepository<T>
e implementam a interface específica.
Exemplo: UserRepository, OrderRepository
Métodos customizados usando LINQ, Include, Where, OrderBy, etc.
*/
<gerar aqui o código csharp para UserRepository e OrderRepository>

```

### 4.5 Data/Configurations - Mapeamento EF Core

```csharp
// src/Data/Configurations/{Feature}Configuration.cs
/*
Classes de configuração que implementam IEntityTypeConfiguration<T> para:
- Definir nome da tabela (ToTable)
- Configurar chave primária (HasKey)
- Configurar propriedades (HasColumnName, HasMaxLength, IsRequired, HasDefaultValue)
- Definir índices (HasIndex, IsUnique)
- Configurar relacionamentos (HasMany, WithOne, HasForeignKey, OnDelete)
- Conversões de enum (HasConversion)
*/
<gerar aqui o código csharp para UserConfiguration e OrderConfiguration>

```

## 🔧 **Services - Lógica de Negócio**

### 5.1 Services/{Feature}Service.cs

```csharp
// src/Services/{Feature}Service.cs
/*
Implementação de serviços de negócio que:
- Implementam a interface I{Feature}Service
- Recebem repositórios e ILogger via construtor
- Contém lógica de negócio
- Fazem logging estruturado
- Implementam retry policies com Polly
- Tratam exceções apropriadamente
- Validam dados antes de persistir
Exemplo: UserService, OrderService
*/
<gerar aqui o código csharp para UserService e OrderService com retry policy usando Polly>

```

## 🛠️ **Utils - Classes Utilitárias**

### 6.1 Utils/IDateTimeProvider.cs e DateTimeProvider.cs

```csharp
// src/Utils/IDateTimeProvider.cs e DateTimeProvider.cs
/*
Interface e implementação para abstrair DateTime.UtcNow para facilitar testes.
Propriedades: UtcNow, Now, UtcNowOffset
*/
<gerar aqui o código csharp>

```

### 6.2 Utils/RetryHelper.cs

```csharp
// src/Utils/RetryHelper.cs
/*
Classe estática com métodos auxiliares para criar retry policies usando Polly:
- CreateRetryPolicy (retorna AsyncRetryPolicy)
- ExecuteWithRetryAsync<T> (executa ação com retry)
Usar exponential backoff para delay entre tentativas
*/
<gerar aqui o código csharp>

```

## 🐳 **Docker e Scripts**

### 7.1 Dockerfile

```dockerfile
# docker/Dockerfile
/*
Dockerfile multi-stage com:
- Stage 1 (build): SDK 9.0, restore, build
- Stage 2 (publish): publish da aplicação
- Stage 3 (final): Runtime 9.0, usuário não-root, ENTRYPOINT
*/
<gerar aqui o Dockerfile>

```

### 7.2 docker-compose.yml

```yaml
# docker/docker-compose.yml
/*
Docker compose com:
- Service batch-app (build context, environment variables, depends_on postgres)
- Service postgres (imagem postgres:16-alpine, variáveis de ambiente, volumes, ports)
- Networks
- Volumes para persistência de dados PostgreSQL
*/
<gerar aqui o docker-compose.yml>

```

### 7.3 scripts/build.sh

```bash
#!/bin/bash
# scripts/build.sh
/*
Script bash para build automatizado com:
- dotnet clean
- dotnet restore
- dotnet build
- dotnet test
- dotnet publish
- Mensagens informativas de progresso
*/
<gerar aqui o script bash>

```

## 🧪 **Testes**

### 8.1 UnitTests/{Feature}ServiceTests.cs

```csharp
// tests/UnitTests/{Feature}ServiceTests.cs
/*
Testes unitários usando Xunit e Moq que:
- Mocam dependências (repositórios, logger)
- Testam métodos do service isoladamente
- Usam [Fact] para testes
- Organizam testes em Arrange, Act, Assert
- Verificam chamadas aos mocks com Verify
Exemplo: UserServiceTests
*/
<gerar aqui o código csharp para UserServiceTests>

```

### 8.2 IntegrationTests/{Feature}RepositoryTests.cs

```csharp
// tests/IntegrationTests/{Feature}RepositoryTests.cs
/*
Testes de integração usando Xunit que:
- Usam InMemoryDatabase do EF Core
- Testam repositórios com DbContext real
- Implementam IDisposable para cleanup
- Testam queries complexas e relacionamentos
Exemplo: UserRepositoryTests
*/
<gerar aqui o código csharp para UserRepositoryTests>

```

## 🚀 **CI/CD com GitHub Actions**

### 9.1 .github/workflows/ci-cd.yml

```yaml
# .github/workflows/ci-cd.yml
/*
Workflow do GitHub Actions com:
- Job build-and-test: checkout, setup .NET 9.0, restore, build, test, coverage
- Job docker-build: build e push da imagem Docker
- Job deploy: deploy para produção (condicional, apenas main branch)
- Uso de secrets para credenciais
- Upload de resultados de testes e cobertura
*/
<gerar aqui o workflow yaml>

```

## 📝 **Documentação**

### 10.1 README.md

```markdown
# README.md
/*
Documentação do projeto incluindo:
- Título e descrição
- Requisitos (versões de .NET, PostgreSQL, Docker)
- Instruções de instalação (dotnet CLI e Docker)
- Exemplos de uso dos comandos CLI
- Como executar testes
- Estrutura do projeto
- Configuração
- Licença e autor
*/
<gerar aqui o README.md>

```

### 10.2 {project-name}.csproj

```xml
<!--{project-name}.csproj-->
/*
Arquivo de projeto .NET com:
- TargetFramework: net9.0
- Nullable: enable
- PackageReferences:
  - Npgsql.EntityFrameworkCore.PostgreSQL (8.0.0)
  - Microsoft.EntityFrameworkCore.Design (9.0.0)
  - System.CommandLine (2.0.0-beta4.22272.1)
  - Microsoft.Extensions.* (Configuration, DI, Hosting)
  - Serilog.* (Core, Sinks, Enrichers)
  - Polly (8.x)
  - Xunit, Moq, FluentAssertions (para testes)
- ItemGroup para copiar appsettings*.json para output
*/
<gerar aqui o arquivo .csproj>

```

## ✅ **Boas Práticas - DO's**

### DO's - Práticas Recomendadas

```csharp
/*
Liste e demonstre com exemplos de código:

✅ DO: Use async/await para todas operações I/O
✅ DO: Use logging estruturado com Serilog
✅ DO: Use injeção de dependência via construtor
✅ DO: Implemente retry policies com Polly para operações críticas
✅ DO: Use CancellationToken em métodos async públicos
✅ DO: Valide entrada antes de processar
✅ DO: Use padrão Repository para acesso a dados
✅ DO: Use variáveis de ambiente para configurações sensíveis
✅ DO: Implemente testes unitários e de integração
✅ DO: Use transactions para operações em lote
*/
<gerar aqui exemplos de código para cada DO com comentários explicativos>

```

## ❌ **Boas Práticas - DON'Ts**

### DON'Ts - Práticas a Evitar

```csharp
/*
Liste e demonstre com exemplos de código (correto vs incorreto):

❌ DON'T: Não use .Result ou .Wait() em código async
❌ DON'T: Não ignore exceções sem logging
❌ DON'T: Não faça queries N+1 (use Include/ThenInclude)
❌ DON'T: Não retorne IQueryable de repositórios
❌ DON'T: Não use DbContext diretamente em services
❌ DON'T: Não hardcode connection strings ou secrets
❌ DON'T: Não deixe DbContext aberto por muito tempo
❌ DON'T: Não use variáveis estáticas para estado compartilhado
*/
<gerar aqui exemplos de código mostrando o incorreto e o correto para cada DON'T>

```

## 📋 **Checklist de Implementação**

```markdown
/*
Gere um checklist completo para implementação de aplicação batch console:

### Estrutura
- [ ] Estrutura de diretórios Clean Architecture criada
- [ ] .csproj com todas as dependências
- [ ] appsettings.json e appsettings.Development.json

### Configuração
- [ ] AppSettings.cs criado
- [ ] DependencyInjection.cs com todos os registros
- [ ] Program.cs com CLI e banner

### Core
- [ ] Entidades criadas
- [ ] Interfaces de serviços criadas

### Data
- [ ] AppDbContext configurado
- [ ] BaseRepository implementado
- [ ] Repositórios específicos implementados
- [ ] Configurações EF Core para cada entidade

### Services
- [ ] Serviços implementados com logging
- [ ] Retry policies configuradas

### Testes
- [ ] Testes unitários implementados
- [ ] Testes de integração implementados

### DevOps
- [ ] Dockerfile criado
- [ ] docker-compose.yml criado
- [ ] Script de build criado
- [ ] GitHub Actions workflow criado

### Documentação
- [ ] README.md completo
*/
<gerar aqui o checklist completo em markdown>

```

---

**salve a regra na pasta csharp/inso/app-batch.mdc**
```