# SaaS Funnel Conversion Analysis

🇧🇷 [Português](#português) | 🇬🇧 [English](#english)

---

## Português

### O problema de negócio

Em produtos SaaS B2B, cada etapa do funil tem um custo — de aquisição, de onboarding, de suporte. Quando usuários abandonam antes da conversão, esse custo não retorna. O problema raramente está em "todo o funil": está em **uma etapa específica**, para **um segmento específico**, por razões que os dados conseguem revelar.

Este projeto analisa um funil de conversão B2B SaaS com 1.000 usuários simulados, segmentados por canal de aquisição e perfil de empresa (SMB, Mid-Market, Enterprise), com o objetivo de identificar **onde o funil quebra** e **o que fazer a respeito**.

---

### O que foi encontrado

**Gargalo principal:** a maior queda acontece na etapa **Visitou LP → Iniciou Trial** — ou seja, o problema começa antes mesmo de o usuário entrar no produto. Isso aponta para uma desconexão entre a promessa da landing page e a expectativa de quem chega.

**Segmento que mais converte:** usuários **Enterprise** apresentam a maior taxa de conversão ao longo de todo o funil — comportamento esperado dado o maior nível de intenção na entrada, mas que reforça a necessidade de qualificar melhor os leads de outros segmentos antes de colocá-los no funil.

**Canal com melhor performance:** **Referral** gera usuários que convertem consistentemente melhor em todas as etapas. Isso sugere que o fit produto-mercado é mais forte quando há indicação — e que investir em programas de referral pode ter ROI superior ao de canais pagos.

---

### Da análise para a ação

Os achados geraram três hipóteses priorizadas para teste:

| Hipótese | Etapa do funil | Métrica de sucesso |
|---|---|---|
| Headline da LP orientada a resultado vs. feature | LP → Trial | Taxa de início de trial |
| Guiar o usuário à key feature nas primeiras 24h | Trial → Ativação | Taxa de ativação D1 |
| Sequência de email nurturing segmentada por plano | Ativação → Conversão | Taxa de conversão paga |

---

### Como este projeto foi construído

- Dataset simulado com 1.000 usuários, construído com distribuições realistas por estágio do funil
- Cálculo de taxas de conversão acumuladas e drop-off por etapa
- Segmentação cruzada por canal de aquisição (Organic, Paid, Referral, Outbound) e perfil de empresa
- Visualizações de funil, heatmap de conversão por segmento e análise comparativa de canais

**Stack:** `Python` `Pandas` `NumPy` `Matplotlib` `Seaborn` `Jupyter Notebook`

---

### Arquivos

| Arquivo | Descrição |
|---|---|
| `saas_funnel_analysis.ipynb` | Notebook com análise completa e visualizações |
| `funnel_data.csv` | Dataset simulado com 1.000 usuários |

---

### Próximos passos

- Adicionar camada SQL com extração simulada via CTEs e window functions
- Incluir análise de cohort de ativação (retenção por semana de cadastro)
- Criar dashboard exportável com os principais KPIs do funil

---

*Desenvolvido por [Gabrielle Magalhães Soares](https://www.linkedin.com/in/maggabrielle/) — Data Analyst*

---

## English

### The business problem

In B2B SaaS, every funnel stage has a cost — acquisition, onboarding, support. When users drop off before converting, that cost doesn't return. The problem is rarely "the whole funnel": it's **one specific stage**, for **one specific segment**, for reasons the data can reveal.

This project analyzes a B2B SaaS conversion funnel with 1,000 simulated users, segmented by acquisition channel and company profile (SMB, Mid-Market, Enterprise), with the goal of identifying **where the funnel breaks** and **what to do about it**.

---

### What was found

**Main bottleneck:** the largest drop-off happens at the **Visited LP → Started Trial** stage — meaning the problem starts before the user even enters the product. This points to a disconnect between the landing page's promise and the expectations of those who arrive.

**Highest-converting segment:** **Enterprise** users show the best conversion rate throughout the funnel — expected given their higher intent at entry, but it reinforces the need to better qualify leads from other segments before they enter the funnel.

**Best-performing channel:** **Referral** drives users who convert consistently better across all stages. This suggests product-market fit is stronger when there's a recommendation involved — and that investing in referral programs may yield higher ROI than paid channels.

---

### From analysis to action

The findings generated three prioritized hypotheses for A/B testing:

| Hypothesis | Funnel stage | Success metric |
|---|---|---|
| Outcome-focused vs. feature-focused LP headline | LP → Trial | Trial start rate |
| Guide users to the key feature within the first 24h | Trial → Activation | D1 activation rate |
| Plan-segmented email nurturing sequence | Activation → Conversion | Paid conversion rate |

---

### How this project was built

- Simulated dataset with 1,000 users, built with realistic distributions per funnel stage
- Calculation of cumulative conversion rates and drop-off per stage
- Cross-segmentation by acquisition channel (Organic, Paid, Referral, Outbound) and company profile
- Funnel visualizations, conversion heatmap by segment, and channel comparative analysis

**Stack:** `Python` `Pandas` `NumPy` `Matplotlib` `Seaborn` `Jupyter Notebook`

---

### Files

| File | Description |
|---|---|
| `saas_funnel_analysis.ipynb` | Notebook with full analysis and visualizations |
| `funnel_data.csv` | Simulated dataset with 1,000 users |

---

### Next steps

- Add SQL layer with simulated extraction via CTEs and window functions
- Include cohort activation analysis (retention by signup week)
- Build exportable dashboard with key funnel KPIs

---

*Developed by [Gabrielle Magalhães Soares](https://www.linkedin.com/in/maggabrielle/) — Data Analyst* 
