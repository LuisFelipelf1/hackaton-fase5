# Relatório de Modelagem de Ameaças (STRIDE) — Arquitetura 2 - Azure API Management e Logic Apps

_Gerado em: 2026-02-23 18:29_

## 1. Visão geral
Entrada: diagrama (imagem) → JSON → STRIDE por regras determinísticas (Python).

## 2. Componentes detectados
- **Usuario Externo** — `user` (zone=Internet)
- **Microsoft Entra ID** — `identity_provider` (provider=Microsoft, zone=Azure AD)
- **API Gateway** — `api_gateway` (provider=Microsoft Azure, zone=Resource Group)
- **Developer Portal** — `web_app` (provider=Microsoft Azure, zone=Resource Group)
- **Logic Apps** — `function` (provider=Microsoft Azure, zone=Resource Group)
- **Azure Services** — `cloud_service` (provider=Microsoft Azure, zone=Backend Systems)
- **SaaS Services** — `saas` (zone=Backend Systems)
- **REST Web Services** — `rest_service` (zone=Backend Systems)
- **SOAP Web Services** — `soap_service` (zone=Backend Systems)

## 3. Fluxos identificados
- `f1`: **c1 → c3** | protocolo: HTTPS | fronteira: Internet
- `f2`: **c1 → c2** | protocolo: HTTPS | fronteira: Internet
- `f3`: **c3 → c5** | protocolo: HTTPS | fronteira: Azure Resource Group
- `f4`: **c5 → c6** | protocolo: HTTPS | fronteira: Backend Systems
- `f5`: **c5 → c7** | protocolo: HTTPS | fronteira: Backend Systems
- `f6`: **c5 → c8** | protocolo: HTTPS | fronteira: Backend Systems
- `f7`: **c5 → c9** | protocolo: HTTPS | fronteira: Backend Systems
- `f8`: **c3 → c4** | protocolo: HTTPS | fronteira: Azure Resource Group

## 4. Ameaças por componente (STRIDE)

### Usuario Externo (`user`)
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Microsoft Entra ID (`identity_provider`)
- **Elevation of Privilege (E)** — Risco: **ALTO** (score 8/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **MÉDIO** (score 7/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 7/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **MÉDIO** (score 7/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **MÉDIO** (score 6/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **MÉDIO** (score 6/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### API Gateway (`api_gateway`)
- **Elevation of Privilege (E)** — Risco: **MÉDIO** (score 6/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Developer Portal (`web_app`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Logic Apps (`function`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Azure Services (`cloud_service`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### SaaS Services (`saas`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### REST Web Services (`rest_service`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### SOAP Web Services (`soap_service`)
- **Elevation of Privilege (E)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: RBAC/least privilege + separação de funções (SoD)
  - Mitigação: Revisão de permissões administrativas e hardening
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

## 5. Assumptions / Limitações
- API Gateway representa o Azure API Management.
- Logic Apps executa a orquestracao e integracao com sistemas backend.
- O Resource Group delimita a fronteira logica dos servicos Azure mostrados no diagrama.
- Protocolos foram padronizados como HTTPS para trafego web e integracoes.