
# 🛡️ Tech Challenge 5 – Modelagem de Ameaças com IA (STRIDE)

**Luís Felipe Alves – RM 363734**  
Pós-graduação FIAP – IA para Devs  

---

## 📌 1. Objetivo do Projeto

Desenvolver um MVP capaz de:

- Interpretar um diagrama de arquitetura de software;
- Estruturar seus componentes e fluxos em formato JSON;
- Aplicar automaticamente a metodologia **STRIDE**;
- Gerar relatório de modelagem de ameaças;
- Classificar riscos (BAIXO / MÉDIO / ALTO);
- Comparar duas arquiteturas (AWS e Azure).

---

## 🧠 2. Abordagem Adotada

### 🔹 Decisão Arquitetural

Conforme orientação do professor na apresentação e mentoria do desafio:

> Não foi necessário treinar um modelo supervisionado.

Em vez disso, foi adotada a seguinte estratégia:

1. Utilização de **LLM pré-treinada** para auxiliar na interpretação do diagrama arquitetural;
2. Conversão da arquitetura para uma estrutura JSON (componentes, fluxos e fronteiras de confiança);
3. Aplicação de um **motor determinístico baseado em STRIDE** implementado em Python;
4. Geração automatizada de relatórios e métricas de risco.

Essa abordagem permitiu entregar um MVP funcional e consistente no contexto do hackathon.

---

## 🏗️ 3. Arquiteturas Analisadas

### 📌 Arquitetura 1 – AWS (SEI/SIP)
- VPC
- Subnets públicas e privadas
- Application Load Balancer
- EC2 (App Server)
- RDS (Primary / Secondary)
- ElastiCache
- EFS
- CloudTrail
- KMS
- Backup
- SES
- CloudWatch
- Shield / WAF / CloudFront

### 📌 Arquitetura 2 – Azure
- Microsoft Entra ID
- API Management (API Gateway)
- Developer Portal
- Logic Apps
- Azure Services
- SaaS Services
- REST / SOAP Web Services

---

## ⚙️ 4. Funcionamento do Sistema

### 🔹 Pipeline

Diagrama → JSON estruturado → Motor STRIDE → Cálculo de risco → Relatórios → Comparativo

### 🔹 Processo

1. Arquitetura modelada em JSON;
2. Cada componente é classificado por tipo (database, api_gateway, identity_provider, etc.);
3. Aplicação das categorias STRIDE:
   - S – Spoofing  
   - T – Tampering  
   - R – Repudiation  
   - I – Information Disclosure  
   - D – Denial of Service  
   - E – Elevation of Privilege  
4. Cálculo de **risk_score**;
5. Classificação automática:
   - 0–4 → BAIXO  
   - 5–7 → MÉDIO  
   - 8+ → ALTO  

---

## 📊 5. Resultados Gerados

O sistema produz automaticamente:

- report_aws.md
- report_azure.md
- Resumo executivo
- Tabela consolidada (Top ameaças)
- Comparativo AWS vs Azure
- Gráficos:
  - Score médio por arquitetura
  - Distribuição BAIXO/MÉDIO/ALTO
- Exportação CSV:
  - threats_consolidated.csv
  - threats_aws.csv
  - threats_azure.csv

---

## 📈 6. Análise Comparativa

A solução permite:

- Comparar score médio entre arquiteturas;
- Identificar componentes mais críticos;
- Detectar ameaças classificadas como ALTO risco;
- Priorizar controles de segurança.

Exemplo observado:
- Microsoft Entra ID apresentou risco ALTO (Elevation of Privilege);
- AWS apresentou maior volume total de ameaças devido à maior complexidade arquitetural.

---

## 🚀 7. Como Executar

1. Abrir o notebook no Google Colab:
   TC5_STRIDE_AI_Complete.ipynb

2. Executar todas as células.

O notebook irá:
- Carregar os JSONs
- Aplicar STRIDE
- Gerar relatórios
- Gerar gráficos
- Exportar CSVs

---

## 📌 8. Limitações do MVP

- A leitura do diagrama não é feita automaticamente por visão computacional;
- A modelagem JSON foi assistida por LLM;
- O motor STRIDE é baseado em regras determinísticas;
- Não há integração com bases externas como MITRE ATT&CK, CWE ou CVE.

---

## 🔮 9. Evoluções Futuras

- Integração com LLM para enriquecimento contextual das ameaças;
- Integração com base MITRE ATT&CK;
- Leitura automática de diagramas via OCR / Visão Computacional;
- Dashboard interativo (Streamlit).

---

## 🎥 10. Vídeo de Apresentação

Inserir aqui o link do vídeo (YouTube ou similar).

---

## 🔗 11. Estrutura do Repositório

TC5_STRIDE_AI/
│
├── TC5_STRIDE_AI_Complete.ipynb
├── README.md
├── report_aws.md
├── report_azure.md
├── threats_consolidated.csv

---

## ✅ Conclusão

O projeto entrega um MVP funcional de modelagem de ameaças automatizada utilizando STRIDE, com geração de relatórios, métricas de risco e comparação arquitetural entre ambientes AWS e Azure.

A solução demonstra aplicação prática de conceitos de:

- Segurança em arquitetura de sistemas;
- Modelagem de ameaças;
- Automação com Python;
- Uso estratégico de LLMs em engenharia de software.
