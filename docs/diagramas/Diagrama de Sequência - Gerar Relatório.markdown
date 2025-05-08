🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Gerar Relatório

Este diagrama descreve as interações para gerar relatórios financeiros.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de relatórios
    F->>U: Exibe opções de relatório
    U->>F: Seleciona resumo por período (dataInicio, dataFim)
    F->>B: GET /relatorios/resumo?dataInicio&dataFim
    B->>DB: Consulta transações no período
    DB-->>B: Dados de transações
    B->>B: Calcula totais e percentuais
    B-->>F: Resposta: { totalReceitas, totalDespesas, saldo, receitasPorCategoria, despesasPorCategoria }
    F-->>U: Exibe relatório com gráficos
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de relatórios.
2. O frontend exibe opções (e.g., resumo, fluxo de caixa).
3. O usuário seleciona “resumo por período” e define `dataInicio` e `dataFim`.
4. O frontend envia `GET /relatorios/resumo`.
5. O backend consulta transações no período.
6. Calcula totais e percentuais por categoria.
7. Retorna dados para o frontend.
8. O frontend exibe o relatório com gráficos (pizza, barras).

### Fluxos Alternativos

- **A1**: Período inválido → Backend retorna erro 422 ("Período inválido").
- **A2**: Nenhuma transação → Backend retorna dados vazios `{ totalReceitas: 0, totalDespesas: 0 }`.
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Chart.js para gráficos.
- **Backend**: Laravel com Eloquent (`Transaction`, `Category`).
  - Endpoint: `GET /relatorios/resumo` (controlador `ReportController`).
- **Banco**: Tabela `transactions` para cálculos.
- **Segurança**: JWT para autenticação.