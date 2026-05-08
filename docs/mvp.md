# MVP — Controle Financeiro Pessoal

Documento de escopo do produto mínimo viável (MVP). Serve como referência durante o desenvolvimento para manter o foco no que é essencial e evitar scope creep.

---

## Objetivo

Construir uma aplicação web onde múltiplos usuários possam registrar receitas e despesas, visualizar o saldo por conta e ter um resumo financeiro do mês — de forma simples, segura e extensível.

---

## Usuários

- Pessoas físicas que querem organizar as próprias finanças
- Cada usuário tem seus dados completamente isolados dos demais (multi-tenancy)

---

## Funcionalidades do MVP

### 1. Autenticação
- Cadastro com nome, e-mail e senha
- Login e logout
- Proteção de todas as rotas autenticadas

> ⚠️ Não inclui no MVP: recuperação de senha, OAuth (Google/GitHub), 2FA.

---

### 2. Contas
- Criar conta (ex: Nubank, Carteira, Poupança)
- Listar contas do usuário
- Editar nome da conta
- Excluir conta (somente se não houver transações vinculadas)
- **Saldo calculado dinamicamente** — nunca salvo diretamente no banco

> ⚠️ Não inclui no MVP: transferência entre contas, múltiplas moedas.

---

### 3. Transações
- Registrar transação com:
  - Tipo: `receita` ou `despesa`
  - Valor (usando `Decimal` para precisão monetária)
  - Data
  - Categoria
  - Conta vinculada
  - Descrição (opcional)
- Listar transações com filtro por período e conta
- Editar transação
- Excluir transação

> ⚠️ Não inclui no MVP: transações recorrentes, anexos, importação de extrato, transferências.

---

### 4. Categorias
- Categorias padrão pré-cadastradas no sistema (ex: Alimentação, Transporte, Saúde, Lazer, Salário)
- Usuário utiliza as categorias padrão para registrar transações

> ⚠️ Não inclui no MVP: categorias customizadas por usuário, subcategorias.

---

### 5. Dashboard
- Saldo total (soma de todas as contas)
- Saldo por conta
- Total de receitas do mês atual
- Total de despesas do mês atual
- Resultado do mês (receitas − despesas)

> ⚠️ Não inclui no MVP: gráficos avançados, comparativo entre meses, projeções.

---

## Fora do Escopo (backlog futuro)

Estas funcionalidades foram conscientemente deixadas de fora do MVP, mas a arquitetura deve reservar espaço para elas:

| Funcionalidade | Motivo de adiar |
|---|---|
| Transferência entre contas | Aumenta complexidade do modelo de transações |
| Metas financeiras | Requer lógica de projeção |
| Categorias customizadas | Não é bloqueante para o MVP |
| Transações recorrentes | Complexidade de agendamento |
| Importação de extrato (OFX/CSV) | Parsing e deduplicação são trabalhosos |
| Relatórios avançados e gráficos | Pode ser adicionado depois do core funcionar |
| Recuperação de senha | Requer integração com serviço de e-mail |
| App mobile | Fase posterior |

---

## Stack Técnica

| Camada | Tecnologia |
|---|---|
| Back-end | Python + FastAPI |
| Banco de dados | PostgreSQL |
| ORM | SQLAlchemy |
| Migrações | Alembic |
| Autenticação | JWT (via `python-jose`) |
| Front-end | A definir (React, Vue ou Jinja2 templates) |
| Deploy | A definir |

---

## Arquitetura de Pastas (sugerida)

```
/
├── app/
│   ├── api/
│   │   └── v1/                  # Versionamento desde o início
│   │       ├── auth.py
│   │       ├── accounts.py
│   │       ├── transactions.py
│   │       └── dashboard.py
│   ├── core/
│   │   ├── config.py
│   │   └── security.py
│   ├── models/                  # Modelos do banco (SQLAlchemy)
│   ├── schemas/                 # Validação de entrada/saída (Pydantic)
│   ├── services/                # Regras de negócio (separadas dos endpoints)
│   └── db/
│       └── session.py
├── alembic/                     # Migrações
├── tests/
├── .env.example
├── requirements.txt
└── README.md
```

---

## Modelo de Dados (simplificado)

```
users
  id, name, email, hashed_password, created_at

accounts
  id, user_id (FK), name, created_at
  [saldo = calculado via query nas transactions]

categories
  id, name, type (income | expense)
  [padrão do sistema, sem user_id no MVP]

transactions
  id, user_id (FK), account_id (FK), category_id (FK)
  type (income | expense), amount (Numeric), date, description
  transfer_id (nullable — reservado para transferências futuras)
  created_at, updated_at
```

> **Regra importante:** o campo `transfer_id` já existe na tabela, mas fica `NULL` no MVP. Quando transferências forem implementadas, as duas transações vinculadas terão o mesmo `transfer_id`.

---

## Decisões Técnicas Relevantes

**Saldo nunca é salvo no banco**
O saldo de uma conta é sempre calculado com `SUM` das transações. Isso evita inconsistências e simplifica a lógica de exclusão/edição de transações.

**Versionamento de API desde o início**
Todos os endpoints ficam sob `/api/v1/`. Quando houver breaking changes, cria-se `/api/v2/` sem remover a anterior.

**Regras de negócio na camada de serviços**
Nenhuma regra de negócio fica dentro dos endpoints. Os endpoints apenas recebem, validam e delegam para `services/`.

**Decimal para valores monetários**
Nunca usar `float` para dinheiro. Usar `Numeric(precision=12, scale=2)` no banco e `Decimal` no Python.

---

## Critérios de Conclusão do MVP

O MVP está pronto quando:

- [ ] Usuário consegue se cadastrar e fazer login
- [ ] Usuário consegue criar e listar suas contas
- [ ] Usuário consegue registrar uma receita ou despesa
- [ ] Usuário consegue editar ou excluir uma transação
- [ ] Dashboard exibe saldo e resumo do mês corretamente
- [ ] Dados de um usuário são inacessíveis por outro
- [ ] API está documentada (FastAPI gera automaticamente via `/docs`)
- [ ] Há ao menos testes unitários nos serviços críticos

---

## Próximos Passos

1. Revisar e validar o modelo de dados
2. Configurar o projeto (estrutura de pastas, dependências, `.env`)
3. Implementar autenticação
4. Implementar CRUD de contas
5. Implementar CRUD de transações
6. Implementar dashboard
7. Testes e ajustes
