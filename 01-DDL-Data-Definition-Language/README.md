# 01 - DDL (Data Definition Language)

A DDL, ou Linguagem de Definição de Dados, é um subconjunto de comandos SQL usado para definir, modificar e remover estruturas de banco de dados, como tabelas, índices e views.
Os comandos DDL não manipulam os dados em si, mas sim os "contêineres" onde os dados são armazenados e organizados.

## Comandos DDL Principais Abordados Nesta Seção:

*   **`CREATE TABLE`**: Usado para criar novas tabelas no banco de dados.
    *   [Ver Detalhes e Exemplos](./CREATE_TABLE.md)
*   **`ALTER TABLE`**: Usado para modificar a estrutura de uma tabela existente (adicionar, remover ou modificar colunas, adicionar ou remover restrições).
    *   [Ver Detalhes e Exemplos](./ALTER_TABLE.md)
*   **`DROP TABLE`**: Usado para remover completamente uma tabela e todos os seus dados do banco de dados.
    *   [Ver Detalhes e Exemplos](./DROP_TABLE.md)

Outros comandos DDL importantes (não detalhados aqui inicialmente, mas para conhecimento) incluem:
*   `CREATE INDEX`: Cria um índice para melhorar a performance de consultas.
*   `DROP INDEX`: Remove um índice.
*   `CREATE VIEW`: Cria uma tabela virtual baseada no resultado de uma consulta SQL.
*   `DROP VIEW`: Remove uma view.
*   `TRUNCATE TABLE`: Remove todos os dados de uma tabela rapidamente (geralmente mais rápido que `DELETE` sem `WHERE`, mas com diferenças importantes na forma como é logado e se dispara gatilhos).

**Importante:** Comandos DDL geralmente são auto-commit, o que significa que suas alterações são salvas permanentemente no banco de dados assim que são executados e não podem ser desfeitos (rollback) da mesma forma que comandos DML. Tenha muito cuidado ao usar comandos DDL, especialmente `DROP TABLE` e `ALTER TABLE` em ambientes de produção.

