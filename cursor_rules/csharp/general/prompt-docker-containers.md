# Prompt: Docker Containers

**Objetivo**: Gerar regras e templates para uso de containers Docker em projetos .NET, com foco em segurança, performance e manutenibilidade.

---

## Instruções para o Copilot

Ao gerar código relacionado a Docker e containers, você deve:

### 1. **Referência Obrigatória**
- SEMPRE referencie `@docker-containers` para obter templates e padrões
- NÃO duplique código que já está documentado na regra compartilhada
- Foque em exemplos específicos do projeto atual

### 2. **Segurança (Prioridade Máxima)**
- **Non-root user**: SEMPRE crie containers com usuário não-root (`USER appuser`)
- **Imagens oficiais**: Use APENAS imagens da Microsoft (`mcr.microsoft.com/dotnet`)
- **Alpine Linux**: Prefira variantes alpine para menor superfície de ataque
- **Multi-stage builds**: SEMPRE separe build de runtime
- **Secrets**: NUNCA hardcode secrets no Dockerfile ou docker-compose.yml
- **Security scanning**: Configure Trivy ou Snyk no CI/CD

### 3. **Dockerfile**
Ao gerar um Dockerfile:
```dockerfile
# ✅ Estrutura obrigatória:
FROM mcr.microsoft.com/dotnet/sdk:9.0-alpine AS build
# ... (restore, build, publish)

FROM mcr.microsoft.com/dotnet/aspnet:9.0-alpine AS runtime
RUN addgroup -g 1000 appgroup && adduser -u 1000 -G appgroup -s /bin/sh -D appuser
WORKDIR /app
RUN chown -R appuser:appgroup /app
COPY --from=build --chown=appuser:appgroup /app/publish .
USER appuser  # ← OBRIGATÓRIO
ENTRYPOINT ["dotnet", "App.dll"]
```

Pontos críticos:
- Multi-stage build (sdk → aspnet)
- Imagens alpine
- Non-root user com UID/GID 1000
- `chown` de arquivos para appuser
- `USER appuser` antes do ENTRYPOINT
- Health check para APIs (`HEALTHCHECK --interval=30s CMD wget http://localhost:8080/healthz`)

### 4. **docker-compose.yml**
Ao gerar um docker-compose.yml:
```yaml
# ✅ Estrutura obrigatória:
services:
  api:
    build: ...
    user: "1000:1000"  # ← Non-root
    restart: unless-stopped
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
    env_file:
      - .env.local  # ← Secrets aqui (gitignored)
    healthcheck:
      test: ["CMD", "wget", "--spider", "http://localhost:8080/healthz"]
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 512M
    security_opt:
      - no-new-privileges:true
```

Pontos críticos:
- `user: "1000:1000"` para non-root
- Secrets em `.env.local` (NÃO commitar)
- Health checks configurados
- Resource limits definidos
- `security_opt` para hardening

### 5. **.dockerignore**
SEMPRE crie .dockerignore ao gerar Dockerfile:
```
**/bin/
**/obj/
**/.vs/
**/.git/
**/tests/
**/*.md
**/.env.local
**/*.user
```

### 6. **Security Scanning**
Sempre que gerar CI/CD pipeline, incluir:
```yaml
# Trivy scan
- name: Scan image
  run: trivy image --exit-code 1 --severity CRITICAL,HIGH myapp:latest
```

### 7. **Kubernetes**
Para deployments em K8s:
```yaml
# Deployment
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  runAsGroup: 1000
  allowPrivilegeEscalation: false
  capabilities:
    drop: [ALL]

resources:
  requests:
    cpu: 250m
    memory: 256Mi
  limits:
    cpu: 500m
    memory: 512Mi
```

### 8. **CI/CD Integration**
Incluir no pipeline:
- Build da imagem com BuildKit
- Security scan (Trivy/Snyk)
- Push para registry
- Deploy no Kubernetes (se aplicável)

### 9. **Observabilidade**
Containers devem expor:
- `/healthz` - Liveness probe
- `/readyz` - Readiness probe
- `/metrics` - Prometheus metrics

### 10. **Checklist de Verificação**
Antes de finalizar, confirme:
- [ ] Non-root user configurado
- [ ] Multi-stage build implementado
- [ ] Imagens alpine utilizadas
- [ ] .dockerignore criado
- [ ] Health checks configurados
- [ ] Resource limits definidos
- [ ] Security scanning no CI/CD
- [ ] Secrets em variáveis de ambiente (não hardcoded)

