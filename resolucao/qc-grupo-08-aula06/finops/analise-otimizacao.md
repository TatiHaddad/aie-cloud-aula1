# Entrega Aula 06 — Grupo 08

**Disciplina:** Cloud & Cognitive Environments — FIAP MBA AI Engineering & Multi-Agents
**Turma:** 1AIER
**Data de entrega:** 09/08/2026


## Grupo

| # | Nome completo | GitHub | E-mail FIAP |
|---|---------------|--------|-------------|
| 1 | Tatiana Mastrogiovanni Haddad | https://github.com/TatiHaddad | rm373809@fiap.com.br |
| 2 | Luciana Chaves D'Olivo | https://github.com/l-cdolivo | rm371277@fiap.com.br |
| 3 | Lucas Marujo Amadeu | https://github.com/lucasmarujo | rm370469@fiap.com.br |


## Reflexão

# FinOps: Análise de Otimização TCO Quantum Commerce

## a) Qual o TCO mensal estimado em USD da arquitetura QC sem otimização?

 
| Componente | Configuração | Custo mensal (USD) |
|---|---|---|
| Blob Storage | 5 TB Hot + 3 TB Cool + 2 TB Archive | ~$125 |
| Azure SQL Database | Hyperscale 4 vCores + 100 GB | ~$1.676 |
| Cosmos DB | 4.000 RU/s + 50 GB | ~$246 |
| Azure AI Search | Standard S1, 3 réplicas | ~$750 |
| Function App | Premium EP1, 5M req/mês | ~$147 |
| Azure AI Services | 1M Language + 5.000h Speech + 500k Vision | ~$7.750 |
| Azure ML | Workspace + B2s + DS3_v2 Endpoint 24/7 | ~$186 |
| Application Insights | 5 GB logs/mês | ~$5 |
| Egress | 500 GB/mês | ~$35 |
| **TOTAL USD** | | **~$10.920/mês** |
| **TOTAL BRL** | cotação ~5,75 | **~R$62.790/mês** |
 
> Azure AI Services representa **71% do custo total** dominado pelo Speech (5.000h × $1/h = $5.000/mês).
 
---


## b) Qual o TCO otimizado (com suas 3 propostas aplicadas)?

### *Otimização 1:* Speech transcreve uma amostra em vez de 100%
*Problema:* 5.000h/mês de transcrição integraç custa $5.000/mês , mas na maioria dos casos o objetivos é análsie de qualidade e não compliance total.

*Proposta:* transcrever apenas 20% das chamadas usando amostra estatisticamente representativa e usar Speech Analytics só em calls sinalizadas como porblemáticas.

```
5.000h × 20% = 1.000h × $1/h = $1.000/mês
Economia: $4.000/mês (80% de redução)
```

### *Otimização 2:* Language: trocar Sentiment API por GPT-4o-mini
*Problema:* Language API cobra por caractere , 1M chamadas x ~200 chars 200M chars x $2/1M = $2.000/mês

*Proposta:* substituir pela Azure OpenAI GPT-4o-mini, que faz sentiment + opnion mining + summary em uma única chamada

```
1M reviews x 60 tokens médios x $0,15/1M input = $9/mês
1M reviews x 15 tokens output x $0,60/1M output = $9/mês

Total GPT-4o-mini: ~$18/mês
Ecnomia: ~$1.982/mês (99% de redução)
```

*Ganho:* GPT-4o-mini retorna aspectos, resumo e sentimento em uma chamada, o que exige menos do que antes, que com 3 chamadas separadas ao Language API resolvia.


### *Otimização 3:* Azuere SQL: Reserved Instance 1 ano
*Problema: * SQL Hyperscale 4 vCores pay-as-you-go= $1.676/mês

*Proposta:* contratar Reserved Instance de 1 ano para o compute (o storage continua pay-as-you-go)

```
Desconto Reserved 1 ano: ~33%
$1.676 x 0.67 = ~$1.123/mês
Economia: ~$553/mês
Investimento upfront (ou mensal): sem upfront no modelo "monthly"
```


---

## TCO Comparativo

| Cenário | Custo mensal (USD) | Custo mensal (BRL) | x baseline |
|---|---|---|---|
| Sem otimização | $10.920 | R$62.790 | - | 
| Com as 3 otimizações | $3.450 | R$19.838 | **-68%** | 
| Economia mensal | $7.470 | R$42.953 |  | 
| Economia anual | $89.640 | R$515.430 |  | 


---


## c) Qual a economia % com Reserved Instances de 1 ano? 

Para os itens 24/7 sempre ativos:

| Serviço | Pay-as-you-go | Reserved 1 ano | Economia |
|---|---|---|---|
| Azure SQL Hyperscale 4 vCores | $1.676/mÊs | ~$1.123/mês | ~33% |
| Azure ML DS3_v2 Endpoint | $184/mês | ~$115/mês | ~31% |
| Azure AI Search S1 | $750/mês | ~$371/mês | ~35% |
| **Total reservado** | | **$852/mês** | |
| **Economia anual** | | **$10.224/ano** | |

Payback de reserva é imadiato, desconto começa no primeiro mês. Faz sentido porque o recurso é 24x7.



---


## d) Como você apresentaria esses números ao CFO da QC (em 1 parágrafo)?

"
A nossa arquitetura cloud atual está estimada em **$10.920/mês** , que representa ~R$62.800, considerando a cotação do dolar a 5,75 que é a cotação do dia de hoje. Esse valor reflete sem nenhuma otimização aplicada. O principal driver de custo não é a infraestrutura de banco de dados ou compute, mas o serviço cognitivo de Speech que sozinho representa 46% da fatura por transcrevermos integralmente as 5.000 horas mensais de atendimento. Com três ajustes de baixo risco e alto retorno conseguimos reduzir a fatura para **$3.450/mês**, uma economia de 68% por ano.
As recomendações de ajustes são: 1) reduzir transcrição para uma amostra representativa de 20%; 2) migrar análsie de sentimento para o GTP-4o-mini , ele entrega mais em uma única chamada por menos custo; 3) reservar os recursos 24/7 por 1 ano. Como não precisa de um investimento inicial no modelo de reserved mensal, a otimização tem um retorno imediato no primeiro mês.
"

