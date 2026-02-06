Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "microservices-complete-observability.mdc" para "microservicos completo sem uso de Dapr e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core API.
- Mensageria (RabbitMQ/Kafka), resiliencia com Polly, outbox pattern, idempotencia.
- Observabilidade: Serilog + OpenTelemetry + distributed tracing + healthcheck.
- MediatR para Commands/Queries/Events.
- Docker/Compose + Kafka/RabbitMQ + Redis.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: microservices-complete-observability.mdc
Description: Padroes para microservicos completos sem Dapr, com observabilidade e resiliencia.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/docker-compose*.yml", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "microservices", "observability", "resilience", "docker", "kafka"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes (Polly, MassTransit/RabbitMQ.Client/Confluent.Kafka, StackExchange.Redis, Serilog, OpenTelemetry)
4) Configuracao (Program.cs com Polly, mensageria, cache, DI)
5) Entidades/Repos/Validators - referências
6) Resiliencia (Polly: retry, circuit breaker, timeout)
7) Mensageria (pub/sub, outbox pattern, idempotencia)
8) Observabilidade (distributed tracing, correlation id, healthcheck)
9) Docker/Compose com Kafka/RabbitMQ/Redis
10) Testes - referência @unit-testing
11) Checklist
