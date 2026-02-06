Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "console-batch-simple-observability.mdc" para "aplicacao console simples com observabilidade com foco em integracao de sistemas legados (processos batch)".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Console app com processamento em lote.
- Hosted services ou BackgroundService para jobs.
- Observabilidade: Serilog + OpenTelemetry.
- MediatR para orquestrar comandos/jobs.
- Dockerfile opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide - Padrões gerais de código C# 12/.NET 9
- @entity-validator - Entidades com validação
- @efcore-repository - Repositório genérico EF Core
- @dapper-repository - Repositório Dapper para aplicações simples
- @fluentvalidation - Validadores FluentValidation
- @use-cases - Padrão CQRS com MediatR (Commands/Queries/Handlers)
- @unit-testing - Padrões de testes

Formato esperado da rule:
---
Rule: console-batch-simple-observability.mdc
Description: Padroes para console app batch simples com observabilidade e integracao legada.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "console", "batch", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas (src/App/Program.cs, Jobs/, Application/, Infrastructure/)
3) Pacotes recomendados (Serilog, OpenTelemetry, MediatR, Polly opcional)
4) Configuracao (Program.cs com Host.CreateDefaultBuilder, DI para MediatR, Serilog, OTel, HostedService)
5) Exemplo de Job (BackgroundService ou HostedService ESPECÍFICO de console)
6) Exemplo de Command/Handler com MediatR (ex: ImportCustomersCommand) - pode incluir código inline OU referenciar @use-cases
7) Entidades - APENAS referência: "Para padronizar entidades, utilize @entity-validator"
8) Repositório EF - APENAS referência: "Para repositórios, utilize @efcore-repository"
9) Repositório Dapper - APENAS referência: "Para aplicações simples, utilize @dapper-repository"
10) Observabilidade (Serilog + OTel + traces em jobs)
11) Dockerfile (opcional)
12) Testes - referência: "Consulte @unit-testing"
13) Checklist
