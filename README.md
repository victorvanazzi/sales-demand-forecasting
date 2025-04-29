# Previsão de Vendas em Varejista Global de Eletrônicos

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=Databricks&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADD8?style=for-the-badge&logo=delta&logoColor=white)
![Prophet](https://img.shields.io/badge/Prophet-007ACC?style=for-the-badge&logo=prophet&logoColor=white)
![SARIMA](https://img.shields.io/badge/SARIMA-4B275F?style=for-the-badge&logo=statsmodels&logoColor=white)

## Sobre o Projeto

Este projeto implementa um pipeline completo de análise e modelagem preditiva para vendas mensais de uma rede varejista global de eletrônicos. Utilizando a arquitetura Medallion (Bronze → Silver → Gold) no Databricks, o trabalho abrange desde a ingestão dos dados brutos até a geração de previsões com modelos de séries temporais.

### Desafio

Prever vendas mensais com precisão, considerando:
- Sazonalidade nas vendas de eletrônicos
- Impacto da pandemia COVID-19 no período final da série de dados
- Necessidade de validar modelos em cenários pré-pandemia e durante a pandemia

### Origem dos Dados

Dados obtidos do [Maven Analytics Data Playground](https://mavenanalytics.io/data-playground) - Global Electronics Retailer.

## Arquitetura

O projeto segue a arquitetura Medallion do Databricks:

```
Raw Data → Bronze → Silver → Gold → Modelos Preditivos
    ↑          ↑        ↑        ↑          ↑
Ingestão    Validação  Joins   Agregação  Previsão
           Schema     Limpeza  Métricas
```

## Tabelas e Estrutura de Dados

O conjunto de dados inclui as seguintes tabelas:

### Tabela: Sales (Vendas)
- `Order Number`, `Line Item`, `Order Date`, `Delivery Date`
- `CustomerKey`, `StoreKey`, `ProductKey`, `Quantity`, `Currency Code`

### Tabela: Customers (Clientes)
- `CustomerKey`, `Name`, `Gender`, `City`, `State`
- `Country`, `Continent`, `Birthday`

### Tabela: Products (Produtos)
- `ProductKey`, `Product Name`, `Brand`, `Color`
- `Unit Cost USD`, `Unit Price USD`, `Category`, `Subcategory`

### Tabela: Stores (Lojas)
- `StoreKey`, `Country`, `State`, `Square Meters`, `Open Date`

### Tabela: Exchange Rates (Taxas de Câmbio)
- `Date`, `Currency`, `Exchange`

## Pipeline de Processamento

### 1. Camada Bronze (Ingestão)

- Ingestão de arquivos CSV brutos
- Padronização dos dados
- Armazenamento em formato Delta
- Registro no catálogo SQL

### 2. Camada Silver (Transformação)

- Joins entre tabelas Bronze
- Conversão de valores para USD usando taxas de câmbio
- Tratamento de nulos e inconsistências de dados
- Armazenamento em Delta Tables para análise posterior

### 3. Camada Gold (Agregação)

- Agregação mensal das vendas totais em USD
- Geração de série temporal contínua
- Preparação dos dados para modelagem

## Modelagem e Análise

### Abordagem Metodológica

Para lidar com a ruptura causada pela pandemia, foram adotadas duas janelas de treinamento/teste:

1. **Cenário Pré-Pandemia**
   - Treino: 2016-2018
   - Teste: 2019

2. **Cenário Pandêmico**
   - Treino: 2016-2019
   - Teste: 2020-Fev/2021

### Modelos Implementados

#### 1. SARIMA (Seasonal ARIMA)
- Implementação via statsmodels
- Tratamento de outliers nos resíduos
- Ajuste de parâmetros para capturar sazonalidade

#### 2. Prophet (Facebook/Meta)
- Modelagem de tendência, sazonalidade e feriados
- Decomposição automática de componentes da série
- Intervalos de confiança para previsões

### Resultados Comparativos (Cenário Pré-Pandemia)

| Modelo  | MAE     | RMSE    | R²     |
|---------|---------|---------|--------|
| SARIMA  | 350.027 | 384.946 | 0,5313 |
| Prophet | 278.796 | 298.397 | 0,7203 |

## Principais Conclusões

- **Prophet superou SARIMA** em todos os indicadores para o período pré-pandemia
- Ambos os modelos apresentaram dificuldades em prever o choque da pandemia (como esperado)
- A série temporal apresenta forte sazonalidade anual que foi bem capturada pelo Prophet
- O modelo final escolhido foi o **Prophet** devido à sua capacidade superior de capturar tendências e sazonalidade

## Execução do Projeto

### Links para Notebooks Databricks

1. [00_bronze_ingestao](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3571773272081801/4034231636992376/latest.html)
2. [01_silver_transformacoes](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3912297781853264/4034231636992376/latest.html)
3. [02_gold_agregacoes_mensais](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3912297781853391/4034231636992376/latest.html)
4. [03a_exploracao_inicial](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3912297781853357/4034231636992376/latest.html)
5. [03b_modelagem_sarima](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3912297781853318/4034231636992376/latest.html)
6. [03c_modelagem_prophet](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4247253332321926/3912297781853344/4034231636992376/latest.html)

## Requisitos

- Databricks Runtime 10.4 LTS ou superior
- Python 3.8+
- Bibliotecas: pandas, numpy, statsmodels, prophet, matplotlib, seaborn

## Trabalhos Futuros

- Incorporação de variáveis exógenas (indicadores econômicos, feriados regionais)
- Teste de modelos híbridos considerando mudanças de regime
- Implementação de validação cruzada com janela rolante
- Agregações em diferentes níveis (categoria de produto, região geográfica)
- Experimentação com modelos mais avançados (LSTM, modelos híbridos)

## Licença

Este projeto é disponibilizado sob a licença MIT.
