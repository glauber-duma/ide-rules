voce e um engenheiro de software senior especializado em C# e FluentValidation.
voce deve gerar uma rule reutilizavel para padronizar entidades de dominio com validacao.
Siga a estrutura abaixo e preencha o conteudo conforme indicado.

---
Rule: entity-validator.mdc
Description: Snippet padrao de entidade EF Core com validacao FluentValidation para reaproveitar em outras rules.
globs: ["**/*.cs"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "entities", "validation", "efcore", "dapper"]
Author: Glauber Duma
Created: 2026-02
---

# Entidades base com FluentValidation

## 1) Objetivo
Explique que o arquivo centraliza um unico snippet de entidade EF Core com setters privados, construtores que validam via FluentValidation e metodos publicos que revalidam. Diga que EF Core e o padrao; Dapper apenas em aplicacoes simples (modelo pequeno, poucos relacionamentos, sem tracking/lazy loading).

## 2) Snippet base (Product + ProductValidator)
Inclua o exemplo completo abaixo, com regioes:
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
#region Validator
public class ProductValidator : AbstractValidator<Product>
{
  public ProductValidator()
  {
    RuleFor(x => x.Name).NotEmpty().MaximumLength(200);
    RuleFor(x => x.Price).GreaterThan(0);
  }
}
#endregion
```

## 3) Orientacoes de uso
Liste bullets reforçando:
- setters privados + métodos coesos de mutação com revalidação.
- validacao no construtor e em cada metodo que altera estado.
- Guid gerado quando id for vazio.
- Em aplicacoes simples (Dapper), manter o mesmo modelo de entidade/validator, trocando apenas o acesso a dados.
- Reutilizar esta rule em outras rules para consistencia.

## 4) Checklist rapido
Forneca um checklist com 5 itens:
- construtor privado + construtor completo com validacao.
- regioes para propriedades/ construtores/ metodos publicos.
- FluentValidation chamado no construtor e nos metodos de alteracao.
- sets privados para todas as propriedades.
- observacao sobre EF Core como padrao e Dapper em cenarios simples.
