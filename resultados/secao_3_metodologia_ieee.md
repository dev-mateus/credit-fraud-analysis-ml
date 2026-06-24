# III. Metodologia

## A. Desenho Experimental

O problema foi formulado como classificação binária supervisionada para detecção de fraude em transações de cartão de crédito, com classe alvo extremamente rara. O protocolo experimental foi estruturado para comparar, de forma controlada, combinações de modelos e estratégias de balanceamento, mantendo a mesma política de validação para todas as configurações.

A implementação foi realizada em Python, com execução reprodutível por semente fixa (random state igual a 42). O pipeline completo foi organizado em etapas sequenciais: carregamento de dados, geração de artefatos de análise exploratória, treinamento/avaliação via validação cruzada estratificada, consolidação de métricas e geração de visualizações comparativas.

## B. Dados e Pré-processamento

Utilizou-se o conjunto Credit Card Fraud Detection (ULB/Kaggle), contendo 284.807 transações e 492 eventos fraudulentos (prevalência aproximada de 0,173%). A variável de interesse foi Class (0 para transação legítima, 1 para fraude).

No carregamento, foi executada validação de integridade mínima da base, exigindo a presença explícita da coluna Class. Para eficiência computacional e redução de memória, colunas em float64 foram convertidas para float32 e a variável alvo foi convertida para int8. Em seguida, foi realizado particionamento em variáveis explicativas X e alvo y.

A etapa de análise exploratória gerou os seguintes artefatos: resumo estrutural (número de linhas/colunas, contagens por classe, razão de fraude e total de ausências), estatísticas descritivas de Time e Amount, histograma univariado dessas variáveis e gráfico de distribuição de classes.

## C. Cenários de Balanceamento

Foram avaliados três cenários de treinamento:

1. Sem balanceamento: treinamento com distribuição original dos dados.
2. Sobreamostragem sintética (SMOTE): geração de exemplos sintéticos da classe minoritária.
3. Subamostragem aleatória (Random UnderSampling): redução da classe majoritária para mitigar assimetria.

A estratégia de balanceamento foi aplicada apenas no conjunto de treinamento de cada fold, por meio de pipeline da biblioteca imbalanced-learn, evitando vazamento de informação para os subconjuntos de teste.

## D. Modelos Avaliados

A comparação principal foi executada com três classificadores:

1. DummyClassifier (most_frequent), utilizado como baseline ingênuo.
2. Regressão Logística (solver liblinear, max_iter igual a 400).
3. SVM com kernel RBF (SVC com probability habilitado).

A normalização por StandardScaler foi aplicada apenas aos modelos sensíveis à escala (Regressão Logística e SVM). O baseline Dummy não utilizou escalonamento. O pipeline também suporta Random Forest opcional, porém esse modelo não compôs a execução principal reportada nos resultados finais.

## E. Validação e Protocolo de Execução

A avaliação foi conduzida por validação cruzada estratificada com 5 folds, embaralhamento habilitado e random state fixo. Em cada configuração (cenário de balanceamento, modelo), todos os folds foram processados e agregados com abordagem out-of-fold (OOF), permitindo cálculo consolidado de desempenho em toda a amostra validada.

Para ganho de desempenho computacional, os folds foram executados em paralelo com joblib, usando múltiplos workers (parâmetro n_jobs). O protocolo preservou comparabilidade entre configurações ao manter a mesma divisão estratificada de folds dentro de uma execução.

## F. Métricas de Avaliação

Dado o desbalanceamento extremo, a acurácia não foi tratada como critério isolado. Foram calculadas, por fold e em agregação OOF, as métricas:

1. Accuracy
2. Precision
3. Recall
4. F1-score
5. AUC-ROC
6. AUC-PR (average precision)
7. MCC (Matthews Correlation Coefficient)
8. Balanced Accuracy
9. G-mean
10. F-beta (beta igual a 2)
11. Brier Score

Para modelos sem probabilidade direta, o escore contínuo foi obtido por decision function e normalizado para o intervalo [0,1], viabilizando cálculo consistente de AUC-ROC, AUC-PR e Brier Score.

## G. Critério de Seleção do Modelo

A seleção da melhor configuração foi realizada por ordenação lexicográfica, priorizando:

1. Maior AUC-PR média em validação cruzada.
2. Em caso de empate, maior recall médio.
3. Persistindo empate, maior MCC médio.

Esse critério foi definido para privilegiar desempenho na classe minoritária, mantendo sensibilidade à recuperação de fraudes e à qualidade global da matriz de confusão.

## H. Artefatos Gerados para Análise

Ao final da execução, foram exportados artefatos tabulares e gráficos para suporte à discussão científica:

1. Métricas por fold.
2. Resumo de média e desvio-padrão por configuração.
3. Tabela comparativa entre acurácia e métricas robustas para desbalanceamento.
4. Gráfico de barras comparativo de métricas principais.
5. Curvas ROC e Precision-Recall da melhor configuração.
6. Curva de calibração da melhor configuração.
7. Matriz de confusão da melhor configuração.
8. Relatório OOF da melhor configuração.

Esse conjunto foi adotado para garantir rastreabilidade dos resultados, comparação reproduzível entre cenários e suporte à análise crítica apresentada na seção de resultados e discussão.
