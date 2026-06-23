# Sistema de Inteligência de Dados para Monitoramento da Saúde Indígena

Projeto final da disciplina de **Business Intelligence** do curso de **Ciência de Dados** do **Centro Universitário de Brasília (CEUB)**. A solução foi desenvolvida para apoiar o monitoramento dos indicadores de atenção primária à saúde indígena brasileira por meio de um dashboard analítico construído no Power BI.

---

## Descrição do Projeto

A Secretaria Especial de Saúde Indígena (SESAI) é responsável pela gestão do Subsistema de Atenção à Saúde Indígena (SasiSUS), coordenando os 34 Distritos Sanitários Especiais Indígenas (DSEIs) distribuídos pelo território nacional.

Devido à dispersão geográfica das comunidades indígenas e à fragmentação das bases de dados, a gestão dos serviços de saúde enfrenta desafios relacionados ao monitoramento dos indicadores e à identificação de desigualdades regionais.

Este projeto desenvolve uma solução de Business Intelligence capaz de consolidar dados de saúde indígena entre os anos de 2022 e 2025, transformando informações operacionais em indicadores estratégicos para apoiar a tomada de decisão baseada em evidências.

---

## Dataset Utilizado

Os dados utilizados no projeto são provenientes de bases públicas relacionadas aos indicadores de saúde indígena disponibilizados pela Secretaria Especial de Saúde Indígena (SESAI) e pelo Ministério da Saúde.

- **Repositório:** https://github.com/Caua-Godoy/projeto-populacao-indigena
- **Período analisado:** 2022 a 2025
- **Unidade de análise:** Distritos Sanitários Especiais Indígenas (DSEIs)

---

## Problema de Negócio

**Como os indicadores de atenção primária à saúde indígena evoluíram entre 2022 e 2025 e quais DSEIs apresentaram maior eficiência na prestação de serviços à população indígena?**

A solução busca permitir análises históricas e comparativas entre os territórios atendidos, apoiando a identificação de áreas críticas e subsidiando o planejamento de políticas públicas de saúde indígena.

---

## Principais Indicadores (KPIs)

### Cobertura Média Nacional (%)

- **Fórmula:** Média(Cobertura dos DSEIs)
- **Meta:** ≥ 70%
- **Objetivo:** Avaliar o nível geral de cobertura dos serviços de saúde indígena no país.

### Percentual de DSEIs Acima da Meta (%)

- **Fórmula:** (Número de DSEIs com cobertura ≥ 70% ÷ Total de DSEIs) × 100
- **Objetivo:** Identificar quantos territórios alcançaram o nível mínimo de desempenho esperado.

### Melhor Cobertura por DSEI (%)

- **Fórmula:** Máximo(Cobertura Média dos DSEIs)
- **Objetivo:** Identificar os territórios de melhor desempenho e disseminar boas práticas.

### Coeficiente de Variação entre os DSEIs (%)

- **Fórmula:** (Desvio Padrão das Coberturas ÷ Média das Coberturas) × 100
- **Meta:** < 15%
- **Objetivo:** Avaliar o grau de desigualdade na prestação dos serviços de saúde entre os territórios.

---

## Tecnologias Utilizadas

- Power BI Desktop
- Power Query (ETL)
- Linguagem DAX
- Modelagem Dimensional (Star Schema)
- Row-Level Security (RLS)
- Git e GitHub

---

## Instruções de Execução do Projeto

### 1. Clonar o Repositório

```bash
git clone https://github.com/Caua-Godoy/projeto-populacao-indigena.git
```

### 2. Abrir o Projeto

- Certifique-se de possuir o Power BI Desktop instalado e atualizado.
- Abra a pasta do projeto clonada em seu computador.
- Abra o arquivo `.pbip` localizado na raiz do repositório.

### 3. Atualizar as Fontes de Dados

Caso seja necessário, ajuste os caminhos das fontes de dados e selecione a opção **Atualizar** no Power BI Desktop para recarregar o modelo semântico e os relatórios.

---

## Arquitetura da Solução

```text
Dados Públicos
      ↓
Power Query (ETL)
      ↓
Modelagem Dimensional (Star Schema)
      ↓
Medidas DAX
      ↓
Dashboard Executivo
      ↓
Tomada de Decisão
```

---

## Autores

**Lanna Correa Soares**  
Graduanda em Ciência de Dados  
Centro Universitário de Brasília (CEUB)

**Cauã de Godoy**  
Graduando em Ciência de Dados  
Centro Universitário de Brasília (CEUB)

---

**Disciplina:** Business Intelligence  
**Professor:** Pedro M. Pereira  
**Instituição:** Centro Universitário de Brasília (CEUB)
