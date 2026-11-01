# Tech Challenge Fase 2: Classificando a qualidade de vinhos com Machine Learning

## Objetivo

Desenvolver um modelo de classificação capaz de prever se um vinho é de alta qualidade (nota 7 ou mais) ou de baixa/média qualidade (nota abaixo de 7), a partir das suas características físico-químicas.

## Base de dados

Wine Quality Dataset (Kaggle), com 1.143 amostras de vinho tinto e 11 variáveis físico-químicas, além da nota de qualidade atribuída por especialistas.

## Metodologia

O projeto foi estruturado com base na metodologia CRISP-DM:

1. Compreensão do problema
2. Análise exploratória de dados (EDA)
3. Pré-processamento
4. Desenvolvimento dos modelos
5. Avaliação dos modelos
6. Interpretação dos resultados

## Principais análises

### Etapa 1: Compreensão do Problema

A variável alvo é a qualidade do vinho (quality), uma nota dada por especialistas. Para simplificar o problema, transformamos essa nota em uma classificação binária:

1 = alta qualidade (nota 7 ou mais)
0 = baixa/média qualidade (nota abaixo de 7)

### Etapa 2: Análise Exploratória de Dados (EDA)

Nesta etapa investigamos a distribuição das variáveis, as correlações entre elas, a presença de outliers e o balanceamento das classes.

A distribuição do teor alcoólico apresenta um pico de frequência logo acima de 9%, indicando que a maior parte das amostras possui graduação alcoólica relativamente moderada. A distribuição se concentra entre 9% e 11%, com média de 10,45% e desvio-padrão de 1,10, o que mostra que valores muito baixos ou muito elevados são menos frequentes.

### Presença de outliers

A análise do boxplot do teor alcoólico identificou a presença de outliers. Como esses valores podem refletir características reais de determinados vinhos e não necessariamente erros de medição, optou-se por mantê-los na base de dados.

Resumo do que sabemos até aqui sobre o álcool

Pico de frequência logo acima de 9%
Média de 10,45%
Distribuição concentrada em torno dos valores centrais
Presença de poucos outliers
Variável aparentemente consistente

### Maiores Correlações

quality e quality_binary = 0,71
density e fixed acidity = 0,68
citric acid e fixed acidity = 0,67
free sulfur dioxide e total sulfur dioxide = 0,66

### Menores Correlações

pH e fixed acidity = -0,69
citric acid e pH = -0,55
citric acid e volatile acidity = -0,54
density e alcohol = -0,50

### Resultados da Matriz de Correlação

A análise da matriz de correlação revelou relações relevantes entre algumas das variáveis físico-químicas do conjunto de dados. Entre as correlações positivas mais fortes, destacam-se as relações entre densidade e acidez fixa (r = 0,68), entre acidez fixa e ácido cítrico (r = 0,67) e entre dióxido de enxofre livre e dióxido de enxofre total (r = 0,66). Esses resultados indicam que essas variáveis tendem a aumentar juntas nas amostras analisadas.

Entre as correlações negativas, a mais expressiva foi observada entre pH e acidez fixa (r = -0,69), seguida das relações entre ácido cítrico e pH (r = -0,55), ácido cítrico e acidez volátil (r = -0,54) e densidade e teor alcoólico (r = -0,50). Esses resultados sugerem relações inversas entre as características químicas dos vinhos.

A correlação entre a variável original de qualidade e a variável binária criada para a classificação foi de 0,71, resultado esperado, já que uma foi criada a partir da outra.

### Discussão da Matriz de Correlação

As correlações identificadas são coerentes com as propriedades físico-químicas dos vinhos. A forte correlação negativa entre pH e acidez fixa (r = -0,69) indica que amostras com maior acidez tendem a apresentar menores valores de pH, comportamento esperado do ponto de vista químico.

A correlação positiva entre acidez fixa e ácido cítrico (r = 0,67) sugere que ambos os componentes contribuem juntos para a composição ácida do vinho. Da mesma forma, a correlação positiva entre dióxido de enxofre livre e dióxido de enxofre total (r = 0,66) era esperada, já que as duas variáveis medem partes do mesmo composto presente na bebida.

