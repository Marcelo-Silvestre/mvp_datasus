# MVP Datasus — Engenharia de Dados (PUC-Rio | Nov/2025)

Este repositório contém o MVP desenvolvido para a Sprint Engenharia de Dados da Pós Graduação em Ciência de Dados e Analytics da PUC-Rio.  

O objetivo é construir um **pipeline completo de dados** utilizando Databricks, desde a ingestão até a análise final, utilizando dados públicos do **DATASUS TABNET** relacionados a **óbitos por HIV** no Brasil.

---

## Objetivo do MVP

Construir um pipeline de dados capaz de:

- Reunir e tratar dados de internações e óbitos do SUS relacionados ao HIV, no período **jan/2020 a set/2025**.
- Modelar os dados em camadas (Bronze → Silver → Gold).
- Avaliar qualidade dos dados.
- Responder às perguntas analíticas definidas no escopo.

---

## Perguntas que o MVP responde:

1- Quantos óbitos, registrados a partir de internações no SUS, ocorreram de janeiro de 2020 a setembro de 2025?  
2. Quantos óbitos por HIV ocorreram de janeiro de 2020 a setembro de 2025?  
3- Qual o percentual de óbitos por HIV ocorridos de janeiro de 2020 a setembro de 2025, em relação a todos os óbitos no mesmo período?  
4. Qual UF teve o maior total de óbitos por HIV no período e quantos foram?  
5. Qual UF teve o menor total de óbitos por HIV no período e quantos foram?  
6. Qual é a distribuição dos óbitos por HIV por faixa etária?  
7. Qual faixa etária concentrou o maior número de óbitos?  
8. Qual faixa etária concentrou o menor número de óbitos?  
9. Como se distribuem os óbitos por HIV por sexo?  

Essas perguntas são respondidas **na camada Gold**.

---

## Fontes de Dados

Todos os dados são públicos e foram obtidos do **DATASUS TABNET**, a partir dos seguintes módulos:

- *Morbidade Hospitalar do SUS — por local de internação*  
- *Lista Morb CID-10: Doença pelo vírus da imunodeficiência humana [HIV]*  
- Período filtrado: **2020 → 2025**

As bases foram exportadas manualmente da interface TABNET, nos formatos CSV, no seguinte endereço:
http://tabnet.datasus.gov.br/cgi/deftohtm.exe?sih/cnv/niuf.def

**Data do download:** 10 de Novembro de 2025.

---

## Arquitetura do Projeto (Databricks)




## Pipeline ETL (Resumo)

### Bronze
- Ingestão dos CSVs.
- Armazenamento bruto.
- Criação das tabelas Delta com *schema original*.

###  Silver
- Padronização dos nomes de colunas.
- Conversões de tipos.
- Limpeza de dados (`-` → 0, null → 0).
- Normalização de anos, faixas e categorias.

###  Gold
- Geração da tabela fato `fato_obitos_hiv`.
- Criação de agregações por:
  - UF  
  - Ano  
  - Faixa etária  
  - Sexo  
    
- Materialização das análises que respondem às perguntas 1–9.

---

## Qualidade dos Dados

- Verificação de schema em todas as 22 tabelas.  
- Detecção e correção de valores nulos.  
- Conversão de colunas numéricas inconsistentes.  
- Contagem de registros por UF × Ano validada.

---

## © Licença dos Dados

Todos os dados utilizados são públicos e disponibilizados por órgãos do Governo Federal do Brasil.

---

## Autor

**Marcelo Alexandre Machado Silvestre**  
Projeto desenvolvido para a Sprint Engenharia de Dados da Pós em Ciência de Dados e Analytics da PUC-Rio.




