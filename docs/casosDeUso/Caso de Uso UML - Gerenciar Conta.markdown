🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Gerenciar Conta

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Gerenciar Conta)
```

## Descrição

Permite que o usuário crie, edite ou exclua contas bancárias.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).

## Pós-condições

- Conta é criada/editada/excluída em `accounts`.
- Lista de contas é atualizada.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de contas no frontend.
2. Escolhe ação:
   - **Criar**: Preenche `{ name, category, icon, color, balance }` (ex.: “Conta Corrente”, categoria “Banco”).
     - Frontend envia `POST /api/accounts`.
     - Backend valida: `name` único por `user_id`, `balance >= 0`.
     - Salva em `accounts`.
   - **Editar**: Seleciona conta, atualiza formulário, envia `PUT /api/accounts/{id}`.
     - Backend valida e atualiza.
   - **Excluir**: Seleciona conta, envia `DELETE /api/accounts/{id}`.
     - Backend verifica `transactions` e `cards`; se vinculada, retorna erro.
     - Remove conta (soft delete).
3. Frontend atualiza lista.
4. Backend registra ação em `logs` (ação: `create_account`, `update_account`, `delete_account`).

## Fluxos Alternativos

- **A1**: Dados inválidos → Backend retorna erro 422 ("Nome já existe ou dados inválidos").
- **A2**: Conta vinculada → Backend retorna erro 409 ("Conta vinculada a transações/cartões").

## Regras de Negócio

- Nomes são únicos por usuário (`accounts.name` + `user_id`).
- Contas vinculadas não podem ser excluídas.
- Ícones e cores: Personalizáveis (ex.: “bank”, “#FF0000”).

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Account`).
- **Endpoints**:
  - `POST /api/accounts`: `{ name: string, category: string, icon: string, color: string, balance: decimal }`.
  - `PUT /api/accounts/{id}`: Atualiza dados.
  - `DELETE /api/accounts/{id}`: Exclui (soft delete).
- **Banco**: Tabela `accounts` (`name`, `user_id`, `category`, `icon`, `color`, `balance`); `logs` (`action`).
- **Integrações**: Aparece em `transactions`, `cards`, relatórios.
- **Segurança**: JWT, validação de `user_id`.
