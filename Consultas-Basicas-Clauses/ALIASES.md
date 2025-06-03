# Aliases (AS)

Aliases (apelidos) são usados em SQL para dar a uma tabela ou a uma coluna um nome temporário.
Isso é útil para:

*   Tornar os nomes das colunas mais legíveis nos resultados.
*   Encurtar nomes longos de tabelas ou colunas.
*   Evitar ambiguidades quando se junta múltiplas tabelas que podem ter colunas com o mesmo nome.
*   Quando se usa funções agregadas ou outras expressões na lista `SELECT`.

A palavra-chave `AS` é opcional na maioria dos SGBDs para definir aliases, mas usá-la torna o código mais claro.

## Sintaxe Básica

**Alias de Coluna:**

```sql
SELECT nome_da_coluna AS nome_do_alias
FROM nome_da_tabela;

-- Ou sem AS (menos comum para colunas, mas funciona em muitos SGBDs)
SELECT nome_da_coluna nome_do_alias
FROM nome_da_tabela;

Alias de Tabela:

SELECT alias_tabela.coluna
FROM nome_da_tabela AS alias_tabela;

-- Ou sem AS (muito comum para tabelas)
SELECT t.coluna
FROM nome_da_tabela t;

Exemplos
Usando a tabela Funcionarios:

CREATE TABLE Funcionarios (
    FuncionarioID INT PRIMARY KEY,
    NomeCompleto VARCHAR(150),
    DataContratacao DATE,
    SalarioBase DECIMAL(10,2),
    DepartamentoID INT
);

INSERT INTO Funcionarios VALUES (1, 'Ana Beatriz Silva', '2020-03-15', 5500.00, 10);
INSERT INTO Funcionarios VALUES (2, 'Carlos José Pereira', '2019-07-01', 7200.00, 20);
INSERT INTO Funcionarios VALUES (3, 'Fernanda Costa Lima', '2021-01-20', 4800.00, 10);

Exemplo 1: Alias de Coluna para Legibilidade

SELECT
    FuncionarioID AS ID,
    NomeCompleto AS Nome_Funcionario,
    DataContratacao AS Data_Admissao,
    SalarioBase AS Salario
FROM Funcionarios;

Resultado:

ID | Nome_Funcionario     | Data_Admissao | Salario
---|----------------------|---------------|--------
1  | Ana Beatriz Silva    | 2020-03-15    | 5500.00
2  | Carlos José Pereira  | 2019-07-01    | 7200.00
3  | Fernanda Costa Lima  | 2021-01-20    | 4800.00

Exemplo 2: Alias de Coluna com Expressão
Suponha que queremos calcular um bônus de 10% sobre o SalarioBase.

SELECT
    NomeCompleto,
    SalarioBase,
    (SalarioBase * 0.10) AS Bonus
FROM Funcionarios;

Resultado:

NomeCompleto         | SalarioBase | Bonus
---------------------|-------------|-------
Ana Beatriz Silva    | 5500.00     | 550.00
Carlos José Pereira  | 7200.00     | 720.00
Fernanda Costa Lima  | 4800.00     | 480.00

Exemplo 3: Alias de Tabela (útil principalmente com JOINs)
Embora o benefício seja mais aparente com JOINs, podemos mostrar a sintaxe.

SELECT
    f.NomeCompleto, -- Usando o alias da tabela 'f'
    f.SalarioBase
FROM Funcionarios AS f -- Definindo 'f' como alias para Funcionarios
WHERE f.DepartamentoID = 10;

Resultado:

NomeCompleto         | SalarioBase
---------------------|------------
Ana Beatriz Silva    | 5500.00
Fernanda Costa Lima  | 4800.00

Sem o AS (também comum):

SELECT
    f.NomeCompleto,
    f.SalarioBase
FROM Funcionarios f -- 'f' é o alias
WHERE f.DepartamentoID = 10;

Considerações
Escopo: Aliases definidos na cláusula SELECT geralmente não podem ser usados na cláusula WHERE da mesma consulta (alguns SGBDs permitem, mas não é padrão ANSI SQL). Eles podem ser usados na cláusula ORDER BY na maioria dos SGBDs.

-- ISTO GERALMENTE NÃO FUNCIONA (usar alias 'Bonus' no WHERE):
-- SELECT NomeCompleto, (SalarioBase * 0.10) AS Bonus
-- FROM Funcionarios
-- WHERE Bonus > 500;

-- Forma correta (repetir a expressão ou usar subconsulta):
SELECT NomeCompleto, (SalarioBase * 0.10) AS Bonus
FROM Funcionarios
WHERE (SalarioBase * 0.10) > 500;

-- Usar alias no ORDER BY (geralmente funciona):
SELECT NomeCompleto, (SalarioBase * 0.10) AS Bonus
FROM Funcionarios
ORDER BY Bonus DESC;

Aspas em Aliases: Se o seu alias contiver espaços ou caracteres especiais, ou for uma palavra reservada do SQL, você precisará colocá-lo entre aspas duplas (padrão SQL) ou colchetes [] (SQL Server) ou crases ` (MySQL).

SELECT NomeCompleto AS "Nome do Funcionário" FROM Funcionarios;

Clareza: Use aliases de forma consistente para melhorar a legibilidade do seu código SQL, especialmente em consultas complexas.
