voce e um engenheiro de software senior especializado em C# e EF Core.
voce deve gerar uma rule reutilizavel para padronizar o repositorio generico EF Core.
Siga a estrutura abaixo e preencha o conteudo conforme indicado.

---
Rule: efcore-repository.mdc
Description: Snippet padrao de repositorio generico EF Core com CRUD, filtro e paginacao, reutilizavel em outras rules.
globs: ["**/*.cs"]
rules: @csharp-style-guide, @entity-validator
alwaysApply: false
tags: ["csharp", "efcore", "repository", "data-access"]
Author: Glauber Duma
Created: 2026-02
---

# Repositorio generico EF Core

## 1) Objetivo
Explique que a rule centraliza a interface `IRepository<TClass, TType>` e a implementacao `EfRepository<TClass, TType>` (CRUD, filtro, paginacao). Reforce que EF Core e o padrao; Dapper apenas para aplicacoes simples. Mencione que `DbContext` deve carregar configuracoes via `ApplyConfigurationsFromAssembly` e que cada `IEntityTypeConfiguration` fica em arquivo separado.

## 2) Snippet base (EF Core)
Inclua exatamente o codigo abaixo:
```csharp
public interface IRepository<TClass, TType> where TClass : class
{
  Task<TClass?> GetByIdAsync(TType id, CancellationToken ct);
  Task<IReadOnlyList<TClass>> ListAsync(Expression<Func<TClass, bool>>? filter, CancellationToken ct);
  Task<PagedResult<TClass>> PagedAsync(Expression<Func<TClass, bool>>? filter, int page, int pageSize, CancellationToken ct);
  Task AddAsync(TClass entity, CancellationToken ct);
  Task UpdateAsync(TClass entity, CancellationToken ct);
  Task DeleteAsync(TClass entity, CancellationToken ct);
}

public class EfRepository<TClass, TType> : IRepository<TClass, TType> where TClass : class
{
  private readonly AppDbContext _db;
  private readonly DbSet<TClass> _set;
  public EfRepository(AppDbContext db) { _db = db; _set = db.Set<TClass>(); }

  public Task<TClass?> GetByIdAsync(TType id, CancellationToken ct) => _set.FindAsync(new object[] { id! }, ct).AsTask();
  public async Task<IReadOnlyList<TClass>> ListAsync(Expression<Func<TClass, bool>>? filter, CancellationToken ct) => filter is null ? await _set.ToListAsync(ct) : await _set.Where(filter).ToListAsync(ct);
  public async Task<PagedResult<TClass>> PagedAsync(Expression<Func<TClass, bool>>? filter, int page, int pageSize, CancellationToken ct)
  {
    var query = filter is null ? _set.AsQueryable() : _set.Where(filter);
    var total = await query.CountAsync(ct);
    var items = await query.Skip((page - 1) * pageSize).Take(pageSize).ToListAsync(ct);
    return new PagedResult<TClass>(items, total, page, pageSize);
  }
  public async Task AddAsync(TClass entity, CancellationToken ct) { await _set.AddAsync(entity, ct); }
  public Task UpdateAsync(TClass entity, CancellationToken ct) { _set.Update(entity); return Task.CompletedTask; }
  public Task DeleteAsync(TClass entity, CancellationToken ct) { _set.Remove(entity); return Task.CompletedTask; }
}

public record PagedResult<T>(IReadOnlyList<T> Items, int Total, int Page, int PageSize);
```

## 3) DbContext e configuracao de entidades (IEntityTypeConfiguration)
Peça para incluir um exemplo conciso:
- `DbContext` com DbSets essenciais e `OnModelCreating` chamando `ApplyConfigurationsFromAssembly`.
- Classe de configuracao por entidade, cada uma em seu arquivo, implementando `IEntityTypeConfiguration<TEntity>` (nao mapear inline dentro do DbContext).
- Exemplo deve mostrar `ToTable`, `HasKey`, `Property` com `HasMaxLength` e tipos, evitando mapeamento extenso no contexto.

## 4) Orientacoes de uso
Liste bullets sobre:
- EF Core como padrao; Dapper reservado a aplicacoes simples.
- Registrar `IRepository<>`/`EfRepository<>` na DI (`AddScoped`).
- Manter `PagedResult<T>` junto ao repositorio ou DTOs compartilhados.
- Usar em conjunto com `entity-validator.mdc` para entidades validadas e, quando aplicavel, com `dapper-repository.mdc` como alternativa simples.
- Cada `IEntityTypeConfiguration` em arquivo proprio; evitar mapeamentos inline em `DbContext`.

## 5) Checklist rapido
Inclua checklist com 6 itens:
- Interface `IRepository<TClass, TType>` com CRUD, filtro, paginacao.
- Implementacao `EfRepository<TClass, TType>` usando `DbContext`/`DbSet` injetados.
- `PagedResult<T>` presente.
- Registro em DI (scoped) para interface/implementacao.
- `DbContext` carregando configuracoes via `ApplyConfigurationsFromAssembly`.
- Uma classe `IEntityTypeConfiguration` por entidade, em arquivo separado, sem mapeamento inline no contexto.
