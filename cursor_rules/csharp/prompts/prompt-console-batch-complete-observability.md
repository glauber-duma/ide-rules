Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "aplicacao console completa com observabilidade com foco em integracao de sistemas legados (processos batch)".
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9 e C#.
- Console app batch com resiliencia (Polly), retry, idempotencia, orquestracao de jobs.
- Observabilidade com OpenTelemetry + Serilog.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: console-batch-complete-observability.mdc
Description: Padroes para console app batch completo com observabilidade e integracao legada.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "console", "batch", "observability", "resilience"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao
5) Observabilidade
6) Processamento em lote e resiliencia
7) Camadas e padroes
8) Testes
9) Checklist de boas praticas
