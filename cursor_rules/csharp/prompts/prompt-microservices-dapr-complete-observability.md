Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "microservices-dapr-complete-observability.mdc" para "microservicos completos com Dapr, resiliencia e observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core API.
- Dapr: service invocation, pub/sub, state store, bindings, secrets.
- Resiliencia com Dapr (retries, circuit breaker), idempotência.
- Observabilidade: Serilog + OpenTelemetry + distributed tracing.
- MediatR para CQRS.
- Docker/Compose + Kafka/Redis + Dapr sidecars.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: microservices-dapr-complete-observability.mdc
Description: Padroes completos para microservicos com Dapr, resiliencia, observabilidade e entrega via containers.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/dapr/**/*.yaml", "**/docker-compose*.yml", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "microservices", "dapr", "observability", "resilience", "docker", "kafka", "redis"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura (src/, dapr/components/, deploy/)
3) Pacotes (Dapr.AspNetCore, Dapr.Client, Serilog, OpenTelemetry)
4) Configuracao Dapr (resiliency.yaml, policies)
5) Entidades/Repos/Validators - referências às regras compartilhadas
6) Dapr Service invocation com resiliencia
7) Dapr Pub/Sub com outbox/idempotência
8) Dapr State management + cache
9) Dapr Bindings/Secrets
10) Observabilidade (distributed tracing, Dapr instrumentation)
11) Docker/Compose completo
12) Testes - referência @unit-testing
13) Checklist

