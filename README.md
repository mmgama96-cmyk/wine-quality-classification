# 🍷 Tech Challenge – Fase 2: Classificação da Qualidade de Vinhos com Machine Learning

## 📌 Sobre o projeto

Este projeto tem como objetivo desenvolver modelos de Machine Learning capazes de classificar vinhos de acordo com sua qualidade, utilizando características físico-químicas como teor alcoólico, acidez, densidade, sulfatos e outros componentes.

A proposta consiste em transformar a avaliação original dos vinhos em um problema de classificação binária, identificando quais características estão mais associadas a vinhos de alta qualidade e avaliando a capacidade dos modelos de realizar essas previsões.

O projeto foi desenvolvido utilizando Python, análise exploratória de dados e algoritmos de aprendizado supervisionado.

## 🎯 Objetivos

- Analisar as características físico-químicas dos vinhos.
- Identificar padrões e correlações entre as variáveis.
- Investigar quais características estão mais associadas à qualidade.
- Desenvolver e comparar diferentes modelos de classificação.
- Avaliar o desempenho dos modelos considerando o desbalanceamento das classes.
- Identificar o modelo mais adequado de acordo com o objetivo do negócio.

## 📊 Base de dados

O projeto utiliza o dataset `WineQT.csv`, que contém informações sobre as propriedades físico-químicas dos vinhos e suas respectivas avaliações de qualidade.

A variável alvo original é `quality`, que foi transformada em uma variável binária:

| Classificação | Critério |
|---|---|
| 1 – Alta qualidade | Nota igual ou superior a 7 |
| 0 – Baixa/média qualidade | Nota inferior a 7 |

Após a remoção da coluna de identificação (`Id`) e de 125 registros duplicados, a base utilizada na análise ficou com **1.018 vinhos**.

Distribuição das classes:

- Baixa/média qualidade: 881 vinhos (86,5%).
- Alta qualidade: 137 vinhos (13,5%).

O desbalanceamento das classes foi considerado durante o desenvolvimento e a avaliação dos modelos.

## 🔎 Etapas do projeto

### 1. Compreensão do problema
Definição da variável alvo e transformação do problema em uma classificação binária.

### 2. Análise Exploratória de Dados (EDA)
Investigação da distribuição das variáveis, correlações, outliers e diferenças entre vinhos de alta e baixa/média qualidade.

Principais descobertas:
- O teor alcoólico apresentou a maior correlação positiva com a qualidade (aproximadamente 0,49).
- A acidez volátil apresentou a maior correlação negativa (aproximadamente -0,41).
- Vinhos de maior qualidade apresentaram, em geral, maior teor alcoólico e menor acidez volátil.
- Outliers foram mantidos por representarem possíveis características reais dos vinhos.

### 3. Pré-processamento
- Remoção de registros duplicados.
- Exclusão da coluna de identificação.
- Separação entre variáveis preditoras e variável alvo.
- Divisão dos dados em treino (80%) e teste (20%).
- Estratificação para preservar a proporção das classes.
- Padronização das variáveis para os modelos sensíveis à escala.

### 4. Desenvolvimento dos modelos

Foram desenvolvidos três modelos de classificação:

- **Regressão Logística:** modelo utilizado como abordagem de classificação linear.
- **Random Forest:** modelo baseado em múltiplas árvores de decisão.
- **Gradient Boosting:** modelo baseado no treinamento sequencial de árvores.

Foi utilizada a estratégia `class_weight='balanced'` nos modelos em que essa opção estava disponível, buscando reduzir o impacto do desbalanceamento das classes.

### 5. Avaliação dos modelos

Os modelos foram avaliados utilizando:

- Acurácia (Accuracy)
- Precisão (Precision)
- Recall
- F1-score
- ROC-AUC

Também foi aplicada validação cruzada estratificada com 5 divisões (Stratified K-Fold), para avaliar a consistência dos resultados.

### 6. Ajuste do ponto de corte

