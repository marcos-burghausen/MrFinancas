🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Gerenciar Categoria

Este documento descreve o processo de criação, visualização, edição e exclusão de categorias e subcategorias.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de categorias (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Criar nova categoria?"}
    D -->|Sim| E["Preenche: { name, icon, color, type, parent_id }"]
    E --> F["Frontend envia POST /api/categories"]
    F --> G{"Dados válidos?"}
    G -->|Sim| H{"Subcategoria?"}
    H -->|Sim| I["Vincula a parent_id"]
    I --> J["Backend salva em categories"]
    H -->|Não| J
    J --> K["Registra ação em logs (ação: create_category)"]
    K --> L["Frontend exibe: 'Categoria criada'"]
    L --> M["Retorna à lista"]
    M --> N["Fim"]
    G -->|Não| O["Backend retorna erro 422: 'Nome já existe'"]
    O --> P["Frontend retorna ao formulário"]
    P --> E
    D -->|Não| Q["Frontend envia GET /api/categories"]
    Q --> R["Exibe lista de categorias"]
    R --> S["Seleciona categoria"]
    S --> T{"Editar?"}
    T -->|Sim| U["Edita formulário"]
    U --> V["Frontend envia PUT /api/categories/{id}"]
    V --> W["Backend valida e salva"]
    W --> X["Registra ação em logs (ação: update_category)"]
    X --> M
    T -->|Não| Y{"Excluir?"}
    Y -->|Sim| Z["Confirma exclusão"]
    Z --> AA["Frontend envia DELETE /api/categories/{id}"]
    AA --> AB{"Categoria tem transações?"}
    AB -->|Sim| AC["Backend retorna erro 409: 'Categoria vinculada'"]
    AC --> M
    AB -->|Não| AD["Backend remove (soft delete)"]
    AD --> AE["Registra ação em logs (ação: delete_category)"]
    AE --> M
    Y -->|Não| M
```

## Descrição do Processo

1. Usuário acessa a tela de categorias no frontend.
2. Escolhe ação:
   - **Criar**:
     - Preenche formulário: `{ name, icon, color, type, parent_id }` (ex.: “Alimentação”).
     - Frontend envia `POST /api/categories`.
     - Backend valida: `name` único por `user_id`, `parent_id` válido.
     - Salva em `categories`, registra em `logs`, exibe confirmação.
   - **Visualizar**:
     - Frontend envia `GET /api/categories`.
     - Exibe lista com Vuetify.
   - **Editar**:
     - Edita formulário, envia `PUT /api/categories/{id}`.
     - Backend valida, salva, registra em `logs`.
   - **Excluir**:
     - Confirma exclusão, envia `DELETE /api/categories/{id}`.
     - Backend verifica `transactions`; se vinculada, retorna erro 409.
     - Remove (soft delete), registra em `logs`.
3. Retorna à lista.

## Regras de Negócio

- Nomes únicos por usuário (`categories.name` + `user_id`).
- Subcategorias exigem `parent_id`.
- Categorias vinculadas não excluíveis.
- Predefinidas: “Alimentação”, “Transporte”, “Lazer”.
- Suporta hierarquia multinível.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Category`).
- **Endpoints**:
  - `POST /api/categories`: `{ name: string, icon: string, color: string, type: string, parent_id: int|null }`.
  - `GET /api/categories`: Lista categorias.
  - `PUT /api/categories/{id}`: Atualiza.
  - `DELETE /api/categories/{id}`: Exclui.
- **Banco**: Tabelas `categories`, `logs`.
- **Integrações**: Transações, relatórios.
- **Segurança**: JWT, HTTPS.