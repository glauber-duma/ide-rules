você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com endpoints.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-endpoints.mdc
Description: Diretrizes para implementações de endpoints REST com FastAPI, cobrindo design de APIs, status codes, versionamento e melhores práticas.
globs: ["**/endpoints/**/*.py", "**/api/**/*.py", "**/routes/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "endpoints", "best-practices"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI - Implementação de Endpoints REST

## 1. Objetivo

<incluir os objetivos principais da rule>

## 2. Estrutura dos Endpoints

### 🗂️ Organização dos Arquivos

```
<gerar aqui a estrutura de pastas e arquivos sugerida para endpoints em FastAPI>
```

### 🚀 Router Principal
```Python
# src/api/v1/router.py

<gerar aqui o código python>

```

## 3. Padrões de endpoints CRUD

### 👤 **Exemplo completo: User endpoints**

```Python
# src/api/v1/users.py

<gerar aqui o código python>

```


## 4. Schemas Pydantic

### 📄 **Schemas de usuários**

```Python
# src/api/v1/user_schemas.py
"""Schemas pydantic para usuários"""

<gerar aqui o código python>

```

### 📄 **Schemas comuns**

```Python
# src/api/v1/common_schemas.py
"""Schemas comuns reutilizáveis"""

<gerar aqui o código python>

```



## 5. Versionamento de API

### ♻️ **Estratégias de Versionamento**

```Python
# src/api/main.py
"""Configuração principal da aplicação com versionamento"""

<gerar aqui o código python>

```


### ♻️ **Versionamento por header**

```Python
# src/api/core/versioning.py
"""Sistema de versionamento por header"""

<gerar aqui o código python>

```


## 6. Documentação OpenAPI

### 📚 **Configuração avançada**

```Python
# src/api/core/openapi.py
"""Sistema avançada OpenAPI"""

<gerar aqui o código python>

```


## 7. Tratamento de Erros

### ❗ **Handlers de Exceções Globais **

```Python
# src/api/core/exception_handlers.py
"""Handlers Globais de Exceção"""

<gerar aqui o código python>

```


## 8. Boas Práticas
### ✅ **Checklist de Boas Práticas Recomendadas**

```Python
Gerar aqui as instruções e exemplo de código

Exemplo de como você deve gerar:

# ✅ Use status codes apropriados
@router.post("/", status_code=status.HTTP_201_CREATED)

...Demais boas práticas

### ❌ **DONT`s práticas a evitar**

# ❌ não use status codes genéricos
return {"message":"success"} # Sem status code específico e sem mensagem intuitiva

...Demais práticas a evitar


```

salve a regra na pasta python/api/fastapi-endpoints.mdc
