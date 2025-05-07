# Guia de Contribuição para o MrFinancas

Obrigado pelo seu interesse em contribuir para o MrFinancas! Este documento fornece diretrizes e instruções para ajudar você a contribuir com o projeto.

## Código de Conduta

Ao participar deste projeto, você concorda em manter um ambiente respeitoso e colaborativo para todos os colaboradores.

## Como Posso Contribuir?

### Reportando Bugs

Se você encontrou um bug, por favor, abra uma issue no GitHub com as seguintes informações:

- Título claro e descritivo
- Passos detalhados para reproduzir o problema
- Comportamento esperado e comportamento atual
- Screenshots (se aplicável)
- Informações sobre seu ambiente (navegador, sistema operacional, etc.)

### Sugerindo Melhorias

Para sugerir melhorias ou novas funcionalidades:

1. Verifique se sua ideia já foi sugerida nas issues existentes
2. Abra uma nova issue descrevendo sua sugestão em detalhes
3. Explique por que essa melhoria seria útil para o projeto

### Pull Requests

1. Fork o repositório e crie seu branch a partir da `main`
2. Se você adicionou código que deve ser testado, adicione testes
3. Se necessário, atualize a documentação
4. Certifique-se de que todos os testes passam
5. Verifique se seu código segue os padrões de estilo do projeto
6. Envie seu pull request!

## Configuração do Ambiente de Desenvolvimento

### Pré-requisitos

- [Node.js](https://nodejs.org/) versão X.X ou superior
- [NPM](https://www.npmjs.com/) ou [Yarn](https://yarnpkg.com/)
- [Banco de dados XYZ]

### Configuração Inicial

1. Clone o repositório:
```bash
git clone https://github.com/SEU_USUARIO/MrFinancas.git
cd MrFinancas
```

2. Instale as dependências:
```bash
npm install
# ou
yarn install
```

3. Configure as variáveis de ambiente:
```bash
cp .env.example .env
# Edite o arquivo .env com suas configurações
```

4. Execute as migrações do banco de dados:
```bash
npm run migrate
# ou
yarn migrate
```

5. Inicie o servidor de desenvolvimento:
```bash
npm run dev
# ou
yarn dev
```

## Estrutura do Projeto

```
MrFinancas/
├── src/
│   ├── components/   # Componentes reutilizáveis
│   ├── pages/        # Páginas da aplicação
│   ├── services/     # Serviços e APIs
│   └── utils/        # Funções utilitárias
├── public/           # Arquivos públicos
├── assets/           # Recursos estáticos
├── tests/            # Testes automatizados
└── docs/             # Documentação
```

## Diretrizes de Estilo

### Estilo de Código

- Use [Prettier](https://prettier.io/) para formatação
- Siga o [ESLint](https://eslint.org/) configurado para o projeto
- Nomeie variáveis e funções de forma descritiva em camelCase
- Comente código complexo ou não óbvio

### Commits

- Use mensagens de commit claras e descritivas
- Siga o padrão [Conventional Commits](https://www.conventionalcommits.org/)
- Exemplos:
  - `feat: adiciona função de exportação de relatórios`
  - `fix: corrige cálculo incorreto no resumo mensal`
  - `docs: atualiza documentação da API`

### Documentação

- Atualize a documentação quando necessário
- Documente novas funcionalidades
- Mantenha o README atualizado

## Processo de Review

- Todos os pull requests devem ter pelo menos uma revisão antes de serem mesclados
- Os revisores devem fornecer feedback construtivo
- As correções solicitadas devem ser endereçadas antes da mesclagem

## Testes

- Adicione testes para novas funcionalidades
- Certifique-se de que todos os testes passam antes de enviar um pull request
- Execute os testes:
```bash
npm test
# ou
yarn test
```

## Recursos Adicionais

- [Link para documentação da API]
- [Link para guia de estilo detalhado]
- [Link para informações do projeto]

## Dúvidas?

Se você tiver dúvidas ou precisar de ajuda, entre em contato conosco:

- Abra uma issue no GitHub
- Envie um e-mail para [endereço de e-mail do projeto]

Agradecemos muito sua contribuição para tornar o MrFinancas ainda melhor!
