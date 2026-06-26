# Análise Preditiva do Preço de Fechamento do Bitcoin (BTC-USD)

**Centro Universitário - Católica de Santa Catarina**  
**Curso:** Engenharia de Software | **Disciplina:** Análise Preditiva

**Grupo:**
- Ana Carolina Fanhani Stralioti
- Gustavo do Prado
- Jairson Steinert

---

## 1. Introdução e Problema

O mercado de criptomoedas, especialmente o Bitcoin, é caracterizado por extrema volatilidade e complexidade, influenciado por uma vasta gama de fatores como os geopolíticos.
A incapacidade de antecipar com precisão os movimentos do preço de fechamento resulta em riscos acentuados e dificuldades no planejamento estratégico.
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

| Coluna | Descrição |
|--------|-----------|
| Date | Data do registro da transação |
| Open / High / Low / Close | Preços de abertura, máximo, mínimo e fechamento diário |
| Volume | Volume total de negociação no dia |
| SMA_7 | Média móvel simples de 7 dias (sentimento de curto prazo) |
| SMA_30 | Média móvel simples de 30 dias (tendência de médio/longo prazo) |
| Target | Variável alvo representando o preço de fechamento do dia seguinte ($D+1$) |

## 5. Metodologia Científica

### Etapa 1: Auditoria de Dados (Diagnóstico)
* **Tratamento de Nulos:** Identificação de valores `NaN` no início da série devido ao cálculo das médias móveis. Tratamento realizado via `dropna()`, resultando em 1874 linhas consistentes.
* **Outliers:** Optou-se pela manutenção dos outliers para preservar a não-linearidade real do mercado cripto, aplicando-se normalização.
* **Consistência:** Padronização da frequência diária com índice temporal na coluna `Date`.

### Etapa 2: Tratamento e Preparação (ETL)
* **Armazenamento:** Consolidação em base de dados local SQLite (`bitcoin_data.db`).
* **Escalonamento:** Utilização do `MinMaxScaler` do *scikit-learn* (escala 0 a 1) para tratar a amplitude dos valores nominais.
* **Engenharia de Features:** Criação das médias móveis e deslocamento da variável alvo via `shift(-1)`.

### Etapa 3: Análise Exploratória (EDA)
* **Validação de Hipóteses:**
    * **H1:** A combinação de SMA_7 e SMA_30 possui maior peso preditivo do que o preço `Open` isolado.
    * **H2:** Picos anômalos de volume precedem dias com maior margem de erro (RMSE) devido à volatilidade irracional.

## 6. Data Storytelling: Sinal vs. Ruído

A análise demonstrou que o preço isolado não conta toda a história. É a relação temporal — as médias móveis combinadas com o volume — que dita a força da tendência.
* **Cruzamentos:** Momentos em que a SMA_7 cruza a SMA_30 precedem frequentemente movimentos direcionais fortes.
* **Desvios:** Picos onde o preço real descola drasticamente da SMA_30 sinalizam comportamentos de risco e complexidade elevada.

## 7. Tecnologias Utilizadas (N1/N2)

`yfinance` · `pandas` · `SQLite3` · `scikit-learn` · `matplotlib`

---

## N3 — Produtização de ML

### Estrutura de Arquivos

```
projeto_ap/
│
├── N3/                                  ← ENTREGA N3
│   ├── N3_Bitcoin_Predicao.ipynb        ← Notebook principal (10 etapas)
│   ├── modelos/                         ← Modelo campeão serializado
│   │   └── arima_campeao_meta.json      ← Metadados e métricas do modelo salvo
│   └── figuras/                         ← Gráficos gerados pelo notebook
│       ├── fig_01_serie_historica.png
│       ├── fig_02_split.png
│       ├── fig_03_baselines.png
│       ├── fig_04_decomposicao.png
│       ├── fig_05_acf_pacf.png
│       ├── fig_06_prophet_componentes.png
│       ├── fig_07_overfitting.png
│       ├── fig_08_comparacao_modelos.png
│       ├── fig_09_diagnostico_residuos.png
│       └── fig_10_previsao_final.png
│
├── Atividade_AP.ipynb                   ← Notebook N1/N2
├── dados_bitcoin_preparados.csv         ← Base gerada pelo N1/N2
├── .env                                 ← Credenciais (não vai ao GitHub)
├── .env.example                         ← Template de credenciais
└── .gitignore
```