Outro resultado relevante foi a correlação negativa entre densidade e teor alcoólico (r = -0,50), indicando que vinhos com maior concentração alcoólica tendem a apresentar menor densidade. Esse comportamento está ligado ao processo de fermentação, em que o açúcar é convertido em álcool, que é menos denso que a água.

Essas correlações ajudam a compreender como os parâmetros químicos interagem entre si e trazem informações importantes para a etapa de modelagem, ao indicar quais variáveis podem influenciar a qualidade do vinho.

### Quais variáveis influenciam a qualidade do vinho?

Para responder quais variáveis são mais importantes e quais são irrelevantes, medimos a correlação de cada característica com a nota de qualidade.

### Resultados da Correlação entre as características químicas e qualidade do vinho

Variáveis com correlação positiva:

- Álcool (≈ 0,49) É a variável com maior correlação positiva, indicando que valores mais altos tendem a estar associados a vinhos de melhor qualidade.
- Sulfatos (≈ 0,26) Apresentam correlação positiva fraca, mas é a segunda maior entre as variáveis, sugerindo que concentrações maiores podem estar relacionadas a vinhos mais bem avaliados.
- Ácido cítrico (≈ 0,24) Também apresenta correlação positiva, ainda que mais discreta.
- Acidez fixa (≈ 0,12) Correlação positiva fraca, mas presente.
- Açúcar residual (≈ 0,02) Correlação praticamente nula, indicando pouca influência na qualidade.

Variáveis com correlação negativa:

- Acidez volátil (≈ -0,41) É a variável com maior correlação negativa, indicando que níveis mais altos tendem a estar associados a vinhos de menor qualidade.
- Densidade (≈ -0,19) Correlação negativa fraca.
- Dióxido de enxofre total (≈ -0,18) Correlação negativa fraca.
- Cloretos (≈ -0,12) Correlação negativa leve.
- Dióxido de enxofre livre (≈ -0,07) Correlação negativa muito fraca.
- pH (≈ -0,06) Correlação negativa mínima, indicando que o pH praticamente não diferencia vinhos bons de ruins nesta base.

## Principais resultados

A análise identificou que:

- o crescimento da receita está fortemente relacionado ao aumento da quantidade de pedidos;
- os períodos de maior volume não apresentam necessariamente maior ticket médio;
- a maior parte dos pedidos foi entregue dentro do prazo estimado;
- atrasos mais graves estão associados a uma redução relevante na nota média;
- Produto e Assuntos Logísticos concentram a maior parte das reclamações;
- nos pedidos entregues no prazo, predominam reclamações relacionadas ao produto;
- nos pedidos com atraso leve ou grave, os assuntos logísticos tornam-se o principal motivo de reclamação;
- o cumprimento do prazo é importante, mas não é suficiente para garantir a satisfação do cliente;
- qualidade do produto, conformidade com o anúncio e desempenho dos sellers também influenciam a experiência.

## Recomendações de negócio

Com base nos resultados, recomenda-se:

1. Reforçar a capacidade logística nos períodos de maior demanda.
2. Monitorar pedidos com risco de atraso e comunicar o cliente de forma proativa.
3. Acompanhar sellers com baixa nota média e alta taxa de reclamação.
4. Melhorar os controles de qualidade, descrição e conformidade dos produtos.
5. Revisar processos de embalagem, separação e expedição.
6. Priorizar ocorrências financeiras devido ao impacto elevado sobre a satisfação.
7. Implantar indicadores recorrentes de satisfação, SLA e desempenho por seller.
8. Criar planos de melhoria para sellers com volume relevante e desempenho crítico.

## Tecnologias utilizadas

- Python
- Google Colab
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Scikit-learn

## Estrutura do repositório

- `Tech_Challenge_Olist.ipynb`: notebook principal com preparação, análises, gráficos e conclusões
- `README.md`: documentação do projeto
- `.gitignore`: definição dos arquivos que não devem ser publicados

## Como executar o projeto

1. Faça o download ou clone este repositório.
2. Abra o notebook no Google Colab.
3. Faça o download das bases públicas da Olist.
4. Salve os arquivos no Google Drive.
5. Ajuste a variável `caminho` no notebook.
6. Execute as células na ordem apresentada.

Exemplo de configuração:

```python
# Altere o caminho conforme o local em que as bases foram salvas
caminho = '/content/drive/MyDrive/Tech Challenge Fase 1/Bases/Olist/'
