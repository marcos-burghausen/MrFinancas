🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Visualizar Fatura

Este diagrama descreve as interações para visualizar faturas de cartão.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de cartões
    F->>B: GET /cards
    B-->>F: Lista de cartões
    F-->>U: Exibe lista
    U->>F: Seleciona cartão
    F->>B: GET /cards/{id}/invoices?dataInicio&dataFim
    B->>DB: Consulta faturas
    DB-->>B: Lista de faturas
    B-->>F: Resposta: { total, paginas, paginaAtual, faturas }
    F-->>U: Exibe faturas com transações
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de cartões.
2. O frontend lista cartões via `GET /cards`.
3. O usuário seleciona um cartão.
4. O frontend envia `GET /cards/{id}/invoices` com filtros (`dataInicio`, `dataFim`).
5. O backend retorna lista paginada de faturas com transações.
6. O frontend exibe faturas (abertas/fechadas) e suas transações.

### Fluxos Alternativas

- **A1**: Nenhuma fatura encontrada → Backend retorna dados vazios `{ total: 0, faturas: [] }`.
- **A2**: Cartão inválido → Backend retorna erro 404 ("Cartão não encontrado").
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`Card`, `Invoice`, `Transaction`).
  - Endpoint: `GET /cards/{id}/invoices` (controlador `InvoiceController`).
- **Banco**: Tabelas `cards`, `invoices` (`card_id`, `due_date`, `closing_date`, `amount`), `transactions` (`invoice_id`).
- **Segurança**: JWT para autenticação.