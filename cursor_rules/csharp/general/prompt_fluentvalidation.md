voce e um engenheiro de software senior especializado em C# e FluentValidation.
voce deve gerar uma rule reutilizavel para padronizar validadores FluentValidation e registro no DI.
Siga a estrutura abaixo e preencha o conteudo conforme indicado.

---
Rule: fluentvalidation.mdc
Description: Snippet padrao para validadores FluentValidation (requests/entidades), registro em DI e uso seguro.
globs: ["**/*.cs"]
rules: @csharp-style-guide, @entity-validator
alwaysApply: false
tags: ["csharp", "fluentvalidation", "validation"]
Author: Glauber Duma
Created: 2026-02
---

# FluentValidation - Padrao de Validadores

## 1) Objetivo
Explique que a rule centraliza validadores FluentValidation para DTOs/entidades e o registro no DI, com mensagens claras e integracao com `entity-validator.mdc`.

## 2) Snippets base
Peça para incluir:
- Um validador simples de DTO (UserRequestValidator) com regras NotEmpty, MaximumLength, EmailAddress.
- Um validador composto para OrderRequest com `RuleForEach` para itens e validacao de soma (`TotalAmount`).
- O validador de item (OrderItemValidator) reaproveitado.

## 3) Registro no DI
Solicite um snippet com extensao `AddValidatorsFromAssemblyContaining<T>()` para registrar validadores no container.

## 4) Uso em services/controllers
Instrua a mostrar injecao de `IValidator<T>`, chamada `ValidateAsync`, mapeamento de erros para uma lista e lancamento de excecao de validacao.

## 5) Orientacoes de uso
Liste bullets reforcando: um validador por tipo; regras claras; evitar logica de negocio ou acesso a dados direto; descoberta por assembly; combinar com `entity-validator.mdc` para entidades que se auto-validam.

## 6) Checklist rapido
Peça checklist com 5 itens: um validador por tipo; mensagens definidas; uso de `RuleForEach` quando necessario; registro no DI por assembly; sem logica de negocio pesada/IO dentro do validador.
