# ide-rules

Conjunto de regras para uso no VS Code e Cursor, com guias completos e prompts para padronizar boas praticas em C# e Python (com foco em FastAPI).

## Visao geral

Este repositorio organiza rules (.mdc) e prompts (.md) para:

- Padroes de codigo e estilo.
- Arquitetura e organizacao de projetos.
- Boas praticas especificas (API, middleware, validacao, seguranca, cache, dados, mensageria, etc.).
- Templates de referencia para equipes.

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
3. Use os prompts quando precisar gerar uma nova versao da regra ou ampliar exemplos.

## Regras disponiveis (resumo)

### C#

- `csharp-style-guide.mdc`: guia de estilo e boas praticas para projetos .NET.
- `app-batch.mdc`: padroes para console apps e processamento em lote.

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