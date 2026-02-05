você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-style-guide.mdc
Description: Guia para padrões de código Python, inclui nomenclatura, formatação, type hints e boas práticas de desenvolvimento
globs: ["**/*.py", "**/*.pyi"]
rules: @python-project-structure
alwaysApply: false
tags: ["python", "style", "best pratices"]
Author: Glauber Duma
Created: 2026-01
---

# Python Style Guide - Padrões e boas práticas

Este guia estabelece padrões consistentes para escrita de código Python, garantindo legibilidade, manutenabilidade e qualidade em todos os projetos.


## 🎯 **Princípios fundamentais**

### The Zen of Python (PEP 20)

```Python
"""
Beatiful is better than ugly.
Explicity is better than implicity.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparce is better than dense.
Readability counts.
"""
```

## 📏 **Formatação e Estilo**

### 1.1 Tamanho da linha e quebras

```python

# ✅ Correto - Máximo 88 caracteres (padrão Black)

<gerar código de exemplo aqui>

# ✅ Correto - Quebra de linha em expressões longa

<gerar código de exemplo aqui>


# ❌ Incorreto - linha muito longa

<gerar código de exemplo aqui>

```

### 1.2 Espaçamento e Identação

```python

# ✅ Correto - quatro espaços para identação

<gerar código de exemplo aqui>

# ✅ Correto - Espaçamento em operadores

<gerar código de exemplo aqui>


# ❌ Incorreto - Espaçamento inconsistente

<gerar código de exemplo aqui>

```

### 1.3 Oraganização dos imports

```python

# ✅ Correto - ordem e agrupamento padrão

# standard lybrary
<gerar código de exemplo aqui>

# third-party packages
<gerar código de exemplo aqui>

# local imports
<gerar código de exemplo aqui>

# relative imports
<gerar código de exemplo aqui>


# ❌ Incorreto - Imports misturados e desordenados

<gerar código de exemplo aqui>

```

## 🏷️​ **Nomenclatura e convenções**

### 2.1 variáveis e funções (snake_case)

```python

# ✅ Correto - snake case para variáveis e funções

<gerar código de exemplo aqui>


# ❌ Incorreto - Camel case para variáveis e funções

<gerar código de exemplo aqui>

```

### 2.2 Classes (PascalCase)

```python

# ✅ Correto - pascal case para classes

<gerar código de exemplo aqui>


# ❌ Incorreto - nomenclatura inadequada para classes

<gerar código de exemplo aqui>

```

### 2.3 Constantes (UPPER_SNAKE_CASE)

```python

# ✅ Correto - maíusculas para constantes

<gerar código de exemplo aqui>


# ❌ Incorreto - nomenclatura inadequada para constantes

<gerar código de exemplo aqui>

```

### 2.4 Private and protected members

```python

# ✅ Correto - convenções de privacidade

<gerar código de exemplo aqui>


# ❌ Incorreto - não seguir convenções de privacidade

<gerar código de exemplo aqui>

```

## 📊​ **Type Hints e Anotações**

### 3.1 Function type anotations

```python

# ✅ Correto - type hints completos e claros

<gerar código de exemplo aqui>


# ❌ Incorreto - type hints ausentes ou incompletos

<gerar código de exemplo aqui>

```


### 3.2 Class Type Anotations

```python

# ✅ Correto - Atributes com type hints

<gerar código de exemplo aqui>


# ❌ Incorreto - Atributes sem o tipo

<gerar código de exemplo aqui>

```


### 3.3 Generic types e Advanced Annotations

```python

# ✅ Correto - Generics e types avançados

<gerar código de exemplo aqui>

```


## 🏗️​ **Estrutura de classes e funções**

### 4.1 Class organization

```python

# ✅ Correto - organização clara de classe

<gerar código de exemplo aqui>


```


### 4.2 Function organization and documentation

```python

# ✅ Correto - função bem documentada e estruturada

<gerar código de exemplo aqui>


# ❌ Incorreto - função mal documentada e estruturada

<gerar código de exemplo aqui>

```

## 🐍​ **Python Idioms and best pratices**

### 5.1 Pythonic code patterns

```python

# ✅ Correto - List comprehensions e iterators pythonicos

<gerar código de exemplo aqui>


# ❌ Incorreto - padrões não pythonicos

<gerar código de exemplo aqui>

```


### 5.2 Exception Handling

```python

# ✅ Correto - Exception handling específico e informativo

<gerar código de exemplo aqui>


# ❌ Incorreto - Exception handling genérico

<gerar código de exemplo aqui>

```

### 5.3 Resource Management

```python

# ✅ Correto - Context manager e resource cleanup

<gerar código de exemplo aqui>

# ✅ Correto - Async context managers

<gerar código de exemplo aqui>

```

## ⚡️​ **Performance and Optimization**

### 6.1 Estrutura de dados eficiente

```python

# ✅ Correto - Estrutura de dados eficientes

<gerar código de exemplo aqui>


# ❌ Incorreto - Estrutura de dados ineficientes

<gerar código de exemplo aqui>

```


### 6.2 Memory and CPU Optimization

```python

# ✅ Correto - Generator expressions para economia de memória

<gerar código de exemplo aqui>


# ❌ Incorreto - Operações ineficientes

<gerar código de exemplo aqui>

```

## 🔧​ **Configurations and Enviroments**

### 7.1 Quality metrics and standards

```python

# Metricas de qualidade obrigatória
# - Code Coverage: >= 80%
# - Complexity: < 10 (McCabe)
# - Duplication: < 3%
# - Security: 0 high/critical vulnerabilities
# - Performance: response time < 200ms

```
salve a regra na pasta python/general/python-style-guide.mdc