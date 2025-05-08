🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Criar Transação

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Criar Transação)
```

## Descrição

Permite que o usuário crie transações financeiras (despesas, receitas, cartões, estornos) com suporte a recorrência.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem contas (`accounts`), cartões (`cards`) e categorias (`categories`).

## Pós-condições

- Transação é salva em `transactions`.
- Saldos de `accounts` ou `cards` são atualizados.
- Notificação de vencimento é agendada (se aplicável).
- Log de criação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de transações no frontend.
2. Seleciona tipo: Despesa, Receita, Cartão, Estorno.
3. Preenche formulário: `{ amount, date, description, account_id, card_id, category_id, recurring, frequency, end_date }`.
4. Frontend envia `POST /api/transactions`.
5. Backend valida:
   - `amount > 0` (exceto estornos).
   - `account_id`, `card_id`, `category_id` válidos para `user_id`.
6. Se `recurring = true`, agenda instâncias futuras (`frequency`: daily, weekly, monthly; `end_date`).
7. Salva transação em `transactions` e atualiza `accounts.balance` ou `cards` (via `invoices`).
8. Agenda notificação em `notifications` (se `date` definida).
9. Backend registra ação em `logs` (ação: `create_transaction`).
10. Frontend exibe confirmação.

## Fluxos Alternativos

- **A1**: Dados inválidos → Backend retorna erro 422 ("Dados inválidos, verifique os campos").
- **A2**: Saldo insuficiente → Backend envia alerta, mas permite salvar (configurável).

## Regras de Negócio

- Valores: `amount > 0` (exceto estornos).
- `account_id` ou `card_id` obrigatório.
- Recorrência: Gera instâncias até `end_date`.
- Vencimentos associados a `notifications`.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Transaction`).
- **Endpoint**: `POST /api/transactions`: `{ amount: decimal, date: date, description: string, account_id: int, card_id: int|null, category_id: int, recurring: boolean, frequency: string|null, end_date: date|null }`.
- **Banco**: Tabela `transactions` (`amount`, `date`, `account_id`, `card_id`, `category_id`, `recurring`, `frequency`, `end_date`); `accounts` (`balance`); `notifications`; `logs`.
- **Integrações**: Laravel Queue (recorrência, notificações).
- **Segurança**: JWT, transações SQL para consistência.
