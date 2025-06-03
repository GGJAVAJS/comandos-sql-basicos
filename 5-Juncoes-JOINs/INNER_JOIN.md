# Junção Interna: INNER JOIN (ou JOIN)

O `INNER JOIN` (ou simplesmente `JOIN`, já que `INNER` é o padrão) seleciona todas as linhas de múltiplas tabelas desde que a condição de junção seja satisfeita. Ele retorna apenas as linhas que têm valores correspondentes em AMBAS as tabelas que estão sendo unidas.

Se uma linha em uma tabela não tiver uma correspondência na outra tabela (com base na condição de junção), essa linha não será incluída no resultado.

## Sintaxe Básica

```sql
SELECT
    tabela1.coluna1,
    tabela1.coluna2,
    tabela2.colunaA,
    ...
FROM
    tabela1
INNER JOIN  -- Ou apenas JOIN
    tabela2 ON tabela1.coluna_chave_comum = tabela2.coluna_chave_comum
[WHERE condicoes_adicionais];

Componentes:

tabela1, tabela2: As tabelas que você deseja unir.

ON tabela1.coluna_chave_comum = tabela2.coluna_chave_comum: A condição de junção. Esta é a parte mais importante, pois define como as linhas das duas tabelas são relacionadas. Geralmente, envolve a chave primária de uma tabela e a chave estrangeira correspondente na outra.

É uma boa prática usar aliases de tabela para tornar a consulta mais curta e legível, especialmente ao referenciar colunas.

Exemplo
Vamos criar duas tabelas: Alunos e Cursos.

CREATE TABLE Cursos (
    CursoID INT PRIMARY KEY,
    NomeCurso VARCHAR(100) NOT NULL,
    Instrutor VARCHAR(100)
);

INSERT INTO Cursos VALUES (101, 'Introdução ao SQL', 'Prof. Silva');
INSERT INTO Cursos VALUES (102, 'Python para Dados', 'Prof. Costa');
INSERT INTO Cursos VALUES (103, 'Web Design Básico', 'Prof. Lima');
INSERT INTO Cursos VALUES (104, 'Marketing Digital', 'Prof. Alves'); -- Curso sem alunos ainda

CREATE TABLE Alunos (
    AlunoID INT PRIMARY KEY,
    NomeAluno VARCHAR(100) NOT NULL,
    Email VARCHAR(100),
    CursoID_FK INT -- Chave estrangeira referenciando Cursos.CursoID
    -- CONSTRAINT FK_Aluno_Curso FOREIGN KEY (CursoID_FK) REFERENCES Cursos(CursoID) -- Opcional para o exemplo, mas boa prática
);

INSERT INTO Alunos VALUES (1, 'João', 'joao@email.com', 101);
INSERT INTO Alunos VALUES (2, 'Maria', 'maria@email.com', 102);
INSERT INTO Alunos VALUES (3, 'Carlos', 'carlos@email.com', 101);
INSERT INTO Alunos VALUES (4, 'Ana', 'ana@email.com', 103);
INSERT INTO Alunos VALUES (5, 'Pedro', 'pedro@email.com', NULL); -- Aluno sem curso definido
INSERT INTO Alunos VALUES (6, 'Sofia', 'sofia@email.com', 999); -- Aluno com CursoID_FK inválido (não existe em Cursos)

Exemplo 1: Selecionar o nome do aluno e o nome do curso que ele está fazendo

SELECT
    a.NomeAluno,
    c.NomeCurso
FROM
    Alunos AS a -- Alias 'a' para Alunos
INNER JOIN
    Cursos AS c ON a.CursoID_FK = c.CursoID; -- Condição de junção

Resultado:

NomeAluno | NomeCurso
----------|--------------------
João      | Introdução ao SQL
Maria     | Python para Dados
Carlos    | Introdução ao SQL
Ana       | Web Design Básico

Observações sobre o resultado:

Pedro (CursoID_FK = NULL) não aparece porque NULL não é igual a nenhum CursoID.

Sofia (CursoID_FK = 999) não aparece porque não há CursoID = 999 na tabela Cursos.

O curso Marketing Digital (CursoID = 104) não aparece porque nenhum aluno está matriculado nele (não há correspondência em Alunos para esse CursoID).

Exemplo 2: INNER JOIN com uma cláusula WHERE adicional
Selecionar alunos do curso 'Introdução ao SQL'.

SELECT
    a.NomeAluno,
    a.Email,
    c.NomeCurso,
    c.Instrutor
FROM
    Alunos a
INNER JOIN
    Cursos c ON a.CursoID_FK = c.CursoID
WHERE
    c.NomeCurso = 'Introdução ao SQL';

Resultado:

NomeAluno | Email            | NomeCurso         | Instrutor
----------|------------------|-------------------|------------
João      | joao@email.com   | Introdução ao SQL | Prof. Silva
Carlos    | carlos@email.com | Introdução ao SQL | Prof. Silva

Considerações
Chaves: A condição ON geralmente usa uma chave primária de uma tabela e uma chave estrangeira da outra.

Múltiplas Condições de Junção: Você pode ter múltiplas condições na cláusula ON usando AND.

Múltiplas Tabelas: Você pode unir mais de duas tabelas encadeando cláusulas JOIN.

-- SELECT ...
-- FROM TabelaA
-- INNER JOIN TabelaB ON TabelaA.id = TabelaB.a_id
-- INNER JOIN TabelaC ON TabelaB.id = TabelaC.b_id;

Performance: Certifique-se de que as colunas usadas na condição ON estejam indexadas para melhor performance, especialmente em tabelas grandes.
