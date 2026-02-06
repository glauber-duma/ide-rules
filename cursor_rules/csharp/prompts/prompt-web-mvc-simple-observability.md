Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "aplicacao web MVC simples sem autenticacao e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9 e C#.
- Estruture o projeto com MVC e separacao por camadas.
- Inclua observabilidade com OpenTelemetry + Serilog.
- Nao incluir autenticacao/autorizacao.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR para mediator.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: web-mvc-simple-observability.mdc
Description: Padroes para aplicacao web MVC simples sem autenticacao, com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "mvc", "web", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao (appsettings, DI)
5) Observabilidade (logging, tracing, metrics)
6) Camadas e padroes (controllers, services, repositories)
7) Testes
8) Checklist de boas praticas
