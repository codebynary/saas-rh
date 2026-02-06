# 🌐 API — SaaS RH

## 1. Objetivo

Documentar o contrato da API REST do sistema SaaS RH, garantindo padronização, segurança e fácil consumo pelo frontend.

---

## 2. Padrões Gerais

* RESTful
* JSON
* Versionamento por URL

Base URL:

```
/api/v1
```

---

## 3. Autenticação

Todas as rotas (exceto login):

```
Authorization: Bearer <token>
```

---

## 4. Rotas Principais

### Auth

```
POST   /auth/login
POST   /auth/logout
```

### Tenants (ROOT)

```
POST   /tenants
GET    /tenants
```

### Empresas

```
POST   /companies
GET    /companies
PUT    /companies/:id
```

### Funcionários

```
POST   /employees
GET    /employees
PUT    /employees/:id
```

---

## 5. Respostas Padrão

### Sucesso

```json
{
  "success": true,
  "data": {}
}
```

### Erro

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "Acesso negado"
  }
}
```

---

## 6. Regras Críticas

* Toda query usa `tenant_id` do token
* Nunca aceitar `tenant_id` do body
* Logs para ações sensíveis

---

## 7. Versionamento

* `/v1` inicial
* Mudanças breaking → `/v2`

---

## 8. Referências

* Autenticação: `docs/auth.md`
* Banco: `docs/database.md`
