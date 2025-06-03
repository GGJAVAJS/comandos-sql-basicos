# Cláusula DISTINCT

A palavra-chave `DISTINCT` é usada em uma instrução `SELECT` para retornar apenas valores diferentes (únicos) para a(s) coluna(s) especificada(s).
Se múltiplas colunas forem especificadas após `DISTINCT`, a combinação de valores dessas colunas será considerada para determinar a unicidade.

## Sintaxe Básica

```sql
SELECT DISTINCT coluna1, [coluna2, ...]
FROM nome_da_tabela
[WHERE condicao]
[ORDER BY ...];

Componentes:

coluna1, [coluna2, ...]: As colunas cujos valores únicos você deseja selecionar.

Exemplos
Usando a tabela Pedidos:

CREATE TABLE Pedidos (
    PedidoID INT,
    ClienteID INT,
    Produto VARCHAR(100),
    DataPedido DATE
);

INSERT INTO Pedidos VALUES (101, 1, 'Laptop', '2023-01-10');
INSERT INTO Pedidos VALUES (102, 2, 'Mouse', '2023-01-11');
INSERT INTO Pedidos VALUES (103, 1, 'Teclado', '2023-01-12');
INSERT INTO Pedidos VALUES (104, 3, 'Monitor', '2023-01-10');
INSERT INTO Pedidos VALUES (105, 2, 'Mouse', '2023-01-15'); -- Cliente 2 pediu Mouse novamente
INSERT INTO Pedidos VALUES (106, 1, 'Laptop', '2023-01-18'); -- Cliente 1 pediu Laptop novamente

Exemplo 1: Selecionar todos os ClienteID distintos que fizeram pedidos

SELECT DISTINCT ClienteID
FROM Pedidos
ORDER BY ClienteID;

Resultado:

ClienteID
-----------
1
2
3

Mesmo que os clientes 1 e 2 tenham feito múltiplos pedidos, seus IDs aparecem apenas uma vez.

Exemplo 2: Selecionar todos os Produtos distintos que foram pedidos

SELECT DISTINCT Produto
FROM Pedidos
ORDER BY Produto;

Resultado:

Produto
---------
Laptop
Monitor
Mouse
Teclado

Exemplo 3: Selecionar combinações distintas de ClienteID e Produto
Isto mostrará cada par único de cliente e produto que ele pediu, mesmo que o cliente tenha pedido o mesmo produto várias vezes (o que não acontece no nosso exemplo atual de dados para a mesma combinação, mas se o cliente 1 pedisse 'Laptop' em datas diferentes, essa combinação ainda apareceria apenas uma vez aqui).

SELECT DISTINCT ClienteID, Produto
FROM Pedidos
ORDER BY ClienteID, Produto;

Resultado (baseado nos dados atuais):

ClienteID | Produto
----------|---------
1         | Laptop
1         | Teclado
2         | Mouse
3         | Monitor

Se o Cliente 2 pedisse 'Mouse' novamente em outra DataPedido, a combinação (2, 'Mouse') ainda apareceria apenas uma vez com DISTINCT ClienteID, Produto.

DISTINCT vs GROUP BY
Para obter uma lista de valores únicos de uma única coluna, DISTINCT é geralmente mais simples.
Para tarefas mais complexas que envolvem agregar dados baseados em valores únicos (como contar quantos pedidos cada cliente fez), a cláusula GROUP BY é mais apropriada (ver seção sobre Funções Agregadas).

Por exemplo, para obter a contagem de pedidos por cliente (que não é o mesmo que apenas listar clientes distintos):

SELECT ClienteID, COUNT(*) AS TotalPedidos
FROM Pedidos
GROUP BY ClienteID;

Considerações
Performance: Usar DISTINCT em grandes conjuntos de dados pode impactar a performance, pois o SGBD precisa processar e comparar todas as linhas para identificar as únicas.

Todas as Colunas Selecionadas: DISTINCT se aplica a todas as colunas listadas na cláusula SELECT. Não é possível aplicar DISTINCT a apenas uma coluna se você estiver selecionando múltiplas colunas (a menos que use subconsultas ou funções de janela, que são tópicos mais avançados).
