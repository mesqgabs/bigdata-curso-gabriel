# Big Data para Negócios — Gabriel Mesquita

Repositório desenvolvido para a disciplina de Introdução à Análise em Big Data da pós-graduação em Ciências de Dados da UFC.

O projeto apresenta um pipeline completo de dados, desde a organização da camada bruta até análise exploratória, dashboard e modelo preditivo de fraude.

## Ambiente utilizado

Foi utilizada a rota sem privilégios administrativos:

- Python 3.13
- Jupyter Notebook no VS Code
- DuckDB
- SQLite
- Apache Spark em modo local
- PyArrow e Parquet
- Plotly

O Java utilizado pelo PySpark foi o OpenJDK 17.

## Estrutura

### Dia 1 — Fundamentos e ingestão

- Lab 1: organização das camadas e simulação da replicação do HDFS
- Lab 2: importação de banco relacional simulada com SQLite

### Dia 2 — Transformação e análise

- Lab 3: tabelas Managed e External
- Lab 4: particionamento em Parquet por ano e mês
- Lab 5: limpeza e tipagem da camada Bronze
- Lab 6: enriquecimento da camada Silver
- Lab 7: agregações e indicadores da camada Gold
- Lab 8: análise exploratória com Finding → Insight → Ação

### Dia 3 — Serving, BI e Machine Learning

- Lab 9: exportação da Gold para CSV e SQLite
- Lab 10: consultas com Spark SQL e API de DataFrame
- Lab 11: dashboard HTML interativo com Plotly
- Lab 12: regressão logística para detecção de fraude

## Principais resultados dos laboratórios

- 100.000 transações processadas
- 9.993 clientes
- 1.833 fraudes identificadas
- taxa geral de fraude de 1,83%
- nenhuma perda entre Bronze, Silver e Gold
- segmento High-Risk com taxa de fraude de 7,70%
- dashboard HTML autônomo
- modelo de regressão logística com AUC-ROC de 0,7675

## Como executar

1. Crie e ative um ambiente virtual.
2. Instale as bibliotecas listadas em `requirements.txt`.
3. Disponibilize os arquivos de dados na estrutura local indicada nos notebooks.
4. Execute os notebooks na ordem dos Labs 1 a 12.

Os datasets e arquivos intermediários não são versionados no repositório.

## Avaliação final

A pasta `avaliacao_final` conterá:

- arquitetura de Big Data;
- pipeline Raw → Bronze → Silver → Gold;
- análises de negócio;
- dashboard;
- modelo preditivo opcional.