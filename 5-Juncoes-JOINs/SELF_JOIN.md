# Junção Consigo Mesmo: SELF JOIN

Um `SELF JOIN` é um tipo de junção regular (`INNER JOIN`, `LEFT JOIN`, etc.), mas onde uma tabela é unida a ela mesma.
Isso é útil quando você tem dados hierárquicos ou relações dentro da mesma tabela. Para realizar um `SELF JOIN`, você deve usar aliases de tabela para tratar a mesma tabela como se fossem duas tabelas diferentes.

## Cenários Comuns para `SELF JOIN`

*   **Hierarquias de Funcionários:** Encontrar o nome de um funcionário e o nome de seu gerente, onde ambos são funcionários na mesma tabela, mas relacionados por uma coluna como `GerenteID`.
*   **Comparações Dentro da Mesma Tabela:** Encontrar pares de clientes que moram na mesma cidade, ou produtos que têm o mesmo fornecedor.

## Sintaxe Básica

```sql
SELECT
    alias1.colunaA,
    alias1.colunaB,
    alias2.colunaA AS ColunaA_Relacionada,
    alias2.colunaC AS ColunaC_Relacionada
FROM
    nome_da_tabela AS alias1
JOIN -- Pode ser INNER, LEFT, etc.
    nome_da_tabela AS alias2 ON alias1.coluna_chave = alias2.coluna_referencia
[WHERE condicoes_adicionais];

Chave: O uso de alias1 e alias2 é essencial para o SGBD entender que você está tratando a nome_da_tabela como duas instâncias separadas para o propósito da junção.

Exemplo: Hierarquia de Funcionários
Vamos criar uma tabela Funcionarios onde cada funcionário pode ter um GerenteID que se refere ao FuncionarioID de outro funcionário.

CREATE TABLE Funcionarios (
    FuncionarioID INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Cargo VARCHAR(50),
    GerenteID INT NULL, -- Pode ser NULL para o CEO ou quem não tem gerente direto na tabela
    FOREIGN KEY (GerenteID) REFERENCES Funcionarios(FuncionarioID) -- Auto-referência
);

INSERT INTO Funcionarios VALUES (1, 'Alice Wonderland', 'CEO', NULL);
INSERT INTO Funcionarios VALUES (2, 'Bob The Builder', 'Engenheiro Chefe', 1);
INSERT INTO Funcionarios VALUES (3, 'Charlie Brown', 'Engenheiro Sênior', 2);
INSERT INTO Funcionarios VALUES (4, 'Diana Prince', 'Engenheira Jr', 3);
INSERT INTO Funcionarios VALUES (5, 'Edward Scissorhands', 'Designer Chefe', 1);
INSERT INTO Funcionarios VALUES (6, 'Fiona Apple', 'Designer Sênior', 5);

Objetivo: Listar cada funcionário e o nome de seu gerente.

SELECT
    emp.Nome AS NomeFuncionario,
    emp.Cargo AS CargoFuncionario,
    ger.Nome AS NomeGerente,
    ger.Cargo AS CargoGerente
FROM
    Funcionarios AS emp -- 'emp' representa a instância do funcionário
LEFT JOIN -- Usamos LEFT JOIN para incluir funcionários sem gerente (ex: CEO)
    Funcionarios AS ger ON emp.GerenteID = ger.FuncionarioID; -- 'ger' representa a instância do gerente

Resultado:

NomeFuncionario      | CargoFuncionario   | NomeGerente          | CargoGerente
---------------------|--------------------|----------------------|-----------------
Alice Wonderland     | CEO                | NULL                 | NULL
Bob The Builder      | Engenheiro Chefe   | Alice Wonderland     | CEO
Charlie Brown        | Engenheiro Sênior  | Bob The Builder      | Engenheiro Chefe
Diana Prince         | Engenheira Jr      | Charlie Brown        | Engenheiro Sênior
Edward Scissorhands  | Designer Chefe     | Alice Wonderland     | CEO
Fiona Apple          | Designer Sênior    | Edward Scissorhands  | Designer Chefe

Explicação:

emp é o alias para a "visão" do funcionário.

ger é o alias para a "visão" do gerente (que também é um funcionário).

Juntamos emp.GerenteID com ger.FuncionarioID para encontrar a linha do gerente.

LEFT JOIN garante que Alice (que tem GerenteID NULL) ainda apareça na lista, com NomeGerente e CargoGerente como NULL. Se usássemos INNER JOIN, Alice não apareceria.

Exemplo 2: Encontrar funcionários que são gerentes (usando INNER JOIN e DISTINCT)

SELECT DISTINCT
    ger.FuncionarioID,
    ger.Nome AS NomeGerente,
    ger.Cargo AS CargoGerente
FROM
    Funcionarios emp
INNER JOIN
    Funcionarios ger ON emp.GerenteID = ger.FuncionarioID
ORDER BY ger.FuncionarioID;

Resultado:

FuncionarioID | NomeGerente         | CargoGerente
--------------|---------------------|-----------------
1             | Alice Wonderland    | CEO
2             | Bob The Builder     | Engenheiro Chefe
3             | Charlie Brown       | Engenheiro Sênior
5             | Edward Scissorhands | Designer Chefe

Considerações
Aliases são Obrigatórios: Você deve usar aliases de tabela diferentes para cada "instância" da tabela no SELF JOIN.

Condição de Junção Cuidadosa: A condição ON é crucial e define a relação entre as "duas" instâncias da tabela.

Tipo de JOIN: Escolha o tipo de JOIN (INNER, LEFT, etc.) apropriado para o que você quer mostrar (ex: incluir linhas sem correspondência na "outra" instância ou não).
