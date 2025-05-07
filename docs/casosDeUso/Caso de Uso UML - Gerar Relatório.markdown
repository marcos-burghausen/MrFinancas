# [ <- VOLTAR](../../README.md)

# Caso de Uso UML: Gerar Relatório

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +GerarRelatório()
    }
    Usuário --> Sistema : interage
```

## Descrição
Permite que o usuário gere relatórios financeiros (gráficos, balanços).

## Atores
- **Primário**: Usuário

## Pré-condições
- Usuário está autenticado.
- Existem lançamentos cadastrados.

## Pós-condições
- Relatório é exibido (gráfico ou tabela).
- Relatório pode ser exportado (PDF/CSV).

## Fluxo Principal
1. Usuário acessa tela de relatórios.
2. Seleciona tipo (gráfico, balanço mensal).
3. Aplica filtros (data, categoria, conta).
4. Sistema gera e exibe relatório.
5. Usuário pode exportar em PDF/CSV.

## Fluxos Alternativos
- **A1**: Nenhum dado para o filtro → Exibe mensagem "Sem dados".
- **A2**: Erro na exportação → Exibe erro e oferece retry.

## Regras de Negócio
- Relatórios cobrem até 1 ano de dados.
- Gráficos incluem despesas/receitas por categoria.
- Exportações mantêm formato consistente (CSV estruturado, PDF formatado).

## Integrações
- Usa dados de lançamentos, contas e cartões.
- Integra com biblioteca de gráficos (ex.: Chart.js).
- Exportação usa biblioteca PDF (ex.: jsPDF).