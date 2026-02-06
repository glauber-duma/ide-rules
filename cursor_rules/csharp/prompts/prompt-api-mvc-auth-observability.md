Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "api-mvc-auth-observability.mdc" para "API MVC com autenticacao, autorizacao e observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core MVC API com JWT Bearer.
- Autenticacao/autorizacao policy-based.
- Observabilidade: Serilog + OpenTelemetry + healthcheck.
- MediatR para Commands/Queries.
- Dockerfile opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: api-mvc-auth-observability.mdc
Description: Padroes para API MVC com autenticacao/autorizacao (JWT Bearer) e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "api", "mvc", "auth", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados (JWT Bearer, OpenTelemetry, Serilog, MediatR, EF Core)
4) Configuracao (JWT em Program.cs, policies, DI)
5) Entidades/Repos/Validators - APENAS referências às regras compartilhadas
6) Exemplo de controller PROTEGIDO com [Authorize]
7) Observabilidade
8) Testes - referência @unit-testing
9) Dockerfile
10) Checklist
