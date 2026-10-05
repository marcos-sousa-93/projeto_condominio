# Tabela: `usuarios`

| Campo | Tipo | Restrições | Descrição |
|-------|------|------------|-----------|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Identificador único do usuário, gerado automaticamente |
| `nome` | TEXT | NOT NULL | Nome completo do usuário |
| `email` | TEXT | NOT NULL, UNIQUE | E-mail do usuário (usado para login); não pode repetir |
| `senha_hash` | TEXT | NOT NULL | Hash da senha (senha criptografada, nunca em texto puro) |
| `perfil` | TEXT | NOT NULL, CHECK IN ('admin','sindico','zelador','portaria','morador') | Perfil de acesso do usuário, restrito a um dos 5 valores permitidos |
| `unidade_id` | INTEGER | FOREIGN KEY → unidades(id) ON DELETE SET NULL | Referência à unidade do usuário; se a unidade for excluída, este campo vira NULL |
| `ativo` | INTEGER | DEFAULT 1 | Indica se o usuário está ativo (1) ou inativo (0) |
| `criado_em` | TEXT | DEFAULT CURRENT_TIMESTAMP | Data/hora de criação do registro, preenchida automaticamente |

## Observações

- **Chave primária:** `id`
- **Chave estrangeira:** `unidade_id` referencia a tabela `unidades`
- **Comportamento ao excluir unidade:** `ON DELETE SET NULL` (o usuário permanece, mas sem unidade vinculada)
- **Valores possíveis para `perfil`:**
  - `admin` — Administrador
  - `sindico` — Síndico
  - `zelador` — Zelador
  - `portaria` — Portaria
  - `morador` — Morador

## Resumo dos relacionamentos

| Tabela origem | Campo | Tabela destino | Campo | Ação ao deletar |
|---------------|-------|----------------|-------|-----------------|
| `usuarios` | `unidade_id` | `unidades` | `id` | SET NULL |
