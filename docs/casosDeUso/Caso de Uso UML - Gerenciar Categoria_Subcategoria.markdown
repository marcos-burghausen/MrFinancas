# Caso de Uso UML: Gerenciar Categoria/Subcategoria

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +GerenciarCategoria()
    }
    Usuário --> Sistema : interage
```

## Descrição
Permite que o usuário crie, edite ou exclua categorias e subcategorias.

## Atores
- **Primário**: Usuário

## Pré-condições
- Usuário está autenticado.

## Pós-condições
- Categoria/subcategoria é criada/editada/excluída.
- Lista de categorias é atualizada.

## Fluxo Principal
1. Usuário acessa tela de categorias.
2. Escolhe ação:
   - **Criar**: Preenche formulário (nome, ícone, cor, tipo), valida e salva.
   - **Editar**: Seleciona categoria, atualiza formulário, valida e salva.
   - **Excluir**: Seleciona categoria, confirma exclusão.
3. Sistema verifica lançamentos:
   - Se vinculada, exibe erro.
   - Se não vinculada, remove.
4. Retorna à lista.

## Fluxos Alternativos
- **A1**: Dados inválidos → Exibe erro e retorna ao formulário.
- **A2**: Categoria vinculada → Exibe erro e impede exclusão.

## Regras de Negócio
- Nomes são únicos por usuário.
- Subcategorias exigem categoria pai.
- Suporta hierarquia de múltiplos níveis.
- Oferece categorias predefinidas.

## Integrações
- Categorias aparecem em lançamentos.
- Integra com relatórios.
- Predefinidas são importadas na inicialização.