# ✅ Checklist de Auditoria dos 50 Controles de Segurança

Utilize este checklist operacional para auditorias manuais ou integradas com agentes de IA. Marque `[x]` conforme cada controle for avaliado.

---

### 1. Aplicações e APIs (APP01–APP20)

- [ ] **APP01** — Autenticação robusta (Argon2id/bcrypt), proteção contra enumeração e reset seguro.
- [ ] **APP02** — Sessões e Tokens (OAuth 2.1 com PKCE, validação estrita de JWT/PASETO, cookies HttpOnly/Secure/SameSite).
- [ ] **APP03** — Autorização em Nível de Objeto (BOLA / IDOR — OWASP API1:2023) com validação de tenant no servidor.
- [ ] **APP04** — Autorização de Propriedades e Mass Assignment (OWASP API3:2023) com DTOs e allowlists de campos.
- [ ] **APP05** — Autorização em Nível de Função (BFLA — OWASP API5:2023) no backend.
- [ ] **APP06** — Prevenção de Injeções (SQL com prepared statements, comandos sem shell, NoSQL tipado).
- [ ] **APP07** — Cross-Site Scripting (XSS), sanitização de HTML/Markdown (DOMPurify) e CSP estrita.
- [ ] **APP08** — Proteção contra CSRF (SameSite + anti-CSRF tokens) e CORS sem reflexão arbitrária.
- [ ] **APP09** — Server-Side Request Forgery (SSRF — OWASP API7:2023) com bloqueio a IPs privados e AWS IMDS (`169.254.169.254`).
- [ ] **APP10** — Upload Seguro de Arquivos (validação por magic bytes, nomes aleatórios, prevenção de Zip Slip).
- [ ] **APP11** — Validação Estrita de Schemas e Contratos (Contract-First com Zod/Pydantic/OpenAPI).
- [ ] **APP12** — Consumo Irrestrito de Recursos e Rate Limiting (Token Bucket / Redis, status HTTP 429).
- [ ] **APP13** — Proteção de Fluxos de Negócio Sensíveis (anti-scalping, anti-hoarding, desafios de PoW/Turnstile).
- [ ] **APP14** — Concorrência, Race Conditions e Idempotência (transações com lock pessimista/otimista, `Idempotency-Key`).
- [ ] **APP15** — Gerenciamento de Inventário de APIs (desativação de shadow APIs, cabeçalhos Deprecation/Sunset).
- [ ] **APP16** — Consumo Inseguro de APIs Externas (validação de payloads de terceiros, timeouts curtos e validação HMAC).
- [ ] **APP17** — Hardening de GraphQL (desativação de Introspection, limitação de profundidade/complexidade, Trusted Queries).
- [ ] **APP18** — Headers HTTP de Segurança (HSTS preload, nosniff, frame-ancestors, TLS 1.3 obrigatório).
- [ ] **APP19** — Tratamento de Erros e Logs (ausência de stack traces em prod, Trace ID correlacionado, redação de PII).
- [ ] **APP20** — Criptografia em Trânsito Interno (mTLS entre microsserviços, tokens SPIFFE/SPIRE).

---

### 2. Inteligência Artificial e RAG (AI01–AI10)

- [ ] **AI01** — Injeção de Prompt Direta e Indireta (delimitadores com nonces, nós classificadores Llama Guard / NeMo).
- [ ] **AI02** — Divulgação de Informações Sensíveis e PII (sanitização pré-contexto, cláusula Zero Data Retention).
- [ ] **AI03** — Quebra da Tríade Letal (*Lethal Trifecta*) e aprovação humana HITL para ações de alto impacto.
- [ ] **AI04** — Segurança da Cadeia de Suprimentos de IA (uso de Safetensors, inventário AI-BOM CycloneDX/SPDX).
- [ ] **AI05** — Envenenamento de Dados, Modelos e RAG (*PoisonedRAG*), controle rigoroso de proveniência de dados.
- [ ] **AI06** — Consumo Ilimitado e Denial of Wallet (limites de RPM/TPM, circuit breakers em loops de agentes).
- [ ] **AI07** — Informação Falsa e Alucinação (contrato estrito de citações factuais no RAG, Chain-of-Verification - CoVe).
- [ ] **AI08** — Exposição de System Prompt (regras de segurança enforçadas fora do prompt, zero credenciais em prompts).
- [ ] **AI09** — Segurança em Vetores e Embeddings (prevenção de Vec2Text, CBAC com isolamento multi-tenant real).
- [ ] **AI10** — Tratamento Inseguro de Saídas de IA (saídas tratadas como dado 100% não confiável antes de renderizar/executar).

