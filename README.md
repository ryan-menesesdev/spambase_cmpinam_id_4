# Detecção de spam com o Spambase

Projeto da disciplina **CMPINAM · Introdução ao Aprendizado de Máquina**
IFSP Câmpus Campinas · Tecnologia em Análise e Desenvolvimento de Sistemas · 2º semestre de 2026
Docente: Prof. Everton Meyer da Silva

## Integrantes
- João Victor de Alcantara Gimenez
- Lucas Moreira Lima Prado Rodrigues
- Ryan David Oliveira de Meneses

## Problema
Classificação binária: dado um e-mail descrito por 57 atributos numéricos, prever se ele é **spam** ou **não spam**.
Os erros têm custos diferentes: marcar um e-mail legítimo como spam (falso positivo) costuma ser pior do que deixar passar um spam, então a **precisão** da classe spam recebe atenção especial.

## Base utilizada
- **Spambase**, OpenML, dataset ID 44: https://www.openml.org/d/44 (também no UCI Machine Learning Repository).
- Cada linha é um e-mail. Os 57 atributos são frequências de 48 palavras (`word_freq_*`), frequências de 6 símbolos (`char_freq_*`) e 3 medidas de sequências de letras maiúsculas (`capital_run_length_*`).
- Alvo: `class` (1 = spam, 0 = não spam).
- 4.601 e-mails, dos quais 391 duplicatas (8,5%) foram removidas. Divisão **80% treino / 20% teste**, estratificada, com `random_state=42` (3.368 e-mails de treino e 842 de teste, 39,9% de spam em ambos).

## Metodologia
1. **Preparação:** verificação de tipos, valores inválidos e faltantes; remoção de duplicatas **antes** da divisão treino/teste; outliers mantidos (carregam informação sobre spam) e atenuados com `log1p` nos modelos sensíveis a escala.
2. **Análise exploratória:** 9 perguntas respondidas com gráficos (barras, boxplot, heatmap, histograma, dispersão), usando apenas o conjunto de treino.
3. **Modelagem:** 7 técnicas, com Regressão Logística como baseline: Árvore de Decisão, KNN, SVM (RBF), Random Forest, Rede Neural (MLP) e Naive Bayes.
   - Transformações dentro de `Pipeline` (sem vazamento de dados).
   - Validação cruzada estratificada de 5 dobras; busca de hiperparâmetros com `GridSearchCV` otimizando PR-AUC.
   - Métricas: precisão, recall, F1, ROC-AUC e PR-AUC (acurácia apenas como referência).
4. **Técnicas extras (não vistas em sala):** Naive Bayes, ajuste do limiar de decisão por F0,5 (escolhido só com dados de treino) e importância por permutação.

## Principais resultados
Resultados no conjunto de teste (limiar 0,5):

| Modelo | Precisão | Recall | F1 | PR-AUC |
|---|---|---|---|---|
| **Random Forest** | **0,941** | 0,899 | 0,919 | **0,981** |
| Rede Neural (MLP) | 0,935 | 0,938 | 0,936 | 0,976 |
| KNN | 0,929 | 0,899 | 0,914 | 0,972 |
| Regressão Logística (baseline) | 0,933 | 0,911 | 0,922 | 0,968 |
| SVM (RBF) | 0,932 | 0,941 | 0,936 | 0,956 |
| Árvore de Decisão | 0,876 | 0,860 | 0,868 | 0,936 |
| Naive Bayes (extra) | 0,762 | 0,685 | 0,721 | 0,769 |

- O **Random Forest** foi o melhor por PR-AUC (validação cruzada e teste); SVM e rede neural tiveram o maior recall e F1. O baseline já é forte, e as diferenças entre os melhores modelos são pequenas diante do tamanho do teste (842 e-mails).
- **Ajuste do limiar (0,63) no Random Forest:** a precisão sobe de 0,941 para **0,980** (de 19 para 6 e-mails legítimos bloqueados), com recall de 0,899 para 0,872 (34 para 43 spams que passam).
- Atributos mais importantes (importância por permutação): `word_freq_remove`, `char_freq_!`, `word_freq_hp`, `char_freq_$` e `word_freq_your`.

## Como executar
```bash
git clone <URL-DO-REPOSITORIO>
cd <PASTA-DO-REPOSITORIO>
python -m venv .venv && source .venv/bin/activate   # no Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook projeto_spambase.ipynb              # Run All
```
O notebook usa `random_state=42` e caminhos relativos. Se os arquivos do professor estiverem em `dados/train.csv` e `dados/test.csv`, ele os usa; caso contrário, baixa o Spambase do OpenML (precisa de internet) e salva em `dados/spambase_openml.csv`.

## Estrutura
```
projeto_spambase.ipynb   # relatório técnico completo
dados/                   # base de dados
requirements.txt
README.md
```
