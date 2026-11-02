# Guia de Contribuição

Obrigado pelo interesse em contribuir com o **Framework de Segurança Híbrida**!

Este projeto é uma iniciativa comunitária aberta para estabelecer o padrão de segurança operacional definitivo em arquiteturas que combinam aplicações web/mobile, APIs, bancos de dados, infraestrutura moderna e agentes de IA.

---

## Como Você Pode Ajudar

1. **Novos Controles de Segurança:** Identificou uma nova técnica de ataque ou vulnerabilidade emergente (ex: novo vetor de ataque contra agentes, MCP ou bancos vetoriais)? Proponha um novo controle ou refinamento.
2. **Correções e Refinamentos:** Encontrou imprecisões técnicas, exemplos desatualizados ou erros de digitação? Envie um Pull Request.
3. **Casos de Teste e Automações:** Adicione novos scripts de teste, templates de CI/CD ou exemplos de mitigação em diferentes linguagens e frameworks.
4. **Traduções e Documentação:** Ajude a expandir guias práticos e documentações complementares.

---

## Processo de Contribuição

1. **Faça um Fork** do repositório.
2. **Crie uma Branch** temática para a sua alteração:
   ```bash
   git checkout -b feature/novo-controle-seguranca
   ```
3. **Faça as Alterações:** Mantenha o formato conciso, imperativo, prático e tecnicamente fundamentado adotado no documento mestre.
4. **Teste e Valide:** Certifique-se de que links markdown e tabelas estão bem formatados.
5. **Faça Commit das Alterações:**
   ```bash
   git commit -m "feat(ai): adiciona mitigação avançada para indirect prompt injection em PDFs"
   ```
6. **Abra um Pull Request** detalhando as motivações técnicas e as referências oficiais (OWASP, NIST, CVEs, etc.).

---

## Padrões de Estilo para Controles

Ao propor ou editar um controle, siga rigorosamente a estrutura tripartida padrão:
- **Inspecionar:** O que exatamente o auditor deve buscar no código, na rede ou nas configurações.
- **Corrigir:** A regra de ouro arquitetural e a mitigação concreta recomendada no servidor, sem paliativos.
- **Verificar:** Como testar o abuso e como comprovar a neutralização da falha com testes automatizados e fluxo legítimo.
