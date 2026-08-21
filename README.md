# DRE_PowerBI

PASSOS INICIAIS
Os dados brutos não precisaram de tratamento, então foram prontamente carregados.

Criação da tabela calendário

É uma tabela que contém todas as datas do meu período de análise.

Transformar dados >> Nova Fonte >> Consulta Nula >> Escrever o código List.Dates e paerta enter

= List.dates e adicionas os parâmetro Start 01/01/2025 / 730 / step 1

Cria novas colunas de Ano, último dia do mês, nome do mês 
Mescla Mês e Ano TIPO "Jan/2025"

Criação da relação entre tabela foi no schema estrela

<img width="573" height="638" alt="image" src="https://github.com/user-attachments/assets/4c740ad2-ad26-45d4-bf3b-f98d69dbcb61" />

EXPLICAÇÃO SOBRE AS TABELA

Trabalharemos uma tabela Analítica fAnalitica será nossa tabela FATO, Uma tabela de contas dContas (onde para Códido de Contas ela trás um grupo principal, sub grupo e Descrição da Cona) terceiro teremos a tabela dCamposDRE que será uma tabela desconectada não erá relação.


VISUAÇIZAÃO DE DADOS - TEMA CLÁSSICO

1. Caixa de Texto >> Título >> Fonte 24

2. Criar botão de Segmnetação filtro de ano

Botões >> dcalendario filtro de ano (2025 e 2026)

Fomratção: 
Gerla >> desativar titulo / 
Visual >> Configuraçã ode segmentação >> Seleção unica
Visual >> Botões >> Retangolo cantos redondados raio 5
Visual >> Botões >> Texto explicativo >> Valor >> Forma centralizada

3. Visual de Tabela

Para isso precisamos criar uma nova tabela com o nome de MEDIDAS
Modelagem >> Nova Tabela >> Onde faormula dela vais ser = Medidas {0}

3.1 Criado medidas para usar o Visual de Tbaelas

Nova medida >> Valor Real = sum(fAnalitico [Valor Realizado])
Essa medida vai somar todo valor relizado no período.

Nova medida que vamos precisar pegar o  Valor realizado e calcular ele em cada grupo principal da nossa DRE
Dcontas>> pEGAR >>NOva Medida >> Receita Bruta = CALCULATE ([Valor Real], dContas [Grupo Pincipal] = "(+) RECEITA BRUTA")
Aqui pegamos o nosso valor real mas estamos calculando apenas onde o grupo principal for igual a "(+) RECEITA BRUTA"


4 Criar um medida que nos dê flexbilidade de trablhar com Calculos, deduções e somas natural de uma DRE.

Part retornar os valores dos campos da DRE conforme a imagem
<img width="336" height="392" alt="image" src="https://github.com/user-attachments/assets/8eb19658-df6e-453b-be01-5df998041dea" />



4.1 FORMULA PARA RETORNAR A RECEITA BRUTA
 
 Nova medida >> NOME Valor_DRE 

Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE]) --> Essa variabel vai ta sempre olhando para qualcampo da nossa DRE (ex.receviat bruta ,dedeucç~eos, receita liquida)  vai estra selecionado na nossa tabela.

Switch para substituir um valor por outro --> Vamos pegar o campo de DRE e a criar o primeiro switch. Objetivo so swwitch: Qunado ele for igual "Rceitab ruta" ele trará valor dareceita bruta.

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        blank()) 
```
O blanl() é quando nã ofor receita bruta vai ficar vazio.

4.2 FORMULA PARA RETORNAR OS CAMPOS DE DEDUÇÕES
Nova medida >> Deduçoes = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(-) DEDUÇÕES")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        blank()) 
```
4.3 FORMULA PARA RETORNAR O CAMPO DE RECEITA LIQUIDA

Nada mais nada menos que que a Medida Rceita Bruta menos a medida Deduções

Nova medida >> Receita liquida = [Receita Bruta] + [Deduções] 

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        blank())
```
4.4 FORMULA PARA RETORNAR O CPV

O custo produto vendido vai sero CALCULATE do valor realizado, onde que o Grupo de contas for igual "(-) CPV"

Nova Medida >> CPV = CALCULATE ([Valor Real], dContas [Grupo Principal] = "(-) CPV")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        blank())
```
4.5 FORMULA PARA RETORNAR O LUCRO BRUTO

Vai ser igual receita liquida mais o CPV

NOVA MEDIDA >> Lucro Bruto  = [Receita Liquida] + [CPV]

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        blank())
```
4.6 FORMULA PARA RETORNAR as Despesas Operacionais

Anova medida vai ser Vai ser o calculate da nossa medida Valor Real, onde no grupo principal for igual a "(=) Despesas Operacionais"

NOCA MEDIDA >>  Despesas Operacionais = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(-) Despesas Operacionais")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        blank())
```
4.7 FORMULA PARA RETORNAR "(+/-) OUTRAS RECEITAS/DESPESAS"

Nessa nova medida onde vai ser igual CALCULATE do nosso campo  Valor Real onde o grupo principal for igual 

Nova medida >> Outras Receitas e Despesas = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(+/-) OUTRAS RECEITAS/DESPESAS")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
        blank())
