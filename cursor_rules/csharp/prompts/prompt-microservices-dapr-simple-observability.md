Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "microservicos simples usando Dapr e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9 e C#.
- Use Dapr (service invocation, pub/sub basico, state store).
- Observabilidade com OpenTelemetry + Serilog.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: microservices-dapr-simple-observability.mdc
Description: Padroes para microservicos simples com Dapr e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/dapr/*.yaml"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "microservices", "dapr", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao Dapr (components, sidecar)
5) Observabilidade
6) Comunicacao e eventos
7) Camadas e padroes
8) Testes
9) Checklist de boas praticas
