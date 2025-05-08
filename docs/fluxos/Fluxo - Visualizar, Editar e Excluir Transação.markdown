🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Visualizar, Editar e Excluir Transação

Este documento descreve o processo de visualização, edição e exclusão de transações financeiras no sistema.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de transações (Vue.js)"]
    B --> C["Frontend envia GET /api/transactions"]
    C --> D["Aplica filtros: { start_date, end_date, category_id, account_id, type }"]
    D --> E["Backend consulta transactions"]
    E --> F{"Dados disponíveis?"}
    F -->|Sim| G["Backend retorna lista de transações"]
    G --> H["Frontend exibe lista com Vuetify"]
    H --> I["Usuário seleciona transação"]
    I --> J{"Ação?"}
    J -->|Editar| K["Frontend carrega formulário com dados"]
    K --> L["Edita: { amount, date, description, account_id, card_id, category_id }"]
    L --> M["Frontend envia PUT /api/transactions/{id}"]
    M --> N{"Dados válidos?"}
    N -->|Sim| O["Backend atualiza transação"]
    O --> P["Atualiza accounts.balance ou invoices"]
    P --> Q["Registra ação em logs (ação: update_transaction)"]
    Q --> R["Frontend exibe: 'Transação atualizada'"]
    R --> S["Recarrega lista"]
    S --> T["Retorna à lista"]
    N -->|Não| U["Backend retorna erro 422: 'Dados inválidos'"]
    U --> V["Frontend exibe erro e retorna ao formulário"]
    V --> L
    J -->|Excluir| W["Usuário confirma exclusão"]
    W --> X["Frontend envia DELETE /api/transactions/{id}"]
    X --> Y["Backend verifica dependências (ex.: faturas)"]
    Y --> Z{"Pode excluir?"}
    Z -->|Sim| AA["Backend remove transação (soft delete)"]
    AA --> AB["Atualiza accounts.balance ou invoices"]
    AB --> AC["Registra ação em logs (ação: delete_transaction)"]
    AC --> AD["Frontend exibe: 'Transação excluída'"]
    AD --> S
    Z -->|Não| AE["Backend retorna erro 409: 'Transação vinculada'"]
    AE --> AF["Frontend exibe erro"]
    AF --> T
    J -->|Visualizar| AG["Frontend exibe detalhes em modal"]
    AG --> T
    F -->|Não| AH["Backend retorna erro 404: 'Nenhuma transação'"]
    AH --> AI["Frontend exibe mensagem: 'Nenhuma transação encontrada'"]
    AI --> T
    T --> AJ["Fim"]
```

## Descrição do Processo

1. Usuário acessa a tela de transações no frontend (Vue.js com Vuetify).
2. Frontend envia `GET /api/transactions` com filtros opcionais: `{ start_date, end_date, category_id, account_id, type }`.
3. Backend consulta `transactions` com base nos filtros:
   - Se há dados, retorna lista: `{ id, type, amount, date, description, account_id, card_id, category_id }`.
   - Se não, retorna erro 404, e frontend exibe mensagem.
4. Frontend exibe lista em tabela Vuetify.
5. Usuário seleciona uma transação e escolhe ação:
   - **Visualizar**:
     - Frontend exibe detalhes em modal Vuetify (ex.: tipo, valor, data, conta, categoria).
   - **Editar**:
     - Frontend carrega formulário preenchido com dados da transação.
     - Usuário edita campos: `{ amount, date, description, account_id, card_id, category_id }`.
     - Frontend envia `PUT /api/transactions/{id}`.
     - Backend valida: `amount > 0` (exceto estornos), `account_id`/`card_id`/`category_id` válidos.
     - Se válido, atualiza `transactions`, ajusta `accounts.balance` ou `invoices`, registra em `logs` (ação: `update_transaction`), e frontend exibe confirmação.
     - Se inválido, backend retorna erro 422, e frontend retorna ao formulário.
   - **Excluir**:
     - Usuário confirma exclusão.
     - Frontend envia `DELETE /api/transactions/{id}`.
     - Backend verifica dependências (ex.: transação vinculada a fatura em `invoices`).
     - Se permitido, remove (soft delete), atualiza `accounts.balance` ou `invoices`, registra em `logs` (ação: `delete_transaction`), e frontend exibe confirmação.
     - Se vinculada, backend retorna erro 409, e frontend exibe erro.
6. Frontend recarrega lista e retorna à tela de transações.

## Regras de Negócio

- Filtros: `start_date`/`end_date` (máximo 1 ano), `category_id`/`account_id` válidos, `type` (Despesa, Receita, Cartão, Estorno).
- Edição:
  - `amount > 0` para Despesa/Receita/Cartão; pode ser negativo para Estorno.
  - `account_id` ou `card_id` obrigatório, nunca ambos.
  - `date` não pode ser futura para Despesa/Receita.
- Exclusão:
  - Transações vinculadas a faturas não podem ser excluídas.
  - Soft delete mantém histórico.
- Atualizações de saldo:
  - Despesa: Decrementa `accounts.balance`.
  - Receita: Incrementa `accounts.balance`.
  - Cartão: Adiciona a `invoices` (se `card_id` presente).
  - Estorno: Reverte transação original.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios (requisições HTTP).
- **Backend**: Laravel, Eloquent (modelos `Transaction`, `Account`, `Invoice`, `Category`).
- **Endpoints**:
  - `GET /api/transactions?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD&category_id=int&account_id=int&type=string`: Lista transações filtradas.
  - `PUT /api/transactions/{id}`: `{ amount: decimal, date: date, description: string, account_id: int|null, card_id: int|null, category_id: int }` → `{ id: int }`.
  - `DELETE /api/transactions/{id}`: Exclui transação.
- **Banco**:
  - Tabelas: `transactions` (`id`, `type`, `amount`, `date`, `description`, `account_id`, `card_id`, `category_id`, `user_id`, `deleted_at`), `accounts` (`balance`), `invoices` (`card_id`, `amount`), `logs` (`action`, `details`, `user_id`).
  - Triggers: Atualizam `accounts.balance` após edição/exclusão.
- **Integrações**:
  - Notification Service (AWS SNS) para alertas de saldo crítico.
  - Log Service para auditoria.
  - Relatórios incluem transações editadas/excluídas.
- **Segurança**:
  - JWT para autenticação.
  - Validação de `user_id` para garantir propriedade.
  - HTTPS para comunicações.
  - Transações SQL para consistência (ex.: atualização de saldo e transação).

## Alinhamento com Caso de Uso

Este fluxo corresponde ao `Caso de Uso - Visualizar, Editar e Excluir Transação`, detalhando a interface (Vuetify), endpoints, validações e atualizações de saldo, com maior granularidade técnica. Diferentemente do `Fluxo - Gerenciar Transação`, exclui a criação de transações, focando nas ações de visualização, edição e exclusão.