🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Gerenciar Cartão

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Gerenciar Cartão)
```

## Descrição

Permite que o usuário crie, edite ou exclua cartões de crédito.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem contas em `accounts`.

## Pós-condições

- Cartão é criado/editada/excluído em `cards`.
- Lista de cartões é atualizada.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de cartões no frontend.
2. Escolhe ação:
   - **Criar**: Preenche `{ name, limit, account_id, icon, color, closing_date, due_date }`.
     - Frontend envia `POST /api/cards`.
     - Backend valida: `name` único, `limit > 0`, `account_id` válido.
     - Salva em `cards`.
   - **Editar**: Seleciona cartão, atualiza formulário, envia `PUT /api/cards/{id}`.
     - Backend valida e atualiza.
   - **Excluir**: Seleciona cartão, envia `DELETE /api/cards/{id}`.
     - Backend verifica `transactions` e `invoices`; se vinculado, retorna erro.
     - Remove cartão (soft delete).
3. Frontend atualiza lista.
4. Backend registra ação em `logs` (ação: `create_card`, `update_card`, `delete_card`).

## Fluxos Alternativos

- **A1**: Dados inválidos → Backend retorna erro 422 ("Dados inválidos ou conta inválida").
- **A2**: Cartão vinculado → Backend retorna erro 409 ("Cartão vinculado a transações/faturas").
- **A3**: Uso excede 80% do limite → Backend envia notificação via `notifications`.

## Regras de Negócio

- Cartões exigem `account_id` válido.
- Nomes são únicos por usuário.
- Limites: `limit > 0`.
- Alerta se uso > 80% do limite (calculado via `transactions`).

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Card`).
- **Endpoints**:
  - `POST /api/cards`: `{ name: string, limit: decimal, account_id: int, icon: string, color: string, closing_date: date, due_date: date }`.
  - `PUT /api/cards/{id}`: Atualiza dados.
  - `DELETE /api/cards/{id}`: Exclui (soft delete).
- **Banco**: Tabela `cards` (`name`, `user_id`, `account_id`, `limit`, `closing_date`, `due_date`); `logs` (`action`).
- **Integrações**: Aparece em `transactions`, `invoices`, notificações.
- **Segurança**: JWT, validação de `user_id`.
