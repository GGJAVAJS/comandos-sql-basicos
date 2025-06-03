# DDL: ALTER TABLE

O comando `ALTER TABLE` é usado para modificar a estrutura de uma tabela existente. Isso pode incluir:

*   Adicionar uma nova coluna.
*   Remover uma coluna existente.
*   Modificar o tipo de dado ou as restrições de uma coluna existente.
*   Adicionar ou remover restrições da tabela (como `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`).

## Sintaxe Básica e Exemplos

A sintaxe do `ALTER TABLE` varia dependendo da ação que você deseja realizar.

### 1. Adicionar uma Coluna
```sql
ALTER TABLE Alunos
ADD Telefone VARCHAR(20); -- Adiciona a coluna Telefone, que pode ser nula;


### 2. Remover (DROP) uma Coluna
```sql
ALTER TABLE nome_da_tabela
DROP COLUMN nome_da_coluna;

-- Exemplo: Remover a coluna MediaNotas da tabela Alunos.

ALTER TABLE Alunos
DROP COLUMN MediaNotas;

Cuidado: Remover uma coluna apaga todos os dados contidos nela.

### 3. Modificar uma Coluna (Tipo de Dado ou Restrições)

A sintaxe para modificar uma coluna pode variar significativamente entre SGBDs.
Sintaxe Comum (pode ser MODIFY ou ALTER COLUMN dependendo do SGBD):

-- Exemplo para Oracle, MySQL (com MODIFY)
ALTER TABLE nome_da_tabela
MODIFY nome_da_coluna novo_tipo_de_dado [novas_restrições];

-- Exemplo para PostgreSQL, SQL Server (com ALTER COLUMN)
ALTER TABLE nome_da_tabela
ALTER COLUMN nome_da_coluna TYPE novo_tipo_de_dado; -- Para tipo (PostgreSQL)

ALTER TABLE nome_da_tabela
ALTER COLUMN nome_da_coluna novo_tipo_de_dado [NULL | NOT NULL];
-- Para tipo e nulidade (SQL Server)
Exemplo: Alterar o tipo da coluna Telefone para VARCHAR(15) na tabela Alunos e torná-la NOT NULL.
-- Exemplo genérico (verificar sintaxe específica do SGBD)
-- Para MySQL:
ALTER TABLE Alunos
MODIFY Telefone VARCHAR(15) NOT NULL;

-- Para PostgreSQL:
ALTER TABLE Alunos
ALTER COLUMN Telefone TYPE VARCHAR(15),
ALTER COLUMN Telefone SET NOT NULL;

-- Para Oracle:
ALTER TABLE Alunos
MODIFY Telefone VARCHAR2(15) NOT NULL;

-- Para SQL Server:
ALTER TABLE Alunos
ALTER COLUMN Telefone VARCHAR(15) NOT NULL;
content_copydownload

### 4. Adicionar uma Restrição (ex: FOREIGN KEY)

ALTER TABLE nome_da_tabela
ADD CONSTRAINT nome_da_restricao TIPO_DA_RESTRIÇÃO (coluna(s)) [referências];

Exemplo: Adicionar uma chave estrangeira. Suponha que temos uma tabela Cursos com CursoID como chave primária.

-- Primeiro, adicionamos a coluna CursoID na tabela Alunos, se não existir
ALTER TABLE Alunos
ADD CursoID INT;
-- Agora, adicionamos a restrição de chave estrangeira
ALTER TABLE Alunos
ADD CONSTRAINT FK_Aluno_Curso
FOREIGN KEY (CursoID) REFERENCES Cursos(CursoID):

### 5. Remover uma Restrição

ALTER TABLE nome_da_tabela
DROP CONSTRAINT nome_da_restricao;
Exemplo: Remover a restrição FK_Aluno_Curso.
ALTER TABLE Alunos
DROP CONSTRAINT FK_Aluno_Curso;
(O nome da restrição pode precisar ser descoberto consultando o dicionário de dados do SGBD se você não o souber).

Considerações:

*	Impacto: Alterar tabelas, especialmente em produção, pode ser uma operação demorada e pode bloquear o acesso à tabela enquanto a alteração ocorre.
*	Perda de Dados: Modificar tipos de dados ou remover colunas pode levar à perda de dados se não for feito com cuidado.
*	Sintaxe Específica do SGBD: Sempre consulte a documentação do seu SGBD específico para a sintaxe correta de ALTER TABLE.


