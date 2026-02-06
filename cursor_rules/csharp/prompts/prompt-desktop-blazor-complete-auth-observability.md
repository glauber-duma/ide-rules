Voce e um engenheiro de software senior especialista em C# e arquitetura.

Tarefa: gerar uma rule (.mdc) para projetos "aplicacao desktop completa com autenticacao, autorizacao e observabilidade" usando Blazor Hybrid.
A rule deve seguir o padrao do repositorio e ser compatível com VS Code e Cursor.

Requisitos:
- Use .NET 9, C# e Blazor Hybrid (MAUI).
- Autenticacao com OIDC + PKCE (IdentityModel.OidcClient), roles/claims e policy-based auth.
- Observabilidade com OpenTelemetry + Serilog.
- Incluir caching local, resiliencia, sincronizacao offline.
- Incluir testes unitarios e de integracao (quando aplicavel) usando xUnit, Moq e FluentAssertions.
- Use Repository + Unit of Work, DI, SOLID, DRY, KISS.
- Use MediatR.
- Cite pacotes open source recomendados.
- Incluir exemplos de codigo enxutos e prontos.

Formato esperado da rule:
---
Rule: desktop-blazor-complete-auth-observability.mdc
Description: Padroes para desktop Blazor Hybrid completo com autenticacao e observabilidade.
globs: ["**/*.cs", "**/*.csproj", "**/appsettings*.json", "**/*.razor"]
rules: @csharp-style-guide
alwaysApply: false
tags: ["csharp", "desktop", "blazor", "auth", "observability"]
Author: Glauber Duma
Created: 2026-02
---

Inclua secoes:
1) Objetivo
2) Estrutura de pastas
3) Pacotes recomendados
4) Configuracao (OIDC, DI)
5) Observabilidade
6) Seguranca e autorizacao
7) UI e state management
8) Resiliencia e offline mode
9) Camadas e padroes
10) Testes
11) Checklist de boas praticas
