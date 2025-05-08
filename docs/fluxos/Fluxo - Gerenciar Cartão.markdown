🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Gerenciar Cartão

Este documento descreve o processo de criação, visualização, edição, exclusão e visualização de faturas de cartões de crédito.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de cartões (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Criar novo cartão?"}
    D -->|Sim| E["Preenche: { name, limit, account_id, icon, color, closing_date, due_date }"]
    E --> F["Frontend envia POST /api/cards"]
    F --> G{"Dados válidos?"}
    G -->|Sim| H["Backend salva em cards"]
    H --> I["Registra ação em logs (ação: create_card)"]
    I --> J["Frontend exibe: 'Cartão criado'"]
    J --> K["Retorna à lista"]
    K --> L["Fim"]
    G -->|Não| M["Backend retorna erro 422: 'Dados inválidos'"]
    M --> N["Frontend retorna ao formulário"]
    N --> E
    D -->|Não| O["Frontend envia GET /api/cards"]
    O --> P["Exibe lista de cartões"]
    P --> Q["Seleciona cartão"]
    Q --> R{"Editar?"}
    R -->|Sim| S["Edita formulário"]
    S --> T["Frontend envia PUT /api/cards/{id}"]
    T --> U["Backend valida e salva"]
    U --> V["Registra ação em logs (ação: update_card)"]
    V --> K
    R -->|Não| W{"Excluir?"}
    W -->|Sim| X["Confirma exclusão"]
    X --> Y["Frontend envia DELETE /api/cards/{id}"]
    Y --> Z{"Cartão tem transações/faturas?"}
    Z -->|Sim| AA["Backend retorna erro 409: 'Cartão vinculado'"]
    AA --> K
    Z -->|Não| AB["Backend remove cartão (soft delete)"]
    AB --> AC["Registra ação em logs (ação: delete_card)"]
    AC --> K
    W -->|Não| AD{"Visualizar fatura?"}
    AD -->|Sim| AE["Frontend envia GET /api/cards/{id}/invoices"]
    AE --> AF["Backend retorna fatura"]
    AF --> AG["Frontend exibe: gastos, vencimento, total"]
    AG --> K
    AD -->|Não| K
```

## Descrição do Processo

1. Usuário acessa a tela de cartões no frontend.
2. Escolhe ação:
   - **Criar**:
     - Preenche formulário: `{ name, limit, account_id, icon, color, closing_date, due_date }`.
     - Frontend envia `POST /api/cards`.
     - Backend valida: `name` único, `limit > 0`, `account_id` válido.
     - Salva em `cards`, registra em `logs`, exibe confirmação.
   - **Visualizar**:
     - Frontend envia `GET /api/cards`.
     - Exibe lista com Vuetify.
   - **Editar**:
     - Edita formulário, envia `PUT /api/cards/{id}`.
     - Backend valida, salva, registra em `logs`.
   - **Excluir**:
     - Confirma exclusão, envia `DELETE /api/cards/{id}`.
     - Backend verifica `transactions` e `invoices`; se vinculado, retorna erro 409.
     - Remove (soft delete), registra em `logs`.
   - **Visualizar Fatura**:
     - Frontend envia `GET /api/cards/{id}/invoices`.
     - Backend retorna dados de `invoices` e `transactions`.
     - Frontend exibe com Vuetify.
3. Retorna à lista.

## Regras de Negócio

- Cartões exigem `account_id` válido.
- Nomes únicos por usuário.
- `limit > 0`.
- Alerta se uso > 80% do limite.
- Faturas geradas com base em `closing_date` e `due_date`.
- Pagamentos de faturas registrados como transferências.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, jsPDF para exportação de faturas.
- **Backend**: Laravel, Eloquent (modelos `Card`, `Invoice`, `Transaction`).
- **Endpoints**:
  - `POST /api/cards`: `{ name: string, limit: decimal, account_id: int, icon: string, color: string, closing_date: date, due_date: date }`.
  - `GET /api/cards`: Lista cartões.
  - `PUT /api/cards/{id}`: Atualiza.
  - `DELETE /api/cards/{id}`: Exclui.
  - `GET /api/cards/{id}/invoices`: Retorna faturas.
- **Banco**: Tabelas `cards`, `invoices`, `transactions`, `logs`.
- **Integrações**: Notificações, relatórios, jsPDF.
- **Segurança**: JWT, HTTPS.