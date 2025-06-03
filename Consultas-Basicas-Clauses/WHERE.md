# Cláusula WHERE

A cláusula `WHERE` é usada para filtrar os resultados de uma consulta `SELECT` (e também para especificar quais linhas afetar em comandos `UPDATE` e `DELETE`). 
Somente as linhas que satisfazem a condição especificada na cláusula `WHERE` são retornadas ou afetadas.

## Sintaxe Básica

```sql
SELECT coluna1, coluna2, ...
FROM nome_da_tabela
WHERE condicao;

Componentes:

condicao: Uma expressão lógica que resulta em TRUE, FALSE ou UNKNOWN para cada linha.

Operadores Comuns na Cláusula WHERE
Operador	     Descrição	                                Exemplo

=	             Igual a	                                WHERE preco = 10.50

<> ou !=	     Diferente de (Não igual a)	                WHERE status <> 'Concluído'

>	             Maior que	                                WHERE quantidade > 100

<	             Menor que	                                WHERE idade < 18

>=	             Maior ou igual a	                        WHERE saldo >= 0

<=	             Menor ou igual a	                        WHERE desconto <= 0.15

BETWEEN	         Entre um valor inclusivo e outro	        WHERE data BETWEEN '2023-01-01' AND '2023-12-31'

LIKE	         Procura por um padrão (com % e _)	        WHERE nome LIKE 'Jo%'

IN	             Corresponde a qualquer valor em uma lista	WHERE pais IN ('Brasil', 'Portugal')

IS NULL	         Verifica se o valor é NULO	                WHERE email IS NULL

IS NOT NULL	     Verifica se o valor NÃO é NULO             WHERE telefone IS NOT NULL

Operadores Lógicos

AND: Retorna TRUE se ambas as condições forem verdadeiras.

OR: Retorna TRUE se pelo menos uma das condições for verdadeira.

NOT: Nega uma condição (retorna TRUE se a condição for falsa).

Use parênteses () para controlar a ordem de avaliação quando combinar múltiplos operadores lógicos.

Exemplos
Usando a tabela Produtos:

CREATE TABLE Produtos (
    ID INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Categoria VARCHAR(50),
    Preco DECIMAL(10, 2),
    QuantidadeEstoque INT,
    DataCadastro DATE
);

INSERT INTO Produtos VALUES (1, 'Laptop XPTO', 'Eletrônicos', 3500.00, 10, '2023-01-15');
INSERT INTO Produtos VALUES (2, 'Mouse Gamer', 'Eletrônicos', 150.00, 50, '2023-02-20');
INSERT INTO Produtos VALUES (3, 'Teclado Mecânico', 'Eletrônicos', 300.00, 0, '2023-03-10');
INSERT INTO Produtos VALUES (4, 'Monitor 24"', 'Eletrônicos', 1200.00, 5, '2023-01-25');
INSERT INTO Produtos VALUES (5, 'Cadeira de Escritório', 'Móveis', 800.00, 15, '2023-04-01');
INSERT INTO Produtos VALUES (6, 'Mesa Digitalizadora', NULL, 450.00, 8, '2023-05-05');

Exemplo 1: Selecionar produtos da categoria 'Eletrônicos'

SELECT Nome, Preco
FROM Produtos
WHERE Categoria = 'Eletrônicos';

Resultado:

Nome               | Preco
-------------------|--------
Laptop XPTO        | 3500.00
Mouse Gamer        |  150.00
Teclado Mecânico   |  300.00
Monitor 24"        | 1200.00

Exemplo 2: Selecionar produtos com preço maior que R$ 1000

SELECT Nome, Preco
FROM Produtos
WHERE Preco > 1000;

Resultado:

Nome        | Preco
------------|--------
Laptop XPTO | 3500.00
Monitor 24" | 1200.00

Exemplo 3: Selecionar produtos da categoria 'Eletrônicos' E com preço menor que R$ 500

SELECT Nome, Categoria, Preco
FROM Produtos
WHERE Categoria = 'Eletrônicos' AND Preco < 500;

Resultado:

Nome             | Categoria   | Preco
-----------------|-------------|------
Mouse Gamer      | Eletrônicos | 150.00
Teclado Mecânico | Eletrônicos | 300.00

Exemplo 4: Selecionar produtos cujo nome começa com 'Laptop' OU 'Monitor'

SELECT Nome
FROM Produtos
WHERE Nome LIKE 'Laptop%' OR Nome LIKE 'Monitor%';

Resultado:

Nome
------------
Laptop XPTO
Monitor 24"

Exemplo 5: Selecionar produtos com quantidade em estoque entre 10 e 50 (inclusivo)

SELECT Nome, QuantidadeEstoque
FROM Produtos
WHERE QuantidadeEstoque BETWEEN 10 AND 50;

Resultado:

Nome                  | QuantidadeEstoque
----------------------|------------------
Laptop XPTO           | 10
Mouse Gamer           | 50
Cadeira de Escritório | 15

Exemplo 6: Selecionar produtos que estão nas categorias 'Eletrônicos' ou 'Móveis'

SELECT Nome, Categoria
FROM Produtos
WHERE Categoria IN ('Eletrônicos', 'Móveis');

Exemplo 7: Selecionar produtos cuja categoria é NULA

SELECT Nome, Categoria
FROM Produtos
WHERE Categoria IS NULL;

Resultado:

Nome                | Categoria
--------------------|-----------
Mesa Digitalizadora | NULL

Considerações
Tipos de Dados: Certifique-se de que os valores comparados na condição WHERE são compatíveis. Por exemplo, compare números com números e strings com strings (geralmente entre aspas simples 'valor').

Performance: Condições WHERE em colunas indexadas geralmente resultam em consultas mais rápidas.

Strings Case-Sensitive: A comparação de strings pode ser case-sensitive (diferenciar maiúsculas de minúsculas) ou case-insensitive, dependendo da configuração do SGBD e da COLLATION da coluna. Funções como UPPER() ou LOWER() podem ser usadas para forçar a comparação case-insensitive:
WHERE UPPER(Categoria) = 'ELETRÔNICOS'
