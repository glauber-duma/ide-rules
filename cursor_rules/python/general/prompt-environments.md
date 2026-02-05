você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-environments.mdc
Description: Diretrizes para configuração e gerenciamento de ambientes Python usando venv, conda, pyenv e docker, cobrindo desenvolvimento, teste e produção.
globs: ["**/venv/**", "**/.venv/**", "**/conda.yml", "**/enviroment.yml", "**/.python-version", "**/Dockerfile", "**/docker-compose.yml"]
alwaysApply: false
tags: ["fastapi", "api", "enviropnments"]
Author: Glauber Duma
Created: 2026-01
---

# Python - Configuração de Ambientes

## 1. Objetivo

Estabelecer padrões para configuração e gerenciamento de ambientes Python, garantindo:
- **Isolamento completo** entre projetos e dependências
- **Reprodutibilidade** entre ambientes de desenvolvimento, teste e produção
- **Compatibilidade** de versões de Python e bibliotecas
- **Facilidade de Setup** para novos desenvolvedores
- **Portabilidade** entre diferentes máquinas e sistemas operacionais

---



## 2. Estratégias de Gerenciamento

### ⚖️ **Comparação de Ferramenta**

```python
"""
Comparação de ferramentas de gerenciamento de ambientes
"""

Exemplo:

# ==== VENV (padrão built-in) ====
# Prós: nativo, simples, universal, leve
# Contras: Apenas Python, sem gestão de versão do Python
# Uso: projetos simples, sistemas legados, desenvolvimento básico
# Comando: python -m venv .venv


<inserir aqui o resultado das outras comparações>

```

## 3. Configuração com venv (Recomendado Basico)

### 📂 **Estrutura de Projeto**


```bash
# estrutura de projeto com venv
|---project/
|---.venv/                  # ambiente virtual
|---src/
|---tests/
|---docs/
|---scripts/
|---requirements.txt
|---requirements-dev.txt
|---.python-version
|---.env
|---.env.example
|---.gitignore
|---README.md
```

### ⚙️ **scripts de setup automatizado**

```bash
# !/bin/bash
# scripts/setup.sh - Script para configurar ambiente com venv

<gerar aqui o código bash>

```

```powershell
# scripts/setup.ps1 - Script para configurar ambiente com venv

<gerar aqui o código powershell>

``` 


### 📋 **makefile para gerenciamento de ambiente**

```makefile
# Makefile para gerenciamento de ambiente 

<gerar aqui o código makefile>

```



## 4. Configuração com Conda

### 📋 **environment.yml Completo**


```yaml
# environment.yml - Configuração de ambiente Conda

<gerar aqui o conteúdo completo do arquivo environment.yml>

```

### 📋 **Scripts Conda**

```bash
# scripts/setup_conda.sh - Script para configurar ambiente com Conda

<gerar aqui o código bash>

```


## 5. Configuração com Poetry

### 📋 **Poetry Enviroment Setup**


```bash
# scripts/setup_poetry.sh - Script para configurar ambiente com Poetry
<gerar aqui o código bash>


```

### ⚙️ **Configuração Poetry no pyproject.toml**


```

<gerar aqui o conteúdo completo do arquivo pyproject.toml>

```


## 6. Configuração com Docker

### 📋 **Dockerfile multi-stage**


```dockerfile
# Dockerfile multi-stage para aplicação Python

<gerar aqui o dockerfile completo>

```

### 📋 **docker compose para desenvolvimento**


```yaml
# docker-compose.yml - Configuração Docker Compose para desenvolvimento

<gerar aqui o docker-compose completo>

```

### 📋 **docker compose para testes**


```yaml
# docker-compose-test.yml - Configuração Docker Compose para testes

<gerar aqui o docker-compose completo>

```



## 7. Variáveis de Ambiente

### 🔧 **Gestão de Configuração**


```python
# src/config/settings.py
"""
Configuração centralizada usando pydantic settings
"""

<gerar aqui o código python>

```

## 8. CI/CD integration

### 🚀 **Github Actions com multiplos ambientes**


```yaml
# .github/workflows/enviroment-matrix.yml

<gerar aqui o script de ci/cd>

```


## 9. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use arquivos .python-version para especificar versões do Python

✅ Sempre use ambientes virtuais 

✅ Documente setup no README.md

✅ Use scripts automatizados

✅ Configure variáveis de ambiente adequadamente

✅ Use .gitignore apropriado

✅ Documente dependencias de sistema



### ❌ **DONT`s - Práticas a Evitar**

❌ Não comite ambiente virtual

❌ Não use python global para projetos

❌ Não misture ferramentas inconsistententemente

❌ Não hardcode caminhos específicos

❌ Não ignore configurações de ambiente

❌ Não use versões inconsistentes

❌ Não deixe dependencias desatualizadas

```



salve a regra na pasta python/general/python-enviroment.mdc
