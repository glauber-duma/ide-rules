você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-dependencies.mdc
Description: Diretrizes para gerenciamento de dependências Python usando pip, poetry, conda e ferramentas de segurança, cobrindo versionamento, ambientes virtuais e governança.
globs: ["**/requirements*.txt", "**/pyproject.toml", "**/setup.py", "**/setup.cfg", "**/Pipfile", "**/conda.yml", "**/enviroment.yml"]
alwaysApply: false
tags: ["fastapi", "api", "dependencies"]
Author: Glauber Duma
Created: 2026-01
---

# Python - Gerenciamento de Dependencias

## 1. Objetivo

Estabelecer padrões para gerenciamento de dependencias Python, garantindo:
- **Reprodutibilidade** de ambiente entre desenvolvimento, teste e produção
- **Segurança** através de versionamento controlado e auditoria
- **Performance** com otimização de instalação e cache
- **Governança** de dependencias corporativas e licenças
- **Manutenibilidade** com atualizações controladas e compatibilidade

---



## 2. Estratégias de Gerenciamento

### 📦 **Escolha da Ferramenta**

```python
"""
Comparação de ferramentas de gerenciamento de dependencias
"""

Exemplo:

# ==== PIP + requirements.txt (abordagem tradicional) ====
# Prós: universal, simples, compativel com tudo
# Contras: sem lock file nativo, resolução de dependencias limitada
# Uso: projetos simples, sistemas legados, CI/CD básico


<inserir aqui o resultado das comparações>

```

## 3. Configuração com requeriments.txt

### 📋 **Estrutura de requirements multiplos**


```txt
|---requirements/
|---base.txt         # dependencias base (produção)
|---development.txt. # dependencias de desenvolvimento
|---production.txt.  # dependencias de produção
|---optional.txt.    # dependencias opcionais por feature
```

### 📋 **requirements/base.txt**

```txt
# requirements/base.txt
# Core dependencies for production.

# === WEB framework ===

<informar aqui as dependencias, exemplo: fastapi, uvicorn, pydantic,etc..>

# === Database ===

<informar aqui as dependencias, exemplo: motor, asyncpg, etc..>

# === Http client ===

<informar aqui as dependencias>


# === Logging and monitoring ===

<informar aqui as dependencias>

# === Mesaging ===

<informar aqui as dependencias>

# === Security ===

<informar aqui as dependencias>

# === Utilities ===

<informar aqui as dependencias>


# === Dependency injection (container, providers (dependency_injector framework)) ===

<informar aqui as dependencias>

```

### 📋 **requirements/development.txt**

```txt
# requirements/development.txt
# Dependencias de ambiente de desenvolvimento

-r base.txt



# === Code Quality ===

<informar aqui as dependencias, exemplo: black, isort, flake8, mypy, bandit>

# === Pré Commit ===

<informar aqui as dependencias, exemplo: pre-commit>

# === Development Tools ===

<informar aqui as dependencias>


# === API Documentantion ===

<informar aqui as dependencias>

# === Dependency Management ===


```


### 🧪 **requirements/testing.txt**

```txt
# requirements/testing.txt
# Dependencias de ambiente de teste

-r base.txt

# === Testing framework ===

<informar aqui as dependencias, exemplo: pytest, pytest-asyncio, pytest-xdist,etc.>

# === Coverage ===

<informar aqui as dependencias, exemplo: coverage>

# === Mocking ===

<informar aqui as dependencias>

# === Database testing ===

<informar aqui as dependencias>

# === Performance testing ===

<informar aqui as dependencias>

# === Factories ===

<informar aqui as dependencias>

```



### 🚀 **requirements/production.txt**

```txt
# requirements/production.txt
# Dependencias de ambiente de produção

-r base.txt

# === Production server ===

<informar aqui as dependencias, exemplo: gunicorn, uvloop,etc.>

# === Monitoring ===

<informar aqui as dependencias, exemplo: prometheus-client, datadog>

# === Caching ===

<informar aqui as dependencias>

# === Security ===

<informar aqui as dependencias>

```

## 4. Configuração com Poetry

### 📋 **Poetry.toml Completo**


```toml
# pyproject.toml

<gerar aqui o conteúdo completo do arquivo pyproject.toml>

```


## 5. Versionamento semantico

### 📌 **Estratégias de pinning**


```txt

<gerar aqui as estratégias de pinning>

```

### 🔒 **Lock Files e Reprodutibilidade**


```bash

<gerar aqui pip-tools workflow que cubra:
- criação do requirements.in
- compilação para requirements
- instalação das dependencias 
- atualização das dependencias
- verificação das atualizações disponíveis
>

<gerar aqui poetry workflow que cubra:
- adicionar dependencias
- instalar com lock
- atualização das dependencias
- verificação das atualizações disponíveis
>

```


## 6. Segurança de Dependencias

### 🔒 **Auditoria de segurança**


```Python

"""
Scripts para auditoria de segurança de dependências
"""

<gerar aqui o código python>

```

```Python

"""
Security Audit Report
"""

<gerar aqui o código python>

```

```Python

"""
Scripts de automação
"""

<gerar aqui o código python>

```

## 7. Workflow de CI/CD

### 🚀 **Github Actions para dependencias**


```yaml
# .github/workflows/dependencies

<gerar aqui o script de ci/cd>

```

## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use lock files para reprodutibilidade
<gerar aqui o código python>

✅ Separe dependencias por ambiente
<gerar aqui o código python>

✅ Use versionamento semantico conservador
<gerar aqui o código python>

✅ Execute auditoria de segurança regularmente
<gerar aqui o código python>

✅ Monitore licenças de dependencias
<gerar aqui o código python>

✅ Use cache em CI/CD
<gerar aqui o código python>

✅ Documente dependencias opicionais
<gerar aqui o código python>


### ❌ **DONT`s - Práticas a Evitar**

❌ Não user versões sem restrições
<gerar aqui o código python>

❌ Não misture ferramentas inconsistententemente
<gerar aqui o código python>

❌ Não comite arquivos em ambiente virtual
<gerar aqui o código python>

❌ Não ignore alertas de segurança
<gerar aqui o código python>

❌ Não misture conda com pip
<gerar aqui o código python>

❌ Não use sudo com pip
<gerar aqui o código python>


```



salve a regra na pasta python/general/python-dependencies.mdc
