# Descrição Detalhada do MVP

## 1. Busca pelos Dados

A base selecionada foi o **DATASUS TABNET**, especificamente os dados de **Morbidade Hospitalar do SUS** e **Óbitos por HIV**, por ser uma fonte oficial, atualizada, confiável e adequada aos objetivos analíticos definidos no trabalho.

Os dados do DATASUS não possuem restrição de uso e são disponibilizados publicamente pelo Governo Federal. A referência de origem está documentada no README principal.

---

## 2. Coleta

A coleta dos dados foi realizada manualmente através da interface DATASUS TABNET, respeitando as diretrizes éticas de uso de dados públicos e evitando qualquer forma de web scraping automatizada que pudesse violar termos de acesso.

Após o download dos arquivos em formato CSV, eles foram enviados para o ambiente de armazenamento em nuvem no Databricks Free Edition, compondo a camada **Bronze** do Data Lake.

Como os dados são públicos e amplamente abertos, não houve necessidade de mascaramento, anonimização ou cuidado especial com confidencialidade.

---

## 3. Modelagem

A modelagem do MVP segue uma arquitetura de Data Lake estruturada por camadas:

* **Bronze** — dados brutos, exatamente como fornecidos pelo DATASUS.
* **Silver** — dados tratados, padronizados e com schema consistente.
* **Gold** — dados analíticos prontos para visualização e consumo.

Também foi elaborado um **Catálogo de Dados** contendo:

* descrição dos atributos,
* domínios e categorias válidas,
* limites mínimo/máximo para atributos numéricos,
* tratamentos aplicados,
* linhagem de cada tabela (origem → transformação → destino).

A modelagem Gold segue um padrão próximo a **Esquema Estrela**, formando a tabela fato `fato_obitos_hiv`, relacionada às dimensões UF, ano, faixa etária e sexo.

---

## 4. Carga

A carga dos dados foi implementada na plataforma Databricks utilizando pipelines de ETL distribuídos em notebooks:

1. **Bronze:** criação das tabelas Delta diretamente dos CSVs.
2. **Silver:** normalização de tipos, padronização de colunas e correção de valores nulos.
3. **Gold:** agregações, cálculos derivados e preparação das tabelas finais.

O processo de transformação foi documentado, descrevendo principalmente:

* substituição de valores ausentes,
* padronização de categorias,
* normalização das faixas etárias,
* conciliação entre diferentes arquivos separados por filtros da TABNET.

---

## 5. Análise

A etapa de análise foi dividida conforme orientado pela disciplina:

### 5.a Qualidade dos Dados

Foi realizada uma verificação completa da qualidade dos dados, incluindo:

* análise atributo a atributo,
* detecção de valores nulos ou inconsistentes,
* comparação entre schemas das 22 tabelas brutas,
* correção de divergências de categorias.

Embora o DATASUS seja relativamente estruturado, foram encontrados valores representados por "-" e campos vazios, todos corrigidos na Silver.

O relatório completo encontra-se no arquivo **qualidade_dados.md**.

### 5.b Solução do Problema

Com os dados já estruturados na camada Gold, foram respondidas todas as perguntas analíticas definidas no escopo do MVP.

Cada resposta inclui:

* cálculo SQL correspondente,
* interpretação dos resultados,
* discussão contextual baseada nos números.

Entre os resultados analisados:

* evolução e volume de óbitos relacionados ao HIV,
* distribuição por estados (UF),
* análise de maior/menor ocorrência,
* comportamento por faixas etárias e sexo.

---

## Encerramento

Este documento descreve de forma detalhada o ciclo completo do MVP, alinhado aos requisitos da sprint Engenharia de Dados e às boas práticas de coleta, modelagem, ETL e análise em ambientes de Data Lake no Databricks.

