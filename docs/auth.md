# 🔐 Autenticação & Autorização — SaaS RH

## 1. Objetivo

Definir o modelo de autenticação, autorização e resolução de tenant do sistema SaaS RH, garantindo **segurança, isolamento de dados e controle de acesso por perfil**.

---

## 2. Estratégia de Autenticação

### 2.1 Modelo adotado

* Autenticação baseada em **JWT (JSON Web Token)**
* Tokens assinados no backend
* Stateless (sem sessão no servidor)

Fluxo:

```
Login → Geração JWT → Frontend armazena token → Requests autenticados
```

---

## 3. Estrutura do Token JWT

Payload mínimo:

```json
{
  "sub": "user_id",
  "tenant_id": "tenant_id",
  "role": "ROOT | TENANT_ADMIN | USER",
  "exp": 0000000000
}
```

Regras críticas:

* O `tenant_id` **nunca vem do frontend**
* O backend confia **exclusivamente** no JWT

---

## 4. Perfis de Acesso (Roles)

### 4.1 ROOT

* Administrador do SaaS
* Gerencia tenants
* Não acessa dados operacionais

### 4.2 TENANT_ADMIN

* Admin do cliente
* Gerencia empresas e usuários

### 4.3 USER

* Funcionário
* Acesso limitado

---

## 5. Middleware de Autenticação

Responsabilidades:

* Validar JWT
* Extrair `user_id`, `tenant_id` e `role`
* Injetar contexto na request

Exemplo lógico:

```ts
req.context = {
  userId,
  tenantId,
  role
}
```

---

## 6. Middleware de Autorização

Valida permissões por rota:

* Role
* Tenant

Exemplo:

```ts
requireRole(['TENANT_ADMIN'])
```

---

## 7. Regras de Segurança

* Tokens com expiração curta
* Refresh token (futuro)
* Logout invalida token
* Tentativas de acesso inválidas geram log

---

## 8. Referências

* Banco de dados: `docs/database.md`
* Arquitetura: `docs/architecture.md`
