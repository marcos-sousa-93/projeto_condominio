# 📋 Tabelas do Banco de Dados — Condomínio

## 🏢 Tabela: `unidades`

| Campo | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único |
| `bloco` | TEXT | NOT NULL | Bloco da unidade |
| `numero` | TEXT | NOT NULL | Número da unidade |
| `proprietario` | TEXT | — | Nome do proprietário |
| `telefone` | TEXT | — | Telefone de contato |
| `email` | TEXT | — | E-mail de contato |

> 🔒 **Constraint:** `UNIQUE(bloco, numero)` — não permite duplicar bloco + número.

## 🏢 `unidades`

| id | bloco | numero | proprietario | telefone | email |
|---|---|---|---|---|---|
| 1 | A | 101 | João Silva | (11) 99999-1111 | joao@email.com |
| 2 | A | 102 | Maria Souza | (11) 99999-2222 | maria@email.com |
| 3 | B | 201 | Carlos Lima | (11) 99999-3333 | carlos@email.com |
| 4 | B | 202 | Fernanda Alves | (11) 99999-4444 | fernanda@email.com |
<hr>

## 👥 Tabela: `moradores`

| Campo | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único |
| `nome` | TEXT | NOT NULL | Nome do morador |
| `unidade_id` | INTEGER | FOREIGN KEY | Referência à unidade |
| `tipo` | TEXT | — | Proprietário / Inquilino / etc. |
| `telefone` | TEXT | — | Telefone de contato |
| `email` | TEXT | — | E-mail de contato |

> 🔗 **FK:** `unidade_id → unidades(id)` com `ON DELETE SET NULL`

## 👥 `moradores`

| id | nome | unidade_id | tipo | telefone | email |
|---|---|---|---|---|---|
| 1 | João Silva | 1 | Proprietário | (11) 99999-1111 | joao@email.com |
| 2 | Ana Silva | 1 | Inquilino | (11) 98888-1111 | ana@email.com |
| 3 | Maria Souza | 2 | Proprietária | (11) 99999-2222 | maria@email.com |
| 4 | Carlos Lima | 3 | Proprietário | (11) 99999-3333 | carlos@email.com |
| 5 | Pedro Lima | 3 | Dependente | (11) 98888-3333 | — |
<hr>

## 💰 Tabela: `despesas`

| Campo | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único |
| `descricao` | TEXT | NOT NULL | Descrição da despesa |
| `valor` | REAL | NOT NULL | Valor em reais |
| `mes` | TEXT | NOT NULL | Mês de referência (ex.: 2025-01) |
| `categoria` | TEXT | — | Categoria (Água, Luz, etc.) |
| `data` | TEXT | DEFAULT CURRENT_TIMESTAMP | Data do lançamento |

## 💰 `despesas`

| id | descricao | valor | mes | categoria | data |
|---|---|---|---|---|---|
| 1 | Conta de água | 450.00 | 2025-01 | Água | 2025-01-05 10:00:00 |
| 2 | Energia áreas comuns | 320.50 | 2025-01 | Energia | 2025-01-05 10:05:00 |
| 3 | Limpeza mensal | 800.00 | 2025-01 | Serviços | 2025-01-06 09:00:00 |
| 4 | Portão eletrônico | 1200.00 | 2025-02 | Manutenção | 2025-02-03 14:30:00 |
| 5 | Conta de água | 480.75 | 2025-02 | Água | 2025-02-05 10:00:00 |
<hr>

## 🔧 Tabela: `manutencao`

| Campo | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único |
| `titulo` | TEXT | NOT NULL | Título do chamado |
| `descricao` | TEXT | — | Descrição detalhada |
| `unidade_id` | INTEGER | FOREIGN KEY | Unidade relacionada |
| `status` | TEXT | DEFAULT 'Pendente' | Status atual |
| `prioridade` | TEXT | DEFAULT 'Normal' | Prioridade do chamado |
| `data_abertura` | TEXT | DEFAULT CURRENT_TIMESTAMP | Data de abertura |

> 🔗 **FK:** `unidade_id → unidades(id)` com `ON DELETE SET NULL`

## 🔧 `manutencao`

| id | titulo | descricao | unidade_id | status | prioridade | data_abertura |
|---|---|---|---|---|---|---|
| 1 | Vazamento no banheiro | Vazamento embaixo da pia | 1 | Pendente | Alta | 2025-02-10 08:30:00 |
| 2 | Lâmpada queimada | Corredor do 2º andar | NULL | Em andamento | Normal | 2025-02-11 09:15:00 |
| 3 | Porta do elevador | Travando ao fechar | NULL | Concluído | Alta | 2025-02-01 11:00:00 |
| 4 | Infiltração no teto | Unidade 201 — sala | 3 | Pendente | Urgente | 2025-02-12 16:45:00 |
<hr>

## 📢 Tabela: `avisos`

| Campo | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único |
| `titulo` | TEXT | NOT NULL | Título do aviso |
| `mensagem` | TEXT | — | Conteúdo do aviso |
| `data` | TEXT | DEFAULT CURRENT_TIMESTAMP | Data de publicação |

## 📢 `avisos`

| id | titulo | mensagem | data |
|---|---|---|---|
| 1 | Reunião de condomínio | Reunião dia 15/02 às 19h no salão. | 2025-02-01 09:00:00 |
| 2 | Manutenção do elevador | Elevador parado dia 20/02 das 8h às 12h. | 2025-02-10 14:00:00 |
| 3 | Coleta seletiva | Nova regra para descarte de recicláveis. | 2025-02-12 08:00:00 |
<hr>

