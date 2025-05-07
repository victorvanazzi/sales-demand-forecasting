# Previsão de Demanda de Vendas

![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)
![Databricks](https://img.shields.io/badge/Databricks-PySpark-orange.svg)
![Modelos](https://img.shields.io/badge/Modelos-SARIMA|Prophet-green.svg)
![Status](https://img.shields.io/badge/Status-Completo-brightgreen.svg)

> Previsão de demanda mensal para uma varejista global de eletrônicos, com foco na seleção do melhor modelo de séries temporais para projeções de vendas agregadas.

## Ecossistema de Projetos

Este repositório faz parte de um ecossistema com três projetos modulares:

| Repositório                      | Função                    | Descrição                                                      |
|----------------------------------|---------------------------|----------------------------------------------------------------|
| [`core-data-pipeline`](https://github.com/seuusuario/core-data-pipeline)     | Ingestão e Transformação | Pipeline de dados com arquitetura Medallion (Bronze → Silver → Gold) |
| [`sales-performance-bi`](https://github.com/seuusuario/sales-performance-bi) | Visualização             | Análise de desempenho comercial no Power BI                    |
| **`sales-demand-forecasting`**   | Modelagem Preditiva       | **Previsão de vendas mensais com séries temporais**            |

## Estrutura do Repositório

```

sales-demand-forecasting/
│
├── notebooks/
│   ├── 03a\_exploracao\_inicial.ipynb    # Análise exploratória da série temporal
│   ├── 03b\_modelagem\_sarima.ipynb      # Modelagem SARIMA com validação temporal
│   └── 03c\_modelagem\_prophet.ipynb     # Implementação do Prophet sem e com feriados
│
└── README.md                           # Documentação do projeto

```

## Objetivo

Selecionar o modelo de previsão com melhor desempenho para a série `sales_monthly`, que representa o faturamento mensal (USD) de uma varejista global de eletrônicos.

## Metodologia

### 1. Análise Exploratória

- **Visualização**: Gráficos temporais e análise de dispersão  
- **Decomposição**: Isolamento de tendência, sazonalidade e resíduos via STL  
- **Diagnóstico**: Identificação de outliers e comportamentos atípicos, especialmente durante a pandemia  

### 2. Modelagem SARIMA

- **Pré-processamento**: Tratamento de outliers nos resíduos via winsorização  
- **Configuração**: SARIMA(1,1,1)(1,1,1,12) com inicialização 'approximate_diffuse'  
- **Validação**: Expanding Window com 7 folds sequenciais de 3 meses cada  
- **Métricas**: MAE, RMSE e R² para cada período de validação  

### 3. Modelagem Prophet

- **Variações**: Implementação com e sem feriados dos EUA como variáveis exógenas  
- **Estrutura**: Mesma abordagem de validação temporal usada no SARIMA  
- **Sazonalidade**: Configuração com sazonalidade anual automaticamente detectada  

### 4. Validação Temporal

- **Fold 1**: Treino até Dez/2018 · Validação Jan–Mar/2019  
- **Fold 2**: Treino até Mar/2019 · Validação Abr–Jun/2019  
- **Fold 3**: Treino até Jun/2019 · Validação Jul–Set/2019  
- **Fold 4**: Treino até Set/2019 · Validação Out–Dez/2019  
- **Fold 5**: Treino até Dez/2019 · Validação Jan–Mar/2020  
- **Fold 6**: Treino até Mar/2020 · Validação Abr–Jun/2020  
- **Fold 7**: Treino até Jun/2020 · Validação Jul–Set/2020  

## Resultados

| Período       | Contexto              | Melhor Modelo | R²            | MAE    |
|---------------|-----------------------|---------------|---------------|--------|
| Jan–Jun 2019  | Estabilidade          | **SARIMA**    | 0.67 / 0.71   | 263k / 274k |
| Jul–Set 2019  | Baixa variabilidade   | **SARIMA**    | 0.01          | 44k    |
| Out–Dez 2019  | Sazonalidade definida | **SARIMA**    | 0.89          | 111k   |
| Jan–Mar 2020  | Início da pandemia    | **SARIMA**    | 0.83          | 261k   |
| Abr–Set 2020  | Ruptura COVID-19      | Nenhum        | R² negativo   | >500k  |

## Modelo Promovido

**SARIMA** foi adotado como modelo de produção, por apresentar:

- **Maior precisão**: Menores erros absolutos em todos os períodos estáveis  
- **Explicabilidade**: Maior R² na maioria dos folds analisados  
- **Robustez**: Melhor desempenho frente a mudanças moderadas de comportamento  
- **Adaptabilidade**: Capacidade superior de ajuste a variações sazonais  

## Tecnologias e Ferramentas

| Categoria            | Ferramentas                           |
|----------------------|---------------------------------------|
| **Linguagem**        | Python 3.7+                           |
| **Ambiente**         | Databricks com PySpark               |
| **Análise de Dados** | pandas, NumPy, Matplotlib             |
| **Modelagem**        | statsmodels (SARIMA), Prophet         |
| **Validação**        | Expanding Window com 7 folds temporais |
| **Métricas**         | MAE, RMSE, R²                         |

## Insights Principais

- **Sazonalidade anual**: Padrões recorrentes de alta e baixa nas vendas  
- **Limitação de modelos estatísticos**: Falha na previsão de eventos extremos como a pandemia  
- **Superioridade do SARIMA**: Melhor adaptação aos padrões da série temporal específica  
- **Adição de feriados**: Não melhorou significativamente o desempenho do Prophet  

## Observações

- Os dados foram extraídos do [Maven Analytics Data Playground](https://www.mavenanalytics.io/data-playground)
- Os dados utilizados vão até fevereiro de 2021, impossibilitando a validação completa pós-pandemia  
---

*Projeto desenvolvido como demonstração de habilidades em modelagem de séries temporais e previsão de demanda.*  