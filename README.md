# 📊 Dashboard de DRE no Power BI

> Projeto de Business Intelligence desenvolvido no **Microsoft Power BI** para construção de uma **Demonstração do Resultado do Exercício (DRE)** interativa, utilizando modelagem dimensional, Power Query, DAX e visualizações gerenciais.

---

## 📌 Sumário

- [🎯 Objetivo](#-objetivo)
- [📊 KPIs extraídos da DRE](#-kpis-extraídos-da-dre)
- [📌 Premissas](#-premissas)
- [🗂️ Fonte de dados](#️-fonte-de-dados)
- [🚀 Passos iniciais](#-passos-iniciais)
- [📅 Criação da tabela calendário](#-criação-da-tabela-calendário)
- [🔗 Criação das relações entre as tabelas](#-criação-das-relações-entre-as-tabelas)
- [🧩 Estrutura do modelo](#-estrutura-do-modelo)
- [🎨 Construção da dashboard](#-construção-da-dashboard)
- [🧮 Criação das medidas DAX](#-criação-das-medidas-dax)
- [🎨 Formatação da tabela](#-formatação-da-tabela)
- [📊 Cards de indicadores](#-cards-de-indicadores)
- [📅 Filtro de período](#-filtro-de-período)
- [🏁 Resultado final](#-resultado-final)
- [🛠️ Tecnologias utilizadas](#️-tecnologias-utilizadas)
- [📁 Estrutura do projeto](#-estrutura-do-projeto)
- [📈 Conclusão](#-conclusão)

---

# 🎯 Objetivo

O objetivo deste projeto é desenvolver uma **Dashboard de DRE no Power BI**, transformando dados financeiros em informações gerenciais para acompanhamento da formação do resultado e análise do desempenho da empresa ao longo do tempo.

A dashboard foi desenvolvida para permitir:

- Análise da **Receita Bruta** e **Receita Líquida**;
- Acompanhamento de **custos e despesas**;
- Análise do **Lucro Bruto**;
- Acompanhamento do **EBITDA** e **EBIT**;
- Análise do **Resultado Financeiro**;
- Acompanhamento do **Lucro ou Prejuízo Líquido**;
- Análise de **margens e indicadores financeiros**;
- Comparação entre diferentes períodos;
- Aplicação de filtros de ano e período;
- Visualização gerencial dos principais componentes da DRE.

---

# 📊 KPIs extraídos da DRE

A DRE permite extrair diversos indicadores financeiros e operacionais. Neste projeto, serão considerados os seguintes **10 KPIs**:

| # | KPI | Fórmula | Objetivo |
|---:|---|---|---|
| 1 | **Receita Bruta** | Soma da Receita Bruta | Acompanhar o faturamento antes das deduções |
| 2 | **Receita Líquida** | Receita Bruta + Deduções | Identificar a receita após as deduções |
| 3 | **Lucro Bruto** | Receita Líquida + CPV | Avaliar o resultado após os custos |
| 4 | **Margem Bruta** | Lucro Bruto ÷ Receita Líquida | Avaliar a rentabilidade após os custos |
| 5 | **Despesas Operacionais** | Soma das despesas operacionais | Monitorar os gastos relacionados à operação |
| 6 | **EBITDA** | Lucro Bruto + Despesas Operacionais + Outras Receitas/Despesas | Avaliar o desempenho operacional |
| 7 | **Margem EBITDA** | EBITDA ÷ Receita Líquida | Avaliar a eficiência operacional |
| 8 | **EBIT** | EBITDA + D&A | Avaliar o resultado operacional após D&A |
| 9 | **Lucro/Prejuízo Líquido** | LAIR + IRPJ e CSLL | Identificar o resultado final do período |
| 10 | **Margem Líquida** | Lucro Líquido ÷ Receita Líquida | Avaliar quanto da receita se converte em resultado líquido |

---

# 📌 Premissas

## 📅 Período de análise

A tabela calendário será criada considerando o período de:

**01/01/2025 a 31/12/2026**

A tabela conterá todas as datas do período de análise.

Serão criadas colunas auxiliares para:

- Ano;
- Mês;
- Nome do mês;
- Último dia do mês;
- Mês/Ano.

O campo **Mês/Ano** será utilizado para facilitar a análise temporal, no formato:

```text
Jan/2025
Fev/2025
Mar/2025
...
```

---

## 🧹 Tratamento dos dados

Os dados brutos utilizados no projeto **não precisaram de tratamento prévio**, portanto foram prontamente carregados.

Mesmo assim, durante a construção do modelo serão consideradas validações relacionadas a:

- Datas;
- Códigos de contas;
- Classificação das contas;
- Valores realizados;
- Tipos de dados;
- Duplicidades;
- Valores nulos.

---

## 🗃️ Classificação das contas

Para que a DRE seja construída corretamente, as contas precisam estar classificadas de acordo com um **Grupo Principal**.

A estrutura de classificação utilizada será:

```text
Código da Conta
      ↓
Descrição da Conta
      ↓
Subgrupo
      ↓
Grupo Principal da DRE
```

Essa classificação será utilizada pelas medidas DAX para identificar a qual linha da DRE cada conta pertence.

---

# 🗂️ Fonte de dados

A principal fonte de dados utilizada no projeto é uma planilha denominada:

`Base_DRE`

A `Base_DRE` será utilizada como fonte dos dados financeiros que alimentarão o modelo do Power BI.

## 📦 Quantidade de dados

A quantidade de registros da `Base_DRE` dependerá da base utilizada no projeto.

O modelo foi estruturado para trabalhar com os dados financeiros disponíveis para o período de análise definido, permitindo que novos registros sejam incorporados posteriormente, desde que a estrutura da base seja mantida.

## 🔄 Integração das informações

O fluxo de integração será:

```text
Base_DRE
    ↓
Power Query
    ↓
Modelo de Dados
    ↓
Medidas DAX
    ↓
Dashboard de DRE
```

---

# 🚀 Passos iniciais

Os dados brutos não precisaram de tratamento, então foram prontamente carregados.

A partir da `Base_DRE`, os dados serão disponibilizados no Power BI para construção do modelo dimensional e posteriormente utilizados nas medidas DAX.

---

# 📅 Criação da tabela calendário

É criada uma tabela que contém todas as datas do período de análise.

## Criação da tabela

No Power BI:

**Transformar dados → Nova Fonte → Consulta Nula**

Em seguida, escrever o código `List.Dates` e pressionar **Enter**.

Utilizar os seguintes parâmetros:

- **Start:** 01/01/2025
- **Quantidade de dias:** 730
- **Step:** 1 dia

A partir da tabela calendário, serão criadas novas colunas:

- Ano;
- Último dia do mês;
- Nome do mês;
- Mês/Ano.

O campo **Mês/Ano** será utilizado para apresentar o período no formato:

```text
Jan/2025
```

---

# 🔗 Criação das relações entre as tabelas

A criação da relação entre as tabelas será realizada utilizando o conceito de **Schema Estrela (Star Schema)**.

A estrutura do modelo será composta por:

<img width="573" height="638" alt="image" src="https://github.com/user-attachments/assets/4c740ad2-ad26-45d4-bf3b-f98d69dbcb61" />

Além dessas tabelas, teremos a `dCamposDRE`, que será utilizada como uma **tabela desconectada**.

---

# 🧩 Estrutura do modelo

O modelo será composto pelas seguintes tabelas:

## `fAnalitica`

Será nossa **tabela FATO**, responsável por armazenar os dados financeiros e valores realizados.

---

## `dContas`

Será nossa tabela dimensão de contas.

Para cada **Código de Conta**, teremos informações como:

- Grupo Principal;
- Subgrupo;
- Descrição da Conta.

---

## `dCalendario`

Será nossa dimensão de tempo, contendo as datas utilizadas para análise dos períodos.

---

## `dCamposDRE`

Será uma tabela **desconectada**, utilizada para estruturar e controlar as linhas que serão apresentadas na DRE.

Não haverá relacionamento direto dessa tabela com as demais tabelas do modelo.

---

# 🎨 Construção da dashboard

## 1. Caixa de texto — Título

Adicionar uma **Caixa de Texto**.

Definir o título da dashboard e utilizar:

**Fonte: 24**

Exemplo:

```text
DRE — Demonstração do Resultado do Exercício
```

---

# 2. Criar botão de segmentação — Filtro de ano

Criar um botão de segmentação utilizando a tabela calendário.

### Configuração

Utilizar:

```text
dCalendario[Ano]
```

Os anos disponíveis serão:

```text
2025
2026
```

### Formatação

**Geral**

- Desativar título.

**Visual → Configuração de segmentação**

- Seleção única.

**Visual → Botões**

- Retângulo;
- Cantos arredondados;
- Raio: 5.

**Visual → Botões → Texto explicativo**

- Valor;
- Forma centralizada.

---

# 3. Visual de tabela

Para construir o visual da DRE, primeiro precisamos criar uma nova tabela para organizar nossas medidas.

## Criação da tabela `MEDIDAS`

Acessar:

**Modelagem → Nova Tabela**

Utilizar:

```DAX
MEDIDAS = {0}
```

Essa tabela será utilizada para organizar as medidas DAX desenvolvidas no projeto.

---

# 3.1 Criação das medidas para o visual de tabela

## Valor Real

Criar uma nova medida:

```DAX
Valor Real =
SUM(fAnalitica[Valor Realizado])
```

Essa medida será responsável por somar todo o valor realizado no período selecionado.

---

# 4. Criação das medidas DAX

Agora será criada uma medida que permitirá trabalhar com **cálculos, deduções e somas naturais de uma DRE**.

A ideia é retornar os valores dos diferentes campos da DRE conforme a linha selecionada.

A tabela `dCamposDRE` será responsável por armazenar as linhas da DRE.

A estrutura será:

```text
(+) RECEITA BRUTA
(-) DEDUÇÕES
(=) RECEITA LÍQUIDA
(-) CPV
(=) LUCRO BRUTO
(-) DESPESAS OPERACIONAIS
(+/-) OUTRAS RECEITAS/DESPESAS
(=) EBITDA
(-) DEPRECIAÇÃO E AMORTIZAÇÃO
(=) EBIT
(+/-) RESULTADO FINANCEIRO
(=) LAIR
(-) IRPJ E CSLL
(=) LUCRO/PREJUÍZO LÍQUIDO
```

---

# 4.1 Fórmula para retornar a Receita Bruta

Criar uma nova medida:

```DAX
Receita Bruta =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(+) RECEITA BRUTA"
)
```

Aqui pegamos o nosso `Valor Real`, mas calculamos apenas quando o **Grupo Principal** for igual a:

```text
(+) RECEITA BRUTA
```

Agora vamos adicionar essa medida à medida `Valor DRE`.

A variável `LinhaDRE` estará sempre verificando qual campo da DRE está selecionado na tabela.

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    BLANK()
)
```

O `BLANK()` será utilizado quando a linha selecionada não corresponder à Receita Bruta.

---

# 4.2 Fórmula para retornar os campos de Deduções

Criar uma nova medida:

```DAX
Deduções =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(-) DEDUÇÕES"
)
```

Agora adicionamos a medida à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    BLANK()
)
```

---

# 4.3 Fórmula para retornar o campo de Receita Líquida

A Receita Líquida será calculada a partir da Receita Bruta e das Deduções.

Considerando a convenção de sinais utilizada:

```DAX
Receita Liquida =
[Receita Bruta] + [Deduções]
```

Agora adicionamos a Receita Líquida à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    BLANK()
)
```

---

# 4.4 Fórmula para retornar o CPV

O **Custo dos Produtos Vendidos (CPV)** será calculado utilizando o `CALCULATE` da medida `Valor Real`, filtrando o Grupo Principal:

```text
(-) CPV
```

Criar a medida:

```DAX
CPV =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(-) CPV"
)
```

Agora adicionamos o CPV à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    BLANK()
)
```

---

# 4.5 Fórmula para retornar o Lucro Bruto

O Lucro Bruto será calculado como:

```text
Receita Líquida + CPV
```

Criar a medida:

```DAX
Lucro Bruto =
[Receita Liquida] + [CPV]
```

Agora adicionamos o Lucro Bruto à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    BLANK()
)
```

---

# 4.6 Fórmula para retornar as Despesas Operacionais

A nova medida será o `CALCULATE` da medida `Valor Real`, considerando apenas as contas cujo Grupo Principal seja:

```text
(-) DESPESAS OPERACIONAIS
```

Criar a medida:

```DAX
Despesas Operacionais =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(-) DESPESAS OPERACIONAIS"
)
```

Agora adicionamos a medida à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    BLANK()
)
```

---

# 4.7 Fórmula para retornar "(+/-) OUTRAS RECEITAS/DESPESAS"

A nova medida será o `CALCULATE` da medida `Valor Real`, considerando apenas o Grupo Principal:

```text
(+/-) OUTRAS RECEITAS/DESPESAS
```

Criar a medida:

```DAX
Outras Receitas e Despesas =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(+/-) OUTRAS RECEITAS/DESPESAS"
)
```

Agora adicionamos à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    BLANK()
)
```

---

# 4.8 Fórmula para retornar "(=) EBITDA"

Nessa etapa será criada a medida de EBITDA.

Na estrutura utilizada neste projeto:

```text
EBITDA =
Lucro Bruto
+ Despesas Operacionais
+ Outras Receitas e Despesas
```

Criar a medida:

```DAX
EBITDA =
[Lucro Bruto]
+ [Despesas Operacionais]
+ [Outras Receitas e Despesas]
```

Agora adicionamos o EBITDA à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    BLANK()
)
```

---

# 4.9 Fórmula para retornar "(-) DEPRECIAÇÃO E AMORTIZAÇÃO"

A medida será o `CALCULATE` do `Valor Real`, considerando o Grupo Principal:

```text
(-) DEPRECIAÇÃO E AMORTIZAÇÃO
```

Criar a medida:

```DAX
Depreciação e Amortização =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(-) DEPRECIAÇÃO E AMORTIZAÇÃO"
)
```

Agora adicionamos à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    BLANK()
)
```

---

# 4.10 Fórmula para retornar "(=) EBIT"

O EBIT será calculado como:

```text
EBITDA + Depreciação e Amortização
```

Criar a medida:

```DAX
EBIT =
[EBITDA] + [Depreciação e Amortização]
```

Agora adicionamos o EBIT à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    "(=) EBIT", [EBIT],
    BLANK()
)
```

---

# 4.11 Fórmula para retornar "(+/-) RESULTADO FINANCEIRO"

O Resultado Financeiro será calculado utilizando o `CALCULATE` da medida `Valor Real`, onde o Grupo Principal seja:

```text
(+/-) RESULTADO FINANCEIRO
```

Criar a medida:

```DAX
Resultado Financeiro =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(+/-) RESULTADO FINANCEIRO"
)
```

Agora adicionamos à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    "(=) EBIT", [EBIT],
    "(+/-) RESULTADO FINANCEIRO", [Resultado Financeiro],
    BLANK()
)
```

---

# 4.12 Fórmula para retornar "(=) LAIR"

O LAIR será calculado como:

```text
EBIT + Resultado Financeiro
```

Criar a medida:

```DAX
LAIR =
[EBIT] + [Resultado Financeiro]
```

Agora adicionamos o LAIR à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    "(=) EBIT", [EBIT],
    "(+/-) RESULTADO FINANCEIRO", [Resultado Financeiro],
    "(=) LAIR", [LAIR],
    BLANK()
)
```

---

# 4.13 Fórmula para retornar "(-) IRPJ E CSLL"

A medida será calculada utilizando o `CALCULATE` da medida `Valor Real`, onde o Grupo Principal seja:

```text
(-) IRPJ E CSLL
```

Criar a medida:

```DAX
IRPJ e CSLL =
CALCULATE(
    [Valor Real],
    dContas[Grupo Principal] = "(-) IRPJ E CSLL"
)
```

Agora adicionamos à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    "(=) EBIT", [EBIT],
    "(+/-) RESULTADO FINANCEIRO", [Resultado Financeiro],
    "(=) LAIR", [LAIR],
    "(-) IRPJ E CSLL", [IRPJ e CSLL],
    BLANK()
)
```

---

# 4.14 Fórmula para retornar "(=) LUCRO/PREJUÍZO LÍQUIDO"

O Lucro ou Prejuízo Líquido será calculado como:

```text
LAIR + IRPJ e CSLL
```

Criar a medida:

```DAX
Lucro ou Prejuizo Líquido =
[LAIR] + [IRPJ e CSLL]
```

Agora adicionamos o resultado final à `Valor DRE`:

```DAX
Valor DRE =
VAR LinhaDRE =
    SELECTEDVALUE(dCamposDRE[Linha DRE])

RETURN
SWITCH(
    LinhaDRE,
    "(+) RECEITA BRUTA", [Receita Bruta],
    "(-) DEDUÇÕES", [Deduções],
    "(=) RECEITA LÍQUIDA", [Receita Liquida],
    "(-) CPV", [CPV],
    "(=) LUCRO BRUTO", [Lucro Bruto],
    "(-) DESPESAS OPERACIONAIS", [Despesas Operacionais],
    "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
    "(=) EBITDA", [EBITDA],
    "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Amortização],
    "(=) EBIT", [EBIT],
    "(+/-) RESULTADO FINANCEIRO", [Resultado Financeiro],
    "(=) LAIR", [LAIR],
    "(-) IRPJ E CSLL", [IRPJ e CSLL],
    "(=) LUCRO/PREJUÍZO LÍQUIDO", [Lucro ou Prejuizo Líquido],
    BLANK()
)
```

---

# 🎨 Formatação da tabela

Após concluir as medidas, o próximo passo é formatar a tabela para deixá-la visualmente mais organizada.

## Tamanho dos valores

Configurar o tamanho dos valores, colunas e linhas para aproximadamente:

**11**

---

## 🟢🔴 Subtotais com formatação condicional

Selecionar a tabela e acessar:

**Pincel de formatação → Visual → Elementos da célula**

Configurar a formatação condicional para destacar os resultados de acordo com seu sinal:

```text
Valor positivo → Verde
Valor negativo → Vermelho
```

Essa configuração facilita a interpretação visual dos resultados da DRE.

---

# 📊 Cards de indicadores

Criar cartões para os principais indicadores da DRE:

- **Receita Líquida**
- **Lucro Bruto**
- **EBITDA**
- **EBIT**
- **Lucro/Prejuízo Líquido**

A ideia é permitir que os principais resultados sejam identificados rapidamente sem a necessidade de interpretar toda a tabela.

---

# 📅 Filtro de período

Adicionar um filtro de data utilizando a tabela `dCalendario`.

O filtro deverá permitir que o usuário personalize o período de análise da dashboard.

Exemplo:

<img width="208" height="70" alt="image" src="https://github.com/user-attachments/assets/51e5a59f-0cde-4e05-92a9-8075e4d978e9" />

Dessa forma, todas as medidas e indicadores da DRE serão recalculados de acordo com o período selecionado.

---

# 🏁 Resultado final

Ao final da construção, teremos uma dashboard capaz de apresentar:

<img width="1309" height="730" alt="image" src="https://github.com/user-attachments/assets/d342250e-db3f-4fda-8b04-a9f358787192" />


---

# 🛠️ Tecnologias utilizadas

| Tecnologia | Aplicação |
|---|---|
| **Microsoft Power BI** | Construção da dashboard e modelo de dados |
| **Power Query** | Importação e tratamento dos dados |
| **DAX** | Criação das medidas e regras de negócio |
| **Microsoft Excel** | Fonte de dados `Base_DRE` |
| **Star Schema** | Modelagem dimensional |

---

# 📁 Estrutura do projeto

```text
📁 Dashboard-DRE
│
├── 📄 README.md
│
├── 📁 Dados
│   └── Base_DRE.xlsx
│
├── 📁 PowerBI
    └── Dashboard_DRE.pbix
```

---

# 📈 Conclusão

O projeto apresenta a construção de uma **Dashboard de DRE no Power BI**, partindo de uma base financeira (`Base_DRE`) e passando pelas etapas de:

```text
Fonte de dados
      ↓
Power Query
      ↓
Modelagem dimensional
      ↓
Relacionamentos
      ↓
Medidas DAX
      ↓
DRE dinâmica
      ↓
KPIs
      ↓
Visualizações
      ↓
Dashboard gerencial
```

A utilização do **Star Schema**, da tabela desconectada `dCamposDRE` e da medida dinâmica `Valor DRE` permite estruturar uma DRE flexível e interativa.

O resultado final é uma ferramenta de **Business Intelligence aplicada à análise financeira**, capaz de transformar dados financeiros em informações para acompanhamento de desempenho e suporte à tomada de decisão.

---

## 👤 Projeto

**Dashboard de DRE — Power BI**

Projeto desenvolvido para demonstrar conhecimentos práticos em:

- Business Intelligence;
- Análise financeira;
- Demonstração do Resultado do Exercício;
- Modelagem de dados;
- Power Query;
- DAX;
- Indicadores financeiros;
- Visualização de dados;
- Suporte à tomada de decisão.
