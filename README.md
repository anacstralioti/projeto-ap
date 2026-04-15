# Análise Preditiva do Preço de Fechamento do Bitcoin (BTC-USD)

**Centro Universitário - Católica de Santa Catarina**
**Curso:** Engenharia de Software
**Disciplina:** Análise Preditiva

* **Ana Carolina Fanhani Stralioti**
* **Gustavo do Prado**
* **Jairson Steinert**

---

## 1. Introdução e Problema
O mercado de criptomoedas, especialmente o Bitcoin, é caracterizado por extrema volatilidade e complexidade, influenciado por uma vasta gama de fatores como os geopolíticos. 
A incapacidade de antecipar com precisão os movimentos do preço de fechamento resulta em riscos acentuados e dificuldades no planeamento estratégico.
Este projeto visa mitigar essa lacuna, fornecendo uma ferramenta preditiva baseada em dados para uma melhor compreensão do comportamento do mercado.

## 2. Objetivos e Metas
O objetivo é analisar dados históricos para identificar padrões e tendências, fornecendo insights baseados em análise exploratória e visualização.

**Métricas de Sucesso:**
* **R² Score:** Coeficiente de determinação superior a 0.60.
* **Direção de Previsão:** Acurácia superior a 50% na previsão de alta ou baixa.
* **Minimização de Erros:** Avaliação através de MAE (Erro Médio Absoluto) e RMSE (Erro Quadrático Médio).

## 3. Escopo do Projeto
* **Incluso:** Desenvolvimento de modelo de ML para previsão do preço de fechamento diário ($D+1$); utilização de dados históricos OHLCV do Yahoo Finance; criação de indicadores SMA_7 e SMA_30.
* **Excluso:** Previsão de outras criptomoedas; implementação de sistemas de negociação automatizados; consultoria financeira direta.

## 4. Dicionário de Dados
Os dados abrangem o período de janeiro de 2021 a março de 2026:
* **Date:** Data do registro da transação.
* **Open / High / Low / Close:** Preços de abertura, máximo, mínimo e fechamento diário.
* **Volume:** Volume total de negociação no dia.
* **SMA_7:** Média móvel simples de 7 dias (sentimento de curto prazo).
* **SMA_30:** Média móvel simples de 30 dias (tendência de médio/longo prazo).
* **Target:** Variável alvo representando o preço de fechamento do dia seguinte ($D+1$).

## 5. Metodologia Científica

### Etapa 1: Auditoria de Dados (Diagnóstico)
* **Tratamento de Nulos:** Identificação de valores `NaN` no início da série devido ao cálculo das médias móveis. Tratamento realizado via `dropna()`, resultando em 1874 linhas consistentes.
* **Outliers:** Optou-se pela manutenção dos outliers para preservar a não-linearidade real do mercado cripto, aplicando-se normalização.
* **Consistência:** Padronização da frequência diária com índice temporal na coluna `Date`.

### Etapa 2: Tratamento e Preparação (ETL)
* **Armazenamento:** Consolidação em base de dados local SQLite (`crypto_data.db`).
* **Escalonamento:** Utilização do `MinMaxScaler` do *scikit-learn* (escala 0 a 1) para tratar a amplitude dos valores nominais.
* **Engenharia de Features:** Criação das médias móveis e deslocamento da variável alvo via `shift(-1)`.

### Etapa 3: Análise Exploratória (EDA)
* **Validação de Hipóteses:**
    * **H1:** A combinação de SMA_7 e SMA_30 possui maior peso preditivo do que o preço `Open` isolado.
    * **H2:** Picos anómalos de volume precedem dias com maior margem de erro (RMSE) devido à volatilidade irracional.

## 6. Data Storytelling: Sinal vs. Ruído
A análise demonstrou que o preço isolado não conta toda a história. É a relação temporal — as médias móveis combinadas com o volume — que dita a força da tendência.
* **Cruzamentos:** Momentos em que a SMA_7 cruza a SMA_30 precedem frequentemente movimentos direcionais fortes.
* **Desvios:** Picos onde o preço real descola drasticamente da SMA_30 sinalizam comportamentos de risco e complexidade elevada.

## 7. Tecnologias Utilizadas
* **Bibliotecas:** `yfinance`, `Pandas`, `SQLite3`, `Scikit-learn`, `Matplotlib`.

---
