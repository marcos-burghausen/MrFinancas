🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso UML: Realizar Backup/Restauração

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +RealizarBackup()
        +RealizarRestauração()
    }
    class SistemaExternoBackup {
        +ArmazenarArquivo()
    }
    Usuário --> SistemaExternoBackup : usa
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário realize backup ou restauração de dados (contas, lançamentos, categorias).

## Atores

- **Primário**: Usuário
- **Secundário**: Sistema Externo de Backup (ex.: Google Drive)

## Pré-condições

- Usuário está autenticado.
- Existem dados para backup.

## Pós-condições

- Backup é gerado e baixado.
- Dados são restaurados no banco.

## Fluxo Principal

1. Usuário acessa tela de backup.
2. Escolhe ação:
   - **Backup**: Seleciona dados, gera CSV/JSON, inicia download.
   - **Restauração**: Faz upload de arquivo, valida e importa dados.
3. Exibe confirmação.

## Fluxos Alternativos

- **A1**: Arquivo inválido → Exibe erro e retorna ao upload.
- **A2**: Erro no download → Exibe erro e oferece retry.

## Regras de Negócio

- Backup inclui apenas dados do usuário.
- Restauração sobrescreve dados (após confirmação).
- Arquivos são criptografados.

## Integrações

- Integra com contas, lançamentos, categorias.
- Suporta armazenamento em nuvem.
- Atualiza saldos e relatórios após restauração.
