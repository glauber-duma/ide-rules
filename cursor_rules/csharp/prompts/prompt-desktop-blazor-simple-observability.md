Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "desktop-blazor-simple-observability.mdc" para "aplicacao desktop simples com observabilidade" usando Blazor Hybrid.
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Blazor Hybrid (MAUI).
- Observabilidade: Serilog + OpenTelemetry.
- MediatR para Commands/Queries.
- SQLite local ou EF Core InMemory para dados.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing

Formato esperado da rule:
---
Rule: desktop-blazor-simple-observability.mdc
Description: Padroes para desktop Blazor Hybrid simples com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/*.razor"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing
alwaysApply: false
tags: ["csharp", "desktop", "blazor", "maui", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas (UI/Components, Application/, Domain/, Infrastructure/)
3) Pacotes recomendados (MAUI, Blazor, Serilog, OpenTelemetry, EF Core/SQLite)
4) Configuracao (MauiProgram.cs com DI de MediatR, EF Core, Serilog)
5) Entidades/Repos/Validators - referências às regras compartilhadas
6) UI/State management específico Blazor (componentes, DI de IMediator, state container)
7) Observabilidade (Serilog + file sink para desktop)
8) Testes - referência @unit-testing
9) Checklist
