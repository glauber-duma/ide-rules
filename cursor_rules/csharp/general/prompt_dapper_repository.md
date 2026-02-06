voce e um engenheiro de software senior especializado em C# e Dapper.
voce deve gerar uma rule reutilizavel para padronizar o repositorio generico Dapper para aplicacoes simples.
Siga a estrutura abaixo e preencha o conteudo conforme indicado.

---
Rule: dapper-repository.mdc
Description: Snippet padrao de repositorio generico Dapper para aplicacoes simples, com CRUD, filtro e paginacao.
globs: ["**/*.cs"]
rules: @csharp-style-guide, @entity-validator
alwaysApply: false
tags: ["csharp", "dapper", "repository", "data-access"]
Author: Glauber Duma
Created: 2026-02
---

# Repositorio generico Dapper (aplicacoes simples)

## 1) Objetivo
Explique que a rule centraliza a interface/implementacao Dapper para CRUD, filtro e paginacao em aplicacoes simples (modelo pequeno, poucos relacionamentos, sem tracking/lazy loading), usando `PagedResult<T>`.

## 2) Snippet base (Dapper)
Inclua exatamente o codigo abaixo:
```csharp
public interface IDapperRepository<TClass, TType> where TClass : class
{
  Task<TClass?> GetByIdAsync(TType id, CancellationToken ct);
  Task<IEnumerable<TClass>> ListAsync(string? whereClause, object? parameters, CancellationToken ct);
  Task<PagedResult<TClass>> PagedAsync(string? whereClause, object? parameters, int page, int pageSize, CancellationToken ct);
  Task AddAsync(TClass entity, CancellationToken ct);
  Task UpdateAsync(TClass entity, CancellationToken ct);
  Task DeleteAsync(TType id, CancellationToken ct);
}

public class DapperRepository<TClass, TType> : IDapperRepository<TClass, TType> where TClass : class
{
  private readonly IDbConnection _conn;
  private readonly string _table;
  public DapperRepository(IDbConnection conn, string table) { _conn = conn; _table = table; }

  public Task<TClass?> GetByIdAsync(TType id, CancellationToken ct) => _conn.QuerySingleOrDefaultAsync<TClass>($"select * from {_table} where Id = @Id", new { Id = id });

  public Task<IEnumerable<TClass>> ListAsync(string? whereClause, object? parameters, CancellationToken ct)
  {
    var sql = $"select * from {_table} " + (string.IsNullOrWhiteSpace(whereClause) ? string.Empty : $"where {whereClause}");
    return _conn.QueryAsync<TClass>(sql, parameters);
  }

  public async Task<PagedResult<TClass>> PagedAsync(string? whereClause, object? parameters, int page, int pageSize, CancellationToken ct)
  {
    var sqlWhere = string.IsNullOrWhiteSpace(whereClause) ? string.Empty : $"where {whereClause}";
    var total = await _conn.ExecuteScalarAsync<int>($"select count(1) from {_table} {sqlWhere}", parameters);
    var items = await _conn.QueryAsync<TClass>($"select * from {_table} {sqlWhere} order by Id offset @Skip rows fetch next @Take rows only", new { Skip = (page-1)*pageSize, Take = pageSize, parameters });
    return new PagedResult<TClass>(items.ToList(), total, page, pageSize);
  }

  public Task AddAsync(TClass entity, CancellationToken ct) => _conn.ExecuteAsync($"insert into {_table} (...) values (...)", entity);
  public Task UpdateAsync(TClass entity, CancellationToken ct) => _conn.ExecuteAsync($"update {_table} set ... where Id = @Id", entity);
  public Task DeleteAsync(TType id, CancellationToken ct) => _conn.ExecuteAsync($"delete from {_table} where Id = @Id", new { Id = id });
}

public record PagedResult<T>(IReadOnlyList<T> Items, int Total, int Page, int PageSize);
```

## 3) Orientacoes de uso
Liste bullets sobre:
- Definicao de aplicacao simples (modelo pequeno, poucos relacionamentos, sem tracking/lazy loading); fora disso use EF Core.
- `table` passado no ctor; parametrizacao sempre com @params.
- `PagedResult<T>` compartilhado no dominio/application.
- Combine com `entity-validator.mdc` para entidades validadas.
- Registro opcional em DI quando Dapper for o acesso de dados adotado.

## 4) Checklist rapido
Inclua checklist com 5 itens:
- Interface `IDapperRepository<TClass, TType>` com CRUD/filtro/paginacao.
- Implementacao `DapperRepository<TClass, TType>` parametrizada por tabela.
- `PagedResult<T>` presente.
- Observacao de aplicacao simples (sem tracking/lazy loading).
- Registro opcional em DI.
