# DML: INSERT INTO

O comando `INSERT INTO` é usado para adicionar novas linhas (registros) a uma tabela existente.

## Sintaxe Básica

Existem duas formas principais de usar o `INSERT INTO`:

**1. Especificando os Nomes das Colunas e os Valores:**
   Esta é a forma recomendada, pois é mais explícita e menos propensa a erros se a ordem das colunas na tabela mudar.

```sql
INSERT INTO nome_da_tabela (coluna1, coluna2, coluna3, ...)
VALUES (valor1, valor2, valor3, ...);

***2. Adicionando Valores para Todas as Colunas (na ordem da tabela):
Se você está fornecendo valores para todas as colunas da tabela, na mesma ordem em que foram definidas, você pode omitir os nomes das colunas. Não recomendado para scripts de longa duração devido à fragilidade à mudança de estrutura da tabela.

INSERT INTO nome_da_tabela
VALUES (valor1, valor2, valor3, ...); -- valor1 corresponde à primeira coluna da tabela, valor2 à segunda, etc.

CREATE TABLE Alunos (
    ID INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    DataNascimento DATE,
    CursoID INT
);

Exemplo 1: Inserindo um novo aluno especificando colunas
INSERT INTO Alunos (ID, Nome, Email, DataNascimento, CursoID)
VALUES (1, 'João Silva', 'joao.silva@email.com', '2000-05-15', 101);

Exemplo 2: Inserindo outro aluno, omitindo uma coluna opcional (DataNascimento)
Se uma coluna permite valores NULL (ou tem um valor DEFAULT), você pode omiti-la da lista de colunas ou fornecer NULL explicitamente.
INSERT INTO Alunos (ID, Nome, Email, CursoID)
VALUES (2, 'Maria Santos', 'maria.santos@email.com', 102);

-- DataNascimento será NULL para este aluno
Exemplo 3: Inserindo um aluno fornecendo NULL explicitamente
INSERT INTO Alunos (ID, Nome, Email, DataNascimento, CursoID)
VALUES (3, 'Carlos Pereira', 'carlos.p@email.com', NULL, 101);

Exemplo 4: Inserindo múltiplos registros (sintaxe pode variar entre SGBDs)
Alguns SGBDs permitem inserir múltiplas linhas com um único comando INSERT.

-- Sintaxe comum (ex: MySQL, PostgreSQL, SQL Server)
INSERT INTO Alunos (ID, Nome, Email, CursoID) VALUES
(4, 'Ana Costa', 'ana.costa@email.com', 103),
(5, 'Pedro Almeida', 'pedro.a@email.com', 102),
(6, 'Sofia Lima', 'sofia.lima@email.com', 103);

-- Sintaxe Oracle (usando INSERT ALL ou múltiplos INSERTs)
-- INSERT ALL
--  INTO Alunos (ID, Nome, Email, CursoID) VALUES (4, 'Ana Costa', 'ana.costa@email.com', 103)
--  INTO Alunos (ID, Nome, Email, CursoID) VALUES (5, 'Pedro Almeida', 'pedro.a@email.com', 102)
--  INTO Alunos (ID, Nome, Email, CursoID) VALUES (6, 'Sofia Lima', 'sofia.lima@email.com', 103)
-- SELECT 1 FROM DUAL; -- (Em Oracle, o SELECT 1 FROM DUAL é necessário para o INSERT ALL)

-- Ou simplesmente múltiplos comandos INSERT:
-- INSERT INTO Alunos (ID, Nome, Email, CursoID) VALUES (4, 'Ana Costa', 'ana.costa@email.com', 103);
-- INSERT INTO Alunos (ID, Nome, Email, CursoID) VALUES (5, 'Pedro Almeida', 'pedro.a@email.com', 102);
-- INSERT INTO Alunos (ID, Nome, Email, CursoID) VALUES (6, 'Sofia Lima', 'sofia.lima@email.com', 103);

Considerações

•	Tipos de Dados: Os valores fornecidos devem corresponder aos tipos de dados definidos para cada coluna. Por exemplo, não insira texto em uma coluna INT.
•	Restrições: O INSERT deve respeitar todas as restrições da tabela (ex: NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY). Se uma restrição for violada, o SGBD gerará um erro e a inserção falhará.
•	Valores Padrão (DEFAULT): Se uma coluna tem um valor padrão definido e você não a inclui na lista de colunas do INSERT (ou insere DEFAULT), o valor padrão será usado.
•	Colunas Auto-Incremento/Identity/Sequence: Para colunas que geram valores automaticamente (como chaves primárias), você geralmente não as inclui na lista de colunas do INSERT, ou o SGBD tem uma forma específica de lidar com isso.
