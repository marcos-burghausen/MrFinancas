🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Gerar Relatório

Este documento descreve o processo de geração de relatórios financeiros (gráficos ou tabelas) com exportação.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de relatórios (Vue.js)"]
    B --> C["Seleciona tipo: gráfico (pizza/linha), tabela (balanço)"]
    C --> D["Aplica filtros: { start_date, end_date, category_id, account_id }"]
    D --> E["Frontend envia GET /api/reports"]
    E --> F["Backend consulta transactions, accounts, categories"]
    F --> G{"Dados disponíveis?"}
    G -->|Sim| H["Backend retorna dados"]
    H --> I["Frontend exibe: Chart.js (gráficos), Vuetify (tabelas)"]
    I --> J{"Exportar?"}
    J -->|Sim| K["Seleciona formato: PDF/CSV"]
    K --> L["Frontend gera: jsPDF (PDF), CSV com colunas"]
    L --> M["Inicia download"]
    M --> N["Registra ação em logs (ação: generate_report)"]
    N --> O["Retorna à tela"]
    O --> P["Fim"]
    J -->|Não| O
    G -->|Não| Q["Backend retorna erro 404: 'Sem dados'"]
    Q --> R["Frontend exibe erro"]
    R --> O
```

## Descrição do Processo

1. Usuário acessa a tela de relatórios no frontend.
2. Seleciona tipo (gráfico ou tabela).
3. Aplica filtros: `{ start_date, end_date, category_id, account_id }`.
4. Frontend envia `GET /api/reports`.
5. Backend consulta `transactions`, `accounts`, `categories`:
   - Se há dados, retorna para exibição (Chart.js para gráficos, Vuetify para tabelas).
   - Se não, retorna erro 404.
6. Usuário pode exportar:
   - PDF: Gera com jsPDF (título, filtros, dados).
   - CSV: Colunas `{ date, amount, category, account }`.
7. Backend registra em `logs` (ação: `generate_report`).
8. Retorna à tela.

## Regras de Negócio

- Relatórios: Até 1 ano (`start_date >= now() - 1 year`).
- Gráficos: Despesas/receitas por categoria, balanço.
- CSV: `{ date: YYYY-MM-DD, amount: decimal, category: string, account: string }`.
- PDF: Inclui título e filtros.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Chart.js, jsPDF.
- **Backend**: Laravel, Eloquent (modelos `Transaction`, `Account`, `Category`).
- **Endpoint**: `GET /api/reports?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD&category_id=int&account_id=int`.
- **Banco**: Tabelas `transactions`, `accounts`, `categories`, `logs`.
- **Integrações**: Chart.js, jsPDF.
- **Segurança**: JWT, HTTPS.