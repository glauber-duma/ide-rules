Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "microservices-simple-observability.mdc" para "microservicos simples sem uso de Dapr e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core API.
- HTTP client com resiliencia (Polly) e mensageria opcional (RabbitMQ/Kafka).
- Observabilidade: Serilog + OpenTelemetry + healthcheck.
- MediatR para Commands/Queries.
- Docker/Compose para deployment.

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: microservices-simple-observability.mdc
Description: Padroes para microservicos simples sem Dapr, com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/docker-compose*.yml", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "microservices", "observability", "docker"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados (Polly, Serilog, OpenTelemetry, RabbitMQ.Client/Confluent.Kafka opcional)
4) Configuracao (Program.cs com HttpClient resiliente, DI)
5) Entidades/Repos/Validators - referências às regras compartilhadas
6) Comunicacao HTTP com Polly (exemplo específico de retry/circuit breaker)
7) Mensageria (exemplo opcional de publicacao/consumo)
8) Observabilidade (Serilog + OTel + healthcheck + distributed tracing)
9) Docker/Compose
10) Testes - referência @unit-testing
11) Checklist
