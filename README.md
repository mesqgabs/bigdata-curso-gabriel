# Big Data para Negócios — Gabriel Mesquita

Repositório desenvolvido para a disciplina de Introdução à Análise em Big
Data da pós-graduação em Ciências de Dados da Universidade Federal do Ceará
(UFC).

O trabalho apresenta um pipeline completo de dados, desde a ingestão e
organização das camadas Raw, Bronze, Silver e Gold até análise exploratória,
dashboard executivo e modelo introdutório de Machine Learning.

## Ambiente utilizado

Foi utilizada a rota sem privilégios administrativos prevista na disciplina:

- Python 3.13;
- Jupyter Notebook no VS Code;
- DuckDB;
- SQLite;
- Apache Spark em modo local;
- PyArrow e Parquet;
- Pandas;
- Plotly;
- OpenJDK 17.

As dependências estão disponíveis em [`requirements.txt`](requirements.txt).

## Estrutura do repositório

### Dia 1 — Fundamentos e ingestão

- Lab 1: organização de arquivos e simulação de replicação do HDFS;
- Lab 2: importação de banco relacional simulada com SQLite.

Acesse: [`dia1_fundamentos`](dia1_fundamentos)

### Dia 2 — Transformação e análise

- Lab 3: tabelas Managed e External;
- Lab 4: particionamento em Parquet por ano e mês;
- Lab 5: limpeza e tipagem da camada Bronze;
- Lab 6: enriquecimento da camada Silver;
- Lab 7: agregações e indicadores da camada Gold;
- Lab 8: análise exploratória com Finding → Insight → Ação.

Acesse: [`dia2_transformacao`](dia2_transformacao)

### Dia 3 — Serving, BI e Machine Learning

- Lab 9: exportação da Gold para CSV e SQLite;
- Lab 10: consultas com Spark SQL e API de DataFrame;
- Lab 11: dashboard HTML interativo com Plotly;
- Lab 12: regressão logística para detecção de fraude.

Acesse: [`dia3_insights_bi`](dia3_insights_bi)

## Resultados dos laboratórios

Os laboratórios utilizaram uma base com 100.000 transações e 9.993 clientes.

Principais resultados:

- 1.833 fraudes identificadas;
- taxa geral de fraude de 1,83%;
- nenhuma perda entre Bronze, Silver e Gold;
- segmento High-Risk com taxa de fraude de 7,70%;
- dashboard HTML interativo;
- modelo de regressão logística com AUC-ROC de 0,7675;
- avaliação de limiares adequada ao desbalanceamento da base.

# Projeto final — TechPay

O projeto final analisa 30.000 transações sintéticas da fintech TechPay,
realizadas durante o ano de 2025 nos canais aplicativo, web, POS e ATM.

## Etapa 1 — Arquitetura de Big Data

A primeira etapa apresenta o fluxo completo dos dados, desde os canais de
origem até o dashboard, incluindo ingestão, camadas Raw, Bronze, Silver e
Gold, serving e evolução para processamento em tempo real.

- [Diagrama da arquitetura](avaliacao_final/etapa1_arquitetura/diagrama.png)
- [Justificativa técnica](avaliacao_final/etapa1_arquitetura/justificativa.md)
- [Arquivo editável do diagrama](avaliacao_final/etapa1_arquitetura/diagrama.drawio)

## Etapa 2 — Execução do pipeline

A segunda etapa implementa:

1. ingestão do CSV;
2. criação da tabela Raw com esquema controlado;
3. limpeza e validações na Bronze;
4. particionamento em Parquet por ano e mês;
5. enriquecimento na Silver;
6. agregações e indicadores na Gold.

Entregáveis:

- [Notebook do pipeline](avaliacao_final/etapa2_bigdata/pipeline_bigdata.ipynb)
- [Explicação da execução](avaliacao_final/etapa2_bigdata/explicacao.md)
- [Gerador da base de avaliação](avaliacao_final/etapa2_bigdata/generate_avaliacao_dataset.py)

## Etapa 3 — Análise e dashboard

A terceira etapa analisa o risco por canal, categoria do estabelecimento,
segmento, período do dia e combinações multidimensionais.

Entregáveis:

- [Notebook de análises](avaliacao_final/etapa3_analise/analises_dashboard.ipynb)
- [Descrição do BI e relatório dos achados](avaliacao_final/etapa3_analise/ideia_bi.md)
- [Dashboard interativo](avaliacao_final/etapa3_analise/dashboard.html)

> Para utilizar toda a interatividade, baixe o arquivo `dashboard.html` e
> abra-o em um navegador.

## Principais resultados do projeto final

- 30.000 transações processadas;
- 7.803 clientes;
- 751 fraudes identificadas;
- taxa geral de fraude de 2,5033%;
- R$ 119.367,93 em valor associado às fraudes;
- nenhuma perda entre Raw, Bronze, Silver e Gold;
- 12 partições mensais em Parquet;
- canal `app` com taxa de fraude de 3,7850%;
- categoria `viagem` com taxa de 5,3156%;
- combinação `app + viagem` com taxa de 8,0032%;
- combinação `app + viagem + madrugada` com taxa de 9,2369%;
- segmento `High-Risk` com taxa de 9,6741%.

## Como reproduzir o projeto final

1. Clone o repositório.
2. Crie e ative um ambiente virtual.
3. Instale as dependências:

   ```bash
   python -m pip install -r requirements.txt