# Relatório de Auditoria de Segurança Técnica

> **Projeto:** `[Nome do Projeto / Repositório]`  
> **Auditor / Responsável:** `[Nome do Auditor ou Agente de IA]`  
> **Data:** `[DD/MM/AAAA - HH:MM]`  
> **Commit Auditado:** `[Hash do Git inicial / final]`  
> **Ambiente:** `[ ] Local  [ ] Homologação / Staging  [ ] Produção`

---

## 1. Resumo Executivo

- **Total de Controles Avaliados:** `50 / 50`
- **Controles em Conformidade (Verificados):** `[X]`
- **Vulnerabilidades Confirmadas:** `[X]`
  - Críticas: `[X]`
  - Altas: `[X]`
  - Médias: `[X]`
  - Baixas: `[X]`
- **Correções Implementadas no Código:** `[X]`
- **Pendências Operacionais / Externas:** `[X]`

---

## 2. Tabela de Cobertura dos 50 Controles

> Estados aceitos: `verificado`, `falha_confirmada`, `suspeita`, `nao_aplicavel`, `bloqueado`.

| Controle | Domínio | Estado | Evidência Sanitizada / Referência de Código |
| :--- | :--- | :---: | :--- |
| **APP01** | Autenticação e Reset de Senha | `verificado` | Implementado com Argon2id 64MB em `auth.service.ts` |
| **APP02** | Sessão, JWT e OAuth 2.1 | `verificado` | PKCE S256 obrigatório e validação de claims em `jwt.ts` |
| **APP03** | BOLA / IDOR (OWASP API1) | `falha_confirmada` | Ver achado **SEC-001** |
| ... | *(demais controles de APP04 a GOV03)* | ... | ... |

---

## 3. Detalhamento dos Achados de Segurança

### SEC-001 — [Título do Achado / Ex: Broken Object Level Authorization em Pedidos]

- **Controle Afetado:** `APP03` (BOLA / IDOR)
- **Severidade:** `Alta`
- **Arquivo / Linha:** `src/controllers/order.controller.ts:45-52`
- **Status:** `corrigido_e_validado`

#### Descrição da Vulnerabilidade
O endpoint `GET /api/v1/orders/:id` consultava o pedido no banco de dados utilizando apenas o identificador numérico enviado na rota, sem validar se o pedido pertencia ao `tenant_id` ou `user_id` do usuário autenticado na sessão. Um usuário comum podia consultar pedidos de qualquer cliente da plataforma alterando sequencialmente o ID.

#### Evidência Sanitizada (Código Vulnerável)
```typescript
// CÓDIGO ANTERIOR:
export async function getOrder(req: Request, res: Response) {
    const order = await db.query('SELECT * FROM orders WHERE id = $1', [req.params.id]);
    return res.json(order);
}
```

#### Remediação Implementada
```typescript
// CÓDIGO CORRIGIDO:
export async function getOrder(req: Request, res: Response) {
    const tenantId = req.user.tenantId; // Extraído do token JWT autenticado
    const order = await db.query(
        'SELECT * FROM orders WHERE id = $1 AND tenant_id = $2',
        [req.params.id, tenantId]
    );
    if (!order.rows[0]) {
        return res.status(404).json({ error: 'Pedido não encontrado' });
    }
    return res.json(order.rows[0]);
}
```

#### Testes de Validação Realizados
- **Teste Negativo (Adversarial):** Usuário A (tenant `111`) tenta acessar `GET /api/v1/orders/50` (pertencente ao tenant `222`). Resultado observado: resposta `404 Not Found`.
- **Teste Positivo (Legítimo):** Usuário B (tenant `222`) acessa `GET /api/v1/orders/50`. Resultado: `200 OK` com os dados do pedido retornados corretamente.

---

## 4. Plano de Ação e Pendências Externas

| ID | Ação Necessária | Responsável | Prazo Recomendado |
| :--- | :--- | :--- | :--- |
| **ACT-01** | Rotacionar chave de API do provedor de IA no painel da nuvem | Equipe de DevOps | Imediato (< 24h) |
| **ACT-02** | Aplicar migration de RLS (`FORCE ROW LEVEL SECURITY`) no banco de produção | DBA / Tech Lead | Próxima janela de release |

---

## 5. Parecer de Release

- [ ] **Aprovado para Produção:** Todos os controles críticos e altos foram mitigados e validados.
- [ ] **Rejeitado / Bloqueado:** Existem vulnerabilidades críticas ou altas abertas no caminho de produção.
