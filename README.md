# Sistema de Inteligência de Dados para Monitoramento da Saúde Indígena

**Projeto Final de Business Intelligence**
**Curso:** Ciência de Dados – Centro Universitário de Brasília (CEUB)
**Disciplina:** Business Intelligence
**Docente:** Prof. Pedro M. Pereira
**Equipe:** Lanna Soares e Cauã de Godoy

---

# Sumário

1. Visão Geral e Contexto de Negócio
2. Definição do Problema de Negócio
3. Objetivos Estratégicos (OKRs)
4. Indicadores de Desempenho (KPIs)
5. Arquitetura da Solução de BI
6. Modelagem Dimensional (Star Schema)
7. Processo de ETL (Power Query)
8. Camada Analítica e Medidas DAX
9. Segurança em Nível de Linha (RLS)
10. Dashboard e Storytelling Visual

---

# 1. Visão Geral e Contexto de Negócio

A Secretaria de Saúde Indígena (SESAI) é o órgão responsável pela formulação e execução das políticas públicas de saúde voltadas às populações indígenas brasileiras. Sua atuação ocorre por meio dos Distritos Sanitários Especiais Indígenas (DSEIs), unidades administrativas distribuídas em todo o território nacional.

A gestão dessa estrutura apresenta desafios relevantes:

* Grande dispersão geográfica;
* Dificuldades logísticas de acesso às comunidades;
* Necessidade de monitoramento contínuo dos indicadores de atenção primária;
* Distribuição heterogênea de recursos entre os territórios.

Portanto, o presente projeto propõe uma solução de Business Intelligence para controle dos indicadores de atenção primária à saúde indígena.

---

# 2. Definição do Problema de Negócio

## Contexto

A SESAI monitora diversos indicadores de saúde relacionados às populações indígenas, tais como:

* Cobertura vacinal;
* Consultas de pré-natal;
* Atendimento odontológico;
* Acompanhamento infantil;
* Outras ações de atenção primária.

Embora exista grande volume de dados, sua utilização estratégica é limitada pela ausência de uma solução integrada de análise.

---

## Problema de Negócio

**Como os indicadores de atenção primária à saúde indígena evoluíram entre 2022 e 2025, e quais DSEIs demonstraram maior eficiência na prestação dos serviços de saúde à população indígena?**

---

# 3. Objetivos Estratégicos (OKRs)

## Objetivo 1

### OKR - Ampliar o acesso aos serviços essenciais de saúde indígena.

---

## Objetivo 2

### OKR 2 — Reduzir desigualdades entre os DSEIs brasileiros.

---

## Objetivo 3

### OKR 3 - Aumentar a proporção de DSEIs com desempenho excelente

---

# 4. Indicadores de Desempenho (KPIs)

| KPI                        | Fórmula                           | Unidade | Meta | Justificativa                                  |
| -------------------------- | --------------------------------- | ------- | ---- | ---------------------------------------------- |
| Cobertura Média            | AVERAGE(Cobertura)                | %       | 70%  | Mede o desempenho geral dos programas de saúde |
| Coeficiente de Variação    | Desvio / Média                    | %       | <15% | Avalia a desigualdade entre os DSEIs           |
| % DSEIs Acima da Meta      | DSEIs acima da meta / Total DSEIs | %       | 50%  | Mede o percentual de unidades de excelência    |
| Melhor Cobertura Observada | MAX(Cobertura)                    | %       | 100% | Identifica referências e benchmarks internos   |

---

# 5. Arquitetura da Solução de BI

A solução foi construída seguindo uma arquitetura clássica de Business Intelligence.

```text
Dados Brutos (Fontes Públicas)
            │
            ▼
Power Query (ETL)
            │
            ▼
Modelagem Dimensional
            │
            ▼
Medidas DAX
            │
            ▼
Dashboard Executivo
            │
            ▼
Tomada de Decisão
```

---

# 6. Modelagem Dimensional (Star Schema)

O modelo foi estruturado utilizando um esquema estrela (Star Schema), visando:

* Melhor desempenho analítico;
* Simplicidade de navegação;
* Integridade referencial;
* Escalabilidade do modelo.

## Granularidade

Cada registro da tabela fato representa:

> Um indicador de saúde para determinado DSEI em um determinado trimestre.

---

## Tabela Fato

### FATO_SAUDE

Centraliza os eventos quantitativos de negócio.

Principais métricas:

* Quantidade de atendimentos;
* Cobertura;
* Classificação de desempenho.

---

## Dimensões

### DIM_DSEI

Atributos:

* Id_DSEI
* Nome_DSEI
* Nome_UF

---

### DIM_INDICADOR

Atributos:

* Id_Indicador
* Nome_Indicador

---

### DIM_TEMPO

Atributos:

* Id_Tempo
* Ano
* Trimestre
* Mês
* Nome_Mes
* Ano_Trimestre

---

## Relacionamentos

```text
DIM_TEMPO
      │
      └───(1:N)──► FATO_SAUDE ◄───(N:1)── DIM_DSEI
                              │
                              └───(N:1)── DIM_INDICADOR
```

