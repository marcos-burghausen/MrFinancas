🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Gerar Relatório

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Gerar Relatório)
```

## Descrição

Permite que o usuário gere relatórios financeiros (gráficos ou tabelas) com exportação em PDF/CSV.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem transações em `transactions`.

## Pós-condições

- Relatório é exibido (gráfico ou tabela).
- Relatório é exportado (se solicitado).
- Log de geração é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de relatórios no frontend.
2. Seleciona tipo: gráfico (pizza, linha) ou tabela (balanço mensal).
3. Aplica filtros: `{ start_date, end_date, category_id, account_id }`.
4. Frontend envia `GET /api/reports` com filtros.
5. Backend consulta `transactions`, `accounts`, `categories` e calcula totais.
6. Frontend exibe relatório usando Chart.js (gráficos) ou Vuetify (tabelas).
7. Usuário clica em "Exportar":
   - PDF: Gera documento com jsPDF.
   - CSV: Gera arquivo com colunas `{ date, amount, category, account }`.
8. Backend registra ação em `logs` (ação: `generate_report`, detalhes: tipo, filtros).

## Fluxos Alternativos

- **A1**: Nenhum dado para o filtro → Backend retorna erro 404 ("Sem dados para o período").
- **A2**: Erro na exportação → Frontend exibe erro 500 ("Falha ao exportar") e oferece retry.

## Regras de Negócio

- Relatórios cobrem até 1 ano (`start_date >= now() - 1 year`).
- Gráficos: Despesas/receitas por categoria, balanço por período.
- CSV: Colunas `{ date: YYYY-MM-DD, amount: decimal, category: string, account: string }`.
- PDF: Inclui título, filtros e dados formatados.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify (tabelas), Chart.js (gráficos), jsPDF (PDF).
- **Backend**: Laravel, Eloquent (modelos `Transaction`, `Account`, `Category`).
- **Endpoint**: `GET /api/reports?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD&category_id=int&account_id=int`.
- **Banco**: Tabela `transactions` (`amount`, `date`, `category_id`, `account_id`); `categories` (`name`); `accounts` (`name`); `logs` (`action`, `details`).
- **Integrações**: Chart.js, jsPDF.
- **Segurança**: Autenticação JWT, validação de filtros.
