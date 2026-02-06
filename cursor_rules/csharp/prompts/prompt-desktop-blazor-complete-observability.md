Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "desktop-blazor-complete-observability.mdc" para "aplicacao desktop completa com observabilidade" usando Blazor Hybrid.
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Blazor Hybrid (MAUI).
- Observabilidade: Serilog + OpenTelemetry.
- Caching local (SQLite), resiliencia, atualização automática (quando aplicável).
- MediatR para Commands/Queries.
- Offline mode opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing

Formato esperado da rule:
---
Rule: desktop-blazor-complete-observability.mdc
Description: Padroes para desktop Blazor Hybrid completo com observabilidade e recursos avançados.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/*.razor"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing
alwaysApply: false
tags: ["csharp", "desktop", "blazor", "maui", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes (MAUI, Blazor, SQLite, Serilog, OpenTelemetry)
4) Configuracao (MauiProgram.cs com DI)
5) Entidades/Repos/Validators - referências
6) UI/State management Blazor (componentes, state container)
7) Cache local (SQLite, EF Core)
8) Resiliencia e offline mode (opcional)
9) Observabilidade (Serilog + file sink)
10) Testes - referência @unit-testing
11) Checklist
