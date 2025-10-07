# AMIIBO API ETL & ANÁLISE DE DADOS (Databricks + Power BI)

## Visão Geral do Projeto

Este projeto demonstra um pipeline completo de **Extração, Transformação e Carga (ETL)**, utilizando dados públicos de amiibos da Nintendo. O objetivo é transformar dados brutos de uma API em **insights acionáveis** sobre o catálogo, a cadência de lançamentos e as disparidades de distribuição global dos produtos.

O resultado final é um **dashboard interativo no Power BI** que permite a análise temporal e logística da coleção.

---

### 🔑 Tecnologias Principais

| Categoria | Ferramenta | Uso no Projeto |
| :--- | :--- | :--- |
| **Orquestração & ETL** | **Databricks / Spark SQL** | Processamento, limpeza e enriquecimento de dados em escala. |
| **Fonte de Dados** | **Amiibo API** | Extração dos dados brutos em formato JSON. |
| **Visualização** | **Power BI** | Criação de um dashboard focado em análise temporal e logística. |
| **Código** | **Python** | Scripts de extração e automação. |

---

## 🏗️ Estrutura do Pipeline de Dados

O projeto segue a arquitetura **Arquitetura medalhão**, garantindo a qualidade e confiabilidade dos dados em cada etapa de processamento no Databricks.

### 1. 🥉 Camada Bronze (Staging)

* **Função:** Ingestão dos dados brutos e originais diretamente da API.
* **Processamento:** Mínimo. Os dados são carregados "como estão", apenas para persistência do estado inicial.

### 2. 🥈 Camada Silver (Limpeza e Padronização)

* **Função:** Limpeza de dados e aplicação de regras de qualidade.
* **Processamento:**
    * Tratamento de valores nulos.
    * **Conversão de Tipo:** Conversão das strings de data (`release_na`, `release_jp`, etc.) para o tipo `DATE` (obrigatório para cálculos temporais).

### 3. 🥇 Camada Gold (Consumo Analítico)

* **Função:** Camada final, contendo dados agregados e métricas prontas para o consumo do Power BI.
* **Enriquecimento (KPIs Criados via Spark SQL):**
    * **`release_global_earliest`**: A data de lançamento mais antiga no mundo (`LEAST`).
    * **`release_global_latest`**: A data de lançamento mais recente no mundo (`GREATEST`).
* **Criação de Views: **
  * **`amiibos_types`**: Amiibos agrupados pelo seu tipo
  * **`avg_date_diff`**: Média de diferença de data de lançamento entre regiões
  * **`amiibos_by_game_series`**: Total de amibos por série de jogos
  * **`amiibos_by_amiibo_series`**: Total de amiibos por série de amiibo

---

## 📊 Análises e Resultados (Power BI)

A Camada Silver alimenta o dashboard, que é estruturado em três pilares principais de insights:

### I. Composição do Catálogo
* **Insights:** Qual a proporção entre **Figuras** vs. **Cartões** e quais séries (`game_series`) dominam o catálogo em volume.

### II. Dinâmica Temporal (Cadência)
* **Visual Principal:** Gráfico de Linha da Cadência de Lançamentos Mensais.
* **Insights:** Identificação dos **picos de produção** e a tendência histórica de distribuição da Nintendo.

### III. Atraso e Distribuição Global
* **Métricas:** **Atraso Médio Regional** em dias (NA e EU vs. JP).
* **Insight:** Quantificação da disparidade logística e cálculo do **Tempo Total de Distribuição** (dias entre o primeiro e o último lançamento global para um amiibo).

### IV. Relatório Geral (Análise Ad-Hoc)
* **Função:** Relatório geral com filtros para todos os campos da tabela, permitindo uma análise rápida e intuitiva de acordo com o que for desejado.


