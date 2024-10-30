``` sql 

-- Selecionando alguns campos: SELECT <lista_de_colunas>

/*select nome, sobrenome, cargo from empregados*/

/*select nome, sobrenome, cargo from empregados*/

/*select escritorio_codigo, cidade, pais from escritorios*/

------------------------------------------------------------------------

-- Selecionando todos os campos : SELECT * FROM <nome_tabela>

/*select * from clientes

select * from empregados

select * from escritorios

select * from produtos*/

-------------------------------------------------------------------------

-- Usando condição : SELECT <lista_de_colunas> FROM <nome_tabela> WHERE <condição_de_seleção>
--Operadores relacionais : >, < , >=, <=, =, <>

/*select nome, sobrenome from clientes
where pais = 'USA'

select nome, sobrenome, pais, limite_credito from clientes
where limite_credito > 100000 

select nome from empregados

select escritorio_codigo, cidade, pais from escritorios
where pais = 'japan'

select pedido_codigo, cliente, data_pedido, status from pedidos
where status = 'Entregue'

select nome, categoria from produtos
where categoria = 5

select * from pagamentos
where cliente = 114

select * from pagamentos 
where valor >= 100000

select pedido_codigo, data_pedido, cliente, status from pedidos 
where status <> 'entregue'

select nome, sobrenome, pais from clientes
where pais <> 'USA'*/

--------------------------------------------------------------

-- Usando operadores lógicos : AND conjunção / OR disjunção / NOT negação

/*select nome, sobrenome, pais, limite_credito from clientes
where pais='USA' AND limite_credito > 100000

select nome, chefe, cargo from empregados
where chefe = 1102 and cargo = 'Vendedor'

set dateformat dmy
select pedido_codigo, cliente, data_pedido, status from pedidos
where data_entregue < '30/07/2003' and status = 'entregue'

select nome, sobrenome, pais, limite_credito from clientes
where (pais = 'USA' AND limite_credito > 100000) or  (pais <> 'USA' AND limite_credito > 50000)

select nome, sobrenome, pais, limite_credito from clientes
where limite_credito >= 50000 and limite_credito <= 80000 

select nome, fabricante, estoque, preco_compra from produtos
where preco_compra >= 60 and preco_compra <= 70*/

--------------------------------------------------------------

--Usando Between para pegar intervalo de valores ou intervalo de datas :
--WHERE <nome_coluna> BETWEEN <valor1> AND <valor2>
--WHERE <nome_coluna> NOT BETWEEN <valor1> AND <valor2>

/*select nome, sobrenome, pais, limite_credito from clientes
where limite_credito between 50000 and 80000

select nome, fabricante, preco_compra from produtos
where preco_compra between 60 and 70

select nome, fabricante, estoque from produtos
where estoque not between 500 and 7000*/


--------------------------------------------------------------

--Usando Like para trabalhar com strings
--WHERE <nome_coluna> LIKE <valor> / WHERE <nome_coluna> NOT LIKE <valor>
--%termo – termina com termo / termo% – começa com termo / %termo% – contém termo

select nome, sobrenome from clientes
where sobrenome like 'br%'

select nome, sobrenome from clientes
where nome like '%chrome%'

select nome, endereco1 from clientes
where endereco1 like '%baden%'

select nome from clientes
where nome like '%aa%'

select * from produtos
where nome like '%ford%' and descricao like '%replica%'

select * from produtos
where nome not like '%ford%' and descricao like '%replica%'

select * from produtos
where nome like '%2003%' and descricao like '%replica%'

select * from produtos
where nome not like '%2003%' and descricao like '%replica%'

select nome from clientes 
where nome like '__A%'

select nome from clientes 
where nome like '__A_O%'

--------------------------------------------------------------

--Usando IN para filtrar dados de uma lista de valores ou strings
--WHERE <nome_coluna> IN (<lista_de_valores>) / WHERE <nome_coluna> NOT IN
--(<lista_de_valores>)

select nome,categoria from produtos
where categoria in (3,6)

select nome, contato from clientes
where contato in ('jean', 'Carine', 'Diego')

select nome, pais from clientes
where pais not in ('france','usa')

select pais, cidade from escritorios
where pais in ('usa') and cidade not in ('boston','nyc') 

--------------------------------------------------------------
--Usando Is NULL no filtro para filtrar valores com conteúdo NULO
--WHERE <nome_coluna> IS NULL
--WHERE <nome_coluna> IS NOT NULL

select nome, cargo, chefe from empregados
where chefe is null

select nome, pais, gerente_conta from clientes
where gerente_conta is null

select nome, endereco1, cidade, estado from clientes
where estado is not null

--------------------------------------------------------------

--Usando Order By para ordenar consulta por ordem ascendente ou descendente
--SELECT <lista_de_colunas> FROM <nome_tabela> WHERE <condição_de_seleção>
--ORDER BY {<nome_coluna>|<num_col> [ASC|DESC]}

SELECT NOME, PRECO_SUGERIDO FROM PRODUTOS
ORDER BY PRECO_SUGERIDO ASC

SELECT NOME, ESTOQUE FROM PRODUTOS
ORDER BY ESTOQUE DESC

SELECT NOME, SOBRENOME, LIMITE_CREDITO FROM CLIENTES
WHERE PAIS = 'USA' AND LIMITE_CREDITO > 100000 
ORDER BY LIMITE_CREDITO DESC

SELECT NOME, FABRICANTE, PRECO_COMPRA FROM PRODUTOS
WHERE PRECO_COMPRA BETWEEN 60 AND 70
ORDER BY NOME

SELECT NOME, SOBRENOME FROM EMPREGADOS
ORDER BY NOME, SOBRENOME 

SELECT NOME, ENDERECO1, CIDADE, ESTADO FROM CLIENTES
WHERE ESTADO IS NOT NULL
ORDER BY ESTADO, CIDADE

--------------------------------------------------------------

--CÁLCULOS SOBRE A CONSULTA

SELECT PRODUTO_CODIGO, NOME, PRECO_SUGERIDO + 10 AS PRECO_NOVO FROM PRODUTOS

SELECT NOME, PAIS, LIMITE_CREDITO, LIMITE_CREDITO * 1.2 AS NOVO_LIMITE FROM CLIENTES

--------------------------------------------------------------
--OBJETOS AGREGADORES
--MAX(<nome_coluna>) – Maior Valor / MIN(<nome_coluna>) – Menor Valor

SELECT MIN(PRECO_UNITARIO)AS MENOR_VALOR FROM DETALHES_PEDIDO
WHERE PRODUTO = 'S10_1678'

SELECT MAX(PRECO_UNITARIO)AS MAIOR_VALOR FROM DETALHES_PEDIDO
WHERE PRODUTO = 'S10_1678'

SELECT MAX(PRECO_UNITARIO) AS MAIOR_VALOR, MIN(PRECO_UNITARIO)AS MENOR_VALOR FROM DETALHES_PEDIDO

-- SUM(<nome_coluna>) – Somatória de Valores

SELECT SUM(VALOR) AS TOTAL FROM PAGAMENTOS

SELECT CLIENTE, SUM(VALOR) AS TOTAL FROM PAGAMENTOS
WHERE CLIENTE <> 121
GROUP BY CLIENTE

SELECT SUM(ESTOQUE) AS ESTOQUE FROM PRODUTOS

SELECT PRODUTO_CODIGO, SUM(ESTOQUE) AS TOTAL FROM PRODUTOS
WHERE PRODUTO_CODIGO ='s12_3891'
GROUP BY PRODUTO_CODIGO

SELECT  PRODUTO_CODIGO,SUM(ESTOQUE) AS TOTAL FROM PRODUTOS
WHERE PRODUTO_CODIGO IN('s12_3891', 'S10_4962','S12_1108')
GROUP BY PRODUTO_CODIGO

-- AVG(<nome_coluna>) – Média de Valores

SELECT AVG(PRECO_SUGERIDO) AS MEDIA FROM PRODUTOS
WHERE PRODUTO_CODIGO = 'S12_1108'

SELECT AVG(VALOR) AS MEDIA FROM PAGAMENTOS

-- COUNT(<nome_coluna>) ou COUNT(*) – Contagem de Valores

SELECT COUNT(*) AS QTD FROM PRODUTOS
WHERE PRECO_SUGERIDO > 100

SELECT COUNT(*) AS QTD FROM PRODUTOS
WHERE ESTOQUE > 5000

SELECT COUNT(*) AS QTD FROM CLIENTES
WHERE  PAIS = 'USA'

SELECT COUNT(*) AS QTD FROM PRODUTOS
WHERE  CATEGORIA IN (3,6)

SELECT COUNT(*) AS QTD FROM PRODUTOS
WHERE NOME LIKE '%FORD%' AND DESCRICAO LIKE '%REPLICA%'

-- DISTINCT(<nome_coluna>) – Excluir Valores Repetidos

SELECT DISTINCT PAIS FROM CLIENTES

SELECT DISTINCT CARGO FROM EMPREGADOS

-- GROUP BY(<nome_coluna>) – Agrupar Valores

SELECT PAIS, COUNT(*) AS QTD FROM CLIENTES
GROUP BY PAIS

SELECT CATEGORIA, SUM(PRECO_SUGERIDO) AS PRECO FROM PRODUTOS
GROUP BY CATEGORIA

-- HAVING – Filtrando registros usando agregadores

SELECT PAIS, COUNT(*) AS QTD FROM CLIENTES
GROUP BY PAIS
HAVING COUNT(*)>10

SELECT PAIS, COUNT(*) AS QTD, AVG(LIMITE_CREDITO) AS MEDIACREDITO FROM CLIENTES
GROUP BY PAIS
HAVING AVG(LIMITE_CREDITO)>0

```
