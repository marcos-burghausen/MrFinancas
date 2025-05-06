# [ <- VOLTAR](../../README.md)

# Fluxograma de Cadastro de Usuário

Este documento descreve o processo completo de cadastro de usuário no sistema, incluindo fluxos de autenticação OAuth e cadastro tradicional.

## Diagrama de Fluxo

```mermaid
flowchart TD
   A["Início"] --> B["Usuário acessa tela de cadastro"]
   B --> C["Usuário escolhe método de cadastro"]
   C --> D{"Método é OAuth?"}
   D -->|Sim| E["Redireciona para provedor OAuth (Facebook/Google/LinkedIn)"]
   E --> F["Provedor autentica usuário"]
   F -->|Autenticação bem-sucedida?| G{"Sim"}
   G --> H["Cria/atualiza usuário no sistema com dados do OAuth"]
   H --> I["Envia e-mail de boas-vindas"]
   I --> J["Redireciona para dashboard"]
   J --> K["Fim"]
   G -->|Não| L["Exibe erro de autenticação"]
   L --> M["Retorna à tela de cadastro"]
   D -->|Não| N["Usuário preenche formulário (e-mail, senha, nome)"]
   N --> O["Valida formulário"]
   O -->|Dados válidos?| P{"Sim"}
   P --> Q["Cria usuário no banco com status 'não verificado'"]
   Q --> R["Envia e-mail de verificação"]
   R --> S["Exibe mensagem: 'Verifique seu e-mail'"]
   S --> T["Usuário clica no link de verificação"]
   T -->|Link válido?| U{"Sim"}
   U --> V["Marca usuário como verificado"]
   V --> W["Redireciona para login"]
   W --> K
   U -->|Não| X["Exibe erro: 'Link inválido'"]
   X --> K
   P -->|Não| Y["Exibe erros de validação"]
   Y --> Z["Retorna ao formulário"]
```

## Descrição do Processo

### Fluxo OAuth

1. Usuário escolhe entrar com provedor OAuth (Facebook/Google/LinkedIn)
2. Sistema redireciona para a página de autenticação do provedor
3. Após autenticação bem-sucedida, o sistema:
   - Cria uma nova conta se o usuário não existir
   - Atualiza informações se o usuário já existir
4. Envia e-mail de boas-vindas
5. Redireciona para o dashboard

### Fluxo Tradicional

1. Usuário preenche formulário com e-mail, senha e nome
2. Sistema valida os dados:
   - Se inválidos: exibe erros e retorna ao formulário
   - Se válidos: cria usuário com status "não verificado"
3. Sistema envia e-mail de verificação
4. Usuário clica no link de verificação:
   - Se válido: marca usuário como verificado e redireciona para login
   - Se inválido: exibe mensagem de erro

## Notas Adicionais

- Todos os e-mails são enviados de forma assíncrona usando filas
- Links de verificação expiram após 24 horas
- OAuth não requer verificação adicional de e-mail
