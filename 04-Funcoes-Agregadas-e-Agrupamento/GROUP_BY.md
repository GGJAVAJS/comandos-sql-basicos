# Cláusula GROUP BY

A cláusula `GROUP BY` é usada em conjunto com funções agregadas (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) para agrupar linhas que têm os mesmos valores em uma ou mais colunas.
Para cada grupo, a função agregada calcula um valor de resumo.

## Sintaxe Básica

```sql
SELECT coluna_agrupamento1, [coluna_agrupamento2, ...], FUNCAO_AGREGADA(coluna_agregada) AS alias_agregado
FROM nome_da_tabela
[WHERE condicao_filtragem_linhas] -- Filtra linhas ANTES do agrupamento
GROUP BY coluna_agrupamento1, [coluna_agrupamento2, ...]
[HAVING condicao_filtragem_grupos] -- Filtra grupos DEPOIS do agrupamento
[ORDER BY ...];

Regra Importante:
Qualquer coluna na lista SELECT que não seja parte de uma função agregada DEVE estar listada na cláusula GROUP BY.

Exemplos
Usando a tabela Vendas da seção anterior:

Exemplo 1: Contar o número de vendas por Regiao

SELECT
    Regiao,
    COUNT(*) AS TotalVendasPorRegiao
FROM Vendas
GROUP BY Regiao
ORDER BY Regiao;

Resultado:

Regiao   | TotalVendasPorRegiao
---------|---------------------
Nordeste | 2
Sudeste  | 2
Sul      | 1
NULL     | 1  -- Linhas com Regiao NULL formam seu próprio grupo

Exemplo 2: Calcular o valor total vendido (Quantidade * ValorUnitario) por Produto

SELECT
    Produto,
    SUM(Quantidade * ValorUnitario) AS ValorTotalVendidoPorProduto
FROM Vendas
GROUP BY Produto
ORDER BY Produto;

Resultado:

Produto  | ValorTotalVendidoPorProduto
---------|----------------------------
Laptop   | 9700.00  -- (2*3200 + 1*3300)
Monitor  | 3300.00
Mouse    | 1400.00
Teclado  | 1400.00
Webcam   |    0.00  -- (0*200)

Exemplo 3: Encontrar a quantidade média vendida e o preço unitário máximo por Regiao e Produto

SELECT
    Regiao,
    Produto,
    AVG(Quantidade) AS MediaQuantidadeVendida,
    MAX(ValorUnitario) AS MaxValorUnitario
FROM Vendas
WHERE Quantidade > 0 -- Aplicando um filtro antes do agrupamento
GROUP BY Regiao, Produto
ORDER BY Regiao, Produto;

Resultado (filtrando apenas vendas com Quantidade > 0):

Regiao   | Produto | MediaQuantidadeVendida | MaxValorUnitario
---------|---------|------------------------|-----------------
Nordeste | Laptop  | 1.00                   | 3300.00
Nordeste | Teclado | 5.00                   |  280.00
Sudeste  | Laptop  | 2.00                   | 3200.00
Sudeste  | Mouse   | 10.00                  |  140.00
Sul      | Monitor | 3.00                   | 1100.00
-- A Webcam não apareceria aqui porque Quantidade = 0 foi filtrado pelo WHERE

Ordem de Execução Lógica (Simplificada)
Para entender GROUP BY, WHERE, e HAVING:

FROM: Define a(s) tabela(s) fonte.

WHERE: Filtra as linhas individuais.

GROUP BY: Agrupa as linhas filtradas.

Funções Agregadas: Calculam valores para cada grupo.

HAVING: Filtra os grupos (baseado nos resultados das funções agregadas).

SELECT: Seleciona as colunas e os resultados das agregações.

DISTINCT: Remove duplicatas do resultado final.

ORDER BY: Ordena o resultado final.

LIMIT/TOP/ROWNUM: Restringe o número de linhas no resultado final.

Considerações

NULLs em GROUP BY: Linhas onde a coluna de agrupamento é NULL formarão um grupo separado.

Colunas no SELECT: Lembre-se da regra: se uma coluna está no SELECT e não está em uma função agregada, ela deve estar no GROUP BY.

Filtragem:

Use WHERE para filtrar linhas antes que o agrupamento e as agregações ocorram.

Use HAVING para filtrar grupos depois que as agregações ocorreram (próxima seção).
