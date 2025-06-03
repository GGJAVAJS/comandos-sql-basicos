# DML: DELETE FROM

O comando `DELETE FROM` é usado para remover linhas (registros) existentes de uma tabela.

Assim como no comando `UPDATE`, a cláusula `WHERE` é **crucial** para especificar quais linhas devem ser removidas. Se você omitir a cláusula `WHERE`, **TODAS AS LINHAS DA TABELA SERÃO REMOVIDAS!**

## Sintaxe Básica

```sql
DELETE FROM nome_da_tabela
WHERE condicao; -- MUITO IMPORTANTE!

Componentes:

* nome_da_tabela: O nome da tabela da qual as linhas serão removidas.

* WHERE condicao: Especifica as linhas que serão afetadas pela exclusão. Apenas as linhas que satisfazem a condicao serão removidas.

Usando nossa tabela Alunos:

-- Supondo que a tabela Alunos tem estes dados:
-- ID | Nome           | Email                  | DataNascimento | CursoID
-- ---|----------------|------------------------|----------------|---------
-- 1  | João Silva     | joao.novo@email.com    | 2000-05-15     | 105
-- 2  | Maria Santos   | maria.santos@email.com | 2001-02-20     | 103
-- 3  | Carlos Pereira | carlos.p@email.com     | 1999-11-20     | 105
-- 4  | Ana Costa      | ana.costa@email.com    | NULL           | 103

Exemplo 1: Remover o aluno com ID = 3

DELETE FROM Alunos
WHERE ID = 3;

Após a execução, a linha de Carlos Pereira será removida da tabela.

Exemplo 2: Remover todos os alunos do CursoID 103

DELETE FROM Alunos
WHERE CursoID = 103;

Isso removerá Maria Santos e Ana Costa da tabela.

Exemplo 3: O PERIGO DE OMITIR WHERE (NÃO EXECUTE EM DADOS IMPORTANTES SEM TESTAR!)
Se você executar o seguinte comando, TODAS as linhas da tabela Alunos serão removidas:

-- NÃO FAÇA ISSO A MENOS QUE SEJA INTENCIONAL E TESTADO!
-- DELETE FROM Alunos;

Isto deixa a tabela vazia, mas a estrutura da tabela (colunas, tipos de dados) ainda existe.

DELETE vs TRUNCATE TABLE vs DROP TABLE
DELETE FROM nome_da_tabela WHERE condicao;:

Remove linhas específicas que atendem à condição.

É uma operação DML.

Geralmente é logada (pode ser desfeita com ROLLBACK antes do COMMIT).

Pode disparar gatilhos (triggers) definidos na tabela para cada linha removida.

Pode ser mais lento se muitas linhas forem removidas.

DELETE FROM nome_da_tabela; (sem WHERE):

Remove todas as linhas da tabela.

Ainda é uma operação DML, logada, e dispara gatilhos.

TRUNCATE TABLE nome_da_tabela;:

Remove todas as linhas da tabela muito rapidamente.

É uma operação DDL (em muitos SGBDs).

Geralmente não é logada da mesma forma que DELETE (ou tem log mínimo), tornando o ROLLBACK impossível ou diferente.

Normalmente não dispara gatilhos de linha.

Pode resetar contadores de colunas auto-incremento (dependendo do SGBD).

Requer privilégios diferentes do DELETE.

DROP TABLE nome_da_tabela;:

Remove a tabela inteira (estrutura e dados).

É uma operação DDL.

Irreversível sem backups.

Considerações
A Cláusula WHERE é Fundamental: Assim como no UPDATE, sempre use uma cláusula WHERE específica. Teste sua condição WHERE com um SELECT primeiro.

-- Antes de um DELETE, faça um SELECT:
SELECT * FROM Alunos WHERE CursoID = 103;
-- Se o SELECT retornar as linhas corretas, então você pode fazer o DELETE:
-- DELETE FROM Alunos WHERE CursoID = 103;

Integridade Referencial: Se outras tabelas tiverem chaves estrangeiras apontando para as linhas que você está tentando deletar, o SGBD pode impedir a exclusão para manter a integridade. Você pode precisar definir ações de ON DELETE (como CASCADE ou SET NULL) nas chaves estrangeiras ou deletar os registros dependentes primeiro.

Transações: DELETE é um comando DML. Use COMMIT para salvar permanentemente ou ROLLBACK para desfazer antes do COMMIT.
