🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Gerenciar Categoria

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Gerenciar Categoria)
```

## Descrição

Permite que o usuário crie, edite ou exclua categorias e subcategorias financeiras.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).

## Pós-condições

- Categoria/subcategoria é criada/editada/excluída em `categories`.
- Lista de categorias é atualizada.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de categorias no frontend.
2. Escolhe ação:
   - **Criar**: Preenche `{ name, icon, color, parent_id, type }` (ex.: “Alimentação”, ícone “utensils”).
     - Frontend envia `POST /api/categories`.
     - Backend valida: `name` único por `user_id`, `parent_id` válido.
     - Salva em `categories`.
   - **Editar**: Seleciona categoria, atualiza formulário, envia `PUT /api/categories/{id}`.
     - Backend valida e atualiza.
   - **Excluir**: Seleciona categoria, envia `DELETE /api/categories/{id}`.
     - Backend verifica `transactions` e subcategorias; se vinculada, retorna erro.
     - Remove categoria (soft delete).
3. Frontend atualiza lista.
4. Backend registra ação em `logs` (ação: `create_category`, `update_category`, `delete_category`).

## Fluxos Alternativos

- **A1**: Dados inválidos → Backend retorna erro 422 ("Nome já existe ou dados inválidos").
- **A2**: Categoria vinculada → Backend retorna erro 409 ("Categoria vinculada a transações/subcategorias").

## Regras de Negócio

- Nomes são únicos por usuário (`categories.name` + `user_id`).
- Subcategorias exigem `parent_id`.
- Suporta hierarquia multinível.
- Categorias predefinidas: “Alimentação”, “Transporte”, “Lazer”.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify (formulários e listas).
- **Backend**: Laravel, Eloquent (modelo `Category`).
- **Endpoints**:
  - `POST /api/categories`: `{ name: string, icon: string, color: string, parent_id: int|null, type: string }`.
  - `PUT /api/categories/{id}`: Atualiza dados.
  - `DELETE /api/categories/{id}`: Exclui (soft delete).
- **Banco**: Tabela `categories` (`name`, `user_id`, `parent_id`, `icon`, `color`); `logs` (`action`, `details`).
- **Integrações**: Aparece em `transactions` e relatórios.
- **Segurança**: JWT, validação de `user_id`.
