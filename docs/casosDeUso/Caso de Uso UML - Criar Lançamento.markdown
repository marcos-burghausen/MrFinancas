🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso UML: Criar Lançamento

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +CriarLançamento()
    }
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário crie lançamentos financeiros (despesas, receitas, cartões, estornos), com suporte a recorrência.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado.
- Existem contas/cartões e categorias cadastradas.

## Pós-condições

- Lançamento é salvo no banco.
- Saldo da conta/cartão é atualizado.
- Notificação de vencimento é agendada (se aplicável).

## Fluxo Principal

1. Usuário acessa tela de lançamentos.
2. Seleciona tipo (Despesa, Receita, Cartão, Estorno).
3. Preenche formulário (valor, data, descrição, conta/cartão, categoria).
4. Sistema valida (valor positivo, conta/categoria válidos).
5. Se recorrente, configura frequência (diária, semanal, mensal).
6. Salva lançamento (único ou recorrente).
7. Atualiza saldo e exibe confirmação.

## Fluxos Alternativos

- **A1**: Dados inválidos → Exibe erro e retorna ao formulário.
- **A2**: Conta/cartão sem saldo suficiente → Exibe alerta, mas permite salvar.

## Regras de Negócio

- Valores são positivos (exceto estornos).
- Contas/cartões e categorias são obrigatórios.
- Lançamentos recorrentes geram instâncias futuras.
- Vencimentos são associados a notificações.

## Integrações

- Atualiza saldos de contas/cartões.
- Integra com notificações.
- Alimenta relatórios e faturas.
