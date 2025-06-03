# Cláusula ORDER BY

A cláusula `ORDER BY` é usada para ordenar as linhas do resultado de uma consulta `SELECT`. Você pode ordenar por uma ou mais colunas, em ordem ascendente ou descendente.

Se a cláusula `ORDER BY` não for especificada, a ordem das linhas retornadas não é garantida e pode variar.

## Sintaxe Básica

```sql
SELECT coluna1, coluna2, ...
FROM nome_da_tabela
[WHERE condicao]
ORDER BY coluna_para_ordenar1 [ASC | DESC],
         coluna_para_ordenar2 [ASC | DESC], ...;

Componentes:

coluna_para_ordenarX: A coluna pela qual você deseja ordenar os resultados. Pode ser o nome da coluna ou o número da posição da coluna na lista SELECT (ex: ORDER BY 1 para ordenar pela primeira coluna selecionada - embora usar o nome seja mais legível).

ASC (Ascendente): Ordena do menor para o maior (para números) ou em ordem alfabética (para strings). Este é o padrão se nada for especificado.

DESC (Descendente): Ordena do maior para o menor ou em ordem alfabética inversa.

Exemplos
Usando a tabela Produtos da seção WHERE:

Exemplo 1: Selecionar todos os produtos ordenados pelo Nome em ordem ascendente

SELECT Nome, Preco
FROM Produtos
ORDER BY Nome ASC; -- ASC é opcional aqui, pois é o padrão

Resultado (ordenado alfabeticamente por Nome):

Nome                  | Preco
----------------------|--------
Cadeira de Escritório |  800.00
Laptop XPTO           | 3500.00
Mesa Digitalizadora   |  450.00
Monitor 24"           | 1200.00
Mouse Gamer           |  150.00
Teclado Mecânico      |  300.00

Exemplo 2: Selecionar todos os produtos ordenados pelo Preco em ordem descendente

SELECT Nome, Preco
FROM Produtos
ORDER BY Preco DESC;

Resultado (do mais caro para o mais barato):

Nome                  | Preco
----------------------|--------
Laptop XPTO           | 3500.00
Monitor 24"           | 1200.00
Cadeira de Escritório |  800.00
Mesa Digitalizadora   |  450.00
Teclado Mecânico      |  300.00
Mouse Gamer           |  150.00

Exemplo 3: Selecionar produtos ordenados pela Categoria e, dentro de cada categoria, pelo Preco (ascendente)

SELECT Nome, Categoria, Preco
FROM Produtos
ORDER BY Categoria ASC, Preco ASC;

Resultado (produtos agrupados por categoria, e dentro de cada categoria, ordenados do mais barato para o mais caro):

Nome                  | Categoria   | Preco
----------------------|-------------|--------
Mouse Gamer           | Eletrônicos |  150.00
Teclado Mecânico      | Eletrônicos |  300.00
Monitor 24"           | Eletrônicos | 1200.00
Laptop XPTO           | Eletrônicos | 3500.00
Cadeira de Escritório | Móveis      |  800.00
Mesa Digitalizadora   | NULL        |  450.00  -- NULLs geralmente aparecem primeiro ou por último, dependendo do SGBD

Exemplo 4: Ordenando por uma coluna que não está no SELECT (nem todos SGBDs permitem ou é boa prática)
Alguns SGBDs permitem, mas é mais comum e claro ordenar por colunas que estão na lista SELECT.

-- Ordenar pela DataCadastro, mesmo que não seja exibida
SELECT Nome, Preco
FROM Produtos
ORDER BY DataCadastro DESC;

Exemplo 5: Ordenando pela posição da coluna (menos legível)

SELECT Nome, Preco, Categoria
FROM Produtos
ORDER BY 3 DESC, 2 ASC; -- Ordena pela terceira coluna (Categoria DESC) e depois pela segunda (Preco ASC)

Tratamento de NULLs
O comportamento de ordenação para valores NULL pode variar entre SGBDs:

Alguns SGBDs tratam NULLs como os menores valores (aparecem primeiro em ASC).

Outros tratam NULLs como os maiores valores (aparecem por último em ASC).

Alguns SGBDs oferecem sintaxe explícita para controlar isso, como NULLS FIRST ou NULLS LAST.

-- Exemplo (PostgreSQL, Oracle):
-- ORDER BY Categoria ASC NULLS LAST;

Considerações
Performance: Ordenar grandes conjuntos de resultados pode ser uma operação custosa em termos de performance, especialmente se as colunas de ordenação não estiverem indexadas.

Legibilidade: Usar nomes de colunas em ORDER BY é geralmente mais legível e fácil de manter do que usar números de posição.
