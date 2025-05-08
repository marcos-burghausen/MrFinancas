🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Gerenciar Transação

Este documento descreve o processo de criação, visualização, edição e exclusão de transações financeiras, incluindo recorrência.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de transações (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Criar nova transação?"}
    D -->|Sim| E["Seleciona tipo: Despesa, Receita, Cartão, Estorno"]
    E --> F["Preenche: { amount, date, description, account_id, card_id, category_id, recurring, frequency, end_date }"]
    F --> G["Frontend envia POST /api/transactions"]
    G --> H{"Dados válidos?"}
    H -->|Sim| I{"Recorrente?"}
    I -->|Sim| J["Configura recorrência: daily, weekly, monthly"]
    J --> K["Backend salva transação e instâncias futuras"]
    K --> L["Atualiza accounts.balance ou invoices"]
    I -->|Não| M["Backend salva transação única"]
    M --> L
    L --> N["Registra ação em logs (ação: create_transaction)"]
    N --> O["Frontend exibe: 'Transação salva'"]
    O --> P["Retorna à lista"]
    P --> Q["Fim"]
    H -->|Não| R["Backend retorna erro 422: 'Dados inválidos'"]
    R --> S["Frontend retorna ao formulário"]
    S --> F
    D -->|Não| T["Frontend envia GET /api/transactions"]
    T --> U["Aplica filtros: start_date, end_date, category_id, account_id"]
    U --> V["Exibe transações filtradas"]
    V --> W{"Editar ou excluir?"}
    W -->|Editar| X["Edita formulário"]
    X --> Y["Frontend envia PUT /api/transactions/{id}"]
    Y --> Z["Backend valida e salva"]
    Z --> AA["Registra ação em logs (ação: update_transaction)"]
    AA --> P
    W -->|Excluir| AB["Confirma exclusão"]
    AB --> AC["Frontend envia DELETE /api/transactions/{id}"]
    AC --> AD["Backend remove (soft delete)"]
    AD --> AE["Registra ação em logs (ação: delete_transaction)"]
    AE --> P
    W -->|Não| P
```

## Descrição do Processo

1. Usuário acessa a tela de transações no frontend.
2. Escolhe ação:
   - **Criar**:
     - Seleciona tipo (Despesa, Receita, Cartão, Estorno).
     - Preenche formulário.
     - Frontend envia `POST /api/transactions`.
     - Backend valida: `amount > 0` (exceto estornos), `account_id`/`card_id`/`category_id` válidos.
     - Se recorrente, agenda instâncias futuras.
     - Salva, atualiza saldos, registra em `logs`, exibe confirmação.
   - **Visualizar**:
     - Frontend envia `GET /api/transactions` com filtros.
     - Exibe lista filtrada com Vuetify.
   - **Editar**:
     - Edita formulário, envia `PUT /api/transactions/{id}`.
     - Backend valida, salva, atualiza saldos, registra em `logs`.
   - **Excluir**:
     - Confirma exclusão, envia `DELETE /api/transactions/{id}`.
     - Backend remove (soft delete), atualiza saldos, registra em `logs`.
3. Retorna à lista.

## Regras de Negócio

- `amount > 0` (exceto estornos).
- `account_id` ou `card_id` obrigatório.
- Recorrência: Instâncias até `end_date`.
- Despesas diminuem saldo, receitas aumentam, cartões geram faturas, estornos revertem.
- Edição de recorrentes afeta instâncias futuras.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Transaction`).
- **Endpoints**:
  - `POST /api/transactions`: `{ amount: decimal, date: date, description: string, account_id: int, card_id: int|null, category_id: int, recurring: boolean, frequency: string|null, end_date: date|null }`.
  - `GET /api/transactions`: Filtros como query params.
  - `PUT /api/transactions/{id}`: Atualiza.
  - `DELETE /api/transactions/{id}`: Exclui.
- **Banco**: Tabelas `transactions`, `accounts`, `invoices`, `logs`.
- **Integrações**: Laravel Queue, notificações, relatórios.
- **Segurança**: JWT, transações SQL.