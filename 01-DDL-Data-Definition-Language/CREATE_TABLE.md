# DDL: CREATE TABLE

O comando `CREATE TABLE` é usado para criar uma nova tabela em seu banco de dados.
Ao criar uma tabela, você define suas colunas e os tipos de dados que cada coluna pode armazenar.

## Sintaxe Básica

CREATE TABLE Alunos (
    ID INT PRIMARY KEY,         -- Chave primária: identificador único, não nulo
    Nome VARCHAR(100) NOT NULL, -- Nome do aluno, não pode ser nulo
    Email VARCHAR(100) UNIQUE,  -- Email do aluno, deve ser único
    DataNascimento DATE,
    MediaNotas DECIMAL(4,2) DEFAULT 0.0 -- Média com 2 casas decimais, valor padrão 0.0
);
