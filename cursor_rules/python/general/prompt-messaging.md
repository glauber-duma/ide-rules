você é um engenheiro de software senior especialista no desenvolvimento de APIs Python/FastAPI.
você domina padrões de arquitetura de software e tem muito zelo em relação a clean code e melhores práticas no desenvolvimento de software.
por isso foi solicitado que você desenvolva um conjunto de rules para ser utilizado como padrão na empresa. estas rules serão utilizadas por todos os desenvolvedores.
Neste momento você deve desenvolver as rules para uso da fastapi cobrindo boas práticas com projetos FastAPI.
A estrutura deseja e sugerida da rule é a seguinte:

---
Rule: python-messaging.mdc
Description: Diretrizes para implementação de mensageria e eventos em Python, cobrindo Kafka, RabbitMQ, padrões pub/sub e processamento assíncrono.
globs: ["**/messaging/**/*.py", "**/events/**/*.py", "**/kafka/**/*.py", "**/rabbitmq/**/*.py", "**/publishers/**/*.py", "**/consumers/**/*.py"]
alwaysApply: false
tags: ["fastapi", "api", "messaging", "rabbitmq","kafka"]
Author: Glauber Duma
Created: 2026-01
---

# Python - Mensageria e Eventos

## 1. Objetivo

Estabelecer padrões para implementação robusta de mensageria e processamento de eventos, garantindo:
- **Comunicação Assíncrona** confiável entre serviços
- **Processamento de Eventos** escalável e resiliente
- **Padrões Pub/Sub** bem estruturados
- **Tratamento de Falhas** com retry e dead letter queues
- **Monitoramento** e observabilidade de mensagens

---



## 2. Arquitetura de Mensageria

### 🗂️ **Estrutura de organização**


```
fastapi_project
|---src/
|.   |---messaging/
|.   |     |---__init__.py
|.   |     |---interface.py                              # Interface abstrata
|.   |     |---brokers/                                  # Implementação de brokers
|.   |     |    |---__init__.py
|.   |     |    |---memory.py                            # In-memory broker (testes)
|.   |     |.   |---kafka/ 
|.   |     |.   |    |---producer.py                     # kafka producer
|.   |     |    |.   |---consumery                       # kafka consumer
|.   |     |    |.   |---admin.py                        # kafka admin
|.   |     |.   |---rabbitmq/ 
|.   |     |.   |    |---producer.py                     # rabbitmq producer
|.   |     |    |.   |---consumery                       # rabbitmq consumer
|.   |     |    |.   |---admin.py                        # rabbitmq admin
|.   |     |.   |---azuresb/ 
|.   |     |.   |    |---producer.py                     # azure service bus producer
|.   |     |    |.   |---consumery                       # azure service bus  consumer
|.   |     |    |.   |---admin.py                        # kaazure service bus ka admin
|.   |     |.   |---eventhub/ 
|.   |     |.        |---producer.py                     # azure event hub  producer
|.   |     |     .   |---consumery                       # azure event hub consumer
|.   |     |     .   |---admin.py                        # azure event hub admin
|.   |     |---events/                                   # definição de eventos
|.   |     |    |---__init__.py
|.   |     |    |---base.py                              # Event base class
|.   |     |    |---user_events.py                       # User domain events
|.   |     |    |---order_events.py                      # Order domain events
|.   |     |    |---system_events.py                     # System events
|.   |     |---handlers/                                 # definição de eventos
|.   |     |    |---__init__.py
|.   |     |    |---base.py                              # base handler
|.   |     |    |---user_handler.py                      
|.   |     |    |---notification_handler.py                      
|.   |     |---serialization.py                          # message serialization
|.   |     |---routing.py                                # message routing
|.   |     |---retry.py                                  # retry mechanisms
|.   |     |---monitoring.py                             # metrics and monitoring
|.   |---core/
|.   |     |---mesaging/                                 
|.   |     |    |---__init__.py
|.   |     |    |---config.py                            # messaging configuration
|.   |     |    |---dependencies.py                      # FastAPI dependencies
|.   |     |    |---middleware.py                        # messaging middleware
|.   |     |---events/                                 
|.   |          |---__init__.py
|.   |          |---dispatcher.py                        # event dispatcher
|.   |          |---registry.py                          # handler registry
|.   |---business/
|.   |     |---services/
 .              |---event_service.py                     # business event service

```

### ⚙️ **Messaging Configuration**

```Python
# src/core/messaging/config.py
"""
Configuração de mensageria.
"""

<gerar aqui o código python>

```

## 3. Event System

### 📡 **Base Event class**


```Python
# src/messaging/events/base.py
"""
Classe base para eventos
"""

<gerar aqui o código python>

```


## 4. Message interface

### 🔌 **Messaging interface**


```Python
# src/messaging/interface.py
"""
Interface abstrata para messaging.
"""

<gerar aqui o código python>

```


## 5. Kafka Implementation

### 🔵 **Kafka Producer e Consumer**


```Python
# src/messaging/brokers/kafka/producer.py
"""
Kafka producer implementation
"""

<gerar aqui o código python>

```

```Python
# src/messaging/brokers/kafka/consumer.py
"""
Kafka consumer implementation
"""

<gerar aqui o código python>

```

```Python
# src/messaging/brokers/kafka/admin.py
"""
Kafka admin operations.
"""

<gerar aqui o código python>

```

## 6. Event Handlers 

### 🎯 **Handler Systens**


```Python
# src/messaging/handlers/base.py
"""
Base handler para eventos
"""

<gerar aqui o código python>

```

## 7. Event Dispatcher

### 📩 **Event Publishing and Routing**


```Python
# src/core/events/dispatcher.py
"""
Event dispatcher central.
"""

<gerar aqui o código python>

```



## 8. Boas práticas

### ✅ **DO`s - Práticas Recomendadas**

```Python

✅ Use eventos descritivos e versionados
<gerar aqui o código python>

✅ Inclua contexto suficiente nos eventos
<gerar aqui o código python>

✅ Implemente idenpotência nos handlers
<gerar aqui o código python>

✅ User dead letter queues para falhas
<gerar aqui o código python>

✅ Monitore métricas de mensageria
<gerar aqui o código python>

✅ Implemente circuit breaker para falhas
<gerar aqui o código python>



### ❌ **DONT`s - Práticas a Evitar**

❌ Não inclua dados sensíveis em eventos
<gerar aqui o código python>

❌ Não faça operações síncronas em handlers
<gerar aqui o código python>

❌ Não ignore falhas silenciosamente
<gerar aqui o código python>

❌ Não crie dependencia circulares entre eventos
<gerar aqui o código python>

❌ Não use eventos para operações síncronas críticas
<gerar aqui o código python>

❌ Não esqueça de versionar eventos
<gerar aqui o código python>


```



salve a regra na pasta python/general/python-messaging.mdc
