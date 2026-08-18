# DRE_PowerBI






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


3.2 Criar um medida que nos dê flexbilidade de trablhar com Calculos, deduções e somas natural de uma DRE.

 FORMULA PARA RETORNAR A RECEITA BRUTA
 
 Nova medida >> NOME Valor_DRE 

Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE]) --> Essa variabel vai ta sempre olhando para qualcampo da nossa DRE (ex.receviat bruta ,dedeucç~eos, receita liquida)  vai estra selecionado na nossa tabela.

Switch para substituir um valor por outro --> Vamos pegar o campo de DRE e a criar o primeiro switch. Objetivo so swwitch: Qunado ele for igual "Rceitab ruta" ele trará valor dareceita bruta.

Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        blank()) 

O blanl() é quando nã ofor receita bruta vai ficar vazio.

PARA OS CAMPOS DE DEDUÇÕES
Nova medida >> Deduçoes = CALCULATE ([Valor Real], dContas[Grupo Principal] = "(-) DEDUÇÕES")

Valor DRE = 
var LinhaDRE = selectedvalue(dCamposDRE[Linha DRE])
switch (  
        LinhaDRE,
        "(+) RECEITA BRUTA", [Receita Bruta]
        "(-) Deduções", [Deduções],
        blank()) 

 





























