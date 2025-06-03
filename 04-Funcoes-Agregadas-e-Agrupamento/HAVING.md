# Cláusula HAVING

A cláusula `HAVING` é usada para filtrar os resultados de uma consulta `SELECT` *após* as linhas terem sido agrupadas pela cláusula `GROUP BY` e as funções agregadas terem sido calculadas.
Enquanto a cláusula `WHERE` filtra linhas individuais *antes* do agrupamento, a cláusula `HAVING` filtra os grupos formados.

Você só pode usar `HAVING` se também estiver usando `GROUP BY` (ou se estiver aplicando uma função agregada a toda a tabela, o que é menos comum para `HAVING`). As condições em `HAVING` geralmente envolvem funções agregadas.

## Sintaxe Básica

```sql
SELECT coluna_agrupamento1, FUNCAO_AGREGADA(coluna_agregada) AS alias_agregado
FROM nome_da_tabela
[WHERE condicao_filtragem_linhas]
GROUP BY coluna_agrupamento1
HAVING condicao_filtragem_grupos; -- Condição geralmente envolve funções agregadas
[ORDER BY ...];

WHERE vs HAVING
Característica	  WHERE	                                          HAVING
Quando opera	    Antes do agrupamento (GROUP BY)	                Depois do agrupamento e das funções agregadas
O que filtra	    Linhas individuais	                            Grupos de linhas
Pode usar agrg.	  Não (funções agregadas ainda não calculadas)	  Sim (condições são baseadas nos resultados das agrg.)
Requer GROUP BY?	Não                                            	Sim (ou quando agregações são aplicadas à tabela toda)

Exemplos

Usando a tabela Vendas das seções anteriores:

Exemplo 1: Selecionar regiões que tiveram mais de 1 venda

SELECT
    Regiao,
    COUNT(*) AS TotalVendas
FROM Vendas
GROUP BY Regiao
HAVING COUNT(*) > 1 -- Filtra os grupos (Regioes) com mais de 1 venda
ORDER BY TotalVendas DESC;

Resultado (a região Sul e a NULL são excluídas porque têm apenas 1 venda cada):

Regiao   | TotalVendas
---------|------------
Nordeste | 2
Sudeste  | 2

Exemplo 2: Selecionar produtos cujo valor total vendido (Quantidade * ValorUnitario) é maior que R$ 2000

SELECT
    Produto,
    SUM(Quantidade * ValorUnitario) AS ValorTotalVendido
FROM Vendas
GROUP BY Produto
HAVING SUM(Quantidade * ValorUnitario) > 2000
ORDER BY ValorTotalVendido DESC;

Resultado:

Produto | ValorTotalVendido
--------|------------------
Laptop  | 9700.00
Monitor | 3300.00

(Mouse, Teclado e Webcam são excluídos porque seu ValorTotalVendido não é > 2000)

Exemplo 3: Combinando WHERE e HAVING
Selecionar produtos (excluindo 'Webcam' antes do agrupamento) que tiveram uma quantidade total vendida maior que 5.

SELECT
    Produto,
    SUM(Quantidade) AS QuantidadeTotalVendida
FROM Vendas
WHERE Produto <> 'Webcam' -- Filtra linhas ANTES do agrupamento
GROUP BY Produto
HAVING SUM(Quantidade) > 5 -- Filtra grupos DEPOIS do agrupamento
ORDER BY QuantidadeTotalVendida DESC;

Resultado:

Produto | QuantidadeTotalVendida
--------|-----------------------
Mouse   | 10

A 'Webcam' é removida pelo WHERE.

'Laptop' (qtd 3) e 'Teclado' (qtd 5) são agrupados, mas depois removidos pelo HAVING porque SUM(Quantidade) não é > 5.

'Monitor' (qtd 3) também é removido pelo HAVING.

Considerações
Ordem de Execução: Lembre-se que HAVING é aplicado depois de WHERE e GROUP BY.

Funções Agregadas: As condições em HAVING tipicamente se referem aos resultados das funções agregadas calculadas para cada grupo. Você também pode usar colunas que estão na cláusula GROUP BY dentro da condição HAVING.

Performance: Tente filtrar o máximo possível com a cláusula WHERE antes do agrupamento, pois isso reduz o número de linhas que precisam ser processadas pelo GROUP BY e pelas funções agregadas, o que pode melhorar a performance.
