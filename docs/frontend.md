# 🖥️ Frontend — SaaS RH

## 1. Objetivo

Definir a estrutura lógica do frontend, fluxos de navegação e controle de acesso baseado em perfil.

---

## 2. Stack Sugerida

* React / Vue / Angular
* SPA
* Consumo via API REST

---

## 3. Fluxo de Acesso

```
Login
 ↓
Dashboard
 ↓
Empresas → Funcionários
```

---

## 4. Telas Principais

### Login

* Email
* Senha

### Dashboard

* Visão geral
* Indicadores

### Empresas

* Listagem
* Cadastro
* Edição

### Funcionários

* Listagem
* Cadastro
* Edição

---

## 5. Controle de Permissões

* Menus renderizados por role
* Rotas protegidas

Exemplo:

```
if (role !== 'TENANT_ADMIN') hideMenu()
```

---

## 6. Gerenciamento de Estado

* Auth state
* User context
* Tenant context

---

## 7. Boas Práticas

* Nunca armazenar tenant_id manualmente
* Token apenas em memória ou storage seguro
* Tratamento global de erros

---

## 8. Referências

* API: `docs/api.md`
* Auth: `docs/auth.md`
