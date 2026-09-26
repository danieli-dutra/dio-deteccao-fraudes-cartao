# Detecção de Fraudes em Transações de Cartão de Crédito

Projeto desenvolvido durante o bootcamp **Bradesco - GenAI & Dados**, da DIO.

O objetivo é aplicar técnicas de Machine Learning para identificar possíveis fraudes em transações de cartão de crédito, trabalhando com uma base altamente desbalanceada.

## Sobre o projeto

Em problemas de detecção de fraude, a quantidade de transações legítimas é muito maior que a quantidade de transações fraudulentas. Por isso, a acurácia sozinha pode apresentar uma visão distorcida do desempenho do modelo.

Neste projeto, o foco principal foi o **recall da classe de fraude**, considerando também precision, F1-score e ROC-AUC.

Foram comparados três modelos:

- Regressão Logística
- Random Forest
- XGBoost

Também foi realizado ajuste do threshold de decisão e uma análise de explicabilidade utilizando SHAP.

## Dataset

Foi utilizado o dataset público de transações de cartão de crédito disponibilizado na aula do bootcamp.

O dataset é carregado diretamente por URL no notebook e não está armazenado neste repositório.

As principais colunas são:

- `Time`: tempo decorrido desde a primeira transação;
- `Amount`: valor da transação;
- `Class`: variável alvo, onde `0` representa uma transação normal e `1` representa uma fraude;
- `V1` a `V28`: variáveis transformadas por PCA.

## Etapas do projeto

### 1. Exploração dos dados

Foi realizada uma análise inicial da estrutura da base e da distribuição das classes, identificando o forte desbalanceamento entre transações normais e fraudes.

### 2. Preparação dos dados

Foram realizadas as seguintes etapas:

- criação da variável `LogAmount`;
- separação entre variáveis preditoras e variável alvo;
- divisão dos dados em treino, validação e teste;
- utilização de `stratify` para preservar a proporção das classes;
- padronização dos dados utilizando `StandardScaler`.

### 3. Treinamento dos modelos

Foram treinados e comparados:

- Regressão Logística;
- Random Forest;
- XGBoost.

Os modelos foram avaliados principalmente pelo recall da classe de fraude, além de precision, F1-score e ROC-AUC.

## Resultados

| Modelo | Precisão | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|
| Regressão Logística | 6,08% | 90,91% | 11,40% | 96,66% |
| Random Forest | 98,67% | 74,75% | 85,06% | 95,33% |
| XGBoost | 73,68% | 84,85% | 78,87% | 97,38% |

Os resultados mostram que diferentes métricas apresentam perspectivas diferentes sobre o comportamento dos modelos. A Regressão Logística, por exemplo, apresentou recall elevado, mas precision baixa.

## Ajuste do threshold

Além da utilização do threshold padrão de `0,50`, foram testados diferentes limiares utilizando o conjunto de validação.

Foi utilizado o XGBoost nessa etapa e o threshold de `0,70` foi selecionado considerando um recall mínimo de aproximadamente 80% e buscando melhorar a precision.

Na validação, o threshold de `0,70` apresentou:

- Precision: 77,45%;
- Recall: 80,61%;
- F1-score: 79,00%.

No conjunto de teste, a alteração do threshold de `0,50` para `0,70` resultou em:

| Métrica | Threshold 0,50 | Threshold 0,70 |
|---|---:|---:|
| Precision | 73,68% | 82,00% |
| Recall | 84,85% | 82,83% |
| F1-score | 78,87% | 82,41% |

Essa etapa mostrou como o threshold pode alterar o equilíbrio entre a identificação de fraudes e a quantidade de falsos positivos.

## Explicabilidade com SHAP

Foi utilizado o SHAP para analisar quais variáveis tiveram maior influência nas previsões do XGBoost.

Também foi realizada uma explicação individual de uma transação classificada pelo modelo com **99,98% de probabilidade prevista de fraude**.

Na análise individual, `V14` apresentou a maior contribuição positiva para a saída do modelo, seguida por `V4`, `V10` e `V17`.

Como as variáveis `V1` a `V28` são transformações obtidas por PCA, seus nomes não permitem uma interpretação direta como características de negócio. A análise SHAP foi utilizada, portanto, para entender a influência dessas variáveis na decisão do modelo.

## Diferenças em relação à aula

Além de seguir o pipeline apresentado durante o bootcamp, foram realizadas algumas adaptações:

- criação de uma etapa de validação separada do conjunto de teste;
- análise de diferentes thresholds;
- organização dos resultados dos modelos em uma tabela;
- utilização adicional da curva Precision-Recall;
- análise global e individual utilizando SHAP.

A etapa de validação foi utilizada para auxiliar na escolha do threshold, evitando fazer essa escolha diretamente sobre o conjunto de teste.

## Possíveis melhorias

Como próximos passos, seria possível:

- testar estratégias de undersampling e oversampling;
- ampliar a busca de hiperparâmetros;
- testar outros algoritmos de classificação;
- criar novas variáveis relacionadas ao comportamento das transações;
- comparar diferentes estratégias de balanceamento das classes.

## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- SHAP
- Matplotlib
- Seaborn
- Google Colab

## Estrutura

```text
dio-deteccao-fraudes-cartao/
├── deteccao_fraudes_cartao.ipynb
└── README.md
```

## Bootcamp

Projeto desenvolvido como desafio de projeto do bootcamp **Bradesco - GenAI & Dados**, da DIO.
