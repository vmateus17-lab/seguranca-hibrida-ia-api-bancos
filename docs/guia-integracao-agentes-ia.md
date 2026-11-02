# Guia de Integração para Agentes e IDEs com IA

Este documento explica como conectar e utilizar o arquivo mestre [`seguranca-hibrida-projetos-ia-api-bancos.md`](../seguranca-hibrida-projetos-ia-api-bancos.md) com as principais ferramentas de desenvolvimento assistido por IA e agentes autônomos.

---

## 1. Cursor IDE

No Cursor, você pode carregar o framework como uma **Regra de Projeto** (*Project Rule*) persistente:

1. Crie ou edite o arquivo `.cursorrules` na raiz do seu projeto.
2. Adicione a seguinte instrução:

```markdown
# Segurança do Projeto
Você atua como auditor e implementador de segurança seguindo rigorosamente as instruções do framework em:
`./seguranca-hibrida-projetos-ia-api-bancos.md`.

Antes de finalizar qualquer tarefa que envolva APIs, banco de dados, autenticação ou chamadas a modelos de IA:
1. Verifique os controles aplicáveis (APP01-APP20, AI01-AI10, DB01-DB08, etc.).
2. Valide parâmetros no servidor.
3. Não insira dados confidenciais em logs ou prompts.
4. Escreva testes negativos comprovando a segurança.
```

---

## 2. Windsurf (Cascade) / Claude Code

Adicione ao arquivo de contexto de memória do projeto (`.windsurfrules` ou `CLAUDE.md`):

```markdown
## Protocolo de Segurança e Auditoria
Consulte e aplique os 50 controles descritos em `seguranca-hibrida-projetos-ia-api-bancos.md`.
- Toda nova rota de API deve validar BOLA (APP03) e schemas de entrada (APP11).
- Toda query SQL deve usar prepared statements com allowlist para ordenações dinâmicas (DB05).
- Agentes de IA e chamadas de ferramentas devem seguir o isolamento estrito de tenants e HITL (AI01-AI03).
```

---

## 3. GitHub Copilot Chat e Chatbots de IDE

No início de uma sessão de revisão de código ou auditoria, referencie o arquivo diretamente usando `@workspace`:

```text
@workspace /explain Avalie os arquivos modificados nesta branch contra os controles de segurança do arquivo seguranca-hibrida-projetos-ia-api-bancos.md e liste eventuais violações de BOLA, injeção de prompt ou grants excessivos de banco de dados.
```

---

## 4. Agentes em Pipelines CI/CD (Roo Code, DevIn, Agentes Customizados)

Em jobs automatizados de CI/CD, você pode alimentar o prompt do agente com o documento mestre como instrução do sistema (*System Instruction*), solicitando que o agente comente no Pull Request:
- Uma tabela com os controles impactados pela PR;
- Comprovação de que não há regressões de segurança;
- Testes unitários de abuso cobrindo os caminhos alterados.
