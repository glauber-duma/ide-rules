Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar a rule (.mdc) "microservices-dapr-simple-observability.mdc" para "microservicos simples usando Dapr e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos obrigatórios:
- .NET 9, C#, ASP.NET Core API.
- Dapr: service invocation, pub/sub basico, state store.
- Observabilidade: Serilog + OpenTelemetry (integrado com Dapr).
- MediatR para Commands/Queries.
- Docker/Compose + Dapr components (dapr/*.yaml).

Regras compartilhadas (usar @-notação):
- @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers

Formato esperado da rule:
---
Rule: microservices-dapr-simple-observability.mdc
Description: Padroes para microservicos Dapr simples com observabilidade e entrega em containers.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/dapr/**/*.yaml", "**/docker-compose*.yml", "**/Dockerfile"]
rules: @csharp-style-guide, @entity-validator, @efcore-repository, @dapper-repository, @fluentvalidation, @use-cases, @unit-testing, @docker-containers
alwaysApply: false
tags: ["csharp", "microservices", "dapr", "observability", "docker", "kafka", "redis"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas (src/, dapr/components/, deploy/)
3) Pacotes (Dapr.AspNetCore, Dapr.Client, Serilog, OpenTelemetry)
4) Configuracao Dapr (Program.cs com AddDapr, MapSubscribeHandler)
5) Entidades/Repos/Validators - referências
6) Dapr Components (pubsub.yaml, statestore.yaml, configuration.yaml)
7) Service invocation (exemplo com DaprClient)
8) Pub/Sub (exemplo de publisher/subscriber)
9) State management (exemplo com DaprClient.GetStateAsync/SaveStateAsync)
10) Observabilidade (Serilog + OTel + Dapr instrumentation)
11) Docker/Compose com Dapr sidecar
12) Testes - referência @unit-testing
13) Checklist
