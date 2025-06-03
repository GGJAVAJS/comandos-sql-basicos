# Junção à Esquerda: LEFT JOIN (ou LEFT OUTER JOIN)

O `LEFT JOIN` (ou `LEFT OUTER JOIN`) retorna **todas as linhas da tabela da esquerda** (a primeira tabela listada na cláusula `JOIN`) e as linhas correspondentes da tabela da direita.
Se não houver correspondência na tabela da direita para uma linha da tabela da esquerda, as colunas da tabela da direita terão valores `NULL` no resultado para essa linha.

## Sintaxe Básica

```sql
SELECT
    tabela_esquerda.coluna1,
    tabela_esquerda.coluna2,
    tabela_direita.colunaA,
    ...
FROM
    tabela_esquerda -- Todas as linhas desta tabela serão incluídas
LEFT JOIN -- Ou LEFT OUTER JOIN
    tabela_direita ON tabela_esquerda.coluna_chave_comum = tabela_direita.coluna_chave_comum
[WHERE condicoes_adicionais];

Componentes:

tabela_esquerda: A tabela cujas linhas serão todas incluídas.

tabela_direita: A tabela que fornecerá dados correspondentes (ou NULLs se não houver correspondência).

ON ...: A condição de junção.

Exemplo
Usando as mesmas tabelas Alunos e Cursos da seção INNER JOIN:

-- Relembrando os dados relevantes:
-- Cursos: (101, SQL), (102, Python), (103, Web), (104, Marketing)
-- Alunos: (1, João, 101), (2, Maria, 102), (3, Carlos, 101), (4, Ana, 103),
--         (5, Pedro, NULL), (6, Sofia, 999)

Exemplo 1: Listar todos os alunos e os cursos que estão fazendo (se houver)

SELECT
    a.NomeAluno,
    a.CursoID_FK,
    c.NomeCurso,
    c.Instrutor
FROM
    Alunos a -- Tabela da ESQUERDA
LEFT JOIN
    Cursos c ON a.CursoID_FK = c.CursoID; -- Tabela da DIREITA

Resultado:

NomeAluno | CursoID_FK | NomeCurso         | Instrutor
----------|------------|-------------------|------------
João      | 101        | Introdução ao SQL | Prof. Silva
Maria     | 102        | Python para Dados | Prof. Costa
Carlos    | 101        | Introdução ao SQL | Prof. Silva
Ana       | 103        | Web Design Básico | Prof. Lima
Pedro     | NULL       | NULL              | NULL  -- Pedro tem CursoID_FK NULL, então não há match em Cursos
Sofia     | 999        | NULL              | NULL  -- Sofia tem CursoID_FK 999, que não existe em Cursos

Observações sobre o resultado:

Todos os alunos (João, Maria, Carlos, Ana, Pedro, Sofia) da tabela Alunos (tabela da esquerda) estão presentes.

Pedro e Sofia têm NomeCurso e Instrutor como NULL porque seus CursoID_FK não encontraram correspondência na tabela Cursos.

Exemplo 2: Listar todos os cursos e os alunos matriculados (invertendo a ordem das tabelas no LEFT JOIN)
Se quisermos ver todos os cursos, mesmo aqueles sem alunos, Cursos deve ser a tabela da esquerda.

SELECT
    c.NomeCurso,
    c.Instrutor,
    a.NomeAluno
FROM
    Cursos c -- Tabela da ESQUERDA
LEFT JOIN
    Alunos a ON c.CursoID = a.CursoID_FK
ORDER BY c.NomeCurso, a.NomeAluno;

Resultado:

NomeCurso         | Instrutor   | NomeAluno
-------------------|-------------|----------
Introdução ao SQL | Prof. Silva | Carlos
Introdução ao SQL | Prof. Silva | João
Marketing Digital | Prof. Alves | NULL  -- Curso sem alunos
Python para Dados | Prof. Costa | Maria
Web Design Básico | Prof. Lima  | Ana

Observação: O curso Marketing Digital aparece com NomeAluno como NULL porque nenhum aluno está matriculado nele.

Outros Tipos de OUTER JOINs
RIGHT JOIN (ou RIGHT OUTER JOIN): Funciona de forma oposta ao LEFT JOIN. Retorna todas as linhas da tabela da DIREITA e as correspondentes da esquerda (ou NULLs).

Um Cursos c RIGHT JOIN Alunos a ON c.CursoID = a.CursoID_FK produziria o mesmo resultado do primeiro exemplo de LEFT JOIN (Alunos a LEFT JOIN Cursos c ...). Muitos preferem reescrever RIGHT JOINs como LEFT JOINs para consistência, trocando a ordem das tabelas.

FULL JOIN (ou FULL OUTER JOIN): Retorna todas as linhas de AMBAS as tabelas. Se houver uma correspondência entre as tabelas, as colunas de ambas as tabelas são preenchidas. Se não houver correspondência para uma linha de uma das tabelas, as colunas da outra tabela terão NULL.

SELECT a.NomeAluno, c.NomeCurso
FROM Alunos a
FULL OUTER JOIN Cursos c ON a.CursoID_FK = c.CursoID;

Isso mostraria todos os alunos (incluindo Pedro e Sofia com cursos NULL) E todos os cursos (incluindo Marketing Digital com aluno NULL).

Quando Usar LEFT JOIN?
Quando você quer todas as linhas de uma tabela principal (a da esquerda) e quer complementar essas informações com dados de outra tabela, mesmo que nem todas as linhas da tabela principal tenham correspondência.

Para encontrar linhas em uma tabela que NÃO têm correspondência em outra (usando LEFT JOIN ... WHERE tabela_direita.coluna_chave IS NULL).
Exemplo: Alunos que não estão matriculados em nenhum curso válido

SELECT a.NomeAluno, a.CursoID_FK
FROM Alunos a
LEFT JOIN Cursos c ON a.CursoID_FK = c.CursoID
WHERE c.CursoID IS NULL; -- Pega apenas os alunos para os quais não houve match em Cursos

Resultado:

NomeAluno | CursoID_FK
----------|------------
Pedro     | NULL
Sofia     | 999
