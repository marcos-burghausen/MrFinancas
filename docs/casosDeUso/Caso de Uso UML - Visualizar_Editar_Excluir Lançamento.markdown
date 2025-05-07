🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso UML: Visualizar/Editar/Excluir Lançamento

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +VisualizarLançamento()
        +EditarLançamento()
        +ExcluirLançamento()
    }
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário visualize, edite ou exclua lançamentos financeiros.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado.
- Existem lançamentos cadastrados.

## Pós-condições

- Lista de lançamentos é exibida.
- Lançamento é editado/excluído, com saldos atualizados.

## Fluxo Principal

1. Usuário acessa tela de lançamentos.
2. Aplica filtros (data, categoria, conta).
3. Visualiza lista filtrada.
4. Seleciona lançamento:
   - **Editar**: Atualiza formulário, valida e salva.
   - **Excluir**: Confirma exclusão, remove lançamento e atualiza saldos.
5. Retorna à lista.

## Fluxos Alternativos

- **A1**: Nenhum lançamento encontrado → Exibe mensagem "Sem resultados".
- **A2**: Dados inválidos na edição → Exibe erro e retorna ao formulário.

## Regras de Negócio

- Exclusão de lançamentos recorrentes remove instâncias futuras.
- Edição atualiza saldos e notificações.
- Filtros suportam até 1 ano de dados.

## Integrações

- Atualiza saldos de contas/cartões.
- Integra com relatórios e faturas.
- Exclusão remove notificações associadas.
