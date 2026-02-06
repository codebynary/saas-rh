# 🏗️ Infraestrutura & Deploy — SaaS RH

## 1. Objetivo

Definir a infraestrutura do sistema SaaS RH, incluindo ambientes, containerização, variáveis de ambiente e diretrizes para deploy seguro e escalável.

---

## 2. Ambientes

O sistema deve suportar múltiplos ambientes isolados:

* **Development (dev)**
* **Staging (homologação)**
* **Production (prod)**

Cada ambiente possui:

* Banco de dados próprio
* Variáveis de ambiente independentes
* Tokens e segredos distintos

---

## 3. Containerização (Docker)

### 3.1 Serviços principais

```yaml
services:
  api:
    build: ./backend
    env_file: .env
    depends_on:
      - db

  frontend:
    build: ./frontend

  db:
    image: postgres:16
    volumes:
      - pgdata:/var/lib/postgresql/data
```

---

### 3.2 Benefícios

* Ambiente reproduzível
* Facilita deploy
* Isolamento de dependências

---

## 4. Variáveis de Ambiente

Exemplo:

```env
APP_ENV=production
APP_PORT=3000
JWT_SECRET=super_secret
JWT_EXPIRES_IN=1h
DB_HOST=db
DB_PORT=5432
DB_NAME=saas_rh
DB_USER=postgres
DB_PASSWORD=postgres
```

Regras:

* Nunca versionar `.env`
* Usar `.env.example`

---

## 5. Banco de Dados

* PostgreSQL
* Um banco por ambiente
* Migrations versionadas
* Backup automático

---

## 6. Segurança

* HTTPS obrigatório
* Secrets fora do código
* Rate limit no login
* Logs de auditoria

---

## 7. Deploy

Estratégia inicial:

* VPS / servidor próprio
* Docker Compose

Futuro:

* CI/CD
* Kubernetes
* Banco gerenciado

---

## 8. Observabilidade

* Logs estruturados
* Monitoramento de erros
* Métricas básicas

---

## 9. Referências

* Arquitetura: `docs/architecture.md`
* API: `docs/api.md`
* Banco: `docs/database.md`
