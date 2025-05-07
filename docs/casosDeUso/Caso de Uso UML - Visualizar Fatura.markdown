# [ <- VOLTAR](../../README.md)

# Caso de Uso UML: Visualizar Fatura

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +VisualizarFatura()
    }
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário visualize faturas de cartões de crédito.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado.
- Existem cartões com lançamentos.

## Pós-condições

- Fatura é exibida (gastos, vencimento, valor total).

## Fluxo Principal

1. Usuário acessa tela de cartões.
2. Seleciona cartão e clica em "Visualizar fatura".
3. Sistema exibe fatura (gastos, vencimento, valor total).
4. Retorna à lista.

## Fluxos Alternativos

- **A1**: Nenhuma fatura disponível → Exibe mensagem "Sem fatura".

## Regras de Negócio

- Faturas são geradas com base no ciclo do cartão.
- Incluem todos os lançamentos do período.
- Valor total é calculado automaticamente.

## Integrações

- Usa dados de lançamentos.
- Integra com notificações de vencimento.
- Faturas são exportáveis em PDF.
