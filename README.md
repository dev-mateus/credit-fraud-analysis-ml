# Detecção de Fraudes em Cartões de Crédito com Aprendizado de Máquina

Pipeline de aprendizado de máquina para detecção de fraudes em transações de cartão de crédito, com foco em cenários de desbalanceamento extremo de classes.

## Contexto

**Instituição:** UNIFEI (Universidade Federal de Itajubá)  
**Curso:** [PPG-CTC](https://ppg-ctc.unifei.edu.br/)  
Programa de Pós-Graduação em Ciência e Tecnologia da Computação  
**Nível:** Mestrado  
**Disciplina:** PCO213 - Aprendizado de Máquina / Mineração de Dados  
**Professor:** Giovani Bernardes Vitor

## Resumo

Este projeto apresenta uma análise comparativa de modelos supervisionados para detecção de fraudes em cartões de crédito utilizando o dataset Credit Card Fraud Detection (Kaggle/ULB). O trabalho enfatiza os impactos do desbalanceamento extremo (0,172% de fraudes) na avaliação de desempenho e demonstra por que métricas tradicionais, como acurácia, podem induzir a conclusões equivocadas. O pipeline contempla pré-processamento, estratégias de balanceamento (SMOTE e undersampling), treinamento de modelos e avaliação com métricas padrão e adicionais voltadas a dados desbalanceados.

**Palavras-chave:** Detecção de fraude, aprendizado supervisionado, desbalanceamento de classes, SMOTE, AUC-PR, métricas de classificação.

## Finalidade do Pipeline

Este pipeline foi desenvolvido para:

- Investigar desempenho de classificadores em um problema real de fraude financeira.
- Comparar cenários com e sem balanceamento de classes.
- Avaliar modelos com métricas mais robustas para eventos raros.
- Apoiar análise acadêmica com rigor metodológico e reprodutibilidade.

## Problema de Pesquisa

Em bases de fraude, a classe positiva é rara. Assim, o problema central é:

> Como comparar e avaliar modelos de classificação de forma justa e confiável quando a classe de interesse é extremamente rara?

## Arquitetura do Pipeline

```text
Dataset -> EDA -> Preprocessamento -> Balanceamento -> Modelagem -> Avaliacao -> Discussao
```

### Etapas

1. EDA: análise de distribuição de classes e variáveis principais.
2. Pré-processamento: normalização de `Amount` e `Time`, checagem de ausências.
3. Balanceamento: sem balanceamento, SMOTE e Random Undersampling.
4. Modelagem: Regressão Logística, SVM (RBF) e baseline com DummyClassifier.
5. Validação: StratifiedKFold (k=5).
6. Avaliação: métricas clássicas e métricas para desbalanceamento extremo.
7. Visualização: curvas ROC/PR, calibração e matrizes de confusão.
8. Discussão: interpretação crítica dos resultados e trade-offs.

## Métricas Utilizadas

### Padrão

- Acurácia
- Precisão
- Recall
- F1-score
- AUC-ROC
- AUC-PR
- MCC

### Adicionais para classes desbalanceadas

- Balanced Accuracy
- G-Mean
- F-beta (beta = 2)
- Brier Score

## Dataset

**Nome:** Credit Card Fraud Detection  
**Fonte:** https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

Características principais:

- 284.807 transações
- 492 fraudes (0,172%)
- 30 features (`Time`, `Amount`, `V1` a `V28`)
- Variável alvo: `Class` (0 = legítima, 1 = fraude)

## Como Executar

### 1. Criar ambiente virtual (opcional, recomendado)

```bash
python -m venv .venv
```

### 2. Ativar ambiente

**Windows (cmd):**

```bash
.venv\Scripts\activate
```

### 3. Instalar dependências

```bash
pip install -r requirements_projeto_final.txt
```

### 4. Executar pipeline

```bash
python fraud_detection_pipeline.py
```

## Reprodutibilidade

- Utilizar sementes aleatórias fixas (`random_state`) em todas as etapas.
- Preservar a estratificação das classes na validação cruzada.
- Reportar métricas por fold e agregadas (média e desvio padrão).

## Resultados e Discussão

Os resultados e análises complementares estão organizados na pasta `resultados/`, com foco em:

- Comparação entre métodos de balanceamento.
- Diferenças de conclusão ao usar AUC-ROC versus AUC-PR.
- Impacto das métricas adicionais na escolha do modelo final.

## Referências

1. N. V. Chawla et al., "SMOTE: Synthetic Minority Over-sampling Technique," *JAIR*, 2002.
2. C. Cortes and V. Vapnik, "Support-vector networks," *Machine Learning*, 1995.
3. J. Davis and M. Goadrich, "The relationship between Precision-Recall and ROC curves," *ICML*, 2006.
4. H. He and E. A. Garcia, "Learning from Imbalanced Data," *IEEE TKDE*, 2009.
5. T. Saito and M. Rehmsmeier, "The Precision-Recall Plot Is More Informative...," *PLOS ONE*, 2015.

## Licença e Uso Acadêmico

Projeto desenvolvido para fins acadêmicos na disciplina PCO213 (UNIFEI), no contexto de pesquisa em nível de mestrado.
