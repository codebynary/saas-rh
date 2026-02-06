# SaaS RH — Sistema de Gestão de Recursos Humanos (Multi-Tenant)

Este repositório contém a base arquitetural de um **sistema SaaS de RH multi-tenant**, projetado para atender múltiplas empresas (clientes) com **isolamento total de dados**, segurança e escalabilidade desde o primeiro commit.

---

## 🎯 Objetivo do Projeto

Criar uma plataforma SaaS onde:

- Um **Administrador do Sistema (Root)** gerencia os clientes do SaaS
- Cada **Cliente (Tenant)** representa uma empresa contratante
- Cada cliente pode criar:
  - Usuários administradores
  - Empresas
  - Funcionários
- Nenhum cliente tem acesso aos dados de outro cliente

O foco do projeto é **arquitetura limpa**, **segurança** e **facilidade de evolução**.

---

## 🧱 Conceito de Multi-Tenancy

O sistema é **multi-tenant por isolamento lógico**, onde:

- Todo dado persistente pertence a um `tenant_id`
- O `tenant_id` é resolvido via autenticação (JWT)
- O backend **nunca confia** em `tenant_id` vindo do frontend
- Vazamento de dados entre tenants é tratado como erro crítico

Hierarquia funcional:

## 📚 Documentação

A documentação do sistema está organizada na pasta `/docs` e cobre todos os pilares da arquitetura do SaaS.

- 📐 **Arquitetura Geral**  
  `docs/architecture.md`  
  Visão macro do sistema, camadas, responsabilidades e separação backend/frontend.

- 🗄️ **Banco de Dados (Multi-Tenant)**  
  `docs/database.md`  
  Modelagem das entidades, relacionamentos e regras de isolamento por tenant.

- 🔐 **Autenticação & Autorização**  
  `docs/auth.md`  
  Estratégia de login, JWT, roles, middlewares e resolução segura de tenant.

- 🌐 **API (Contrato Backend)**  
  `docs/api.md`  
  Rotas, padrões REST, versionamento, respostas e regras críticas de segurança.

- 🖥️ **Frontend (Fluxos e Permissões)**  
  `docs/frontend.md`  
  Telas, fluxos de navegação, controle de acesso e boas práticas no cliente.

- 🗺️ **Roadmap Técnico**  
  `docs/roadmap.md`  
  Planejamento técnico, épicos e tarefas que serão convertidas em Issues do GitHub.


