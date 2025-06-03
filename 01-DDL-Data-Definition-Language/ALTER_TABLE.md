# DDL: ALTER TABLE

O comando `ALTER TABLE` é usado para modificar a estrutura de uma tabela existente. Isso pode incluir:

*   Adicionar uma nova coluna.
*   Remover uma coluna existente.
*   Modificar o tipo de dado ou as restrições de uma coluna existente.
*   Adicionar ou remover restrições da tabela (como `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`).

## Sintaxe Básica e Exemplos

A sintaxe do `ALTER TABLE` varia dependendo da ação que você deseja realizar.

### 1. Adicionar uma Coluna

ALTER TABLE Alunos
ADD Telefone VARCHAR(20); -- Adiciona a coluna Telefone, que pode ser nula
