você é um engenheiro de software senior especialista no desenvolvimento C#.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso do C# cobrindo boas práticas com projetos .net.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: csharp-style-guide.mdc
Description: Guia para padrões de código C#, inclui nomenclatura, formatação, boas práticas e desenvolvimento em .NET
globs: ["**/*.cs"]
rules: @csharp-project-structure
alwaysApply: false
tags: ["csharp", "style", "best pratices"]
Author: Glauber Duma
Created: 2026-01
---

# C# Style Guide - Padrões e boas práticas

Este guia estabelece padrões consistentes para escrita de código C#, garantindo legibilidade, manutenabilidade e qualidade em todos os projetos.


## 🎯 **Convenções de Nomenclatura Genéricas**

### 1.1. Namespace Genéricos

```csharp
/*
Namespaces devem seguir o padrão PascalCase e refletir a estrutura do projeto.
Exemplo:
namespace MinhaEmpresa.MinhasAplicacoes.MinhasBibliotecas
*/
```

### 1.2. Classes e Interfaces Genéricas

```csharp

// ✅ Correto - Padrões genéricos para classes e interfaces

// Interfaces principais devem começar com "I" seguido de PascalCase.
public interface I{Feature}Service // ICacheService, IMessagingService

// Classes de implementação devem usar PascalCase.
public class {Feature}Service : I{Feature}Service // CacheService, MessagingService

// Classes auxiliares ou utilitárias devem usar sufixos descritivos.
public class {Feature}Helper // DateTimeHelper, StringHelper

// ❌ Incorreto - Nomenclaturas genéricas inadequadas
public interface {Feature}Interface // CacheInterface, MessagingClass
public class I{Feature}Implementation // ICacheImplementation, IMessagingImplementation

```


### 1.3. Modelos, DTOs e Entidades Genéricas

```csharp

// ✅ Correto - Modelos Genéricos

public class I{Feature}Request // CacheRequest, MessagingRequest
public class I{Feature}Response // CacheResponse, MessagingResponse
public class {Feature}Entity // CacheEntity, MessagingEntity

// ✅ Correto - Métodos descritivos e genéricos

Task<{Feature}Response> Get{Feature}Async(int id); // GetCacheAsync, GetMessagingAsync
Task Create{Feature}Async({Feature}Request request); // CreateCacheAsync, CreateMessagingAsync
Task Update{Feature}Async(int id, {Feature}Request request); // UpdateCacheAsync, UpdateMessagingAsync
Task Delete{Feature}Async(int id); // DeleteCacheAsync, DeleteMessagingAsync


```

### 1.4. Entidades

Para modelo de entidade/validator, reutilize a rule compartilhada `entity-validator.mdc` (sets privados, construtores validados com FluentValidation, métodos que revalidam). Nao duplique o snippet aqui.

## 2. **Estrutura de Classes Genéricas**

### 2.1. Organização de Members (Template)

```csharp
// ✅ Correto - Fields, properties, constructors, methods organizados devem estar entre regiões
#region Fields
private readonly I{Feature}Repository _{feature}Repository;
#endregion
#region Properties
public int Id { get; set; }
public string Name { get; set; }
#endregion
#region Constructors
public {Feature}Service(I{Feature}Repository {feature}Repository)
{
    _{feature}Repository = {feature}Repository;
}
#endregion
#region Public Methods
public async Task<{Feature}Response> Get{Feature}Async(int id)
{
    // implementação
}

#endregion
#region Private Methods
private void Validate{Feature}({Feature}Request request)
{
    // implementação
}
#endregion
````


### 2.2. Builder Patttern Genérico

```csharp
// ✅ Correto - Implementação genérica do Builder Pattern

<gerar aqui o código csharp>

```

## 3. Interfaces e Contratos Genéricos

### 3.1. Interface Principal (Template)

```csharp

// ✅ Correto - Interface principal genérica

public interface I{Feature}Service<T> : IDisposable
{
    <gerar aqui o código csharp>
}

## Style Guide Checklist (Qualquer SDK)

<Gerar aqui o checklist em markdown>


```

## 4. **Frameworks e Bibliotecas Recomendadas**

### 4.1. Bibliotecas ORM e Persistência de Dados

```csharp
// ✅ Correto - Sempre utilizar o padrão Repository com Unit of Work para persistência de dados, quando possível e viável

<gerar aqui o código csharp>

// ✅ Correto - Uso do Entity Framework Core para ORM para aplicações do tipo web e desktop

<gerar aqui o código csharp>

// ✅ Correto - Uso do Dapper Framework Core para ORM para aplicações do console

<gerar aqui o código csharp>

```

### 4.2. Bibliotecas de Log

```csharp
// ✅ Correto - Uso do Serilog e Prometheus para logging estruturado

<gerar aqui o código csharp>

```
### 4.3. Bibliotecas de Validação

```csharp
// ✅ Correto - Uso do FluentValidation para validação de modelos
<gerar aqui o código csharp>
```


### 4.4. Injeção de Dependência

```csharp
// ✅ Correto - Separe a configuração de injeção de dependência em uma classe dedicada, onde seus métodos são estáticos de extensão para IServiceCollection, sempre retornando o IServiceCollection para permitir encadeamento.

Exemplo:

public static class ServiceCollectionExtensions
{
    public static IServiceCollection Add{Feature}Services(this IServiceCollection services)
    {
        services.AddScoped<I{Feature}Service, {Feature}Service>();
        services.AddScoped<I{Feature}Repository, {Feature}Repository>();
        return services;
    }
}


```



## 8. Boas Práticas

### ✅ **DO`s - Práticas Recomendadas**

```csharp
✅ Use injeção de dependência para gerenciar dependências
✅ Sempre utilize async/await para operações assíncronas
✅ Utilize logging estruturado para monitoramento
✅ Implemente tratamento de erros consistente
✅ Utilize padrões de design apropriados (Factory, Singleton, Repository, etc.)
```
### ❌ **DONT`s - Práticas a Evitar**

```csharp
❌ Não use variáveis globais ou estáticas para estado compartilhado
❌ Não ignore exceções sem tratamento adequado
❌ Não faça consultas diretas ao banco de dados sem abstração
❌ Não misture lógica de negócios com lógica de apresentação
```


salve a regra na pasta csharp/general/csharp-style-guide.mdc