---

### 3. Bancos de Dados Relacionais, NoSQL e Vetoriais (DB01–DB08)

- [ ] **DB01** — Identidades de Conexão e Menor Privilégio (separação entre migrações DDL e runtime DML).
- [ ] **DB02** — PostgreSQL Row-Level Security (`FORCE ROW LEVEL SECURITY`, políticas restritivas com variáveis de sessão).
- [ ] **DB03** — Criptografia em Repouso (TDE), em Trânsito (TLS 1.3 com verify-full) e em Nível de Coluna (CLE).
- [ ] **DB04** — Connection Pooling Seguro (PgBouncer com reset de variáveis de sessão `DISCARD ALL` e SCRAM-SHA-256).
- [ ] **DB05** — Prevenção de SQL Injection (prepared statements em ORMs, allowlist estrita para ordenação dinâmica).
- [ ] **DB06** — Prevenção de NoSQL Injection (tipagem estrita de objetos, remoção de operadores MongoDB no middleware).
- [ ] **DB07** — Hardening de Bancos Vetoriais (portas privadas em VPC, autenticação mandatória, sem PII em metadados).
- [ ] **DB08** — Auditoria e Backups (pgAudit em tabelas sensíveis, Regra de Backup 3-2-1-1-0 com cópia WORM imutável).

---

### 4. Integração IA → API → Banco (INT01–INT03)

- [ ] **INT01** — Identidade do Cliente Preservada (backend autentica e injeta `user_id`/`tenant_id`, sem confiar em afirmações no chat).
- [ ] **INT02** — Ferramentas Atômicas e Queries Mediadas (schemas de ferramentas fechados, proibição de queries SQL livres).
- [ ] **INT03** — Isolamento de Conversas e Memória (segregação por tenant em memória de longo prazo e caches semânticos).

---

### 5. Infraestrutura, DevSecOps e Redes Zero Trust (INF01–INF06)

- [ ] **INF01** — Contêineres Distroless e Kubernetes (execução non-root, Pod Security Standards Restricted, eBPF Falco).
- [ ] **INF02** — Segurança de IaC (scans de Terraform/CloudFormation com Checkov/Trivy em pull requests).
- [ ] **INF03** — CI/CD Supply Chain e SLSA (pinning por SHA-256, assinatura com Cosign, runners efêmeros, OIDC).
- [ ] **INF04** — Gestão Centralizada de Segredos (HashiCorp Vault / AWS Secrets Manager com credenciais dinâmicas em `tmpfs`).
- [ ] **INF05** — Redes Zero Trust (subnets de 3 camadas, mTLS universal via Service Mesh, WAF as Code).
- [ ] **INF06** — Governança de Nuvem e CSPM (monitoramento contínuo de IAM e identidades não-humanas).

---

### 6. Hardening por Stack (STK01–STK04)

- [ ] **STK01** — WordPress e PHP (XML-RPC desativado, uploads sem execução de PHP, `wp-config.php` protegido).
- [ ] **STK02** — React / Next.js (Server Actions protegidas individualmente com autorização BOLA, sem segredos em SSR).
- [ ] **STK03** — Node.js / Python / Go (proteção contra Prototype Pollution, memory leaks e goroutine leaks).
- [ ] **STK04** — Model Context Protocol (MCP stdio isolado em sandbox, validação em servidores remotos, anotações não-confiáveis).

---

### 7. Governança e Compliance (GOV01–GOV03)

- [ ] **GOV01** — Conformidade LGPD (prazo ANPD de 3 dias úteis para incidentes) e GDPR (72h), ROPA e direito ao esquecimento.
- [ ] **GOV02** — Plano de Resposta a Incidentes para IA (playbooks alinhados a MITRE ATLAS e Kill Switch para agentes).
- [ ] **GOV03** — AI Red Teaming Contínuo (testes automatizados com Promptfoo/PyRIT e avaliação de viés algorítmico).