## 🔗 Resumo dos Relacionamentos

| Tabela Origem | Campo | → | Tabela Destino | Campo | Ação ao Excluir |
|---|---|---|---|---|---|
| `moradores` | `unidade_id` | → | `unidades` | `id` | SET NULL |
| `manutencao` | `unidade_id` | → | `unidades` | `id` | SET NULL |

---

## 📊 Visão Geral

| Tabela | Registros Esperados | Categoria |
|---|---|---|
| `unidades` | Cadastro fixo | Cadastro |
| `moradores` | Variável | Cadastro |
| `despesas` | Por mês | Financeiro |
| `manutencao` | Sob demanda | Operacional |
| `avisos` | Periódico | Comunicação |

<hr>



# 🗺️ Diagrama ER (texto)

```
┌─────────────────────┐
│      UNIDADES       │
├─────────────────────┤
│ 🔑 id               │
│    bloco            │
│    numero           │
│    proprietario     │
│    telefone         │
│    email            │
│ 🔒 UNIQUE(bloco,numero) │
└──────────┬──────────┘
           │
           │ 1
           │
           ├───────────────────────┐
           │                       │
           │ N                     │ N
           ▼                       ▼
┌─────────────────────┐   ┌─────────────────────┐
│     MORADORES       │   │     MANUTENCAO      │
├─────────────────────┤   ├─────────────────────┤
│ 🔑 id               │   │ 🔑 id               │
│    nome             │   │    titulo           │
│ 🔗 unidade_id  ─────┘   │    descricao        │
│    tipo             │   │ 🔗 unidade_id  ─────┘
│    telefone         │   │    status           │
│    email            │   │    prioridade       │
└─────────────────────┘   │    data_abertura    │
                          └─────────────────────┘

┌─────────────────────┐   ┌─────────────────────┐
│      DESPESAS       │   │       AVISOS        │
├─────────────────────┤   ├─────────────────────┤
│ 🔑 id               │   │ 🔑 id               │
│    descricao        │   │    titulo           │
│    valor            │   │    mensagem         │
│    mes              │   │    data             │
│    categoria        │   └─────────────────────┘
│    data             │
└─────────────────────┘
   (tabela isolada)        (tabela isolada)
```

---

# 🔗 Legenda do Diagrama

| Símbolo | Significado |
|---|---|
| 🔑 | Chave Primária (PK) |
| 🔗 | Chave Estrangeira (FK) |
| 🔒 | Constraint UNIQUE |
| 1 → N | Relacionamento um-para-muitos |
| ON DELETE SET NULL | FK vira NULL ao excluir o pai |

---

# 📥 SQL — INSERTs completos

```sql
-- Unidades
INSERT INTO unidades (bloco, numero, proprietario, telefone, email) VALUES
('A', '101', 'João Silva', '(11) 99999-1111', 'joao@email.com'),
('A', '102', 'Maria Souza', '(11) 99999-2222', 'maria@email.com'),
('B', '201', 'Carlos Lima', '(11) 99999-3333', 'carlos@email.com'),
('B', '202', 'Fernanda Alves', '(11) 99999-4444', 'fernanda@email.com');

-- Moradores
INSERT INTO moradores (nome, unidade_id, tipo, telefone, email) VALUES
('João Silva', 1, 'Proprietário', '(11) 99999-1111', 'joao@email.com'),
('Ana Silva', 1, 'Inquilino', '(11) 98888-1111', 'ana@email.com'),
('Maria Souza', 2, 'Proprietária', '(11) 99999-2222', 'maria@email.com'),
('Carlos Lima', 3, 'Proprietário', '(11) 99999-3333', 'carlos@email.com'),
('Pedro Lima', 3, 'Dependente', '(11) 98888-3333', NULL);

-- Despesas
INSERT INTO despesas (descricao, valor, mes, categoria) VALUES
('Conta de água', 450.00, '2025-01', 'Água'),
('Energia áreas comuns', 320.50, '2025-01', 'Energia'),
('Limpeza mensal', 800.00, '2025-01', 'Serviços'),
('Portão eletrônico', 1200.00, '2025-02', 'Manutenção'),
('Conta de água', 480.75, '2025-02', 'Água');

-- Manutenção
INSERT INTO manutencao (titulo, descricao, unidade_id, status, prioridade) VALUES
('Vazamento no banheiro', 'Vazamento embaixo da pia', 1, 'Pendente', 'Alta'),
('Lâmpada queimada', 'Corredor do 2º andar', NULL, 'Em andamento', 'Normal'),
('Porta do elevador', 'Travando ao fechar', NULL, 'Concluído', 'Alta'),
('Infiltração no teto', 'Unidade 201 — sala', 3, 'Pendente', 'Urgente');

-- Avisos
INSERT INTO avisos (titulo, mensagem) VALUES
('Reunião de condomínio', 'Reunião dia 15/02 às 19h no salão.'),
('Manutenção do elevador', 'Elevador parado dia 20/02 das 8h às 12h.'),
('Coleta seletiva', 'Nova regra para descarte de recicláveis.');
```

---

Quer que eu adicione também:
- 🧮 **Views** prontas (ex.: gastos por mês, chamados abertos por unidade)?
- ⚡ **Triggers** (ex.: atualizar `data` automaticamente)?
- 🔍 **Índices** para acelerar consultas por `mes`, `status` e `bloco`?
