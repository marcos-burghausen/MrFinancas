🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Visualizar Fatura

Este documento descreve o processo de visualização de faturas de cartões de crédito.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de cartões (Vue.js)"]
    B --> C["Seleciona cartão"]
    C --> D["Clica em 'Visualizar fatura'"]
    D --> E["Frontend envia GET /api/cards/{id}/invoices"]
    E --> F{"Fatura disponível?"}
    F -->|Sim| G["Backend retorna: invoice_id, due_date, total_amount, transactions"]
    G --> H["Frontend exibe: tabela Vuetify com gastos, vencimento, total"]
    H --> I{"Exportar PDF?"}
    I -->|Sim| J["Frontend gera PDF com jsPDF"]
    J --> K["Inicia download"]
    K --> L["Registra ação em logs (ação: view_invoice)"]
    L --> M["Retorna à tela"]
    M --> N["Fim"]
    I -->|Não| L
    F -->|Não| O["Backend retorna erro 404: 'Nenhuma fatura'"]
    O --> P["Frontend exibe erro"]
    P --> M
```

## Descrição do Processo

1. Usuário acessa a tela de cartões no frontend.
2. Seleciona um cartão e clica em “Visualizar fatura”.
3. Frontend envia `GET /api/cards/{id}/invoices`.
4. Backend consulta `invoices` e `transactions` (filtrado por `card_id`, período):
   - Se há fatura, retorna `{ invoice_id, due_date, total_amount, transactions }`.
   - Se não, retorna erro 404.
5. Frontend exibe fatura com Vuetify (tabela de transações, vencimento, total).
6. Usuário pode exportar PDF com jsPDF.
7. Backend registra em `logs` (ação: `view_invoice`).
8. Retorna à tela.

## Regras de Negócio

- Faturas baseadas em `cards.closing_date` a `due_date`.
- `total_amount` soma `transactions.amount` do período.
- PDF inclui período, transações e total.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, jsPDF.
- **Backend**: Laravel, Eloquent (modelos `Card`, `Invoice`, `Transaction`).
- **Endpoint**: `GET /api/cards/{id}/invoices?period=YYYY-MM`.
- **Banco**: Tabelas `invoices`, `transactions`, `cards`, `logs`.
- **Integrações**: jsPDF, notificações.
- **Segurança**: JWT, HTTPS.