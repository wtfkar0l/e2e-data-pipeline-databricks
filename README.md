# 🚀 Pipeline de Dados End-to-End: Análise de Vendas Olist no Databricks

Este projeto demonstra a construção de um pipeline de dados "end-to-end" (E2E), desde a ingestão de dados brutos até um modelo dimensional pronto para análise, utilizando as melhores práticas da arquitetura Medallion (Bronze, Silver, Gold) dentro do Databricks.

**Dataset:** [Olist E-commerce Dataset (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
| **Plataforma:** Databricks (Community Edition)

---

## 🏛️ Arquitetura do Pipeline

O projeto segue a **Arquitetura Medallion**, que organiza os dados em três camadas lógicas para garantir governança, qualidade e facilidade de reprocessamento.



<img width="2288" height="1100" alt="image" src="https://github.com/user-attachments/assets/6c389d0f-04c8-4433-a612-b4634b6a997f" />




* **Bronze 🥉 (Ingestão):** Camada de dados brutos. Os 9 arquivos CSV originais são lidos de um Volume do Unity Catalog e salvos como tabelas Delta, sem nenhuma transformação, garantindo um "espelho" da origem.
* **Silver 🥈 (Validação e Limpeza):** Camada de dados validados. As tabelas da Bronze são lidas, limpas, tipadas (ex: strings para datas) e enriquecidas (ex: *join* de produtos com a tradução). Esta camada representa a "fonte única da verdade".
* **Gold 🥇 (Modelo de Negócio):** Camada de consumo. As tabelas Silver são agregadas e modeladas em um esquema dimensional (Fato e Dimensões) otimizado para consultas de BI e análise.

---

## 🛠️ Tecnologias Utilizadas

* **Plataforma:** Databricks (com Unity Catalog)
* **Engenharia de Dados (ETL):** PySpark (para ler, transformar e carregar os dados entre as camadas)
* **Armazenamento e Gerenciamento:** Delta Lake (para as tabelas B/S/G)
* **Análise de Dados:** Spark SQL (para consultas na camada Gold)
* **Governança:** Unity Catalog (para gerenciar os Schemas, Volumes e Tabelas)

---

## 📈 Análises e Descobertas (Camada Gold)

Após a construção do pipeline, a camada Gold permitiu responder perguntas de negócio chave usando SQL.

### 1. Faturamento Mensal
A análise do faturamento mostra uma clara tendência de crescimento ao longo do período.

*(Cole aqui um screenshot do seu gráfico de barras de faturamento)*
![Faturamento Mensal](link-para-a-imagem-do-grafico)

### 2. Top 5 Estados por Faturamento
O Sudeste domina o faturamento, com São Paulo (SP) liderando com folga.
*(Cole aqui um screenshot do seu gráfico/tabela dos estados)*

### 3. Variação Percentual do Faturamento (Mês a Mês)
O gráfico mais importante para a saúde do negócio. Ele mostra os meses de crescimento (acima de 0%) e os meses de queda (abaixo de 0%).

*(Cole aqui um screenshot do seu gráfico de linha da "variacao_percentual")*
![Variação Percentual do Faturamento](link-para-a-imagem-do-grafico)

---

## 📖 Como Executar (Estrutura do Projeto)

O projeto está dividido em dois notebooks principais:

1.  `[Nome do notebook de ETL].ipynb`: Contém todo o código PySpark para o pipeline Bronze -> Silver -> Gold.
2.  `[Nome do notebook de Análise].ipynb`: Contém todas as queries SQL de análise feitas na camada Gold.
