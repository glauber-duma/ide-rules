você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-project.mdc
Description: Diretrizes para implementação de middlewares e interceptadores em FastAPI, cobrindo logging, autenticação, CORS, rate limiting e monitoramento.
globs: ["**/middleware/**/*.py", "**/middlewares/**/*.py", "**/interceptors/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "middleware", "interceptor"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI Project - Middlewares e Interceptors

## 1. Objetivo

Estabelecer padrões para implementação de middlewres em FastAPI, garantindo:
- **Interceptação transversal** de requisições e respostas
- **Logging estruturado** de todas operações
- **Segurança robusta** com autenticação e autorização
- **Performance** com cache e rate limiting
- **Monitoramento** com métricas e telemetria

---



## 2. Arquitetura de Middlewares

### 🗂️ **Estrutura de organização**




```
fastapi_project
|---src/
|.   |---api/
|.        |---core/
|.        |    |---middlewares/                       
|.        |    |        |---__init__.py
|.        |    |        |---base.py                     # Base middleware class
|.        |    |        |---logging.py                  # Request/Response logging
|.        |    |        |---autentication.py            # JWT authentication
|.        |    |        |---authorization.py            # role-based access control
|.        |    |        |---rate_limiting.py            # Rate limiting
|.        |    |        |---cors.py                     # CORS handling
|.        |    |        |---security.py                 # Security headers
|.        |    |        |---compression.py              # Response compression
|.        |    |        |---caching.py                  # Response caching
|.        |    |        |---metrics.py                  # Metrics collection
|.        |    |        |---error_handling.py           # Global Error Handling
|.        |    |        |---request_id.py               # Request tracing
|.        |    |---dependencies.py                      # FastAPI dependencies
|.        |    |---config.py                            # Middleware configuration
|--- main.py                                            # Middleware registration

```

### 🏗️ **Base Middleware**

```Python
# src/api/core/middleware/base.py
"""
Base middleware abstrato para reutilização
"""

<gerar aqui o código python>

```

## 3. Middleware de logging

### 📄 **Request/Response logging**


```Python
# src/api/core/middleware/logging.py
"""
Middleware de logging estruturado
"""

<gerar aqui o código python>

```


## 4. Middleware de segurança

### 🔒 **Autenticação e Autorização**


```Python
# src/api/core/middleware/authentication.py
"""
Middleware de autenticação JWT
"""

<gerar aqui o código python>

```


### 🛡️ **Security Headers**


```Python
# src/api/core/middleware/security.py
"""
Middleware de cabeçalhos de segurança
"""

<gerar aqui o código python>

```

## 5. Rate Limiting

### ⏰ **Rate Limiting Middleware**


```Python
# src/api/core/middleware/rate_limiting.py
"""
Middleware de rate limiting
"""

<gerar aqui o código python>

```

## 6. Middleware de cache e performance

### 🚀 **Response caching**


```Python
# src/api/core/middleware/caching.py
"""
Middleware de cache de respostas
"""

<gerar aqui o código python>

```

## 7. Configuração e Registro

### ⚙️ **Configuração Central**


```Python
# src/api/core/middleware/config.py
"""
Configuração central de middlewares
"""

<gerar aqui o código python>

```


### 🔧 **Registro de Middlewares**


```Python
# src/api/main.py
"""
Registro central de middlewares
"""

<gerar aqui o código python>

```

## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use ordem correta dos middlewares
<gerar aqui o código python>

✅ Implemente middleware condicional
<gerar aqui o código python>

✅ Use context request para estado
<gerar aqui o código python>

✅ Trate erros adequadamente
<gerar aqui o código python>

✅ Adicione headers informativos
<gerar aqui o código python>


### ❌ **DONT`s - Práticas a Evitar**

❌ Não bloqueie o thread com operações síncronas
<gerar aqui o código python>

❌ Não ignore erros silenciosamente
<gerar aqui o código python>

❌ Não modifique request/response incorretamente
<gerar aqui o código python>

❌ Não faça operações pesadas em middlewares
<gerar aqui o código python>



```





salve a regra na pasta python/api/fastapi-middleware.mdc
