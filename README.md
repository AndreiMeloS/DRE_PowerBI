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
Dcontas>> pEGAR >>NOva Medida Receita Bruta = CALCULATE ([Valor Real], dContas [Grupo Pincipal] = "(+) RECEITA BRUTA")
Aqui pegamos o nosso valor real mas estamos calculando apenas onde o grupo principal for igual a "(+) RECEITA BRUTA"


3.2 Criar um medida que nos dê flexbilidade de trablhar com Calculos, deduções e somas natural de uma DRE.
 Nova medida >> NOME Valor_DRE 

 Primiero passo criar uma varável cuja dormula:
 
 var DRE LinhaDRE = selectedvalue(dcampos DRE [Linha DRE])
 Returne
 stwitch (   #é a fundação que substutui um valor por  outro.
         LinhaDRE,
         "(+) Receita Bruta", [Rceita Bruta],
         blank())

 Fazer isso para todos os grupos principais da tabela dCampos DRE
 
 




























