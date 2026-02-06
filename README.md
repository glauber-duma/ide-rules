# ide-rules

Conjunto de regras para uso no VS Code e Cursor, com guias completos e prompts para padronizar boas praticas em C# e Python (com foco em FastAPI).

## Visao geral

Este repositorio organiza rules (.mdc) e prompts (.md) para:

- Padroes de codigo e estilo.
- Arquitetura e organizacao de projetos.
- Boas praticas especificas (API, middleware, validacao, seguranca, cache, dados, mensageria, etc.).
- Templates de referencia para equipes, agora com exemplos de codigo, configuracao de observabilidade e conteinerizacao (Docker/Docker Compose para cenarios Dapr).

## Estrutura

```
cursor_rules/
	csharp/
		general/
			csharp-style-guide.mdc
			prompt_style_guide.md
		inso/
			app-batch.mdc
			prompt_app_batch.md
		prompts/
			prompt-api-mvc-auth-observability.md
			prompt-api-mvc-simple-observability.md
			prompt-console-batch-complete-observability.md
			prompt-console-batch-simple-observability.md
			prompt-desktop-blazor-complete-auth-observability.md
			prompt-desktop-blazor-complete-observability.md
			prompt-desktop-blazor-simple-auth-observability.md
			prompt-desktop-blazor-simple-observability.md
			prompt-microservices-complete-observability.md
			prompt-microservices-dapr-complete-observability.md
			prompt-microservices-dapr-simple-observability.md
			prompt-microservices-simple-observability.md
			prompt-web-mvc-auth-observability.md
			prompt-web-mvc-simple-observability.md
		rules/
			api-mvc-auth-observability.mdc
			api-mvc-simple-observability.mdc
			console-batch-complete-observability.mdc
			console-batch-simple-observability.mdc
			desktop-blazor-complete-auth-observability.mdc
			desktop-blazor-complete-observability.mdc
			desktop-blazor-simple-auth-observability.mdc
			desktop-blazor-simple-observability.mdc
			microservices-complete-observability.mdc
			microservices-dapr-complete-observability.mdc
			microservices-dapr-simple-observability.mdc
			microservices-simple-observability.mdc
			web-mvc-auth-observability.mdc
			web-mvc-simple-observability.mdc
	python/
		api/
			fastapi-project.mdc
			fastapi-endpoints.mdc
			fastapi-validation.mdc
			fastapi-middleware.mdc
			fastapi-exception.mdc
			fastapi-security.mdc
			prompt-project.md
			prompt-endpoints.md
			prompt-validation.md
			prompt-middleware.md
			prompt-excecoes.md
			prompt-security.md
		general/
			python-style-guide.mdc
			python-cache.mdc
			python-database.mdc
			python-dependencies.mdc
			python-environments.mdc
			python-messaging.mdc
			prompt-python-style-guide.md
			prompt-cache.md
			prompt-database.md
			prompt-dependencies.md
			prompt-environments.md
			prompt-messaging.md
		skills/
			clean-architecture.mdc
			dapr-integration.mdc
			dependency-injection.mdc
```

## O que sao os arquivos

- `.mdc`: rules prontas para uso em tooling (Cursor/VS Code).
- `.md`: prompts de apoio usados para gerar ou expandir rules.

## Como usar

1. Escolha a regra apropriada na arvore `cursor_rules/`.
2. Aplique no editor (Cursor/VS Code) conforme o fluxo do seu time.
3. Use os prompts quando precisar gerar uma nova versao da regra ou ampliar exemplos; regras de microservicos incluem Dockerfile e docker-compose (sidecar Dapr, Kafka, Redis) e componentes Dapr prontos para pub/sub e state.

## Regras disponiveis (resumo)

### C#

- `csharp-style-guide.mdc`: guia de estilo e boas praticas para projetos .NET.
- `app-batch.mdc`: padroes para console apps e processamento em lote.
- `web-mvc-simple-observability.mdc`: MVC simples sem autenticacao, com observabilidade.
- `web-mvc-auth-observability.mdc`: MVC com autenticacao/autorizacao e observabilidade.
- `api-mvc-simple-observability.mdc`: API MVC simples sem autenticacao, com observabilidade.
- `api-mvc-auth-observability.mdc`: API MVC com autenticacao/autorizacao e observabilidade.
- `microservices-dapr-simple-observability.mdc`: microservicos simples com Dapr, Dockerfile, docker-compose (sidecar), componentes Kafka/Redis e exemplos de publish/subscribe.
- `microservices-dapr-complete-observability.mdc`: microservicos completos com Dapr, resiliencia, Dockerfile, docker-compose (sidecar), componentes Kafka/Redis e exemplos de handlers.
- `microservices-simple-observability.mdc`: microservicos simples sem Dapr, com observabilidade, Dockerfile e testes.
- `microservices-complete-observability.mdc`: microservicos completos sem Dapr, com resiliencia (Polly), mensageria (Kafka), Dockerfile e docker-compose.
- `console-batch-simple-observability.mdc`: batch simples para integracao legada.
- `console-batch-complete-observability.mdc`: batch completo para integracao legada.
- `desktop-blazor-simple-observability.mdc`: desktop Blazor Hybrid simples.
- `desktop-blazor-complete-observability.mdc`: desktop Blazor Hybrid completo.
- `desktop-blazor-simple-auth-observability.mdc`: desktop Blazor Hybrid com auth.
- `desktop-blazor-complete-auth-observability.mdc`: desktop Blazor Hybrid completo com auth.

### Python / FastAPI

- `fastapi-project.mdc`: estrutura de projeto e configuracao base.
- `fastapi-endpoints.mdc`: padroes de endpoints REST.
- `fastapi-validation.mdc`: validacao com Pydantic.
- `fastapi-middleware.mdc`: middlewares e interceptadores.
- `fastapi-exception.mdc`: tratamento de excecoes e handlers.
- `fastapi-security.mdc`: seguranca, autenticacao e autorizacao.

### Python / General

- `python-style-guide.mdc`: convencoes de estilo e type hints.
- `python-cache.mdc`: estrategias de cache.
- `python-database.mdc`: integracao com banco de dados e migrations.
- `python-dependencies.mdc`: gerenciamento de dependencias.
- `python-environments.mdc`: ambientes e setups.
- `python-messaging.mdc`: mensageria e eventos.

### Skills (Python)

- `clean-architecture.mdc`: principios e estrutura.
- `dependency-injection.mdc`: DI com dependency-injector.
- `dapr-integration.mdc`: padroes com Dapr.

## Contribuicao

- Mantenha consistencia com as rules existentes.
- Prefira adicionar exemplos completos e prontos para uso.
- Evite depender de bibliotecas nao padronizadas pela equipe.

## Licenca

Uso interno. 