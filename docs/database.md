# 🗄️ Modelagem de Banco de Dados — SaaS RH Multi-Tenant

## 1. Objetivo

Este documento descreve a **estrutura do banco de dados** do sistema SaaS de RH Multi-Tenant, definindo entidades, relacionamentos, regras de integridade e a estratégia de isolamento de dados entre tenants.

O foco é garantir:

* Isolamento total entre clientes
* Consistência dos dados
* Facilidade de evolução
* Segurança por design

---

## 2. Estratégia de Multi-Tenancy

### 2.1 Modelo adotado

O sistema utiliza **multi-tenancy por isolamento lógico**, onde:

* Todas as tabelas principais possuem a coluna `tenant_id`
* O backend **nunca confia** em `tenant_id` enviado pelo frontend
* O `tenant_id` é sempre resolvido a partir do token JWT

```
Tenant
 ├── Usuários
 ├── Empresas
 │    └── Funcionários
```

---

## 3. Entidades Principais

### 3.1 Tenants

Representa o cliente do SaaS.

```sql
tenants
- id (PK)
- name
- status
- created_at
- updated_at
```

Regras:

* Um tenant representa um cliente do sistema
* Nenhum dado pode existir sem vínculo com um tenant

---

### 3.2 Usuários

Usuários do sistema (root, admins e funcionários).

```sql
users
- id (PK)
- tenant_id (FK -> tenants.id)
- name
- email (unique)
- password_hash
- role (ROOT | TENANT_ADMIN | USER)
- status
- created_at
- updated_at
```

Regras:

* Usuários ROOT podem não estar associados a um tenant específico
* Usuários comuns sempre pertencem a um tenant

---

### 3.3 Empresas

Empresas cadastradas dentro de um tenant.

```sql
companies
- id (PK)
- tenant_id (FK -> tenants.id)
- name
- cnpj
- status
- created_at
- updated_at
```

Regras:

* Um tenant pode ter uma ou mais empresas
* Empresas nunca compartilham dados entre tenants

---

### 3.4 Funcionários

Funcionários vinculados a uma empresa.

```sql
employees
- id (PK)
- tenant_id (FK -> tenants.id)
- company_id (FK -> companies.id)
- name
- cpf
- email
- position
- status
- hired_at
- created_at
- updated_at
```

Regras:

* Funcionários pertencem a uma empresa
* Sempre devem respeitar o `tenant_id`

---

## 4. Relacionamentos

```
tenants
 ├── users
 ├── companies
 │    └── employees
```

Chaves importantes:

* `users.tenant_id → tenants.id`
* `companies.tenant_id → tenants.id`
* `employees.company_id → companies.id`
* `employees.tenant_id → tenants.id`

---

## 5. Regras de Segurança no Banco

### 5.1 Isolamento de Dados

Toda query deve:

```sql
WHERE tenant_id = :tenant_id
```

Nunca:

* Receber `tenant_id` do frontend
* Executar queries sem filtro de tenant

---

### 5.2 Índices Recomendados

```sql
INDEX (tenant_id)
INDEX (tenant_id, company_id)
INDEX (email)
```

---

## 6. Auditoria (Opcional)

Tabela para rastrear ações administrativas:

```sql
audit_logs
- id (PK)
- tenant_id
- user_id
- action
- entity
- entity_id
- created_at
```

---

## 7. Boas Práticas

* Nunca usar `SELECT *`
* Sempre validar permissões no backend
* Transações para operações críticas
* Soft delete para dados sensíveis

---

## 8. Evoluções Futuras

* Separação por schemas
* Banco dedicado por tenant
* Replicação e read replicas
* Data warehouse para relatórios

---

## 9. Referências

* Arquitetura geral: `docs/architecture.md`
* Planejamento técnico: `docs/roadmap.md`
