# Pré-Projeto de Pesquisa — PCO213

## Detecção de Fraudes em Cartões de Crédito com Aprendizado de Máquina: Uma Análise Comparativa de Modelos e Métricas em Cenários com Desbalanceamento Extremo de Classes

**Disciplina:** PCO213 — Aprendizado de Máquina / Mineração de Dados  
**Professor:** Giovani Bernardes Vitor  
**Semestre:** 2026-1  
**Prazo de entrega:** 29/06/2026  

---

## 1. Tema

Aplicação e avaliação comparativa de técnicas de aprendizado de máquina supervisionado para detecção de fraudes em transações de cartão de crédito, com foco no problema de **desbalanceamento extremo de classes** e na escolha adequada de métricas de avaliação.

---

## 2. Problema

Conjuntos de dados de detecção de fraudes apresentam desbalanceamento severo: menos de 0,2% das transações são fraudulentas. Modelos treinados sem tratamento adequado tendem a ignorar a classe minoritária e apresentar alta acurácia ilusória. O problema central é:

> **Como comparar e avaliar modelos de classificação de forma justa e confiável quando a classe de interesse é extremamente rara?**

---

## 3. Objetivos

### Objetivo Geral

Desenvolver e comparar modelos preditivos para detecção de fraudes em cartões de crédito, avaliando o impacto do desbalanceamento de classes por meio de métricas padrão e métricas adicionais não abordadas em sala, além de investigar técnicas de balanceamento de dados.

### Objetivos Específicos

1. Realizar análise exploratória do dataset *Credit Card Fraud Detection* (Kaggle/ULB)
2. Aplicar e comparar pelo menos dois algoritmos de classificação: **Regressão Logística** e **SVM** (e opcionalmente Random Forest como baseline avançado)
3. Avaliar o impacto do balanceamento via **SMOTE** e **undersampling** no desempenho dos modelos
4. Comparar métricas padrão de avaliação: acurácia, precisão, recall, F1-score, AUC-ROC, AUC-PR e MCC (*Matthews Correlation Coefficient*)
5. Incluir e analisar métricas adicionais para desbalanceamento extremo: *Balanced Accuracy*, *G-Mean*, F-beta (com ênfase em recall) e Brier Score
6. Discutir em profundidade os impactos da escolha de métricas na interpretação dos resultados e na seleção do modelo final
7. Demonstrar, de forma experimental, por que a acurácia é uma métrica potencialmente enganosa em cenários de fraude

### Perguntas de Pesquisa

- A técnica SMOTE melhora significativamente o recall para a classe de fraude sem degradar excessivamente a precisão?
- A curva Precision-Recall (AUC-PR) é mais informativa que a AUC-ROC neste cenário?
- Métricas adicionais para desbalanceamento extremo levam a conclusões diferentes das métricas padrão sobre o melhor modelo?
- Qual modelo apresenta melhor custo-benefício entre desempenho e interpretabilidade?

---

## 4. Dataset

**Nome:** Credit Card Fraud Detection  
**Fonte:** [Kaggle — Machine Learning Group, Université Libre de Bruxelles (ULB)](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)  
**Características:**

- 284.807 transações de cartões de crédito europeus (setembro de 2013)
- 492 fraudes → **0,172% da amostra**
- 30 features: `Time`, `Amount` e V1–V28 (componentes PCA, por confidencialidade)
- Variável alvo: `Class` (0 = legítima, 1 = fraude)

---

## 5. Metodologia Planejada

```text
Dataset → EDA → Pré-processamento → Balanceamento → Modelagem → Avaliação → Discussão
```

1. **EDA:** distribuição das classes, estatísticas descritivas, heatmap de correlação, distribuição de `Amount` e `Time`
2. **Pré-processamento:** normalização de `Amount` e `Time`, verificação de valores ausentes
3. **Balanceamento:** comparar 3 cenários — sem balanceamento, SMOTE, Random Undersampling
4. **Modelos:** Regressão Logística, SVM (kernel RBF), DummyClassifier (baseline)
5. **Validação:** StratifiedKFold (k=5) para preservar proporção das classes em cada fold
6. **Métricas padrão:** Acurácia, Precisão, Recall, F1, AUC-ROC, AUC-PR, MCC
7. **Métricas adicionais (não vistas em sala):** *Balanced Accuracy*, *G-Mean*, F-beta (beta=2), Brier Score e curva de calibração
8. **Visualizações:** Curvas ROC, Curvas Precision-Recall, Curvas de calibração, Matrizes de Confusão, importância de features (coeficientes da Regressão Logística)
9. **Discussão crítica:** análise comparativa profunda entre métricas padrão e adicionais, destacando implicações práticas para decisão em detecção de fraude

---

## 6. O Que Estudar / Roadmap de Leitura

### 6.1 Fundamentos (base obrigatória)

| Tópico | O que ler |
| --- | --- |
| Regressão Logística | Hastie et al. (2009), Cap. 4 — *The Elements of Statistical Learning* |
| SVM — conceito e kernel trick | Cortes & Vapnik (1995) — artigo original |
| Métricas de classificação (F1, AUC, MCC) | Sokolova & Lapalme (2009) — *A systematic analysis of performance measures for classification tasks* |
| Desbalanceamento de classes | He & Garcia (2009) — *Learning from Imbalanced Data* |
| SMOTE | Chawla et al. (2002) — *SMOTE: Synthetic Minority Over-sampling Technique* |
| Curva Precision-Recall vs ROC | Davis & Goadrich (2006) — *The Relationship Between Precision-Recall and ROC Curves* |

