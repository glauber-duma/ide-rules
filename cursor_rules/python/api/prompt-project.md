você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-project.mdc
Description: Diretrizes completa para projetos FastAPI, baseado em práticas de aplicações modernas.
globs: ["**/*.py", "**/requirements*.txt", "**/pyproject.tom"]
alwaysApply: false
tags: ["fastapi", "api", "project", "modern"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI Project - Diretrizes para projetos FastAPI

Esta regra estabele padrões para o desenvolvimento de API´s baseado em análise de projetos reais e melhores práticas.

## 🚀 **Estrutura do projeto FastAPI**

### 1.1. Arquitetura Hexagonal(ports and adapters) / Clean Arquiteture


```
fastapi_project
|---src/
|.   |---app/
|.        |---__init__.py
|.        |---main.py                                  # ponto de entrada da aplicação
|.        |---core/
|.        |.    |---configs/
|.        |.    |      |---__init__.py
|.        |.    |      |---config.py                                # Configurações (Pydantic Setting)
|.        |.    |      |---database.py                              # Configurações de banco de dados
|.        |.    |      |---security.py                              # Configurações de segurança
|.        |.    |      |---logging.py                               # Configurações de logs
|.        |.    |      |---exception.py                             # Configurações de exceções
|.        |.    |---containers/
|.        |.           |---__init__.py
|.        |.           |---app_container.py              # Configurações de injeção de dependência da aplicação
|.        |.           |---infra_container.py       # Configurações de injeção de dependência da camada de infraestrutura
|.        |.           |---repositories_container.py     # Configurações de injeção de dependência da camada de repositórios
|.        |.           |---services_container.py         # Configurações de injeção de dependência da camada de serviços
|.        |.           |---usecases_container.py         # Configurações de injeção de dependência da camada de casos de uso
|.        |.           |---external_container.py    # Configurações de injeção de dependência da camada de serviços externos
|.        |---api/
|.        |    |---__init__.py
|.        |    |---v1/                                   # Versionamento
|.        |.   |    |---__init__.py
|.        |.   |    |---router.py                        # router principal v1
|.        |.   |    |---endpoints/                       # endpoints rest
|.        |.   |    |       |---__init__.py
|.        |.   |    |       |---auth.py.                 # autenticação/autorização
|.        |.   |    |       |---user.py                  # usuários
|.        |.   |    |       |---items.py                 # produtos/itens
|.        |.   |    |       |---health.py                # health checl
|.        |.   |    |---middleware.py                    # middlewares específicos da v1
|.        |.   |    |---schemas/                         # schemas pydantic
|.        |.   |            |---__init__.py
|.        |.   |            |---auth.py.                 # autenticação/autorização
|.        |.   |            |---user.py                  # usuários
|.        |.   |            |---items.py                 # produtos/itens
|.        |.   |            |---common.py                # esquemas comuns e reaproveitáveis
|.        |    |---v2/                                   # Versionamento
|.        |.   |   |---__init__.py
|.        |.   |   |---router.py                        # router principal v2
|.        |.   |   |---endpoints/                       # endpoints rest
|.        |.   |   |       |---__init__.py
|.        |.   |   |       |---auth.py.                 # autenticação/autorização
|.        |.   |   |       |---user.py                  # usuários
|.        |.   |   |       |---items.py                 # produtos/itens
|.        |.   |   |       |---health.py                # health checl
|.        |.   |   |---middleware.py                    # middlewares específicos da v2
|.        |.   |   |---schemas/                         # schemas pydantic
|.        |.   |          |---__init__.py
|.        |.   |          |---auth.py.                 # autenticação/autorização
|.        |.   |          |---user.py                  # usuários
|.        |.   |          |---items.py                 # produtos/itens
|.        |.   |          |---common.py                # esquemas comuns e reaproveitáveis
|.        |    |---middlewares/                        # Middlewares Globais
|.        |            |---__init__.py
|.        |            |---correlationa.py             # Correlation ID
|.        |            |---logging.py                  # Request/Response logging
|.        |            |---security.py                 # Security headers
|.        |            |---error_handling.py           # Error Handling
|.        |---business/                                # Lógica de negócios / use cases
|.        |    |---__init__.py
|.        |    |---services/                           # Serviços de domínio
|.        |    |.     |---__init__.py
|.        |    |.     |---auth_service.py
|.        |    |.     |---user_service.py
|.        |    |.     |---item_service.py
|.        |    |---validators/                         # Validações de negócio
|.        |    |.     |---__init__.py
|.        |    |.     |---business_rules.py
|.        |    |---exceptions/                         # Exceções de negócio
|.        |    |.     |---__init__.py
|.        |    |.     |---auth_exceptions.py
|.        |    |.     |---validations_exceptions.py
|.        |---data/                                   # Camada de dados (adapters)
|.        |    |---__init__.py
|.        |    |---repositories/                      # Padrão repository
|.        |    |.     |---__init__.py
|.        |    |.     |---user_repository.py 
|.        |    |.     |---item_repository.py
|.        |    |---models/                           # Modelo de dados ORM/ODM
|.        |    |.     |---__init__.py
|.        |    |.     |---base.py
|.        |    |.     |---user.py
|.        |    |.     |---item.py
|.        |    |---migrations/                       # Migrações de banco
|.        |    |-------|versions/
|.        |---adapters/                              # Adapters para serviços externos
|.        |    |---__init__.py
|.        |    |---cache/                            # Cache (Redis/Memcache,etc)
|.        |    |.     |---__init__.py
|.        |    |.     |---base.py 
|.        |    |.     |---redis_cache.py
|.        |    |---messagind/                        # Mensageria(RabitMQ, Kafka, Azure Service Bus)
|.        |    |.     |---__init__.py
|.        |    |.     |---producer.py
|.        |    |.     |---consumer.py
|.        |    |---external_apis/                    # APIs externas
|.        |    |.     |---__init__.py
|.        |    |.     |---payment_api.py
|.        |---domain/                              # Domínio da aplicação
|.        |    |---__init__.py
|.        |    |---entities/                       # Entidades do domínio
|.        |    |.     |---__init__.py
|.        |    |.     |---base.py 
|.        |    |.     |---user.py
|.        |    |.     |---item.py
|.        |    |---value_objects/                  # Objetos de Valor
|.        |    |.     |---__init__.py
|.        |    |.     |---email.py
|.        |    |.     |---address.py
|.        |    |---events/                         # Eventos do domínio
|.        |     .     |---__init__.py
|.        |     .     |---user_events.py
|.        |---utils/                              # Utilitários gerais
|.             |---__init__.py
|.             |---helpers.py
|.             |---constants.py
|.             |---decorators.py
|
|---tests/            # Estrutura de Testes
|---docs/             # Documentação
|---scripts/.         # scritps utilitários
|---docker/           # Configurações docker
|---env.example.      # Exemplo de variáveis de ambiente
|---pyproject.toml.   # Configuração do projeto
|---requirementes.txt # Dependencias
|---README.md         # Documentação principal

```



## 🛠️ **Configuração da Aplicação FASTAPI**
### 2.1. main.py - ponto de entrada principal
```Python
# src/app/main.py

<gerar aqui o código python>

```

### 2.2. Configurações com Pydantic Settings
```Python
# src/app/core/config.py

<gerar aqui o código python>

```

## 📄 **Esquemas Pydantic**

### 3.1 Bases Eschemas e Padrões

```Python
# src/app/api/v1/schemas/common.py

<gerar aqui o código python>

```

### 3.2 Domain-Specific Schemas

```Python
# src/app/api/v1/schemas/user.py

<gerar aqui o código python>

```

## 🗺️ **Endpoints e Roteamento**

### 4.1. Endpoint implementation patterns

```Python
# src/app/api/v1/endpoints/user.py

<gerar aqui o código python>

```

### 4.2. Dependencies pattern

```Python
# src/app/api/v1/dependencies.py

<gerar aqui o código python>

```

salve a regra na pasta python/api/fastapi-project.mdc
