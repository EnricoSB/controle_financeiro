# 🎯 MVP — Controle Financeiro Pessoal

---

## 📌 Visão Geral

Este MVP tem como objetivo criar uma aplicação web simples para **controle financeiro pessoal**, permitindo o registro de **entradas e saídas** e a visualização do **saldo atual por banco**.

O foco está em **funcionalidade, clareza e aprendizado técnico**, não em estética ou completude.

---

## 🧠 Objetivo do MVP

Ao final do MVP, o usuário deve conseguir:

- Registrar movimentações financeiras
- Consultar o histórico de movimentações
- Visualizar o saldo atual por banco

---

## ✅ Funcionalidades Incluídas

### 💸 Cadastro de Movimentações

O sistema deve permitir o cadastro de movimentações com os seguintes campos:

- **Valor** (obrigatório, numérico e positivo)
- **Tipo**: `Entrada` ou `Saída`
- **Banco** (selecionável)
- **Data da movimentação**
- **Observação** (opcional)

**Regras:**

- O valor deve ser sempre positivo
- O tipo define se o valor será somado ou subtraído do saldo

---

### 📄 Listagem de Movimentações

O sistema deve exibir uma lista contendo todas as movimentações cadastradas, apresentando:

- Data
- Tipo
- Valor
- Banco
- Observação

A listagem deve ser ordenada por **data decrescente** (mais recente primeiro).

---

### 🏦 Cadastro de Bancos

O sistema deve permitir o cadastro manual de bancos (ex: Nubank, Itaú, Caixa), que serão utilizados no cadastro das movimentações.

---

### 📊 Visualização de Saldos

O sistema deve exibir:

- O **saldo atual por banco**
- O **saldo total geral**

**Regra de cálculo:**

- Saldo = Soma das Entradas − Soma das Saídas


---

## ❌ Fora do Escopo do MVP

As funcionalidades abaixo **não fazem parte** deste MVP e devem ser tratadas como evoluções futuras:

- Cartão de crédito
- Parcelamento
- Categorias de gastos
- Autenticação / login
- Multiusuário
- Dashboard com gráficos
- Exportação de dados
- Edição ou exclusão de movimentações

---

## 🧪 Critérios de Sucesso

O MVP será considerado concluído quando:

- Uma movimentação puder ser cadastrada com sucesso
- A movimentação cadastrada aparecer na listagem
- O saldo por banco for calculado corretamente
- O saldo total refletir corretamente entradas e saídas
- A aplicação funcionar localmente via Docker

---

## ⚙️ Restrições Técnicas

- **Backend**: FastAPI (Python)
- **Banco de Dados**: PostgreSQL
- **Frontend**: HTML + JavaScript
- **Execução**: Ambiente local com Docker
- **Autenticação**: Não implementada

---

## 📝 Observações Finais

Este MVP serve como base inicial para aprendizado e evolução futura do projeto, permitindo a adição de novas funcionalidades sem grandes refatorações.

---
