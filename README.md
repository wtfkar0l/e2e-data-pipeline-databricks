# 🚀 Pipeline de Dados End-to-End: Análise de Vendas Olist no Databricks

Este projeto demonstra a construção de um pipeline de dados "end-to-end" (E2E), desde a ingestão de dados brutos até um modelo dimensional pronto para análise, utilizando as melhores práticas da arquitetura Medallion (Bronze, Silver, Gold) dentro do Databricks.

### 📈 Data Analysis with SQL | PySpark | Databricks | Python

--- 

### Dataset e Contexto

* **Fonte de Dados:** Olist E-commerce Dataset (Kaggle)
* **Período:** 2016-2018
* **Domínio:** E-commerce brasileiro com dados reais de pedidos, produtos, clientes e avaliações
* **Plataforma:** Databricks Community Edition

---

## 🏛️ Arquitetura do Pipeline

O projeto adota a **Arquitetura *Medallion*** — um modelo em camadas amplamente utilizado em *data lakes* modernos para garantir **governança, qualidade, rastreabilidade e facilidade de reprocessamento** dos dados.  
Essa estrutura organiza o fluxo em três níveis progressivos: **Bronze**, **Silver** e **Gold**.

<img width="2288" height="1100" alt="image" src="https://github.com/user-attachments/assets/a473cc1d-3400-4701-9e65-d0135b6ba57b" />

### 🥉 Bronze — Ingestão
Camada de **dados brutos**.  
Nesta etapa, os **9 arquivos CSV originais** são lidos diretamente de um *Volume* do Unity Catalog e armazenados como **tabelas Delta**, sem qualquer transformação.  
👉 Representa um **espelho fiel da origem**, permitindo reprocessamentos e auditorias.

### 🥈 Silver — Validação e Limpeza
Camada de **dados tratados e confiáveis**.  
As tabelas da Bronze são lidas, **limpas, validadas e tipadas** (por exemplo, conversão de *strings* em *datas*). Além disso, há **enriquecimento semântico**, como *joins* entre tabelas de produtos e suas traduções.  
👉 Essa camada consolida uma **“fonte única da verdade”**, utilizada por times de análise e engenharia.

### 🥇 Gold — Modelo de Negócio
Camada de **consumo e análise**.  
Aqui, os dados da Silver são **agregados, transformados e modelados** em um **esquema dimensional (Fato e Dimensões)**, otimizado para ferramentas de BI e consultas analíticas.  
👉 Permite gerar **insights de negócio** com alta performance e consistência.

---

## 🛠️ Tecnologias Utilizadas

| Componente | Tecnologia | Versão |
|------------|-----------|--------|
| Plataforma | Databricks | Community Edition |
| Linguagem | SQL | Spark SQL |
| Storage | Delta Lake | 3.0+ |
| Catálogo | Unity Catalog | - |
| Notebook | Jupyter/Databricks | - |


## 📑 Índice de Análises

