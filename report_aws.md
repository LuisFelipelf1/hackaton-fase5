# Relatório de Modelagem de Ameaças (STRIDE) — Arquitetura 1 (AWS) — SEI/SIP

_Gerado em: 2026-02-23 18:29_

## 1. Visão geral
Entrada: diagrama (imagem) → JSON → STRIDE por regras determinísticas (Python).

## 2. Componentes detectados
- **Usuários SEI** — `user`
- **AWS Shield** — `ddos_protection` (provider=AWS)
- **Amazon CloudFront** — `cdn` (provider=AWS)
- **AWS WAF** — `waf` (provider=AWS)
- **VPC** — `network_boundary` (provider=AWS, zone=sa-east-1 (São Paulo))
- **Public Subnet A** — `subnet_public` (provider=AWS, zone=Availability Zone A)
- **Public Subnet B** — `subnet_public` (provider=AWS, zone=Availability Zone B)
- **Public Subnet C** — `subnet_public` (provider=AWS, zone=Availability Zone C)
- **Application Load Balancer A** — `load_balancer` (provider=AWS, zone=Availability Zone A)
- **Application Load Balancer B** — `load_balancer` (provider=AWS, zone=Availability Zone B)
- **Application Load Balancer C** — `load_balancer` (provider=AWS, zone=Availability Zone C)
- **SEI/SIP App Server A** — `app_server` (provider=AWS, zone=Availability Zone A)
- **SEI/SIP App Server B** — `app_server` (provider=AWS, zone=Availability Zone B)
- **SEI/SIP App Server C** — `app_server` (provider=AWS, zone=Availability Zone C)
- **Solr** — `app_server` (provider=AWS, zone=Availability Zone C)
- **Amazon RDS (Primary)** — `database` (provider=AWS, zone=Multi-AZ)
- **Amazon RDS (Secondary)** — `database` (provider=AWS, zone=Multi-AZ)
- **Amazon ElastiCache (Memcached)** — `cache` (provider=AWS, zone=Multi-AZ)
- **Amazon Elastic File System (NFS)** — `filesystem` (provider=AWS, zone=Multi-AZ)
- **AWS CloudTrail** — `logging_audit` (provider=AWS)
- **AWS Key Management Service** — `kms` (provider=AWS)
- **AWS Backup** — `backup` (provider=AWS)
- **Amazon CloudWatch** — `monitoring` (provider=AWS)
- **Amazon Simple Email Service (SES)** — `email_service` (provider=AWS)

## 3. Fluxos identificados
- `f1`: **c1 → c2** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f2`: **c2 → c3** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f3`: **c3 → c4** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f4`: **c4 → c9** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f5`: **c9 → c12** | protocolo: HTTPS | fronteira: Subnet Pública ↔ Subnet Privada
- `f6`: **c10 → c13** | protocolo: HTTPS | fronteira: Subnet Pública ↔ Subnet Privada
- `f7`: **c11 → c14** | protocolo: HTTPS | fronteira: Subnet Pública ↔ Subnet Privada
- `f8`: **c12 → c16** | protocolo: TCP | fronteira: Camada Aplicação ↔ Camada Dados
- `f9`: **c13 → c16** | protocolo: TCP | fronteira: Camada Aplicação ↔ Camada Dados
- `f10`: **c14 → c16** | protocolo: TCP | fronteira: Camada Aplicação ↔ Camada Dados
- `f11`: **c12 → c18** | protocolo: TCP | fronteira: Camada Aplicação ↔ Camada Dados
- `f12`: **c12 → c19** | protocolo: NFS | fronteira: Camada Aplicação ↔ Camada Dados
- `f13`: **c12 → c15** | protocolo: TCP | fronteira: Camada Aplicação ↔ Camada Dados
- `f14`: **c12 → c24** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f15`: **c12 → c23** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f16`: **c12 → c20** | protocolo: HTTPS | fronteira: Internet ↔ AWS
- `f17`: **c16 → c22** | protocolo: TCP | fronteira: Internet ↔ AWS
- `f18`: **c12 → c21** | protocolo: HTTPS | fronteira: Internet ↔ AWS

## 4. Ameaças por componente (STRIDE)

### Usuários SEI (`user`)
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### AWS Shield (`ddos_protection`)
- **Denial of Service (D)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS

### Amazon CloudFront (`cdn`)
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS

### AWS WAF (`waf`)
- **Denial of Service (D)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração

### VPC (`network_boundary`)
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

### Public Subnet A (`subnet_public`)
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração

### Public Subnet B (`subnet_public`)
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração

### Public Subnet C (`subnet_public`)
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração

### Application Load Balancer A (`load_balancer`)
- **Denial of Service (D)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Spoofing (S)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)

### Application Load Balancer B (`load_balancer`)
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)

### Application Load Balancer C (`load_balancer`)
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Spoofing (S)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)

### SEI/SIP App Server A (`app_server`)
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

### SEI/SIP App Server B (`app_server`)
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

### SEI/SIP App Server C (`app_server`)
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

### Solr (`app_server`)
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

### Amazon RDS (Primary) (`database`)
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
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Amazon RDS (Secondary) (`database`)
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
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Amazon ElastiCache (Memcached) (`cache`)
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Denial of Service (D)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Rate limiting, throttling, circuit breaker
  - Mitigação: Auto scaling e proteção anti-DDoS

### Amazon Elastic File System (NFS) (`filesystem`)
- **Tampering (T)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **BAIXO** (score 3/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Repudiation (R)** — Risco: **BAIXO** (score 2/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### AWS CloudTrail (`logging_audit`)
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### AWS Key Management Service (`kms`)
- **Tampering (T)** — Risco: **MÉDIO** (score 7/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 7/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Spoofing (S)** — Risco: **MÉDIO** (score 6/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)

### AWS Backup (`backup`)
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Amazon CloudWatch (`monitoring`)
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

### Amazon Simple Email Service (SES) (`email_service`)
- **Tampering (T)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: Validação de entrada (schema validation) e sanitização
  - Mitigação: Princípio do menor privilégio para escrita/alteração
- **Information Disclosure (I)** — Risco: **MÉDIO** (score 5/10)
  - Mitigação: TLS obrigatório em trânsito e criptografia em repouso (KMS)
  - Mitigação: RBAC/ABAC + segmentação de rede
- **Spoofing (S)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Autenticação forte (OIDC/OAuth2), MFA quando aplicável
  - Mitigação: mTLS entre serviços internos (quando possível)
- **Repudiation (R)** — Risco: **BAIXO** (score 4/10)
  - Mitigação: Logs imutáveis e trilhas de auditoria com correlação de IDs
  - Mitigação: Monitoramento e alertas para ações sensíveis

## 5. Assumptions / Limitações
- Application Load Balancers estão em subnets públicas.
- Servidores SEI/SIP e Solr estão em subnets privadas.
- RDS está configurado em Multi-AZ com primário e secundário.
- Protocolos internos não especificados foram assumidos como HTTPS para web e TCP para banco/cache.