### 10 Etapas do Notebook

| Etapa | Descrição | Célula |
|-------|-----------|--------|
| 1 | Auditoria da série temporal (nulos, gaps, outliers) | `c04`, `c05` |
| 2 | Engenharia de atributos (lags, médias móveis, encoding cíclico) | `c08`, `c09` |
| 3 | Baselines e régua de desempenho (Naïve, MM7) | `c12` |
| 4 | EDA formal: decomposição, ADF, ACF/PACF, estacionariedade | `c14`, `c15`, `c16` |
| 5 | 3 modelos de famílias distintas (ARIMA, Holt-Winters, Prophet) | `c18`, `c19`, `c20` |
| 6 | Hiperparâmetros, seleção por AIC, demonstração de overfitting | `c22`, `c23` |
| 7 | Avaliação comparativa (MAE, RMSE, MAPE) e diagnóstico de resíduos | `c25`, `c26` |
| 8 | Validação temporal walk-forward (expanding window, step=14 dias) | `c28` |
| 9 | Previsão final 30 dias com IC 95% + storytelling executivo | `c30`, `c31` |
| 10 | Rastreio de experimentos com Weights & Biases (wandb) | `c33`, `c34` |

### Modelos Implementados

| Modelo | Família | Seminário |
|--------|---------|-----------|
| ARIMA(p,1,q) — seleção por AIC | Modelos lineares AR/MA | Seminário 2 |
| Holt-Winters Aditivo (p=7) | Suavização exponencial | Seminário 4 |
| Prophet Multiplicativo | Modelos modernos / ML | Seminário 5 |

### Correções em Relação ao N1/N2

* **Data leakage corrigido:** o `MinMaxScaler` era ajustado em todos os dados antes do split. No N3, os modelos usam preços brutos (USD) e o scaler é ajustado apenas no conjunto de treino.
* **EDA formal adicionada:** decomposição sazonal, teste ADF, ACF/PACF e distribuição dos retornos.
* **Feature engineering expandido:** lags autorregressivos, retornos logarítmicos, volatilidade realizada e encoding cíclico de data.
* **Diagnóstico de resíduos:** teste Ljung-Box, Q-Q plot e ACF dos resíduos do modelo campeão.
* **Rastreio de experimentos:** todos os runs logados no wandb com hiperparâmetros e métricas.

### Rastreio de Experimentos — wandb

Projeto: **`bitcoin-predicao-n3`**  
Runs: `baseline_persistencia`, `baseline_mm7`, `arima_p_1_q`, `holtwinters_add_p7`, `prophet_multiplicativo`

> A chave de API fica no arquivo `.env` (não enviado ao GitHub). Use `.env.example` como template.

### Como Executar o N3 Localmente

```bash
# 1. Clonar o repositório
git clone https://github.com/anacstralioti/projeto-ap.git
cd projeto_ap

# 2. Criar e ativar o ambiente virtual
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Linux/Mac

# 3. Instalar dependências
pip install yfinance statsmodels prophet wandb scikit-learn matplotlib seaborn scipy python-dotenv

# 4. Configurar credenciais
copy .env.example .env         # editar e inserir WANDB_API_KEY

# 5. Abrir o notebook
jupyter notebook N3/N3_Bitcoin_Predicao.ipynb
```

### Tecnologias Utilizadas (N3)

`Python 3.13` · `yfinance` · `pandas` · `statsmodels` · `prophet` · `scikit-learn` · `wandb` · `matplotlib` · `seaborn` · `scipy` · `python-dotenv`
