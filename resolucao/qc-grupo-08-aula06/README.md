# Entrega Aula 06 — Grupo 08 — FinOps: TCO Quantum Commerce

Estimativa de custo mensal da arquitetura completa da Quantum Commerce em escala real
usando o **Azure Pricing Calculator**, com análise de otimização e comparação com
Reserved Instances.

Documento principal: [`entrega-grupo-aula06.md`](entrega-grupo-aula06.md)

## Estrutura

```
qc-grupo-08-aula06/
├── entrega-grupo-aula06.md          # documento principal (N1 FinOps + reflexão)
├── finops/
│   ├── estimativa-qc.xlsx           # exportado do Pricing Calculator (baseline)
│   ├── otimizacao-estimativa-qc.xlsx # estimativa com Reserved Instances 1 ano
│   ├── pricing-calculator-link.md   # links compartilháveis da calculadora
│   └── analise-otimizacao.md        # top 3 caros + otimizações + cenário Reserved
└── README.md                        # este arquivo
```

## Cenário

9 serviços da arquitetura QC configurados em **East US 2**, volumes de escala real
(não free tier). TCO estimado, otimizado e comparado com Reserved Instances de 1 ano.

| Componente | Configuração |
|---|---|
| Blob Storage | 5 TB Hot + 3 TB Cool + 2 TB Archive |
| Azure SQL Database | Hyperscale 4 vCores + 100 GB |
| Cosmos DB | 4.000 RU/s + 50 GB |
| Azure AI Search | Standard S1, 3 réplicas, 12 partitions |
| Function App | Premium EP1, 5M req/mês |
| Azure AI Services | 1M Language + 5.000h Speech + 500k Vision |
| Azure ML | Workspace + B2s + DS3_v2 Endpoint 24/7 |
| Application Insights | 5 GB logs/mês |
| Egress | 500 GB/mês |
