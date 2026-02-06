Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "desktop-blazor-complete-auth-observability.mdc" para "aplicacao desktop completa com autenticacao, autorizacao e observabilidade" usando Blazor Hybrid.
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Blazor Hybrid (MAUI).
- Autenticação OIDC + PKCE (IdentityModel.OidcClient), roles/claims, policy-based auth.
- Observabilidade: Serilog + OpenTelemetry.
- Caching local (SQLite/IndexedDB), sincronização offline, fila de comandos.
- MediatR para Commands/Queries.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing

Formato esperado da rule:
---
Rule: desktop-blazor-complete-auth-observability.mdc
Description: Padroes para desktop Blazor Hybrid completo com autenticacao, sincronizacao offline e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/*.razor"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing
alwaysApply: false
tags: ["csharp", "desktop", "blazor", "maui", "auth", "offline", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes (MAUI, Blazor, IdentityModel.OidcClient, SQLite, Serilog, OpenTelemetry)
4) Configuracao (MauiProgram.cs com OIDC, DI)
5) Entidades/Repos/Validators - referências
6) Autenticação OIDC + PKCE (exemplo completo de fluxo)
7) Autorização (policies em componentes Blazor)
8) Offline/Sync (fila de comandos, cache local, reenvio)
9) Observabilidade (Serilog + file sink + traces de auth/sync)
10) Testes - referência @unit-testing
11) Checklist
