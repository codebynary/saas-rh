# 🏗️ Arquitetura do Sistema — SaaS RH Multi‑Tenant

## 1. Visão Geral

Este documento descreve a arquitetura técnica do sistema **SaaS de RH Multi‑Tenant**, projetado para permitir que múltiplas empresas utilizem a mesma aplicação com **isolamento total de dados**, **controle de acesso por papéis** e **escalabilidade horizontal**.

O sistema segue princípios de **arquitetura modular**, **separação de responsabilidades** e **segurança por design**.

---

## 2. Conceito de Multi‑Tenancy

### 2.1 Modelo adotado

O sistema utiliza **multi‑tenancy por tenant lógico**, onde:

* Um único backend e frontend atendem todos os clientes
* Cada empresa (tenant) possui seus próprios dados
* O isolamento é garantido por `tenant_id`

```
Tenant (Empresa Cliente)
 ├── Usuários Administradores
 ├── Empresa(s)
 │    └── Funcionários
```

### 2.2 Tipos de usuários

| Papel        | Descrição                                |
| ------------ | ---------------------------------------- |
| Root Admin   | Administra o SaaS (plataforma inteira)   |
| Tenant Admin | Administra sua empresa dentro do sistema |
| Usuário      | Funcionário comum                        |

---

## 3. Arquitetura Geral

```
[ Frontend (Web App) ]
          │
          ▼
[ API Gateway / Backend ]
          │
          ▼
[ Banco de Dados ]
```

### 3.1 Frontend

Responsável por:

* Autenticação
* Controle de sessão
* Navegação baseada em permissões
* Interface de gestão de RH

Características:

* SPA (Single Page Application)
* Comunicação via API REST
* Controle de rotas por papel do usuário

---

### 3.2 Backend

Responsável por:

* Autenticação e autorização
* Isolamento multi‑tenant
* Regras de negócio
* Auditoria e logs

Componentes:

* Auth Service
* Tenant Resolver
* RBAC (Role‑Based Access Control)
* API REST

---

### 3.3 Banco de Dados

Modelo relacional com isolamento lógico por tenant.

Entidades principais:

```
users
 ├── id
 ├── tenant_id
 ├── role

companies
 ├── id
 ├── tenant_id

employees
 ├── id
 ├── company_id
 ├── tenant_id
```

Regras importantes:

* Toda tabela sensível possui `tenant_id`
* Queries sempre filtram pelo `tenant_id`

---

## 4. Fluxo de Autenticação

1. Usuário acessa a aplicação
2. Envia credenciais
3. Backend valida login
4. JWT é gerado contendo:

   * user_id
   * tenant_id
   * role
5. Frontend armazena token
6. Requests subsequentes usam o token

---

## 5. Controle de Acesso (RBAC)

Exemplo de permissões:

| Ação                      | Root | Tenant Admin | Usuário |
| ------------------------- | ---- | ------------ | ------- |
| Criar tenant              | ✅    | ❌            | ❌       |
| Criar empresa             | ❌    | ✅            | ❌       |
| Criar funcionário         | ❌    | ✅            | ❌       |
| Visualizar dados próprios | ❌    | ❌            | ✅       |

---

## 6. Isolamento e Segurança

Medidas adotadas:

* JWT com `tenant_id`
* Middleware obrigatório de tenant
* Validação de permissões
* Logs de ações administrativas

---

## 7. Escalabilidade

* Backend stateless
* Suporte a múltiplas instâncias
* Banco preparado para crescimento

---

## 8. Evoluções Futuras

* Microserviços
* Filas (async jobs)
* Auditoria avançada
* Integração com sistemas externos

---

## 9. Referências

* Roadmap técnico: `docs/roadmap.md`
* Issues do GitHub
