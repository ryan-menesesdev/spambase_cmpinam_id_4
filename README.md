# Projeto: Classificação de Spam (Spambase)

## Problema

O problema central consiste em uma tarefa de classificação binária para determinar se uma mensagem de e-mail é spam (`yes`) ou legítima (`no`). Como um fator crítico de sucesso para este problema, é essencial minimizar a taxa de falsos positivos, uma vez que enviar mensagens legítimas por engano para a caixa de spam é extremamente prejudicial. Os textos originais dos e-mails não estão disponíveis para uso, portanto, a classificação deve ser feita unicamente a partir de atributos numéricos já extraídos.

## Base Utilizada

A base de dados utilizada é a **Spambase** (Dataset 06), originalmente coletada em 1999 e disponibilizada pelo UCI Machine Learning Repository.

* **Atributos:** Possui 57 atributos preditores, todos numéricos. Eles representam frequências percentuais de palavras (`word_freq_*`) e caracteres (`char_freq_*`), além de métricas sobre o comprimento de sequências de letras maiúsculas (média, maior e total).

* **Variável Alvo:** `is_spam`.

* **Arquivos e Distribuição:**

  * `spambase_train.csv`: 3.680 registros (2.230 legítimos, 1.450 spam).

  * `spambase_test.csv`: 921 registros (558 legítimos, 363 spam).

## Integrantes

* João Victor de Alcantara Gimenez

* Lucas Moreira Lima Prado Rodrigues

* Ryan Davi Oliveira de Meneses

## Metodologia

## Principais Resultados

## Instruções de Execução