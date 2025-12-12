# Modelagem dos Dados

## 1. Introdução

Este documento apresenta a modelagem adotada no MVP de Engenharia de Dados baseado em informações exportadas do DATASUS TABNET, cobrindo morbidade hospitalar e óbitos por HIV entre 2020 e 2025. A modelagem foi estruturada conforme boas práticas de arquitetura moderna (medallion architecture), com foco em padronização, rastreabilidade e preparação dos dados para análises na camada Gold.

---

## 2. Arquitetura Adotada (Medallion Architecture)

### **Bronze – Dados Brutos**

* Ingestão direta dos arquivos CSV exportados do TABNET.
* Armazenamento em formato Delta para garantir versionamento e confiabilidade.
* Sem transformações além da leitura inicial.

### **Silver – Dados Tratados e Padronizados**

* Padronização dos schemas.
* Conversão de tipos.
* Tratamento de valores nulos e inconsistentes.
* Uniformização de colunas: `uf`, `ano`, `faixa_etaria`, `sexo`, `quantidade`.
* Geração de tabelas normalizadas prontas para agregações.

### **Gold – Dados Modelados para Consumo Analítico**

* Aplicação de agregações e cálculos por estado, ano e faixa etária.
* Tabelas modeladas para responder às perguntas de negócio definidas na sprint.

---

## 3. Modelagem Conceitual

A modelagem segue uma abordagem simples em estrela (**Star Schema**) apropriada para análises descritivas.

### **Fato Principal**

**Fato_Obitos_HIV**

* Contém registros de óbitos decorrentes de internações no SUS por HIV.
* Granularidade: *UF + Ano + Faixa Etária + Sexo*.

### **Dimensões**

* **Dim_UF** — lista de Unidades da Federação com nome e sigla.
* **Dim_Ano** — intervalo 2020–2025.
* **Dim_FaixaEtaria** — faixas padronizadas utilizadas no MVP.
* **Dim_Sexo** — Masculino, Feminino ou Total.

---

## 4. Modelagem Lógica

### **Tabela: fato_obitos_hiv**

| Coluna       | Tipo   | Descrição                              |
| ------------ | ------ | ---------------------------------------|
| id_fato      | STRING | Chave primária da tabela               |
| uf           | INT    |Chave estrangeira para dim_uf           |
| ano          | INT    |Chave estrangeira para dim_ano          |
| faixa        | INT    |Chave estrangeira para dim_faixa_etaria |
| sexo         | INT    |Chave estrangeira para dim_ano          |
| quantidade   | INT    |Quantidade de óbitos           |

### **Tabela: dim_uf**

| Coluna  | Tipo   | Descrição                              |
| ------- | ------ | ---------------------------------------|
|id_uf    | INT    | Chave primária da tabela               |
|cod_ibge | INT    |Código dos estados pelo IBGE            |
|sigla_uf | STRING |Sigla representativa de todos os estados|
|nome_uf  | STRING |Nome dos estados                        |


### **Tabela: dim_faixa_etaria**

| Coluna  | Tipo   | Descrição                                                                        |
| ------- | ------ | ---------------------------------------------------------------------------------|
| id_faixa_etaria  |INT    | Chave primária da tabela                                                 |
| faixa_etaria   | STRING  | Rótulo indicando os intervalos estabelecidos entre as faixas de idade    |
| descricao      | STRING  | Descrição do intervalo de idade                                          |


### **Tabela: dim_sexo**

| Coluna  | Tipo     | Descrição                                |
| ------- | ------   | -----------------------------------------|
|id_sexo    | INT    | Chave primária da tabela                 |
| sexo      | STRING | Definição do sexo (masculino, feminino)  |

### **Tabela: dim_ano**

| Coluna  | Tipo   | Descrição                                |
| ------- | ------ | -----------------------------------------|
| id_ano    | INT  | Chave primária da tabela                 |
| ano       | INT  | ano considerado (2020 a 2025)            |

---

## 5. Regras de Transformação da Silver para Gold

* Remoção de valores nulos.
* Conversão de "-" para 0.
* Normalização dos nomes das colunas em todas as tabelas.
* Agregações por UF, ano, sexo e faixa etária.
* Geração de tabelas resumidas:

  * **obitos_por_uf**
  * **obitos_por_ano**
  * **obitos_por_faixa_etaria**
  * **obitos_por_sexo**

---

## 6. Diagrama Simplificado

<img width="829" height="469" alt="esquema_estrela_mvp" src="https://github.com/user-attachments/assets/15d99fe9-206d-4b18-a5f6-74054460e8c2" />




## 7. Considerações Finais

A modelagem proposta segue princípios de simplicidade, rastreabilidade e aderência ao escopo do MVP. Foi construída para suportar consultas analíticas diretas, dashboards e exportações para análises adicionais. Está alinhada às boas práticas da arquitetura em camadas (Bronze → Silver → Gold) e permite fácil expansão futura.

Caso deseje, posso gerar também a **versão final do README**, **dicionário de dados**, **qualidade dos dados** e **estrutura final para GitHub**.
