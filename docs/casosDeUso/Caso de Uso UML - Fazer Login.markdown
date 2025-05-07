# [ <- VOLTAR](../../README.md)

# Caso de Uso UML: Fazer Login

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +FazerLogin()
    }
    class ProvedorOAuth {
        +Autenticar()
    }
    Usuário --> ProvedorOAuth : usa
    Usuário --> Sistema : interage
```

## Descrição
Permite que o usuário acesse o sistema via e-mail/senha ou OAuth, com suporte a MFA e recuperação de senha.

## Atores
- **Primário**: Usuário
- **Secundário**: Provedor OAuth

## Pré-condições
- Usuário possui conta verificada (tradicional) ou registrada (OAuth).
- Provedores OAuth estão configurados.

## Pós-condições
- Usuário recebe token de acesso e é redirecionado ao dashboard.
- Senha é redefinida (se recuperação solicitada).

## Fluxo Principal
1. Usuário acessa a tela de login.
2. Usuário escolhe método:
   - **Tradicional**:
     - Insere e-mail e senha.
     - Sistema valida credenciais.
     - Se MFA ativado, solicita código (SMS/app autenticador).
     - Gera token e redireciona para dashboard.
   - **OAuth**:
     - Redireciona para provedor OAuth.
     - Provedor autentica e retorna dados.
     - Sistema valida usuário, gera token e redireciona para dashboard.
3. Se credenciais inválidas, oferece recuperação de senha.

## Fluxos Alternativos
- **A1**: Credenciais inválidas → Exibe erro e limita tentativas (máximo 5).
- **A2**: Código MFA inválido → Exibe erro e solicita novo código.
- **A3**: Recuperação de senha → Envia link, usuário redefine senha e retorna ao login.
- **A4**: Falha OAuth → Exibe erro e retorna à tela de login.

## Regras de Negócio
- Usuários não verificados não podem logar (tradicional).
- MFA é opcional.
- Links de redefinição expiram em 1 hora.
- Tokens expiram em 24 horas.

## Integrações
- Usa **Laravel Sanctum** para tokens.
- Integra com Twilio/Google Authenticator para MFA.
- Usa **Laravel Socialite** para OAuth.
- Logins alimentam logs de auditoria.