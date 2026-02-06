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

- [Arquitetura do Sistema](docs/architecture.md)
- [Roadmap Técnico](docs/roadmap.md)


