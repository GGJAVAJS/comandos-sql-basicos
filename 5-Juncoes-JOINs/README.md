# 05 - Junções (JOINs)

Em bancos de dados relacionais, os dados são frequentemente distribuídos em múltiplas tabelas para evitar redundância e melhorar a organização (normalização).
As cláusulas `JOIN` são usadas para combinar linhas de duas ou mais tabelas com base em uma coluna relacionada entre elas.

## Por Que Usar JOINs?

Imagine ter uma tabela `Alunos` e uma tabela `Cursos`. A tabela `Alunos` pode ter uma coluna `CursoID` que se refere ao `ID` de um curso na tabela `Cursos`.
Se você quiser ver o nome do aluno junto com o nome do curso que ele está fazendo, você precisará de um `JOIN` para combinar as informações dessas duas tabelas.

## Tipos de JOINs Abordados (Inicialmente):

*   **`INNER JOIN` (ou apenas `JOIN`)**: Retorna apenas as linhas que têm correspondência em AMBAS as tabelas. Se um aluno estiver em um `CursoID` que não existe na tabela `Cursos`, esse aluno não aparecerá no resultado.
    *   [Ver Detalhes e Exemplos](./INNER_JOIN.md)
*   **`LEFT JOIN` (ou `LEFT OUTER JOIN`)**: Retorna TODAS as linhas da tabela da ESQUERDA (a primeira tabela mencionada) e as linhas correspondentes da tabela da DIREITA. Se não houver correspondência na tabela da direita, colunas dessa tabela aparecerão como `NULL`.
    *   [Ver Detalhes e Exemplos](./LEFT_JOIN.md)
    *   (Também mencionaremos brevemente `RIGHT JOIN` e `FULL OUTER JOIN` como variações)
*   **`SELF JOIN`**: Um tipo especial de junção onde uma tabela é unida a ela mesma. Útil para modelar relações hierárquicas ou comparar linhas dentro da mesma tabela.
    *   [Ver Detalhes e Exemplos](./SELF_JOIN.md)

Outros tipos de `JOIN` incluem `RIGHT JOIN` (ou `RIGHT OUTER JOIN`), `FULL JOIN` (ou `FULL OUTER JOIN`), e `CROSS JOIN` (produto cartesiano), que podem ser explorados posteriormente.

Dominar `JOIN`s é crucial para trabalhar efetivamente com bancos de dados relacionais.
