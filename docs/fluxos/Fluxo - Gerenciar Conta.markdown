🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Gerenciar Conta

Este documento descreve o processo de criação, visualização, edição e exclusão de contas bancárias.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de contas (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Criar nova conta?"}
    D -->|Sim| E["Preenche: { name, category, icon, color, balance }"]
    E --> F["Frontend envia POST /api/accounts"]
    F --> G{"Dados válidos?"}
    G -->|Sim| H["Backend salva em accounts"]
    H --> I["Registra ação em logs (ação: create_account)"]
    I --> J["Frontend exibe: 'Conta criada'"]
    J --> K["Retorna à lista"]
    K --> L["Fim"]
    G -->|Não| M["Backend retorna erro 422: 'Dados inválidos'"]
    M --> N["Frontend retorna ao formulário"]
    N --> E
    D -->|Não| O["Frontend envia GET /api/accounts"]
    O --> P["Exibe lista de contas"]
    P --> Q["Seleciona conta"]
    Q --> R{"Editar?"}
    R -->|Sim| S["Edita formulário"]
    S --> T["Frontend envia PUT /api/accounts/{id}"]
    T --> U["Backend valida e salva"]
    U --> V["Registra ação em logs (ação: update_account)"]
    V --> K
    R -->|Não| W{"Excluir?"}
    W -->|Sim| X["Confirma exclusão"]
    X --> Y["Frontend envia DELETE /api/accounts/{id}"]
    Y --> Z{"Conta tem transações/cartões?"}
    Z -->|Sim| AA["Backend retorna erro 409: 'Conta vinculada'"]
    AA --> K
    Z -->|Não| AB["Backend remove (soft delete)"]
    AB --> AC["Registra ação em logs (ação: delete_account)"]
    AC --> K
    W -->|Não| K
```

## Descrição do Processo

1. Usuário acessa a tela de contas no frontend.
2. Escolhe ação:
   - **Criar**:
     - Preenche formulário: `{ name, category, icon, color, balance }`.
     - Frontend envia `POST /api/accounts`.
     - Backend valida: `name` único, `balance >= 0`.
     - Salva em `accounts`, registra em `logs`, exibe confirmação.
   - **Visualizar**:
     - Frontend envia `GET /api/accounts`.
     - Exibe lista com Vuetify.
   - **Editar**:
     - Edita formulário, envia `PUT /api/accounts/{id}`.
     - Backend valida, salva, registra em `logs`.
   - **Excluir**:
     - Confirma exclusão, envia `DELETE /api/accounts/{id}`.
     - Backend verifica `transactions` e `cards`; se vinculada, retorna erro 409.
     - Remove (soft delete), registra em `logs`.
3. Retorna à lista.

## Regras de Negócio

- Nomes únicos por usuário (`accounts.name` + `user_id`).
- Contas vinculadas não excluíveis.
- `balance >= 0`.
- Ícones e cores personalizáveis.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Account`).
- **Endpoints**:
  - `POST /api/accounts`: `{ name: string, category: string, icon: string, color: string, balance: decimal }`.
  - `GET /api/accounts`: Lista contas.
  - `PUT /api/accounts/{id}`: Atualiza.
  - `DELETE /api/accounts/{id}`: Exclui.
- **Banco**: Tabelas `accounts`, `logs`.
- **Integrações**: Transações, relatórios, transferências.
- **Segurança**: JWT, HTTPS.