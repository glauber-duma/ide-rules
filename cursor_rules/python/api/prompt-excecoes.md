você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com exceções.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-exception.mdc
Description: Diretrizes para implementações de tratamento de erros em FastAPI, cobrindo exceções globais, exceções customizadase loggind estruturado.
globs: ["**/exceptions/**/*.py", "**/handlers/**/*.py", "**/errors/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "exceptions", "handlers"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI - Tratamento de Excessões

## 1. Objetivo

<incluir os objetivos principais da rule>

## 2. Hierarquia

### 🗂️ Estrutura de Exceções

```
src/api
|---core/
|.    |-----exceptions/
|     |     |---init.py
|.    |.    |---base.py # Excessões base
|.    |.    |---business.py # Excessões de negócio
|.    |.    |---auth.py # Excessões de autenticação
|.    |.    |---validation.py # Excessões de validação
|.    |.    |---external.py # Excessões de serviços externo
|.    |-----handlers/
|     |     |---init.py
|.    |.    |---exception_handlers.py # Handlers Globais
|.    |.    |---http_handlers.py # handlers http específicos
|.    |.    |---async_handlers.py # handlers assincronos
|.    |-----responses/
|           |---init.py
|.          |---error_responses.py # respostas de erros padronizadas
|.          |---success_responses.py # respostas de sucesso padronizadas
|---main.py # onde é registrado os handlers





```

### 🏗️ Excessões Base
```Python
# src/api/core/exceptions/base.py
"""Exceções base da aplicação"""

<gerar aqui o código python>

```

## 3. Exceções específicas por domínio

### 👤 **Exceções de usuário**

```Python
# src/core/exceptions/auth.py
"""Exceções relacionadas à autenticação e autorização"""

<gerar aqui o código python>


# src/core/exceptions/business.py
"""Exceções relacionadas à negócio"""

<gerar aqui o código python>


# src/core/exceptions/validations.py
"""Exceções relacionadas à validações"""

<gerar aqui o código python>


# src/core/exceptions/external.py
"""Exceções para serviços externos"""

<gerar aqui o código python>


```


## 4. Exceptions Handlers

### 🛠️ **Handlers Globais**

```Python
# src/core/handlers/exceptions_handlers.py
"""Exceptions handlers globais para FASTAPI"""

<gerar aqui o código python>

```

### 📄 **Schemas comuns**

```Python
# src/api/v1/common_schemas.py
"""Schemas comuns reutilizáveis"""

<gerar aqui o código python>

# Handlers específicos

<gerar aqui o código python>

# Decorador para exception handling

<gerar aqui o código python>


```



## 5. Respostas de erro padronizada

### 🗂️ **Error response builder**

```Python
# src/api/core/responses/error_responses.py
"""Builder para respostas de erro padronizada"""

<gerar aqui o código python>

# responses específicos

<gerar aqui o código python>

# middleware de response formatting
<gerar aqui o código python>

```

## 6. Contexto e correlação

### 🔎 **Request Tracing**

```Python
# src/api/core/exceptions/context.py
"""Contexto de exceções para rastreamento"""

<gerar aqui o código python>

# Decorador para contexto automático

<gerar aqui o código python>

# middleware de contexto
<gerar aqui o código python>


```


## 7. Configuração e registro

### ⚙️ **Setup de Exception Handlers**

```Python
# src/api/main.py
"""Registro de exception handlers"""

<gerar aqui o código python>

```


## 8. Boas Práticas
### ✅ **Checklist de Boas Práticas Recomendadas**

```Python
Gerar aqui as instruções e exemplo de código

Exemplo de como você deve gerar:

# ✅ Use exceções específicas por tipo de erro
raise UserNotFoundError(user_id) # específica

...Demais boas práticas

### ❌ **DONT`s práticas a evitar**

# ❌ não use exceções genéricas
raise Exception("Algum Erro")  # muito genérico

...Demais práticas a evitar


```

salve a regra na pasta python/api/fastapi-exception.mdc
