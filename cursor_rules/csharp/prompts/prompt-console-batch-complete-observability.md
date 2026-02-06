Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "console-batch-complete-observability.mdc" para "aplicacao console completa com observabilidade com foco em integracao de sistemas legados (processos batch)".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, Console app batch.
- Resiliencia (Polly): retry, circuit breaker, timeout.
- Orquestração de jobs, idempotência, processamento paralelo.
- Observabilidade: Serilog + OpenTelemetry.
- MediatR para comandos.
- Dockerfile opcional.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: console-batch-complete-observability.mdc
Description: Padroes para aplicacoes console completas (batch) com observabilidade, resiliencia e conteinerizacao.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "console", "batch", "observability", "resilience", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes (Polly, Serilog, OpenTelemetry, MediatR, Quartz.NET opcional)
4) Configuracao (Program.cs com Host, Polly, DI)
5) Entidades/Repos/Validators - referências
6) Jobs com resiliencia (BackgroundService + Polly)
7) Orquestração (MediatR pipelines, processamento paralelo, idempotência)
8) Observabilidade (Serilog + OTel + traces)
9) Dockerfile
10) Testes - referência @unit-testing
11) Checklist
