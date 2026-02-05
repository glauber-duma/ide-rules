você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-validdation.mdc
Description: Diretrizes para implementação de validação robusta com Pydantic em FastAPI, cobrindo modelos, validadores customizados,
serialização e tratamento de erros.
globs: ["**/schemas/**/*.py", "**/models/**/*.py", "**/validators/**/*.py", "**/validation/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "pydantic", "validators"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI - Validação com Pydantic

## 1. Objetivo

Estabelecer padrões para validação robusta e completa usando Pydantic, garantindo:
- **Validação automática** de entrada e saída de dados
- **Modelos tipados** com documentação automática
- **Validadores customizados** para regras de negócio
- **Serialização consistente** entre diferentes contextos
- **Tratamento adequado** de erros de validação

---


## 2. Estrutura de modelos Pydantic

### 🗂️ **Organização de Schemas**


```
fastapi_project
|---src/
|.   |---api/
|.        |---schemas/                                   # schemas Pydantic
|.            |---__init__.py
|.            |---base.py                               # schemas base reutilizáveis
|.            |---common.py                             # schemas comuns (paginação, erro, etc...)
|.            |---user/                                 # Schemas de usuário
|.        .   |    |---__init__.py
|.        .   |    |---user_schemas.py                  # CRUD
|.        .   |    |---auth_schemas.py                  # Autenticação
|.        .   |    |---profile_schemas.py               # Perfil
|.            |---product/                              # Schemas de produtos
|.        .   |    |---__init__.py
|.        .   |    |---product_schemas.py                  
|.        .   |    |---category_schemas.py   
|.            |---validators/                           # Validadores customizados
|.        .        |---__init__.py
|.        .        |---decorators.py                  
|.        .        |---business_validators.py   
|-- models/                                             # Models de database (SQLAlchemy, etc.)
|-- core/
     |-- validation/                                    # Utilitários de validação
     |      |---__initi__.py
     |      |---decorators.py                           # Decoradores de validação
     |      |---exceptions.py                           # Exceções customizadas
     |---types.py                                       # Tipos customizados

```



## 🏗️ **Base Models**

```Python
# src/api/schemas/base.py
"""
Schemas base para reutilização
"""
<gerar aqui o código python>

```

## 3. Schemas CRUD Avançados

### 👤 **User Schemas Completos**

```Python
# src/api/schemas/user/user_schemas.py
"""
Schemas completos para usuários.
"""

<gerar aqui o código python>

```

## 4. Validadores Customizados

### 🔎 **Validadores especializados**

```Python
# src/api/schemas/validators/password_validator.py
"""
Validadores de senhas robusto
"""

<gerar aqui o código python>

```


## 5. Serialização Avançada

### 📄 **Context-Aware Serialization**

```Python
# src/api/schemas/serialization.py
"""
Serialização consciente de contexto
"""

<gerar aqui o código python>

```


## 6. Validação Assíncrona

### ⚡️ **Async Validators**

```Python
# src/api/schemas/async_validators.py
"""
Validadores assíncronos para verificações que requerem database/API
"""

<gerar aqui o código python>

```


## 7. Testes de Validação 

### 🧪 **Testing Strategies**

```Python
# tests/test_validation.py
"""
Testes para validação Pydantic
"""

<gerar aqui o código python>

```


## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use schemas para cada operação
<gerar aqui o código python>

✅ Valide entrada rigorosamente
<gerar aqui o código python>

✅ Use Field() para documentação e validação
<gerar aqui o código python>

✅ Implemente validação contextual
<gerar aqui o código python>

✅ Use root_validators para validação entre campos
<gerar aqui o código python>


### ❌ **DONT`s - Práticas a Evitar**

❌ Não use echemas para tudo
<gerar aqui o código python>

❌ Não ignore validação de entrada
<gerar aqui o código python>

❌ Não use valores de validação hardcode 
<gerar aqui o código python>

❌ Não esponha campos sensíveis
<gerar aqui o código python>

❌ Não ignore erros de validação
<gerar aqui o código python>


```





salve a regra na pasta python/api/fastapi-validation.mdc
