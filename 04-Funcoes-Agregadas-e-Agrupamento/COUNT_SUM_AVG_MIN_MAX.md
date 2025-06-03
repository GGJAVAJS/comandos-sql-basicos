# Funções Agregadas Comuns: COUNT, SUM, AVG, MIN, MAX

Funções agregadas operam em um conjunto de linhas e retornam um único valor de resumo.

## Funções Principais

| Função        | Descrição                                                                  | Considera `NULL`? |
| :------------ | :------------------------------------------------------------------------- | :---------------- |
| `COUNT(coluna)` | Conta o número de linhas onde `coluna` não é `NULL`.                       | Ignora `NULL`s    |
| `COUNT(*)`    | Conta o número total de linhas no grupo (incluindo `NULL`s em colunas).     | Considera todas   |
| `COUNT(DISTINCT coluna)` | Conta o número de valores distintos e não `NULL`s em `coluna`. | Ignora `NULL`s    |
| `SUM(coluna)`   | Soma todos os valores não `NULL`s de uma coluna numérica.                  | Ignora `NULL`s    |
| `AVG(coluna)`   | Calcula a média de todos os valores não `NULL`s de uma coluna numérica.    | Ignora `NULL`s    |
| `MIN(coluna)`   | Encontra o valor mínimo não `NULL` em uma coluna.                          | Ignora `NULL`s    |
| `MAX(coluna)`   | Encontra o valor máximo não `NULL` em uma coluna.                          | Ignora `NULL`s    |

## Sintaxe Básica (Sem `GROUP BY`)

Quando usadas sem `GROUP BY`, as funções agregadas operam em todas as linhas retornadas pela cláusula `WHERE` (ou em todas as linhas da tabela se não houver `WHERE`).

```sql
SELECT
    FUNCAO_AGREGADA(coluna_ou_*) AS nome_alias
FROM nome_da_tabela
[WHERE condicao];

Exemplos

Usando a tabela Vendas:

CREATE TABLE Vendas (
    IDVenda INT PRIMARY KEY,
    Produto VARCHAR(100),
    Quantidade INT,
    ValorUnitario DECIMAL(10,2),
    DataVenda DATE,
    Regiao VARCHAR(50)
);

INSERT INTO Vendas VALUES (1, 'Laptop', 2, 3200.00, '2023-01-05', 'Sudeste');
INSERT INTO Vendas VALUES (2, 'Mouse', 10, 140.00, '2023-01-08', 'Sudeste');
INSERT INTO Vendas VALUES (3, 'Teclado', 5, 280.00, '2023-01-08', 'Nordeste');
INSERT INTO Vendas VALUES (4, 'Monitor', 3, 1100.00, '2023-01-12', 'Sul');
INSERT INTO Vendas VALUES (5, 'Laptop', 1, 3300.00, '2023-01-15', 'Nordeste');
INSERT INTO Vendas VALUES (6, 'Webcam', 0, 200.00, '2023-01-18', NULL); -- Quantidade 0, Região NULL

Exemplo 1: Contar o número total de vendas registradas

SELECT COUNT(*) AS TotalDeVendas
FROM Vendas;

Resultado: 6

Exemplo 2: Contar quantas regiões distintas foram registradas (ignorando NULL)

SELECT COUNT(DISTINCT Regiao) AS TotalRegioesDistintas
FROM Vendas;

Resultado: 3 (Sudeste, Nordeste, Sul. O NULL é ignorado)

Exemplo 3: Calcular a soma total da quantidade de produtos vendidos (onde quantidade > 0)

SELECT SUM(Quantidade) AS TotalProdutosVendidos
FROM Vendas
WHERE Quantidade > 0;

Resultado: 2 + 10 + 5 + 3 + 1 = 21

Exemplo 4: Calcular o valor médio unitário dos produtos vendidos

SELECT AVG(ValorUnitario) AS MediaValorUnitario
FROM Vendas;

Resultado: (3200 + 140 + 280 + 1100 + 3300 + 200) / 6 = 1370.00 (aproximadamente)

Exemplo 5: Encontrar o menor e o maior valor unitário de um produto vendido

SELECT
    MIN(ValorUnitario) AS MenorValor,
    MAX(ValorUnitario) AS MaiorValor
FROM Vendas;

Resultado:

MenorValor | MaiorValor
-----------|-----------
140.00     | 3300.00

Exemplo 6: Contar vendas com Regiao preenchida
COUNT(Regiao) só conta as linhas onde Regiao não é NULL.

SELECT COUNT(Regiao) AS VendasComRegiao
FROM Vendas;

Resultado: 5

Considerações
Ignorando NULLs: A maioria das funções agregadas (exceto COUNT(*)) ignora valores NULL em seus cálculos. Isso é importante de lembrar, especialmente para AVG e COUNT(coluna).

Tipos de Dados: SUM e AVG são tipicamente usados com colunas numéricas. MIN e MAX podem ser usados com tipos numéricos, de data e de string. COUNT pode ser usado com qualquer tipo de dado.

Uso com GROUP BY: O verdadeiro poder das funções agregadas é revelado quando usadas com a cláusula GROUP BY para calcular métricas para subgrupos de dados, o que será visto na próxima seção.
