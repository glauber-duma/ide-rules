Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "desktop-blazor-simple-auth-observability.mdc" para "aplicacao desktop simples com autenticacao, autorizacao e observabilidade" usando Blazor Hybrid.
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Blazor Hybrid (MAUI).
- Autenticação OIDC + PKCE (IdentityModel.OidcClient).
- Policy-based auth.
- Observabilidade: Serilog + OpenTelemetry.
- MediatR para Commands/Queries.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing

Formato esperado da rule:
---
Rule: desktop-blazor-simple-auth-observability.mdc
Description: Padroes para desktop Blazor Hybrid simples com autenticacao e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/*.razor"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing
alwaysApply: false
tags: ["csharp", "desktop", "blazor", "maui", "auth", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes (MAUI, Blazor, IdentityModel.OidcClient, Serilog, OpenTelemetry)
4) Configuracao (OIDC em MauiProgram.cs, DI)
5) Entidades/Repos/Validators - referências
6) Autenticação OIDC + PKCE (exemplo de fluxo)
7) Autorização (policies em componentes Blazor)
8) Observabilidade (Serilog + traces de auth)
9) Testes - referência @unit-testing
10) Checklist
