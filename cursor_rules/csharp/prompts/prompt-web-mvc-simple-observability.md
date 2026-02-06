Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "web-mvc-simple-observability.mdc" para "aplicacao web MVC simples sem autenticacao e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core MVC (Views/Controllers).
- Sem autenticacao/autorizacao.
- Observabilidade: Serilog + OpenTelemetry + healthcheck.
- MediatR para Commands/Queries.
- Dockerfile opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide - Padrões gerais de código C# 12/.NET 9
- @entity-validator - Entidades com validação
- @efcore-repository - Repositório genérico EF Core
- @dapper-repository - Repositório Dapper
- @fluentvalidation - Validadores FluentValidation
- @use-cases - Padrão CQRS com MediatR
- @unit-testing - Padrões de testes

Formato esperado da rule:
---
Rule: web-mvc-simple-observability.mdc
Description: Padroes para aplicacao web MVC simples sem autenticacao, com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "mvc", "web", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas (Presentation/Controllers/Views, Application/, Domain/, Infrastructure/)
3) Pacotes recomendados (Serilog, OpenTelemetry, MediatR, FluentValidation, EF Core)
4) Configuracao (Program.cs com DI de MediatR, EF Core, Serilog, OTel)
5) Entidades - APENAS referência: "Consulte @entity-validator"
6) Repositório - APENAS referência: "Consulte @efcore-repository"
7) Exemplo de Controller ESPECÍFICO MVC (ProductsController com Views, DI do IMediator, chamada a query)
8) Observabilidade (Serilog + OTel + healthcheck)
9) Testes - referência: "Consulte @unit-testing"
10) Dockerfile (opcional)
11) Checklist
