Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "api-mvc-simple-observability.mdc" para "API MVC simples sem autenticacao e com observabilidade", seguindo o padrão do repositório (VS Code/Cursor).

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core MVC (controllers), sem autenticacao.
- Observabilidade: Serilog + OpenTelemetry (tracing/metrics), healthcheck em `/healthz`.
- Controller + MediatR no padrao use case (controller chama query/command; handler/use case chama repositorios/servicos); inclua exemplo.
- Pacotes recomendados: Serilog.AspNetCore, Serilog.Sinks.Console, OpenTelemetry.* (AspNetCore/Http/Exporter OTLP), MediatR, FluentValidation, EF Core.
- Dockerfile opcional para publicar a API.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide - Padrões gerais de código C# 12/.NET 9
- @entity-validator - Entidades EF Core com setters privados e validação FluentValidation
- @efcore-repository - Repositório genérico EF Core
- @dapper-repository - Repositório Dapper para aplicações simples
- @fluentvalidation - Validadores FluentValidation para DTOs
- @use-cases - Padrão CQRS com MediatR (Commands/Queries/Handlers)
- @unit-testing - Padrões de testes com xUnit, Moq, Bogus, FluentAssertions

Formato esperado da rule:
---
Rule: api-mvc-simple-observability.mdc
Description: Padroes para API MVC simples sem autenticacao, com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "api", "mvc", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao (Program.cs/DI com registro de MediatR, FluentValidation, EF Core, Serilog, OTel)
5) Entidades (EF Core) com validação - APENAS referência: "Para padronizar entidades, utilize @entity-validator"
6) Repositório genérico (EF Core) - APENAS referência: "Para padronizar repositórios, utilize @efcore-repository"
7) Repositório genérico (Dapper) - APENAS referência: "Para aplicações simples, utilize @dapper-repository"
8) Validadores - APENAS referência: "Para padronizar validadores, utilize @fluentvalidation"
9) Use Cases - APENAS referência: "Para padronizar CQRS/MediatR, utilize @use-cases"
10) Exemplo de controller ESPECÍFICO da API (ProductsController com DI do IMediator e chamada a query/command)
11) Observabilidade (Serilog + OTel + healthcheck em /healthz)
12) Dockerfile (opcional)
13) Testes - referência: "Para padrões de testes, consulte @unit-testing"
14) Checklist de boas praticas (referenciando regras compartilhadas)
