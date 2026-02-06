Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "microservicos completo usando Dapr e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9 e C#.
- Use Dapr com pub/sub, state store, bindings, service invocation.
- Incluir resiliencia (Polly), idempotencia, outbox, health checks.
- Observabilidade com OpenTelemetry + Serilog.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: microservices-dapr-complete-observability.mdc
Description: Padroes para microservicos completos com Dapr e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/dapr/*.yaml"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "microservices", "dapr", "observability", "resilience"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao Dapr
5) Observabilidade
6) Resiliencia e confiabilidade
7) Comunicacao e eventos
8) Camadas e padroes
9) Testes
10) Checklist de boas praticas
