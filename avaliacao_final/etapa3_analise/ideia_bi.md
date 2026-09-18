# Etapa 3 — Análise e Dashboard

## 1. Objetivo do produto de BI

O dashboard da TechPay foi desenvolvido para transformar os dados de
transações e fraude em informações acionáveis. O painel combina indicadores
executivos, evolução temporal, comparações entre dimensões e detalhamento das
combinações de maior risco.

O objetivo não é apenas informar quantas fraudes ocorreram, mas indicar onde
a equipe deve concentrar controles, autenticações adicionais e análises
manuais.

## 2. Público-alvo

O público principal é a equipe de prevenção a fraudes e gestão de risco da
TechPay. O painel também pode ser utilizado pela diretoria para acompanhar a
exposição financeira e verificar se as ações antifraude estão produzindo
resultado.

Analistas utilizariam as informações detalhadas para ajustar regras e
investigar padrões. A gestão utilizaria os indicadores consolidados para
priorizar recursos e acompanhar a evolução do risco.

## 3. Decisões apoiadas pelo dashboard

O dashboard ajuda a decidir:

- quais canais precisam de controles adicionais;
- quais categorias de estabelecimento devem receber monitoramento reforçado;
- quais combinações devem gerar alertas prioritários;
- em quais horários a autenticação adicional é mais necessária;
- quais segmentos precisam de políticas diferenciadas;
- onde direcionar a capacidade da equipe de análise manual.

A recomendação principal é priorizar transações realizadas no aplicativo,
especialmente quando associadas à categoria viagem e ao período da
madrugada.

## 4. Indicadores selecionados

Os quatro KPIs principais são:

- total de transações;
- fraudes identificadas;
- taxa geral de fraude;
- valor financeiro associado às fraudes.

O total de transações fornece contexto de volume. A quantidade de fraudes
mostra o impacto operacional. A taxa permite comparar grupos com volumes
diferentes. O valor em risco traduz o problema para uma dimensão financeira.

O ticket médio foi calculado na camada Gold, mas não recebeu destaque na
faixa principal do painel porque não representa sozinho a probabilidade de
fraude. A quantidade de clientes também foi mantida nas tabelas analíticas,
mas foi retirada dos KPIs principais para evitar excesso de informação e
manter o foco na exposição ao risco.

## 5. Principais achados

### 5.1 Canal de aplicativo

**Finding:** o canal `app` registrou 455 fraudes, correspondentes a 60,59%
das ocorrências. A taxa de 3,7850% equivale a 1,51 vez a taxa geral, com
R$ 63.909,91 em risco.

**Insight:** o aplicativo concentra uma parcela de fraudes desproporcional
ao seu volume de transações.

**Ação:** aplicar autenticação adicional e monitoramento reforçado às
operações do aplicativo que também apresentem outros sinais de risco.

### 5.2 Categoria viagem

**Finding:** a categoria `viagem` apresentou taxa de 5,3156%, equivalente a
2,12 vezes a média geral. Foram identificadas 160 fraudes e R$ 25.966,40 em
risco.

**Insight:** as transações de viagem concentram risco superior ao observado
nas demais categorias.

**Ação:** incluir a categoria nas regras antifraude e aumentar o nível de
verificação quando a transação acumular outros fatores de risco.

### 5.3 Combinação aplicativo e viagem

**Finding:** a combinação `app + viagem` registrou taxa de 8,0032%, com 99
fraudes e R$ 15.578,67 em risco. Essa taxa equivale a 3,20 vezes a média
geral.

**Insight:** observar as variáveis em conjunto revela um risco que não seria
adequadamente capturado por regras isoladas.

**Ação:** criar regra combinada para solicitar autenticação adicional ou
direcionar a operação para análise quando houver outros sinais de risco.

### 5.4 Período do dia

**Finding:** a madrugada apresentou taxa geral de 3,4653%, correspondente a
1,38 vez a média da TechPay. Na combinação `app + viagem`, a taxa da
madrugada chegou a 9,2369%.

**Insight:** o horário funciona como agravante do risco, principalmente
quando combinado com canal e categoria críticos.

**Ação:** aumentar a prioridade dos alertas entre 0h e 4h, sem utilizar o
horário como único motivo para recusar uma operação.

### 5.5 Segmento do cliente

**Finding:** o segmento High-Risk apresentou taxa de 9,6741%, equivalente a
3,86 vezes a média geral. Embora represente menos de 10% das transações,
concentrou 37,55% das fraudes.

**Insight:** o segmento diferencia significativamente o risco, mas o
segmento Standard ainda registra a maior quantidade absoluta de ocorrências
por causa do volume.

**Ação:** aplicar controles reforçados ao segmento High-Risk e utilizar
regras multidimensionais no segmento Standard.

## 6. Organização do dashboard

O primeiro bloco apresenta os KPIs executivos. O segundo mostra a evolução
diária das fraudes e uma média móvel de sete dias, reduzindo oscilações
pontuais. O terceiro compara a taxa de fraude entre canais e categorias. O
último bloco detalha as combinações prioritárias de canal e categoria.

Essa organização segue uma sequência de leitura: dimensão geral do problema,
comportamento ao longo do tempo, localização do risco e detalhe necessário
para agir.

## 7. Responsável pelo painel

O proprietário funcional do dashboard deve ser o gestor de prevenção a
fraudes da TechPay. Esse responsável deve acompanhar o painel pelo menos
semanalmente, avaliar alterações nas taxas e direcionar ajustes nas regras de
monitoramento.

A área de dados seria responsável pela atualização e qualidade das
informações, enquanto a equipe de risco validaria os critérios operacionais
e avaliaria falsos positivos produzidos pelas regras.

## 8. Frequência e uso operacional

Em uma operação batch, o dashboard pode ser atualizado diariamente. Caso a
TechPay adote Kafka e Spark Structured Streaming, parte dos indicadores pode
ser atualizada com baixa latência.

Os alertas individuais não devem depender exclusivamente do dashboard. O
painel serve para acompanhamento gerencial e ajuste das políticas, enquanto
a decisão sobre cada transação deve ocorrer em um motor de regras ou modelo
de risco.

## 9. Limitações

Os dados são sintéticos e representam apenas o ano de 2025. Não existem
informações sobre dispositivo, localização, histórico de autenticação,
estorno ou confirmação posterior da fraude.

As associações encontradas demonstram concentração de risco, mas não
comprovam causalidade. Antes de colocar as regras em produção, seria
necessário avaliar falsos positivos, impacto na experiência do cliente e
custo operacional das análises adicionais.

## 10. Conclusão

O dashboard demonstra que o risco de fraude da TechPay não está distribuído
uniformemente. O maior foco está nas transações realizadas pelo aplicativo,
na categoria viagem, durante a madrugada e por clientes High-Risk.

A combinação das dimensões fornece uma estratégia mais eficiente do que
regras isoladas. Assim, a recomendação é utilizar controles progressivos,
priorizando operações que acumulem múltiplos sinais de risco.