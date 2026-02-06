Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "API MVC simples sem autenticacao e com observabilidade".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9 e C#.
- ASP.NET Core MVC API (controllers), sem autenticacao.
- Observabilidade com OpenTelemetry + Serilog.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: api-mvc-simple-observability.mdc
Description: Padroes para API MVC simples sem autenticacao, com observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "api", "mvc", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao (DI, appsettings)
5) Observabilidade
6) API design (versionamento, status codes)
7) Camadas e padroes
8) Testes
9) Checklist de boas praticas
