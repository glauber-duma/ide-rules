você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-cache.mdc
Description: Diretrizes para implementação estratégica de cache em Python, cobrindo Redis, cache em memória, invalidação e padrões de performance.
globs: ["**/cache/**/*.py", "**/caching/**/*.py", "**/redis/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "cache", "redis"]
Author: Glauber Duma
Created: 2026-01
---

# Python - Estratégias de Cache

## 1. Objetivo

Estabelecer padrões para implementação eficaz de cache em aplicações Python, garantindo:
- **Performance otimizada** com redução de latência
- **Escalabilidade** com distribuição de cache
- **Consistência** de dados com invalidação adequada
- **Flexibilidade** com multiplas estratégias de cache
- **Monitoramento** de hit rates e performance

---



## 2. Arquitetura de Cache

### 🗂️ **Estrutura de organização**


```
fastapi_project
|---src/
|.   |---cache/
|.   |     |---__init__.py
|.   |     |---interface.py                              # Interface abstrata de cache
|.   |     |---backends/                                 # Diferentes implementações
|.   |     |    |---__initi__.py
|.   |     |    |---memory.py                            # Cache em memória
|.   |     |    |---redis.py                             # Cache Redis
|.   |     |    |---hybrid.py                            # Cache Híbrido
|.   |     |    |---null.py                              # Cache nulo (para testes)
|.   |     |---decorators.py                             # Decoradores de cache
|.   |     |---keys.py                                   # Geração de chaves
|.   |     |---serializers.py                            # Serialização de dados  
|.   |     |---invalidation.py                           # Estratégias de invalidação
|.   |     |---monitoring.py                             # Métricas de cache
|.   |---core/
|.   |.   |---config.py
|.   |.   |---cache/
|.   |         |---__init__.py
|.   |         |---config.py                            # Configuração de cache
|.   |         |---dependencies.py                      # FastAPI Dependencies
|.   |         |---middleware.py                        # Middleware de cache
|.   |---business/
            |---services/
                  |---cached_services.py              # Serviços com cache

```

### 🔧 **Cache Configuration**

```Python
# src/core/cache/config.py
"""
Configuração de cache
"""

<gerar aqui o código python>

```

## 3. Interface de Cache

### 🔌 **Cache Interface**


```Python
# src/cache/interface.py
"""
Interface abstrata de Cache
"""

<gerar aqui o código python>

```


## 4. Implementação Redis

### 🔴 **Redis Cache Backend**


```Python
# src/cache/backends/redis.py
"""
Implementação de cache utilizando Redis
"""

<gerar aqui o código python>

```


## 5. Cache Serializers

### 📦 **Serialização de Dados**


```Python
# src/cache/serializers.py
"""
Serialização de dados para cache.
"""

<gerar aqui o código python>

```

## 6. Cache Decorators

### 🎭 **Decoradores de Cache**


```Python
# src/cache/decorators.py
"""
Decoradores para cache automático
"""

<gerar aqui o código python>

```

## 7. Cache Management

### ⚙️ **Gerenciamento de Cache**


```Python
# src/cache/management.py
"""
Gerenciamento e manutenção de cache
"""

<gerar aqui o código python>

```



## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use ttl apropriado por tipo de dado
<gerar aqui o código python>

✅ Implemente cache warning para dados críticos
<gerar aqui o código python>

✅ Use chaves estruturadas e versionadas
<gerar aqui o código python>

✅ Implemente fallback gracioso
<gerar aqui o código python>

✅ Use invalidação inteligente
<gerar aqui o código python>


### ❌ **DONT`s - Práticas a Evitar**

❌ Não use cache para dados críticos sem fallback
<gerar aqui o código python>

❌ Não use ttl muito baixo sem necessidade
<gerar aqui o código python>

❌ Não use cache de dados sensíveis sem criptografia
<gerar aqui o código python>

❌ Não use chaves muito longas ou complexas
<gerar aqui o código python>

❌ Não esqueça de limpar cache em operações de escrita
<gerar aqui o código python>


```



salve a regra na pasta python/general/python-cache.mdc
