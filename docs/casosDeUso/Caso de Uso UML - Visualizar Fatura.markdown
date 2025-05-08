🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Visualizar Fatura

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Visualizar Fatura)
```

## Descrição

Permite que o usuário visualize faturas de cartões de crédito, incluindo gastos, vencimento e valor total.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem cartões em `cards` com transações em `transactions`.

## Pós-condições

- Fatura é exibida no frontend.
- Log de visualização é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de cartões no frontend.
2. Seleciona cartão e clica em "Visualizar fatura".
3. Frontend envia `GET /api/cards/{id}/invoices`.
4. Backend consulta `invoices` e `transactions` (filtrado por `card_id`, período).
5. Retorna: `{ invoice_id, due_date, total_amount, transactions: [] }`.
6. Frontend exibe fatura com Vuetify (tabela de transações, vencimento, total).
7. Usuário pode exportar em PDF via jsPDF.
8. Backend registra ação em `logs` (ação: `view_invoice`).

## Fluxos Alternativos

- **A1**: Nenhuma fatura disponível → Backend retorna erro 404 ("Nenhuma fatura disponível").

## Regras de Negócio

- Faturas baseadas no ciclo (`cards.closing_date` a `due_date`).
- `total_amount` soma `transactions.amount` do período.
- Exportação PDF inclui título, período e transações.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, jsPDF.
- **Backend**: Laravel, Eloquent (modelos `Card`, `Invoice`, `Transaction`).
- **Endpoint**: `GET /api/cards/{id}/invoices?period=YYYY-MM` → `{ invoices: [] }`.
- **Banco**: Tabela `invoices` (`card_id`, `due_date`, `total_amount`); `transactions` (`card_id`, `amount`); `logs` (`action`).
- **Integrações**: jsPDF, notificações de vencimento.
- **Segurança**: JWT, validação de `user_id`.