Foi realizado o ajuste do threshold da Random Forest, buscando melhorar o equilíbrio entre precisão e recall.

O ponto de corte foi selecionado por validação cruzada utilizando exclusivamente os dados de treinamento, evitando utilizar o conjunto de teste nessa escolha.

## 📈 Resultados

### Comparação dos modelos – Validação cruzada

Média ± desvio-padrão das cinco rodadas:

| Modelo | Acurácia | Precisão | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Regressão Logística | 0,798 | 0,386 | 0,811 | 0,521 | 0,878 |
| Random Forest | 0,894 | 0,731 | 0,352 | 0,452 | 0,883 |
| Gradient Boosting | 0,882 | 0,586 | 0,395 | 0,466 | 0,870 |

A validação cruzada demonstrou que os modelos apresentam capacidades de separação semelhantes, mas diferentes comportamentos na identificação dos vinhos de alta qualidade.

### Random Forest – Ajuste do threshold

O ajuste do ponto de corte de 50% para 25% apresentou uma melhora importante na identificação dos vinhos de alta qualidade.

| Métrica | Threshold 0,50 | Threshold 0,25 |
|---|---:|---:|
| Acurácia | 0,907 | 0,887 |
| Precisão | 0,900 | 0,556 |
| Recall | 0,333 | 0,741 |
| F1-score | 0,486 | 0,635 |

Com o threshold de 25%, o modelo passou a identificar 20 dos 27 vinhos de alta qualidade presentes no conjunto de teste, contra apenas 9 identificados anteriormente.

O F1-score aumentou de 0,486 para 0,635, representando o melhor resultado obtido no conjunto de teste.

## 🧪 Principais insights

A interpretação dos modelos destacou três características relevantes:

**1. Teor alcoólico**

Foi a variável de maior importância nos dois modelos. Vinhos com maior teor alcoólico apresentaram maior associação com avaliações positivas.

**2. Acidez volátil**

Apresentou associação negativa com a qualidade. Valores mais elevados tendem a estar relacionados a avaliações inferiores.

**3. Sulfatos**

Apresentaram associação positiva com a qualidade e foram identificados como uma das variáveis relevantes para a classificação.

Essas relações representam associações identificadas na base de dados, não necessariamente relações de causa e efeito.

## 🛠️ Tecnologias e bibliotecas utilizadas

- Python
- Google Colab
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Machine Learning
- Análise Exploratória de Dados (EDA)
- Validação Cruzada (Cross-Validation)

## 📂 Estrutura do projeto

```text
Tech-Challenge-Fase-2/
│
├── data/
│   └── WineQT.csv
│
├── notebooks/
│   └── Colab_Tech_Challenge_2_Vinhos.ipynb
│
└── README.md
```

## 🚀 Como executar o projeto

1. Clone este repositório:

```bash
git clone URL_DO_REPOSITORIO
```

2. Instale as bibliotecas necessárias:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

3. Abra o notebook no Google Colab ou Jupyter Notebook.

4. Disponibilize o arquivo `WineQT.csv` na pasta de dados e ajuste o caminho de leitura no notebook, se necessário.

5. Execute as células sequencialmente para reproduzir as análises, o treinamento e a avaliação dos modelos.

## 💡 Conclusão

O projeto demonstrou que técnicas de Machine Learning podem auxiliar na identificação de padrões associados à qualidade dos vinhos a partir de suas características físico-químicas.

A Random Forest com threshold ajustado para 25% apresentou o melhor F1-score no conjunto de teste (0,635), enquanto a Regressão Logística demonstrou maior capacidade de identificar vinhos de alta qualidade na validação cruzada, com recall médio de 0,811.

A escolha do modelo depende do objetivo do produtor: priorizar a identificação da maior quantidade possível de vinhos promissores ou aumentar a segurança das classificações positivas.

Como próximos passos, destacam-se o ajuste de hiperparâmetros e a validação dos modelos com uma base de dados maior e mais representativa.

---

**Projeto desenvolvido como parte do Tech Challenge – Fase 2.**

