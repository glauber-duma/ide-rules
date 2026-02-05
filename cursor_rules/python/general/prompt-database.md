você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-database.mdc
Description: Diretrizes para integração robusta com banco de dados Python, cobrindo SQLAlchemy, Motor(MongoDB),PostgreSQL, migrations e padrões de repositório.
globs: ["**/database/**/*.py", "**/models/**/*.py", "**/repositories/**/*.py", "**/migrations/**/*.py", "**/*_model.py"]
alwaysApply: false
tags: ["fastapi", "api", "database", "sqlalchemy","mongodb"]
Author: Glauber Duma
Created: 2026-01
---

# Python - Integração com banco de dados

## 1. Objetivo

Estabelecer padrões para integração robusta com banco de dados, garantindo:
- **Abstração adequada** entre camada de dados e negócio
- **Performance otimizada** com queries eficientes e cache
- **Transações seguras** com rollback automático em caso de erros
- **Migrations Controladas** para evolução do schema
- **Testabilidade** com mocks e databases de teste

---



## 2. Arquitetura de Dados

### 🗂️ **Estrutura de organização**


```
fastapi_project
|---src/
|.   |---database/
|.   |     |---__init__.py
|.   |     |---connection.py                              # Gerenciamento de conexões
|.   |     |---session.py                                 # Gerenciamento de sessões
|.   |     |---migrations/                                # Database migrations
|.   |     |    |---__init__.py
|.   |     |    |---env.py                               # Alembic enviroment
|.   |     |    |---script.py.mako                       # Migration Template
|.   |     |    |---versions/                            # Migration files
|.   |     |---seed/                                     # Seed data
|.   |          |---__initi__.py
|.   |          |---initial_data.py 
|.   |---models/                                         # Domain models
|.   |     |---__init__.py
|.   |     |---base.py                                   # base model class
|.   |     |---user.py                                   # User domain model
|.   |     |---product.py                                # Product domain model
|.   |     |---mixins.py                                 # Model mixins
|.   |---repositories/                                   # Repository pattern
|.   |     |---__init__.py
|.   |     |---base.py                                   # base repository
|.   |     |---user_repository.py                        # User data access
|.   |     |---product_repository.py                     # Product data access
|.   |     |---unit_of_work.py                           # Unit of Work pattern
|.   |---core/
|.        |---config.py                                   # database configuration
.         |---database/
.               |---__init__.py
.               |---deps.py                               # FastAPI dependencies
.               |---exceptions.py                         # Database exceptions
.               |---utils.py                              # Database utilities


```

### 🔧 **Base database configuration**

```Python
# src/core/config.py
"""
Configuração de banco de dados
"""

<gerar aqui o código python>

```

## 3. SQLAlchemy com PostgreSQL

### ⚙️ **Setup Assíncrono**


```Python
# src/database/connection.py
"""
Gerenciamento de conexões
"""

<gerar aqui o código python>

```

### 📄 **Base Models**


```Python
# src/models/base.py
"""
Models base para SqlAlchemy
"""

<gerar aqui o código python>

```


## 4. Repository Pattern

### 🏭 **Base Repository**


```Python
# src/repositories/base.py
"""
Repository base com operações de CRUD
"""

<gerar aqui o código python>

```


## 5. Unit Of Work

### ♻️ **Transction Management**


```Python
# src/repositories/unit_of_work.py
"""
Unit of work para gerenciamento de transações
"""

<gerar aqui o código python>

```

## 6. MongoDB com Motor

### 🍃 **Setup Assincrono do MongoDB**


```Python
# src/database/mongodb.py
"""
Configuração do MongoDB com motor
"""

<gerar aqui o código python>

```

## 7. FastAPI Dependencies

### 📎 **Database Dependencies**


```Python
# src/core/database/deps.py
"""
Dependencies do FastAPI para database.
"""

<gerar aqui o código python>

```



## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use async/await para operações de banco de dados
<gerar aqui o código python>

✅ Implemente soft delete quando apropriado
<gerar aqui o código python>

✅ Use repository pattern para abstração
<gerar aqui o código python>

✅ Gerencie transações com unity of work
<gerar aqui o código python>

✅ Use indexação adequada
<gerar aqui o código python>

✅ Valide a entrada antes de salvar no database
<gerar aqui o código python>



### ❌ **DONT`s - Práticas a Evitar**

❌ Não use operações sincronas
<gerar aqui o código python>

❌ Não deixe conexões abertas
<gerar aqui o código python>

❌ Não ignore transações
<gerar aqui o código python>

❌ Não faça queries N+1
<gerar aqui o código python>

❌ Não use instruções sql hardcode quando desnecessário
<gerar aqui o código python>

❌ Não exponha models de database diretamente
<gerar aqui o código python>


```



salve a regra na pasta python/general/python-database.mdc
