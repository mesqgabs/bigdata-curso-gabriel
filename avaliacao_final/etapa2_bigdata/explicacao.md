# Etapa 2 — Execução do Pipeline de Big Data

## 1. Ambiente utilizado

A execução foi realizada pela rota sem privilégios administrativos, utilizando Python, DuckDB, Pandas, PyArrow e arquivos Parquet. Essa alternativa permite reproduzir os conceitos de um pipeline de Big Data em ambiente local, incluindo esquema controlado, processamento em camadas, particionamento físico, validações de qualidade e produção de agregações.

O pipeline foi implementado no notebook `pipeline_bigdata.ipynb`. Os dados intermediários foram mantidos em um banco DuckDB e em arquivos Parquet, enquanto as tabelas destinadas ao consumo também foram exportadas em CSV.

## 2. Ingestão

A ingestão trouxe o arquivo `avaliacao_transactions.csv` para a pasta local da camada Raw. A base contém 30.000 transações realizadas pela TechPay durante o ano de 2025.

O arquivo original foi preservado sem alterações para permitir rastreabilidade e eventual reprocessamento. A ingestão identificou corretamente as doze colunas da base, incluindo as novas dimensões `channel` e `merchant_category`.

A validação inicial encontrou 30.000 identificadores únicos e nenhuma duplicidade. Também foram identificadas 751 fraudes, equivalentes a 2,5033% das transações.

## 3. Criação da tabela Raw

A tabela `raw.transactions` foi criada no DuckDB com esquema explicitamente definido. Identificadores foram armazenados como números inteiros, valores monetários e escores como números decimais, a data da transação como timestamp, os campos categóricos como texto e o indicador de fraude como booleano.

A definição dos tipos evita interpretações incorretas e permite que as regras das camadas seguintes sejam executadas de maneira consistente. A tabela Raw manteve as 30.000 linhas do arquivo original, sem transformação ou descarte.

## 4. Particionamento

A estratégia adotada foi o particionamento físico por ano e mês da transação. Os campos de ano e mês foram derivados do timestamp e utilizados na gravação das camadas Bronze e Silver em formato Parquet.

Como todas as transações pertencem a 2025, foram produzidas 12 partições mensais. Essa estratégia permite que consultas sobre determinado mês leiam apenas a partição necessária, reduzindo o volume de dados acessado.

A granularidade mensal foi escolhida por representar um equilíbrio entre desempenho e quantidade de arquivos. O particionamento diário produziria muitos arquivos pequenos, enquanto o particionamento apenas anual obrigaria consultas mensais a percorrer todo o ano.

## 5. Camada Bronze

Na camada Bronze foram aplicadas regras de padronização e qualidade. Os campos textuais foram tratados para evitar diferenças provocadas por espaços ou uso inconsistente de letras maiúsculas e minúsculas.

Foram verificadas duplicidades de `transaction_id`, ausência de campos obrigatórios, valores monetários menores ou iguais a zero, escores de risco fora do intervalo de 0 a 100, escores de crédito fora do intervalo de 300 a 900 e valores desconhecidos nos campos de canal e categoria do estabelecimento.

A base não apresentou registros inválidos: todas as verificações retornaram zero inconsistências. Portanto, as 30.000 linhas sobreviveram à Bronze. Nenhuma linha foi descartada, não porque a camada apenas copiou os dados, mas porque todas as transações passaram pelas regras implementadas.

Os registros aprovados foram armazenados em Parquet e organizados nas 12 partições mensais definidas pela estratégia de arquitetura.

## 6. Camada Silver

A Silver foi criada para enriquecer os dados e prepará-los para análise. As colunas `channel` e `merchant_category` foram preservadas porque representam dimensões relevantes para entender o comportamento das fraudes.

Foram derivadas a data da transação, o ano, o mês, o dia, a hora, o dia da semana e o período do dia. Também foram criadas faixas de valor da transação e faixas de risco, facilitando agrupamentos e comparações posteriores.

A variável de período do dia classifica as transações entre madrugada, manhã, tarde e noite. Essa derivação é especialmente importante porque transações realizadas em horários incomuns podem apresentar comportamento de fraude diferente.

A Silver manteve as 30.000 transações, 30.000 identificadores únicos e 751 fraudes. Nenhuma linha ficou sem as variáveis derivadas, e os dados foram novamente gravados em Parquet com 12 partições mensais.

## 7. Camada Gold

A Gold foi construída com agregações destinadas à análise gerencial e à elaboração do dashboard. Foram criadas cinco tabelas:

- `executive_summary`, com os principais indicadores gerais;
- `fraud_by_channel`, com o risco por canal;
- `fraud_by_category`, com o risco por categoria de estabelecimento;
- `fraud_by_segment`, com o risco por segmento de cliente;
- `daily_metrics`, com a evolução diária das transações e fraudes.

As agregações respondem onde o risco está concentrado, como ele varia entre canais, categorias e segmentos e como evolui ao longo do tempo. Todas as tabelas foram mantidas no DuckDB e exportadas em CSV e Parquet.

A reconciliação confirmou que as agregações por canal, categoria, segmento e data reproduzem exatamente as 30.000 transações e 751 fraudes da Silver. A Gold diária contém 365 datas, cobrindo o período completo entre 1º de janeiro e 31 de dezembro de 2025.

## 8. Resultados encontrados

A base apresenta 30.000 transações de 7.803 clientes, movimentação total de R$ 5.207.843,50 e ticket médio de R$ 173,59. Foram identificadas 751 fraudes, representando taxa geral de 2,5033% e R$ 119.367,93 em valor associado ao risco.

O canal `app` apresentou a maior taxa de fraude, com 3,7850%, 455 ocorrências e R$ 63.909,91 em risco. O aplicativo concentra sozinho aproximadamente 60,6% das fraudes observadas.

A categoria `viagem` apresentou a maior taxa entre os estabelecimentos, com 5,3156%. Apesar de representar 3.010 transações, registrou 160 fraudes e R$ 25.966,40 em risco.

O segmento `High-Risk` apresentou taxa de fraude de 9,6741%, significativamente superior às taxas de 2,9310% do segmento Standard e 0,9550% do segmento Premium. Esses resultados justificam o uso das dimensões de canal, categoria e segmento nas análises da próxima etapa.

## 9. Problemas encontrados e soluções

O conjunto de avaliação não apresentou duplicidades, campos obrigatórios ausentes, valores impossíveis ou categorias desconhecidas. Mesmo assim, o pipeline implementou todas essas verificações para impedir que registros inválidos avancem caso novos arquivos sejam processados futuramente.

A principal adaptação técnica foi configurar o caminho de saída do gerador para o ambiente Windows, pois o script original utilizava o diretório `/tmp`. O arquivo passou a ser gravado na pasta Raw do projeto, preservando a organização definida para a avaliação.

Também foi necessário manter as saídas intermediárias fora do controle de versão, porque arquivos CSV, Parquet e bancos locais são resultados reproduzíveis da execução. O repositório contém o gerador e o notebook necessários para recriar todo o pipeline.

## 10. Conclusão

O pipeline executou as seis etapas exigidas: ingestão, criação da Raw, particionamento, limpeza Bronze, enriquecimento Silver e agregação Gold. As validações demonstraram que todas as 30.000 transações foram processadas sem perdas e que os totais permaneceram consistentes entre as camadas.

A Gold fornece os dados necessários para a Etapa 3, na qual os padrões encontrados serão transformados em análises, recomendações de negócio e dashboard funcional.