```


4.8 FORMULA PARA RETORNAR "(=) EBITDA"

Nessa nova medida será Lucro Bruto mais despas Operacionais mais outras receitas e despesas.

Nova medida >> EBITDA = ´[lUCRO bRUTO[ + ´[Depssas Operacionais] + [Outras Rceitas e Despesas]

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
        blank())
```
4.9 FORMULA PARA RETORNAR "(-) DEPRECIAÇÃO E AMORTIZAÇÃO"

Nessa nova medida é ocalculate do valor real a onde ogrupo pricnipal for igual a "(-) DEPRECIAÇÃO E AMORTIZAÇÃO"

Nova medida >> Depreciação e Armotização = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(-) DEPRECIAÇÃO E AMORTIZAÇÃO" )

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
       "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
        blank())
```
4.10 FORMULA PARA RETORNAR "(=) EBIT"

Nessa nova medida IGUAL AO EBITDA mais Depreciação e Amortização

Nova medida >> EBIT = [EBITDA] + [Depreciação e Armotização]

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
         "(=) EBIT", [EBIT] 
        blank())
```

4.11 FORMULA PARA RETORNAR "(+/-) RESULTADO FINANCEIRO"

Nessa nova medida Resultado financeiro igual  CALCULATE do Valor Real, onde o Grupo Principal for igual a "(+/-) RESULTADO FINANCEIRO"

Nova medida >> Resultado Financeiro = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(+/-) RESULTADO FINANCEIRO")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
         "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
         "(=) EBIT", [EBIT] 
         "(+/-) RESULTADO FINANCEIRO", [RESULTADO FINANCEIRO]
        blank())
```

4.12 FORMULA PARA RETORNAR "(=) LAIR"

Nessa nova medida será o EBIT mais Resultado Financeiro

Nova medida >> LAIR = [EBIT] + [Resultado Financeiro]

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
         "(=) EBIT", [EBIT] 
         "(+/-) RESULTADO FINANCEIRO", [RESULTADO FINANCEIRO]
         "(=) LAIR", [LAIR]
        blank())
```

4.13 FORMULA PARA RETORNAR "(-) IRPJ E CSLL"

Nessa nova medida vai ser igual ao aCALCULATE do  Valor Real, onde o grupos principal fori igual a "(-) IRPJ E CSLL"

Nova medida >> IRPJ e ISLL = CALCULATE([Valor Real], dContas[Grupo Principal] = "(-) IRPJ E CSLL")

Adiciona na medida "Valor DRE"
```DAX
Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
         "(=) EBIT", [EBIT] 
         "(+/-) RESULTADO FINANCEIRO", [RESULTADO FINANCEIRO]
         "(=) LAIR", [LAIR]
         "(-) IRPJ E CSLL", [IRPJ e CSLL]
        blank())
```
4.14 FORMULA PARA RETORNAR "(=) LUCRO/PREJUIZO LÍQUIDO"

Nessa nova medida

Nova medida >> Lucro ou Prejuizo Líquido = [LAIR] + [IRPJ e CSLL]

Adiciona na medida "Valor DRE"
```DAX

Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        "(=) RECEITA LIQUIDA", [Receita Liquida],
        "(-) CPV", [CPV]
        "(=) Lucro Bruto", [Lucro Bruto]
        "(-) Despesas Operacionais", [Despesas Operacionais]
        "(+/-) OUTRAS RECEITAS/DESPESAS", [Outras Receitas e Despesas],
         "(=) EBITDA", [EBITDA]
         "(-) DEPRECIAÇÃO E AMORTIZAÇÃO", [Depreciação e Armotização]
         "(=) EBIT", [EBIT] 
         "(+/-) RESULTADO FINANCEIRO", [RESULTADO FINANCEIRO]
         "(=) LAIR", [LAIR]
         "(-) IRPJ E CSLL", [IRPJ e CSLL]
         "(=) LUCRO/PREJUIZO LÍQUIDO", [Lucro ou Prejuizo Líquido]
        blank())
```

5 PROXIMO PASSO É FORMATAR NOSSA TABELA PARA DEIXARMOS VISULAEMNTE MELHOR.

TAMANHO DOS VALORES /COLUNA / LINHAS TAANHO 11

5.1 Deixar os subtotoais vermelho se for negativo e verde para positivo.

Clica na tabela >> Pincel de formatação >> Visual >> Elementos da célula 
Configura como naimagem abaixo:
<img width="904" height="394" alt="image" src="https://github.com/user-attachments/assets/1d72a820-a52f-40d9-923a-b491c4a78d58" />

5.1 Cria um visual de cartões

Para Rceita líquida / Lucro bruto / EBITDA / EBIT / LUCRO / PREJUIZO

5.2 INSERIR UM FILTRO DE DAT QUE PODE PERSONALIZAR O PERIODE ANALISE

IMAGEM:
<img width="208" height="70" alt="image" src="https://github.com/user-attachments/assets/51e5a59f-0cde-4e05-92a9-8075e4d978e9" />

finalizado nosso dasbhard
<img width="1275" height="712" alt="image" src="https://github.com/user-attachments/assets/262c064e-46b6-42b2-ba30-e8ccf4227ef2" />


