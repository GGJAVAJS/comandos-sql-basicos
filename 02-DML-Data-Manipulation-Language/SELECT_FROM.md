# DML: SELECT FROM

O comando `SELECT` é usado para recuperar (consultar ou buscar) dados de uma ou mais tabelas no banco de dados. É o comando SQL mais frequentemente utilizado.

A cláusula `FROM` especifica a(s) tabela(s) da(s) qual(is) os dados serão recuperados.

## Sintaxe Básica

**1. Selecionar Colunas Específicas:**

```sql
SELECT coluna1, coluna2, ...
FROM nome_da_tabela;

2. Selecionar Todas as Colunas:
Use o asterisco (*) para selecionar todas as colunas de uma tabela.

SELECT *
FROM nome_da_tabela;
Atenção: Usar SELECT * em aplicações de produção pode ser menos eficiente e menos resiliente a mudanças na estrutura da tabela. É geralmente recomendado listar explicitamente as colunas necessárias.

Exemplos
Usando nossa tabela Alunos:
-- Supondo que a tabela Alunos já tem dados:
-- INSERT INTO Alunos (ID, Nome, Email, DataNascimento, CursoID) VALUES (1, 'João Silva', 'joao.silva@email.com', '2000-05-15', 101);
-- INSERT INTO Alunos (ID, Nome, Email, CursoID) VALUES (2, 'Maria Santos', 'maria.santos@email.com', 102);
-- INSERT INTO Alunos (ID, Nome, Email, DataNascimento, CursoID) VALUES (3, 'Carlos Pereira', 'carlos.p@email.com', '1999-11-20', 101);

Exemplo 1: Selecionar o Nome e Email de todos os alunos

SELECT Nome, Email
FROM Alunos;

Resultado esperado:
Nome           | Email
---------------|-----------------------
João Silva     | joao.silva@email.com
Maria Santos   | maria.santos@email.com
Carlos Pereira | carlos.p@email.com

Exemplo 2: Selecionar todas as informações de todos os alunos

SELECT *
FROM Alunos;

Resultado esperado (colunas podem variar na ordem dependendo da definição da tabela):

ID | Nome           | Email                  | DataNascimento | CursoID
---|----------------|------------------------|----------------|---------
1  | João Silva     | joao.silva@email.com   | 2000-05-15     | 101
2  | Maria Santos   | maria.santos@email.com | NULL           | 102
3  | Carlos Pereira | carlos.p@email.com     | 1999-11-20     | 101

Expressões e Funções na Lista SELECT

Você pode usar expressões matemáticas, funções (como concatenação de strings, formatação de datas, etc.) e aliases na lista de colunas do SELECT.

Exemplo 3: Selecionar Nome e Ano de Nascimento

(A função para extrair o ano pode variar: YEAR() em MySQL/SQL Server, EXTRACT(YEAR FROM ...) em PostgreSQL/Oracle)
-- Exemplo para PostgreSQL/Oracle

SELECT
    Nome,
    EXTRACT(YEAR FROM DataNascimento) AS AnoNascimento -- "AS AnoNascimento" é um alias
FROM Alunos;

-- Exemplo para MySQL/SQL Server
-- SELECT
--     Nome,
--     YEAR(DataNascimento) AS AnoNascimento
-- FROM Alunos;
Resultado esperado:
Nome           | AnoNascimento
---------------|---------------
João Silva     | 2000
Maria Santos   | NULL
Carlos Pereira | 1999
