# Caso de Uso UML: Gerenciar Conta

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +GerenciarConta()
    }
    Usuário --> Sistema : interage
```

## Descrição
Permite que o usuário crie, edite ou exclua contas bancárias.

## Atores
- **Primário**: Usuário

## Pré-condições
- Usuário está autenticado.

## Pós-condições
- Conta é criada/editada/excluída.
- Lista de contas é atualizada.

## Fluxo Principal
1. Usuário acessa tela de contas.
2. Escolhe ação:
   - **Criar**: Preenche formulário (nome, categoria, ícone, cor), valida e salva.
   - **Editar**: Seleciona conta, atualiza formulário, valida e salva.
   - **Excluir**: Seleciona conta, confirma exclusão.
3. Sistema verifica vínculos (lançamentos/cartões):
   - Se vinculada, exibe erro.
   - Se não vinculada, remove.
4. Retorna à lista.

## Fluxos Alternativos
- **A1**: Dados inválidos → Exibe erro e retorna ao formulário.
- **A2**: Conta vinculada → Exibe erro e impede exclusão.

## Regras de Negócio
- Nomes de contas são únicos por usuário.
- Contas vinculadas não podem ser excluídas.
- Ícones e cores são personalizáveis.

## Integrações
- Contas aparecem em lançamentos e cartões.
- Atualiza saldos e relatórios.
- Suporta conciliação bancária.