Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "web-mvc-auth-observability.mdc" para "aplicacao web MVC com autenticacao, autorizacao e observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core MVC.
- Identity + Cookie Auth (ou JWT para areas admin), policy-based auth.
- Observabilidade: Serilog + OpenTelemetry + healthcheck.
- MediatR para Commands/Queries.
- Dockerfile opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: web-mvc-auth-observability.mdc
Description: Padroes para aplicacao web MVC com autenticacao/autorizacao (cookies) e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "mvc", "web", "auth", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados (Identity, Cookie Auth, Serilog, OpenTelemetry)
4) Configuracao (Identity + policies em Program.cs)
5) Entidades/Repos/Validators - referências às regras compartilhadas
6) Exemplo de controller protegido com [Authorize]
7) Observabilidade (log de login/logout, traces)
8) Testes - referência @unit-testing
9) Dockerfile
10) Checklist
