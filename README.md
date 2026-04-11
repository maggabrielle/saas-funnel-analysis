# saas-funnel-analysis
Análise do funil de conversão para um SaaS B2B usando Python e Pandas
📊 SaaS Funnel Conversion Analysis
 Português |  English

Português
Sobre o projeto
Análise exploratória do funil de conversão de um produto B2B SaaS, com foco em identificar gargalos entre as etapas do trial e propor hipóteses de melhoria baseadas em dados.
Problema
Em produtos SaaS, entender onde os usuários abandonam o funil é essencial para priorizar ações de CRO e onboarding. Este projeto simula um cenário real de análise de trials, segmentando usuários por canal de aquisição, plano e perfil de empresa.
Metodologia

Geração e exploração de dataset simulado com 1.000 usuários
Cálculo de taxas de conversão acumuladas por etapa do funil
Análise de drop-off entre etapas
Segmentação por canal de aquisição e por perfil de cliente (SMB, Mid-Market, Enterprise)
Formulação de hipóteses para testes A/B

Principais resultados

Maior gargalo identificado na etapa Visitou LP → Iniciou Trial
Usuários Enterprise apresentam a maior taxa de conversão final
Canal Referral gera usuários com melhor desempenho no funil

Hipóteses para A/B teste

Testar headline da LP focada em resultado vs. feature
Guiar o usuário à key feature nas primeiras 24h do trial
Criar sequência de email nurturing segmentada por plano

Tecnologias: 
Python Pandas NumPy Matplotlib Seaborn Jupyter Notebook
Arquivos
ArquivoDescriçãosaas_funnel_analysis.ipynbNotebook com análise completafunnel_data.csvDataset simulado com 1.000 usuários

English
About
Exploratory analysis of a B2B SaaS conversion funnel, focused on identifying drop-off points across trial stages and proposing data-driven improvement hypotheses.
Problem
In SaaS products, understanding where users abandon the funnel is critical for prioritizing CRO and onboarding actions. This project simulates a real-world trial analysis scenario, segmenting users by acquisition channel, plan, and company profile.
Methodology

Generation and exploration of a simulated dataset with 1,000 users
Calculation of cumulative conversion rates per funnel stage
Drop-off analysis between stages
Segmentation by acquisition channel and customer profile (SMB, Mid-Market, Enterprise)
Formulation of A/B test hypotheses

Key Findings

Largest drop-off identified at the Visited LP → Started Trial stage
Enterprise users show the highest final conversion rate
Referral channel drives users with the best funnel performance

A/B Test Hypotheses

Test outcome-focused vs. feature-focused LP headline
Guide users to the key feature within the first 24h of trial
Build a plan-segmented email nurturing sequence during trial

Tech Stack: 
Python Pandas NumPy Matplotlib Seaborn Jupyter Notebook
Files
FileDescriptionsaas_funnel_analysis.ipynbNotebook with full analysisfunnel_data.csvSimulated dataset with 1,000 users
