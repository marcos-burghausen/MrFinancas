🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Visualizar, Editar e Excluir Transação

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Visualizar Transação)
    Usuário --> (Editar Transação)
    Usuário --> (Excluir Transação)
```

## Descrição

Permite que o usuário visualize, edite ou exclua transações financeiras.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem transações em `transactions`.

## Pós-condições

- Lista de transações é exibida.
- Transação é editada/excluída, com saldos atualizados.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de transações no frontend.
2. Aplica filtros: `{ start_date, end_date, category_id, account_id }`.
3. Frontend envia `GET /api/transactions` com filtros.
4. Backend retorna transações de `transactions`.
5. Frontend exibe lista com Vuetify.
6. Usuário seleciona transação:
   - **Editar**:
     - Abre formulário com dados atuais.
     - Frontend envia `PUT /api/transactions/{id}` com `{ amount, date, description, account_id, card_id, category_id }`.
     - Backend valida e atualiza, recalcula `accounts.balance` ou `invoices`.
   - **Excluir**:
     - Confirma exclusão, envia `DELETE /api/transactions/{id}`.
     - Backend remove (soft delete) e atualiza saldos.
     - Remove notificações associadas em `notifications`.
7. Backend registra ação em `logs` (ação: `update_transaction`, `delete_transaction`).
8. Frontend atualiza lista.

## Fluxos Alternativos

- **A1**: Nenhum resultado → Backend retorna erro 404 ("Nenhum resultado encontrado").
- **A2**: Dados inválidos na edição → Backend retorna erro 422 ("Dados inválidos").

## Regras de Negócio

- Exclusão de transações recorrentes remove instâncias futuras.
- Edição atualiza saldos e notificações.
- Filtros: Até 1 ano de dados (`start_date >= now() - 1 year`).

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Transaction`).
- **Endpoints**:
  - `GET /api/transactions?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD&category_id=int&account_id=int`.
  - `PUT /api/transactions/{id}`: Atualiza dados.
  - `DELETE /api/transactions/{id}`: Exclui (soft delete).
- **Banco**: Tabela `transactions` (`amount`, `date`, `account_id`, `card_id`, `category_id`); `accounts` (`balance`); `notifications`; `logs`.
- **Integrações**: Atualiza relatórios, faturas.
- **Segurança**: JWT, transações SQL.