Todos os relacionamentos foram configurados com:

* Cardinalidade 1:N;
* Direção de filtro simples;
* Integridade referencial preservada.

---

# 7. Processo de ETL (Power Query)

Todas as transformações foram realizadas exclusivamente no Power Query.

## Etapas Executadas

### Tipagem

Todas as colunas foram tipadas explicitamente: campos de percentual como Decimal Number, código do DSEI como Text (para evitar tratamento como valor numérico), datas como Date e campos de população como Whole Number.

### Tratamento de valores nulos e duplicatas

Linhas com valores nulos nos campos de cobertura foram identificadas e tratadas com substituição por zero quando o DSEI não registrou atendimentos no período, ou removidas quando representavam registros incompletos sem identificação de território. Duplicatas foram verificadas com base na chave composta cod_DSEI + dt_referencia.

### Criação de colunas calculadas

Foram criadas as colunas AnoMês (concatenação de Ano e Mês para ordenação cronológica), Trimestre (derivada do campo de data), AnoTrimestre (campo de suporte para eixos de gráficos) e Faixa_Cobertura (classificação em Crítica, Moderada, Adequada e Excelente com base nos percentuais de cobertura).

### Padronização de nomes

Todas as colunas foram renomeadas seguindo o padrão snake_case para as tabelas fato e Title Case para as dimensões. Os nomes originais dos indicadores (ex.: PNASPIAS_06_perc_prenat) foram mapeados para nomes legíveis (ex.: Pré-Natal) na Dim_Indicador.

### Unpivot e restauração da tabela fato

Os dados originais chegaram em formato wide — com uma coluna para cada indicador por linha de DSEI × período. Foi aplicado Unpivot nas colunas de indicador para gerar o formato long, com uma linha por DSEI × período × indicador, permitindo a criação da Dim_Indicador e a ligação da tabela fato ao modelo dimensional.

---

# 8. Camada Analítica e Medidas DAX

As medidas foram centralizadas em uma tabela dedicada denominada:

```text
MEDIDAS
```

---

## Medidas Básicas

* Total Atendidos
* Total DSEIs
* Média DSEIs
* Desvio DSEIs
* Cobertura Média

---

## Medidas com CALCULATE

* Qtd DSEIs Acima Meta
* % DSEIs Acima Meta

---

# 9. Segurança em Nível de Linha (RLS)

Foi implementada a funcionalidade de **Row Level Security (RLS)** no modelo semântico do Power BI, com o objetivo de restringir o acesso aos dados de acordo com a Unidade Federativa vinculada ao DSEI.

A regra de segurança foi criada com base no campo:

```text
DIM_DSEI[UF_Nome]
```

Foram configuradas duas funções de RLS:

## RLS — Acre

Permite visualizar apenas os dados dos DSEIs pertencentes ao estado do Acre.

Regra aplicada:

```DAX
[UF_Nome] = "Acre"
```

## RLS — Amazonas

Permite visualizar apenas os dados dos DSEIs pertencentes ao estado do Amazonas.

Regra aplicada:

```DAX
[UF_Nome] = "Amazonas"
```

Com essa implementação, usuários vinculados a uma determinada UF visualizam somente os registros correspondentes ao seu estado. Dessa forma, o relatório passa a contar com uma camada adicional de governança, controle de acesso e segurança da informação, permitindo que diferentes perfis de usuários acessem apenas os dados compatíveis com sua área de atuação.

---

# 10. Dashboard e Storytelling Visual

O dashboard foi estruturado em três páginas com propósitos distintos.

---

# Página 1 — Panorama Geral

Propósito: visão executiva consolidada dos KPIs e evolução histórica.

* 4 cards de KPI no topo: Cobertura Média, Coeficiente de Variação, % DSEIs acima da meta e Melhor DSEI;
* Velocímetro de status da meta (gauge chart);
* Gráfico de linhas: evolução da cobertura média por trimestre (2022–2025);
* Gráfico de barras horizontais: desempenho por indicador;
* Gráfico de barras verticais: índice de variabilidade por indicador;
* Filtros: Trimestre, Ano, DSEI, Estado, Indicador.

---

# Página 2 — Visão Territorial

Propósito: comparação entre DSEIs e identificação de territórios de referência e de atenção.

* Gráfico de barras horizontais: volume de atendimentos por DSEI;
* Gráfico de barras horizontais: volume por UF;
* Donut chart: classificação dos DSEIs por faixa de cobertura (Crítica, Moderada, Adequada, Excelente);
* Gráfico de barras verticais: ranking dos 5 DSEIs com maior cobertura média;
* Filtros: Trimestre, Ano, DSEI, Estado, Indicador.

---

# Conclusão

Este projeto demonstra a aplicação completa do ciclo de Business Intelligence:

**Dados Brutos → ETL → Modelagem → DAX → Governança → Visualização → Tomada de Decisão**

Mais do que construir um dashboard, a solução proposta transforma dados operacionais da saúde indígena em inteligência de negócio capaz de apoiar decisões estratégicas, reduzir desigualdades regionais e contribuir para uma gestão pública mais eficiente e orientada por evidências.
