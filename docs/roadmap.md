
# 🧱 EPIC 0 — Fundação do Projeto

## Issue 0.1 — Definir stack e estrutura base do projeto

### Descrição
Definir a stack principal do sistema SaaS RH e criar a estrutura inicial do projeto, garantindo suporte nativo a arquitetura multi-tenant desde o primeiro commit.

### Tarefas
- [ ] Definir linguagem e framework backend
- [ ] Definir ORM / camada de persistência
- [ ] Definir banco de dados
- [ ] Definir framework frontend
- [ ] Definir padrão de pastas
- [ ] Criar repositório Git
- [ ] Configurar lint, formatter e padrão de commits
- [ ] Criar README com decisões técnicas

### Critérios de Aceite
- Projeto sobe localmente sem ajustes manuais
- Estrutura preparada para multi-tenancy
- README documentado

---

## Issue 0.2 — Configurar ambientes (dev / staging / prod)

### Descrição
Configurar ambientes separados com variáveis próprias, feature flags e isolamento de configuração.

### Tarefas
- [ ] Criar `.env.example`
- [ ] Configurar variáveis por ambiente
- [ ] Criar scripts de bootstrap
- [ ] Configurar logs por ambiente

### Critérios de Aceite
- Aplicação roda em dev imediatamente
- Nenhuma credencial sensível no código

---

# 🧩 EPIC 1 — Core Multi-Tenant

## Issue 1.1 — Implementar entidade Tenant

### Descrição
Criar a entidade Tenant, que representa um cliente do SaaS e é a raiz de isolamento de dados.

### Tarefas
- [ ] Criar tabela `tenants`
- [ ] Criar model/entity
- [ ] Criar repositório tenant-aware
- [ ] Criar migrations
- [ ] Criar seed inicial

### Critérios de Aceite
- Tenant criado e persistido
- Tenant pode ser ativado/desativado

---

## Issue 1.2 — Middleware de contexto do Tenant

### Descrição
Implementar middleware responsável por resolver e injetar o `tenant_id` no contexto da requisição.

### Tarefas
- [ ] Extrair `tenant_id` do JWT
- [ ] Injetar `tenant_id` no request context
- [ ] Bloquear requests sem tenant válido
- [ ] Permitir bypass para Root

### Critérios de Aceite
- Todas as requests possuem tenant resolvido
- Cross-tenant access bloqueado

---

## Issue 1.3 — Repositório tenant-aware

### Descrição
Criar um padrão de repositório que automaticamente aplique filtro por `tenant_id`.

### Tarefas
- [ ] Criar base repository
- [ ] Forçar `tenant_id` em todas as queries
- [ ] Impedir consultas sem filtro de tenant
- [ ] Criar testes de isolamento

### Critérios de Aceite
- Nenhuma query retorna dados de outro tenant

---

# 🔐 EPIC 2 — Autenticação e Autorização

## Issue 2.1 — Autenticação JWT

### Descrição
Implementar autenticação baseada em JWT com contexto de tenant.

### Tarefas
- [ ] Criar endpoint de login
- [ ] Gerar JWT com `user_id`, `tenant_id`, `role`
- [ ] Criar middleware de validação de token
- [ ] Implementar refresh token (opcional)

### Critérios de Aceite
- Login funcional
- JWT válido injeta contexto correto

---

## Issue 2.2 — Controle de papéis (RBAC)

### Descrição
Implementar controle de acesso baseado em papéis.

### Tarefas
- [ ] Definir roles (ROOT, ADMIN, USER)
- [ ] Criar guards/middlewares
- [ ] Restringir rotas por role

### Critérios de Aceite
- Rotas protegidas corretamente
- Root acessa apenas rotas globais

---

# 🏢 EPIC 3 — Gestão de Clientes (Tenants)

## Issue 3.1 — CRUD de Tenants (Root)

### Descrição
Permitir que o administrador do sistema gerencie clientes do SaaS.

### Tarefas
- [ ] Criar endpoints CRUD de tenant
- [ ] Proteger rotas (Root only)
- [ ] Criar validações
- [ ] Criar logs de auditoria

### Critérios de Aceite
- Apenas Root acessa
- CRUD funcional

---

# 👤 EPIC 4 — Usuários do Cliente

## Issue 4.1 — CRUD de Usuários Administradores

### Descrição
Permitir que tenants criem e gerenciem seus usuários administrativos.

### Tarefas
- [ ] Criar tabela `users`
- [ ] Hash de senha
- [ ] Criar endpoints CRUD
- [ ] Validar permissões

### Critérios de Aceite
- Usuário pertence a um único tenant
- Isolamento garantido

---

# 🏭 EPIC 5 — Empresas

## Issue 5.1 — CRUD de Empresas

### Descrição
Permitir que tenants cadastrem empresas.

### Tarefas
- [ ] Criar tabela `companies`
- [ ] Relacionar com tenant
- [ ] Criar endpoints CRUD
- [ ] Validar documentos

### Critérios de Aceite
- Empresa sempre vinculada ao tenant

---

# 👷 EPIC 6 — Funcionários

## Issue 6.1 — CRUD de Funcionários

### Descrição
Permitir que empresas cadastrem funcionários.

### Tarefas
- [ ] Criar tabela `employees`
- [ ] Relacionar com company
- [ ] Criar endpoints CRUD
- [ ] Validar dados básicos

### Critérios de Aceite
- Funcionário pertence a uma empresa válida

---

# 🖥️ EPIC 7 — Frontend

## Issue 7.1 — Autenticação no Frontend

### Descrição
Implementar autenticação e gerenciamento de sessão no frontend.

### Tarefas
- [ ] Tela de login
- [ ] Armazenar JWT com segurança
- [ ] Interceptor HTTP
- [ ] Logout

### Critérios de Aceite
- Sessão persistente
- Logout limpa contexto

---

## Issue 7.2 — Estrutura base de telas

### Descrição
Criar estrutura base de navegação do sistema.

### Tarefas
- [ ] Dashboard
- [ ] Usuários
- [ ] Empresas
- [ ] Funcionários
- [ ] Relatórios (placeholder)

### Critérios de Aceite
- Navegação funcional
- Permissões respeitadas

---

# 🧪 EPIC 8 — Testes e Segurança

## Issue 8.1 — Testes de isolamento multi-tenant

### Descrição
Garantir que não exista vazamento de dados entre tenants.

### Tarefas
- [ ] Testes unitários
- [ ] Testes de integração
- [ ] Testes cross-tenant

### Critérios de Aceite
- 100% das tentativas cross-tenant falham

---

## Issue 8.2 — Logs e auditoria

### Descrição
Implementar logs estruturados com contexto de tenant.

### Tarefas
- [ ] Logar `tenant_id`
- [ ] Logar `user_id`
- [ ] Logar ações críticas

### Critérios de Aceite
- Logs rastreáveis por tenant

---

# 🎯 EPIC 9 — Preparação para Escala

## Issue 9.1 — Feature flags e versionamento de API

### Descrição
Preparar o sistema para evolução sem quebra de compatibilidade.

### Tarefas
- [ ] Implementar feature flags
- [ ] Versionar API
- [ ] Documentar breaking changes

### Critérios de Aceite
- Nova versão não quebra frontend existente
