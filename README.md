# Projeto de Previsão de Demanda em Rede Varejista

## Visão Geral

Este projeto tem como objetivo desenvolver um modelo de previsão de demanda para uma rede varejista, utilizando dados históricos de vendas, informações sobre clientes, produtos, lojas e taxas de câmbio. O projeto é dividido em duas fases principais: desenvolvimento local com Python e escalonamento no Databricks.

## Fases do Projeto

### 1. Desenvolvimento Local com Python

Nesta fase, o foco é explorar, limpar e preparar os dados, além de desenvolver e testar modelos de previsão em um ambiente local.

-   **Análise Exploratória de Dados (EDA):** Compreensão dos dados, identificação de padrões, sazonalidades e tendências.
-   **Limpeza de Dados:** Tratamento de valores ausentes, outliers e inconsistências.
-   **Engenharia de Features:** Criação de novas variáveis relevantes para o modelo, como lags, rolling means e flags temporais.
-   **Modelagem:** Desenvolvimento de modelos de previsão utilizando bibliotecas como Pandas e Scikit-learn.
- **Visualização:** Criação de visualizações utilizando Matplotlib e Seaborn
-   **Validação Cruzada:** Utilização de técnicas como Rolling ou Expanding Window para avaliar o desempenho dos modelos em séries temporais.
-   **Ajuste de Hiperparâmetros:** Otimização dos modelos através de métodos como GridSearchCV, RandomizedSearchCV ou Optuna.

### 2. Escalonamento no Databricks

Nesta fase, o projeto é escalonado para um ambiente de processamento distribuído, o Databricks.

-   **Ingestão de Dados:** Carregamento dos dados tratados para o Databricks (com a possibilidade de utilização do Delta Lake).
-   **Reimplementação com PySpark:** Conversão do pipeline de processamento de dados para PySpark.
-   **Modelagem com Spark MLlib:** Treinamento dos modelos de previsão utilizando as ferramentas do Spark MLlib.
-   **Visualizações e Dashboards:** Criação de dashboards interativos e visualizações utilizando Databricks SQL ou display API.

## Estrutura dos Dados

O projeto utiliza dados de cinco tabelas principais:

-   **Sales (Vendas):** Registros de vendas com informações de data, cliente, loja, produto, quantidade e moeda.
-   **Customers (Clientes):** Informações demográficas dos clientes (nome, gênero, cidade, estado, país, continente, data de nascimento).
-   **Products (Produtos):** Detalhes dos produtos, incluindo nome, marca, cor, custo unitário, preço unitário, categoria e subcategoria.
-   **Stores (Lojas):** Informações geográficas e estruturais das lojas (país, estado, área em metros quadrados, data de abertura).
-   **Exchange Rates (Taxas de Câmbio):** Taxas de conversão de moedas para USD ao longo do tempo.

## Pipeline do Projeto

1.  **Carregamento e Integração:** Leitura dos dados e merge entre as tabelas. Conversão de moeda para USD.
2.  **Análise Exploratória (EDA):** Verificação de sazonalidade, tendência e padrões. Geração de gráficos e estatísticas descritivas.
3.  **Pré-processamento:** Tratamento de dados ausentes, conversão de tipos e criação de colunas temporais.
4.  **Feature Engineering:** Criação de variáveis de lag, rolling mean e flags temporais.
5.  **Modelos de Previsão:** Implementação de baselines estatísticos e modelos supervisionados.
6.  **Ajuste de Hiperparâmetros:** Otimização dos modelos.
7.  **Escalonamento no Databricks:** Reimplementação do pipeline com PySpark e treinamento com Spark MLlib.

## Estrutura de Arquivos



1.  **Carregamento e Integração:** Leitura dos dados e merge entre as tabelas. Conversão de moeda para USD.
2.  **Análise Exploratória (EDA):** Verificação de sazonalidade, tendência e padrões. Geração de gráficos e estatísticas descritivas.
3.  **Pré-processamento:** Tratamento de dados ausentes, conversão de tipos e criação de colunas temporais.
4.  **Feature Engineering:** Criação de variáveis de lag, rolling mean e flags temporais.
5.  **Modelos de Previsão:** Implementação de baselines estatísticos e modelos supervisionados.
6.  **Ajuste de Hiperparâmetros:** Otimização dos modelos.
7.  **Escalonamento no Databricks:** Reimplementação do pipeline com PySpark e treinamento com Spark MLlib.

## Tecnologias Utilizadas

-   **Python:** Pandas, Scikit-learn, Matplotlib, Seaborn.
-   **PySpark e Spark MLlib:** Para processamento e modelagem no Databricks.
-   **GridSearchCV, RandomizedSearchCV, Optuna:** Para ajuste de hiperparâmetros.
-   **Databricks SQL:** Para visualizações e dashboards.
-   **YAML:** Para arquivos de configuração.
-   **Delta Lake (opcional):** Para armazenamento de dados no Databricks.
-   **pytest:** Para teste de qualidade.

## Estrutura de Arquivos
```
previsao_demanda/
├── data/               # raw, processed, features
├── notebooks/          # 01 a 07 (EDA, pré-processamento, engenharia, modelos, tuning)
├── scripts/            # Modularização das funções usadas
├── config/             # config.yaml
├── databricks/         # scripts para ingestão, modelagem e dashboards no ambiente Spark
├── reports/            # resultados, gráficos, imagens
├── README.md           # instruções do projeto
├── CHANGELOG.md        # controle de versões
```
## Contribuindo

Contribuições para este projeto são bem-vindas! Consulte o arquivo `CHANGELOG.md` para acompanhar as atualizações.