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


## Distribuição do trabalho

| Membro | Nível assumido | Item específico |
|--------|----------------|-----------------|
| **Tatiana Mastrogiovanni Haddad** | 🟢 **N1 + Reflexão ** | Passo 1 e 2 |
| **Luciana Chaves D'Olivo** | 🟢 **N1 + Reflexão ** | Passo 3 e 4 |
| **Lucas Marujo Amadeu** | 🟢 **N1 + Reflexão ** | Passo 5 |


---

## Objetivo

Estimar o custo mensal da arquitetura completa da Quantum Commerce em escala real (não free tier). Este resultado entra no projeto integrado final (seção finops/).



| Componente | Volume mensal esperado |
|-------------|-----------------------|
| Blob Storage | 10 TB (5 TB Hot + 3 TB Cool + 2 TB Archive) — imagens + logs |
| Azure SQL Database | Hyperscale 4 vCores + 100 GB (não free para escala real) |
| Cosmos DB | 4.000 RU/s + 50 GB |
| Azure AI Search | Standard S1 (3 réplicas, 12 partitions) |
| Function App | Premium EP1 (sempre warm — 5M req/mês) |
| Azure AI Services | Multi-service S0: 1M chamadas Language + 5.000h Speech + 500k chamadas Vision |
| Azure ML | Workspace + Compute B2s ocasional + 1 Online Endpoint Standard_DS3_v2 (24/7) |
| Application Insights | 5 GB de logs/mês |
| Egress | 500 GB/mês saindo para CDN externa |



---


🟢 **N1 - Passo 1 — Adicionar cada serviço** 


| Componente | Configuração | Custo Mensal |
|-------------|-----------|------------|
| Blob Storage | 10 TB (5 TB Hot + 3 TB Cool + 2 TB Archive) — imagens + logs | $141,67 |
| Azure SQL Database | Hyperscale 4 vCores + 100 GB (não free para escala real) | $265,00 |
| Cosmos DB | 4.000 RU/s + 50 GB | $246,10 |
| Azure AI Search | Standard S1 (3 réplicas, 12 partitions) | $735,84 |
| Function App | Premium EP1 (sempre warm — 5M req/mês) | $145,93 |
| Azure AI Services | Multi-service S0: 1M chamadas Language + 5.000h Speech + 500k chamadas Vision | Language: $875,00 , Speech $900,00 e Visio $375,00 |
| Azure ML | Workspace + Compute B2s ocasional + 1 Online Endpoint Standard_DS3_v2 (24/7) | $170,50 |
| Application Insights | 5 GB de logs/mês | $0,00 |
| Egress | 500 GB/mês saindo para CDN externa | $34,80 |
|**TOtal Mensal:** | | $3.889,84 |
|**Total Anual:** | | $46.678,04 |
|**Total BRL:** | cotação ~5,75 | ~R$22.236/mês |

** Link da estimativa:** https://azure.com/e/f610e4847573467db1def71bc8dc2eb4


🟢 **Passo 2 — Calcular o total e anote os 3 itens mais caros** 

| Item | Custo | % do total |
|-------------|-----------|------------|
| AI Services - Speech | $900,00 | 23,1% |
| AI Services - Language | $875,00 | 22,5% |
| Azure AI Search S1 | $735,84 | 18,9% |

Os serviços juntos (Speech + Language + Vision) representam 53% da fatura total. Representa mais que toda a infraestrutura de dados combinada.


🟢 **Passo 3 — Otimização hipotética** 


| Item caro | Otimização proposta | Economia estimada |
|-------------|------------|------------|
| AI Services - Speech $900,00 | Transcrever uma amostra de 20% das chamadas em vez de 100%  | ~$720/mês
| AI Services - Language $875,00 | Migrar para o GPT-4o-mini , sentimento + opnion + resumo em uma chamada | ~$857/mês
| Azure AI Search $735,84 | Reduzir de 3 para 2 réplicas S1 | ~$245/mês |


** TCO Otimizado:** ~$2.078/mês , com -47% de economia ~$21.744/ano



🟢 **Passo 4 — Exportar** 

Arquivos Excel: finops/estimativa-qc.xlsx
Link compartilhável da calculadora: https://azure.com/e/f610e4847573467db1def71bc8dc2eb4


🟢 **Passo 5 — Comparar com Reserved Instances ** 

Arquivos Excel: finops/otimizacao-estimativa-qc.xlsx
Link compartilhável da calculadora com otimização de 1 ano Reserved: https://azure.com/e/52f563fe502843ee8b0e38f4e74b2405




---


## Reflexão:

A surpresa na análise foi descobrir que os serviços cognitivos dominam o custo da arquitetura QC e não a infraestrutura de dados.
Speech e Language juntos representam 46% , com vision 53%, da fatura. E isso inverte a intuição comum de que dados é sempre o maior custo em cloud.
Ao analisar o custo entre batch e real-time no Speech também foi possível ver uma diferença de 5,5 pelo mesmo resultado ($0,18/hora no batch vs $1,00/hora no real-time), o que mostra que uma decisão de arquitetura pequena que é quando processar o audio tem um impacto direto de ~$4.100/mês na fatura e isso conecta com toda a disciplina:
FinOps não é sobre cortar custo, é sobre alocar custo onde gera valor.
Também foi possível ver que transcrever 5.000 horas integrais enquanto uma amostra de 20% já seria o suficinete para a análise de qualidade é um desperdício. A arquitetura de um sistema agentic precisa pensar em custo por tomada de decisão, não só em custo por componente provisionado. Cada tool do agente tem um custo de inferência e esse custo escala com o volume de interações dos usuários da QC.