### 6.2 Artigos Diretamente Relacionados

| Referência | Por que ler |
| --- | --- |
| Dal Pozzolo et al. (2015) — *Calibrating Probability with Undersampling for Unbalanced Classification* | Publicado pelo próprio grupo que gerou o dataset; discute estratégias de undersampling com calibração |
| Bhattacharyya et al. (2011) — *Data mining for credit card fraud: A comparative study* | Comparativo clássico de algoritmos para detecção de fraude financeira |
| Fernández et al. (2018) — *SMOTE for Learning from Imbalanced Data: Progress and Challenges* | Revisão completa e atual sobre SMOTE e variações |
| Saito & Rehmsmeier (2015) — *The Precision-Recall Plot Is More Informative than the ROC Plot When Evaluating Binary Classifiers on Imbalanced Datasets* | Justifica o uso da curva PR neste trabalho |

### 6.3 Material Complementar (leitura de suporte)

| Recurso | Onde encontrar |
| --- | --- |
| Scikit-learn: `imbalanced-learn` documentação | <https://imbalanced-learn.org/stable/> |
| Scikit-learn: métricas de classificação | <https://scikit-learn.org/stable/modules/model_evaluation.html> |
| Notebook ULB no Kaggle (explorações da comunidade) | <https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud/code> |
| Livro base da disciplina: Witten & Frank — *Data Mining* | Cap. 5 (Credibility: Evaluating What's Been Learned) |

### 6.4 Ordem de Estudo Recomendada

```text
1. He & Garcia (2009)           → entender o problema central (desbalanceamento)
2. Chawla et al. (2002)         → entender SMOTE antes de aplicar
3. Davis & Goadrich (2006)      → entender AUC-PR vs AUC-ROC
4. Sokolova & Lapalme (2009)    → entender cada métrica e quando usar
5. Dal Pozzolo et al. (2015)    → contexto específico do dataset que vamos usar
6. Saito & Rehmsmeier (2015)    → argumento central do artigo
```

---

## 7. Estrutura Prevista do Artigo (6 páginas — template IEEE)

| Seção | Conteúdo Principal | Páginas estimadas |
| --- | --- | --- |
| 1. Introdução | Contexto de fraudes, custo do problema, limitação da acurácia | ~0,8 |
| 2. Revisão Bibliográfica | Estado da arte, técnicas de balanceamento e comparação entre métricas padrão e avançadas para desbalanceamento | ~1,2 |
| 3. Metodologia | Dataset, pipeline, modelos, validação e protocolo de avaliação com métricas padrão e adicionais | ~1,5 |
| 4. Experimentos e Resultados | Tabelas de métricas padrão e adicionais, curvas ROC/PR/calibração, matrizes de confusão | ~1,5 |
| 5. Discussão | Discussão aprofundada dos impactos da escolha de métricas, limitações e contribuições | ~0,5 |
| 6. Conclusão | Síntese e trabalhos futuros | ~0,3 |
| Referências | ~10 referências | ~0,2 |

---

## 8. Referências (formato IEEE)

[1] N. V. Chawla, K. W. Bowyer, L. O. Hall, and W. P. Kegelmeyer, "SMOTE: Synthetic Minority Over-sampling Technique," *Journal of Artificial Intelligence Research*, vol. 16, pp. 321–357, 2002.

[2] C. Cortes and V. N. Vapnik, "Support-vector networks," *Machine Learning*, vol. 20, no. 3, pp. 273–297, 1995.

[3] J. Davis and M. Goadrich, "The relationship between Precision-Recall and ROC curves," in *Proc. 23rd Int. Conf. Machine Learning (ICML)*, Pittsburgh, PA, 2006, pp. 233–240.

[4] A. Dal Pozzolo, O. Caelen, R. A. Johnson, and G. Bontempi, "Calibrating probability with undersampling for unbalanced classification," in *Proc. IEEE SSCI*, Cape Town, South Africa, 2015, pp. 159–166.

[5] A. Fernández, S. García, F. Herrera, and N. V. Chawla, "SMOTE for Learning from Imbalanced Data: Progress and Challenges, Marking the 15-Year Anniversary," *Journal of Artificial Intelligence Research*, vol. 61, pp. 863–905, 2018.

[6] H. He and E. A. Garcia, "Learning from Imbalanced Data," *IEEE Transactions on Knowledge and Data Engineering*, vol. 21, no. 9, pp. 1263–1284, Sep. 2009.

[7] T. Saito and M. Rehmsmeier, "The Precision-Recall Plot Is More Informative than the ROC Plot When Evaluating Binary Classifiers on Imbalanced Datasets," *PLOS ONE*, vol. 10, no. 3, p. e0118432, 2015.

[8] M. Sokolova and G. Lapalme, "A systematic analysis of performance measures for classification tasks," *Information Processing & Management*, vol. 45, no. 4, pp. 427–437, 2009.

[9] T. Hastie, R. Tibshirani, and J. Friedman, *The Elements of Statistical Learning*, 2nd ed. New York, NY: Springer, 2009.

[10] I. H. Witten, E. Frank, M. A. Hall, and C. J. Pal, *Data Mining: Practical Machine Learning Tools and Techniques*, 4th ed. Burlington, MA: Morgan Kaufmann, 2016.