---

## Exemplos de Uso

### Exemplo 1: Gerar Dockerfile para API
**Prompt do usuário**: "Crie um Dockerfile para minha API ASP.NET Core"

**Resposta do Copilot**:
```
Vou criar um Dockerfile seguindo @docker-containers:

[Gerar Dockerfile com]:
- Multi-stage build (sdk:9.0-alpine → aspnet:9.0-alpine)
- Non-root user (appuser UID 1000)
- Health check para /healthz
- Cache otimizado para NuGet packages

[Criar também]:
- .dockerignore para excluir bin/obj/tests
- .env.example para documentar variáveis
```

### Exemplo 2: Gerar docker-compose.yml
**Prompt do usuário**: "Preciso de um docker-compose com API + Postgres + Redis"

**Resposta do Copilot**:
```
Vou criar um docker-compose.yml seguindo @docker-containers:

[Incluir]:
- API com non-root user (1000:1000)
- PostgreSQL com user postgres (não root)
- Redis com user redis (não root)
- Health checks para todos os serviços
- Resource limits
- Volumes persistentes para dados
- Network bridge isolada

[Criar também]:
- .env.example com DB_PASSWORD, REDIS_PASSWORD
- scripts/init-db.sql (se necessário)
```

### Exemplo 3: CI/CD com Security Scan
**Prompt do usuário**: "Configure GitHub Actions para build e scan da imagem Docker"

**Resposta do Copilot**:
```
Vou criar .github/workflows/docker-cicd.yml seguindo @docker-containers:

[Pipeline incluirá]:
- Build com buildx e cache do GitHub
- Trivy security scan (fail em CRITICAL/HIGH)
- Upload de SARIF para GitHub Security
- Push para GHCR
- Deploy no Kubernetes (se configurado)
```

---

## Anti-Patterns a Evitar

❌ **NÃO fazer**:
```dockerfile
# Container rodando como root
FROM mcr.microsoft.com/dotnet/aspnet:9.0
COPY . /app
WORKDIR /app
ENTRYPOINT ["dotnet", "App.dll"]  # ← Roda como root!
```

❌ **NÃO fazer**:
```yaml
# Secrets hardcoded
environment:
  - DB_PASSWORD=minha_senha_secreta  # ← NUNCA!
```

❌ **NÃO fazer**:
```dockerfile
# Imagem sem multi-stage build
FROM mcr.microsoft.com/dotnet/sdk:9.0  # ← Imagem gigante!
COPY . /app
RUN dotnet publish
ENTRYPOINT ["dotnet", "App.dll"]
```

✅ **FAZER**:
```dockerfile
# Multi-stage, alpine, non-root
FROM mcr.microsoft.com/dotnet/sdk:9.0-alpine AS build
# ... build ...
FROM mcr.microsoft.com/dotnet/aspnet:9.0-alpine AS runtime
RUN adduser -u 1000 appuser
USER appuser  # ← Non-root!
```

---

## Tipos de Projeto

### API REST / gRPC
- Dockerfile com health check HTTP/gRPC
- Expor porta 8080 (não 80/443)
- Prometheus metrics em /metrics

### Console App / Worker
- Dockerfile sem EXPOSE (não tem porta)
- Graceful shutdown com CancellationToken
- Logs estruturados para stdout

### Microservices
- docker-compose com múltiplos serviços
- Networks isoladas
- Service discovery via DNS interno
- OpenTelemetry collector

### Background Jobs
- Dockerfile baseado em runtime (não aspnet)
- Cronjobs via Kubernetes CronJob
- Dead letter queue para falhas

---

## Integração com Outras Regras

- **@csharp-style-guide**: Código do projeto segue padrões C# 12
- **@unit-testing**: Tests em stage separado no Dockerfile
- **@efcore-repository**: Connection strings via environment variables
- **@use-cases**: Health checks chamam use cases para readiness

---

## Observações Finais

1. **Priorize segurança**: Non-root user é OBRIGATÓRIO
2. **Escaneie vulnerabilidades**: Trivy/Snyk no CI/CD
3. **Otimize para cache**: COPY de projetos antes do código
4. **Use alpine**: Imagens menores e mais seguras
5. **Documente**: README.md com instruções de build/run
6. **Automatize**: Scripts para build/run/clean
7. **Monitore**: OpenTelemetry + Prometheus + Grafana

---

**Versão do prompt**: 1.0.0  
**Data**: 2024-01-15
