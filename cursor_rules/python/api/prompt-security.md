você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: fastapi-security.mdc
Description: Diretrizes de segurança para apis Python/FastAPI, baseado em práticas de aplicações modernas.
globs: ["**/*.py"]
rules: @fastapi-project,@python-style-guide,@python-logging
alwaysApply: false
tags: ["fastapi", "api", "security", "modern"]
Author: Glauber Duma
Created: 2026-01
---

# FastAPI Security - Diretrizes de segurança para apis Python

Esta regra estabele padrões de segurança para API´s baseado em análise de projetos reais e melhores práticas.

## 🔒 **Autenticação e Autorização**

### 1.1. JWT Autenticação e Autorização


```Python
# src/app/core/security.py

<gerar aqui o código python>


```



## 🧹 **Input validation e Sanitização**
### 2.1. Input Sanitização
```Python
# src/app/core/validators.py

<gerar aqui o código python>

```

### 2.2. File upload security
```Python
# src/app/core/file_security.py

<gerar aqui o código python>

```

## 🔒 **Security headers e Middleware**

### 3.1 security headers middleare

```Python
# src/app/api/middleware/security.py

<gerar aqui o código python>

```


## 🛠️ **Security testing**

### 4.1. Security test suite

```Python
# tests/security/test_autentication.py

<gerar aqui o código python>

```


salve a regra na pasta python/api/fastapi-security.mdc
