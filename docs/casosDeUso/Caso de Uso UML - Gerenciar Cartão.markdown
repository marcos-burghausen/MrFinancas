# [ <- VOLTAR](../../README.md)

# Caso de Uso UML: Gerenciar Cartão

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +GerenciarCartão()
    }
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário crie, edite ou exclua cartões de crédito.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado.
- Existem contas cadastradas.

## Pós-condições

- Cartão é criado/editado/excluído.
- Lista de cartões é atualizada.

## Fluxo Principal

1. Usuário acessa tela de cartões.
2. Escolhe ação:
   - **Criar**: Preenche formulário (nome, limite, conta, ícone, cor), valida e salva.
   - **Editar**: Seleciona cartão, atualiza formulário, valida e salva.
   - **Excluir**: Seleciona cartão, confirma exclusão.
3. Sistema verifica lançamentos:
   - Se vinculado, exibe erro.
   - Se não vinculado, remove.
4. Retorna à lista.

## Fluxos Alternativos

- **A1**: Dados inválidos → Exibe erro e retorna ao formulário.
- **A2**: Cartão vinculado → Exibe erro e impede exclusão.

## Regras de Negócio

- Cartões exigem conta vinculada.
- Nomes são únicos por usuário.
- Limites são positivos.
- Sistema alerta se uso excede 80% do limite.

## Integrações

- Cartões aparecem em lanç- Integra com faturas e notificações.
- Atualiza relatórios.
- Pagamentos de faturas são registrados como transferências.