1. [Faturamento Mensal](#1--faturamento-mensal)
2. [Top 5 Estados por Faturamento](#2-top-5-estados-por-faturamento)
3. [Top 10 Categorias Mais Vendidas](#3--top-10-categorias-mais-vendidas)
4. [Variação Percentual do Faturamento (MoM)](#4--variação-percentual-do-faturamento-mom)

---

### Principais Descobertas

| Período | Faturamento (R$) | Total de Pedidos | Observações |
|---------|------------------|------------------|-------------|
| **2016-09** | R$ 272,46 | 2 | Início das operações |
| **2016-10** | R$ 67.668,71 | 290 | Crescimento explosivo de 24.736% |
| **2017-11** | R$ 1.571.726,72 | 7.421 | **Pico de faturamento** (Black Friday) |
| **2018-05** | R$ 1.498.189,32 | 6.833 | Faturamento estabilizado |
| **2018-09** | R$ 166,46 | 1 | Dados incompletos (final do dataset) |

### Insights Estratégicos

- ✅ **Crescimento Sustentável**: De R$ 67,6k (out/2016) para R$ 1,4M (média em 2018) representa 20x de crescimento
- 📈 **Tendência Anual**: Crescimento médio de 50-80% ao mês nos primeiros 6 meses de 2017
- 🎄 **Sazonalidade Forte**: Novembro de 2017 teve 54,65% de crescimento (Black Friday + Pré-Natal)
- ⚠️ **Retração Pós-Pico**: Dezembro/2017 caiu -33,81% após o recorde de novembro

---

## 1. 💰 Faturamento Mensal
**Qual foi a evolução do faturamento mensal ao longo do período?**

<img width="916" height="704" alt="Sales Over Time" src="https://github.com/user-attachments/assets/2b366d7c-3ad6-4c24-b586-fc91f3efab48" />

---

## 2. Top 5 Estados por Faturamento 🗺️
**Quais são os 5 estados que mais geram receita para o negócio?**

<img width="1538" height="1064" alt="Customer Distribution by customer_state" src="https://github.com/user-attachments/assets/b55b2632-8534-44f6-a513-5ca75e54b4bf" />

### Ranking de Estados

| Posição | Estado | Faturamento (R$) | % do Total* | Região |
|---------|--------|------------------|-------------|--------|
| 🥇 | **SP** | R$ 7.544.659,30 | ~45% | Sudeste |
| 🥈 | **RJ** | R$ 2.753.911,43 | ~16% | Sudeste |
| 🥉 | **MG** | R$ 2.308.330,79 | ~14% | Sudeste |
| 4º | **RS** | R$ 1.128.464,01 | ~7% | Sul |
| 5º | **PR** | R$ 1.049.730,74 | ~6% | Sul |

*Estimativa baseada nos Top 5

### Insights Geográficos

- 🏙️ **Concentração Sudeste**: SP + RJ + MG = ~75% do faturamento total
- 📍 **Domínio de São Paulo**: Sozinho representava quase metade da receita
- 🔄 **Oportunidade Sul**: RS e PR mostraram potencial de crescimento
- ⚠️ **Regiões Ausentes**: Norte e Nordeste não aparecem no Top 5 (oportunidade de expansão)

### Recomendações de Negócio (de acordo com a época do dataset)

1. **Manter Investimento no Sudeste**: Campanhas regionalizadas para SP, RJ e MG
2. **Explorar o Sul**: Aumentar parcerias logísticas em RS e PR
3. **Expansão Estratégica**: Analisar barreiras de entrada no Norte/Nordeste
4. **Centros de Distribuição**: Considerar hubs em SP (já existente) e abrir em RS

---

## 3. 📦 Top 10 Categorias Mais Vendidas

### Pergunta de Negócio
**Quais categorias de produtos geram mais receita e volume de vendas?**

<img width="1538" height="704" alt="Total Orders by Product Category" src="https://github.com/user-attachments/assets/b97a712d-e61a-4d8b-a9b6-6ecee1dbe0b6" />

### Ranking de Categorias

| Posição | Categoria | Faturamento (R$) | Total Pedidos | Ticket Médio (R$) |
|---------|-----------|------------------|---------------|-------------------|
| 1º | **BED_BATH_TABLE** | R$ 1.711.258,08 | 9.399 | R$ 182,07 |
| 2º | **HEALTH_BEAUTY** | R$ 1.653.730,45 | 8.800 | R$ 187,92 |
| 3º | **COMPUTERS_ACCESSORIES** | R$ 1.571.543,81 | 6.654 | R$ 236,18 |
| 4º | **FURNITURE_DECOR** | R$ 1.424.782,52 | 6.425 | R$ 221,73 |
| 5º | **WATCHES_GIFTS** | R$ 1.421.715,28 | 5.604 | R$ 253,68 |
| 6º | **SPORTS_LEISURE** | R$ 1.381.363,23 | 7.673 | R$ 180,01 |
| 7º | **HOUSEWARES** | R$ 1.086.565,32 | 5.847 | R$ 185,80 |
| 8º | **AUTO** | R$ 843.297,65 | 3.872 | R$ 217,83 |
| 9º | **GARDEN_TOOLS** | R$ 823.517,80 | 3.505 | R$ 234,95 |
| 10º | **COOL_STUFF** | R$ 759.644,85 | 3.616 | R$ 210,10 |

### Análise de Categorias

#### 🏆 Campeãs de Volume
- **BED_BATH_TABLE**: Líder absoluto com 9.399 pedidos
- **HEALTH_BEAUTY**: Segundo em pedidos (8.800) e faturamento
- **SPORTS_LEISURE**: Alto volume (7.673) mas ticket médio baixo

#### 💎 Campeãs de Ticket Médio
1. **WATCHES_GIFTS**: R$ 253,68 (produtos de maior valor agregado)
2. **COMPUTERS_ACCESSORIES**: R$ 236,18 (tecnologia)
3. **GARDEN_TOOLS**: R$ 234,95 (produtos especializados)

#### 🎯 Estratégias por Categoria

**Para Categorias de Alto Volume + Baixo Ticket:**
- 🛏️ **BED_BATH_TABLE**: Bundles e cross-sell (ex: "Complete seu quarto")
- 💄 **HEALTH_BEAUTY**: Programa de assinatura/recorrência

**Para Categorias de Alto Ticket + Médio Volume:**
- ⌚ **WATCHES_GIFTS**: Campanhas de datas especiais (Dia dos Pais, Natal)
- 💻 **COMPUTERS_ACCESSORIES**: Parcelamento facilitado + garantia estendida

**Oportunidades de Crescimento:**
- 🚗 **AUTO**: 8ª posição mas ticket médio bom (R$ 217,83) - potencial inexplorado
- 🎨 **COOL_STUFF**: Categoria ampla que pode ser subdividida para marketing direcionado

## 4. 📈 Variação Percentual do Faturamento (MoM)

### Pergunta de Negócio
**Em quais meses houve crescimento ou retração do faturamento em relação ao mês anterior?**

<img width="916" height="704" alt="Revenue Variation Percentage" src="https://github.com/user-attachments/assets/a7fb0304-daeb-4e80-8c58-a6ced987ed4d" />


### Meses de Crescimento Positivo 📈

| Mês | Variação % | Faturamento (R$) | Contexto |
|-----|-----------|------------------|----------|
| **2017-11** | **+54,65%** 🚀 | R$ 1.571.726,72 | Black Friday + Pré-Natal |
| **2017-05** | **+46,97%** | R$ 718.240,45 | Dia das Mães |
| **2018-01** | **+34,80%** | R$ 1.402.385,03 | Volta às aulas + Liquidações |
| **2017-03** | **+52,44%** | R$ 520.460,95 | Crescimento orgânico forte |
| **2017-02** | **+82,06%** | R$ 341.415,86 | Expansão inicial da plataforma |

### Meses de Retração Negativa 📉

| Mês | Variação % | Faturamento (R$) | Causa Provável |
|-----|-----------|------------------|----------------|
| **2017-12** | **-33,81%** ⚠️ | R$ 1.040.318,39 | Ressaca pós-Black Friday |
| **2017-06** | **-16,82%** | R$ 597.442,47 | Sazonalidade (meio do ano) |
| **2018-06** | **-13,58%** | R$ 1.294.791,32 | Copa do Mundo 2018 |
| **2017-04** | **-6,11%** | R$ 488.685,62 | Ajuste pós-crescimento |
| **2018-02** | **-7,45%** | R$ 1.297.956,10 | Pós-Verão + Carnaval |
| **2018-08** | **-7,31%** | R$ 1.223.066,14 | Sazonalidade |

### 🔍 Análise Detalhada

#### Anomalias Identificadas

| Mês | Variação | Status | Explicação |
|-----|----------|--------|------------|
| 2016-10 | +24.736,2% | ⚡ Outlier | Início real das operações (base de comparação muito pequena) |
| 2016-12 | -99,97% | ⚡ Outlier | Praticamente sem operações (novembro teve 0 vendas) |
| 2017-01 | +955.732,72% | ⚡ Outlier | Retomada após pausa de fim de ano |
| 2018-09 | -99,99% | 🚫 Dados incompletos | Fim do dataset (apenas 1 pedido) |

*Estes valores devem ser desconsiderados da análise estatística por representarem início/fim de operações*

#### Padrões Sazonais Identificados

**Crescimento Consistente:**
- 🌸 **Maio**: Dia das Mães (+46,97%)
- 🛍️ **Novembro**: Black Friday (+54,65%)
- 🎒 **Janeiro**: Volta às aulas (+34,80%)

**Retração Recorrente:**
- 🎄 **Dezembro**: Queda após Black Friday (-33,81%)
- ☀️ **Junho**: Meio do ano sem datas comemorativas (-16,82% e -13,58%)

#### Volatilidade do Negócio

**Coeficiente de Variação Médio:** ±25% (excluindo outliers)

**Interpretação:**
- ✅ Negócio maduro em 2018 (variações entre -7% e +13%)
- ⚠️ Alta dependência de sazonalidade (Black Friday = 54% de crescimento)
- 📊 Necessidade de estratégias para suavizar curva de faturamento

### 🎯 Recomendações Estratégicas

#### Curto Prazo (Táticas)
1. **Preparação para Black Friday**: Estoque 50-60% maior em outubro
2. **Campanhas de Retenção em Dezembro**: Evitar queda pós-BF
3. **Ativação de Junho**: Criar campanhas temáticas (Copa, Férias, Dia dos Namorados)

#### Médio Prazo (Estratégicas)
1. **Diversificação de Receita**: Reduzir dependência de eventos pontuais
2. **Programa de Fidelidade**: Aumentar recorrência nos meses de baixa
3. **Previsibilidade Financeira**: Criar modelo de forecast considerando sazonalidade

#### Longo Prazo (Estruturais)
1. **Expansão de Categorias**: Produtos menos sazonais (ex: Tecnologia, Auto)
2. **Modelo de Assinatura**: Para categorias de consumo recorrente (Health & Beauty)
3. **Regionalização**: Aproveitar eventos locais (festas juninas no Nordeste)


---

## 📊 Resumo Executivo

### KPIs Principais (2016-2018)

| Métrica | Valor | Observação |
|---------|-------|------------|
| **Faturamento Total** | R$ ~16,8 milhões | Período completo |
| **Ticket Médio Geral** | R$ ~160 | Varia por categoria |
| **Total de Pedidos** | ~105 mil | Pedidos válidos |
| **Estado Top** | São Paulo (SP) | 45% do faturamento |
| **Categoria Top** | Bed Bath Table | R$ 1,7M em vendas |
| **Pico de Faturamento** | Nov/2017 | R$ 1,57M |
| **Crescimento MoM Médio** | +15% | 2017-2018 |
| **Maior Retração** | Dez/2017 | -33,81% |

### 🎯 Principais Insights de Negócio

1. ✅ **Negócio em Crescimento**: Evolução de R$ 67k para R$ 1,4M/mês em 18 meses
2. 🗺️ **Concentração Regional**: 75% do faturamento vem do Sudeste
3. 🏠 **Categorias de Lifestyle Dominam**: Casa, Beleza e Esporte lideram
4. 📈 **Sazonalidade Forte**: Black Friday representa 54% de crescimento
5. ⚠️ **Volatilidade Alta**: Necessidade de estratégias de suavização


## 📖 Como Executar (Estrutura do Projeto)

O projeto está dividido em dois notebooks principais:

1.  `[Nome do notebook de ETL].ipynb`: Contém todo o código PySpark para o pipeline Bronze -> Silver -> Gold.
2.  `[Nome do notebook de Análise].ipynb`: Contém todas as queries SQL de análise feitas na camada Gold.


---

## 🧑‍💻 Author

**Ana Karolina Costa da Silva** 📍 Software Engineer & Data Science Researcher  
🎓 M.Sc. Computer Science — PUC-Rio  
💼 [LinkedIn](https://www.linkedin.com/in/karolyneehcs/) | [GitHub](https://github.com/karolyneehcs)


---

## 📜 Licença

Este projeto foi desenvolvido para fins educacionais utilizando dados públicos do Kaggle (Olist Brazilian E-commerce Dataset).
