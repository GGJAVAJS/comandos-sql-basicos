# DML: UPDATE SET

O comando `UPDATE` é usado para modificar dados existentes em uma ou mais linhas de uma tabela.

A cláusula `SET` especifica qual(is) coluna(s) deve(m) ser atualizada(s) e com quais novos valores.
A cláusula `WHERE` é **crucial** para especificar quais linhas devem ser atualizadas. Se você omitir a cláusula `WHERE`, **TODAS AS LINHAS DA TABELA SERÃO ATUALIZADAS!**

## Sintaxe Básica

```sql
UPDATE nome_da_tabela
SET coluna1 = valor1,
    coluna2 = valor2,
    ...
WHERE condicao; -- MUITO IMPORTANTE!

Componentes:

* nome_da_tabela: O nome da tabela a ser atualizada.

* SET colunaX = valorX: Define o novo valor para a coluna especificada. Você pode atualizar múltiplas colunas separando-as por vírgulas.

* WHERE condicao: Especifica as linhas que serão afetadas pela atualização. Apenas as linhas que satisfazem a condicao serão modificadas.

Exemplos
Usando nossa tabela Alunos:

-- Supondo que a tabela Alunos tem estes dados:
-- ID | Nome           | Email                  | DataNascimento | CursoID
-- ---|----------------|------------------------|----------------|---------
-- 1  | João Silva     | joao.silva@email.com   | 2000-05-15     | 101
-- 2  | Maria Santos   | maria.santos@email.com | NULL           | 102
-- 3  | Carlos Pereira | carlos.p@email.com     | 1999-11-20     | 101

Exemplo 1: Atualizar o email do aluno com ID = 1

UPDATE Alunos
SET Email = 'joao.novo@email.com'
WHERE ID = 1;

Após a execução, a linha do João Silva terá o email joao.novo@email.com.

Exemplo 2: Atualizar o CursoID e a DataNascimento da aluna Maria Santos

UPDATE Alunos
SET CursoID = 103,
    DataNascimento = '2001-02-20'
WHERE Nome = 'Maria Santos'; -- Usar nome pode ser arriscado se houver homônimos. ID é mais seguro.

Após a execução, Maria Santos estará no CursoID 103 e terá sua DataNascimento preenchida.

Exemplo 3: Aumentar o CursoID de todos os alunos do CursoID 101 para 105 (ATENÇÃO!)

UPDATE Alunos
SET CursoID = 105
WHERE CursoID = 101;

Isso mudará o CursoID de João Silva e Carlos Pereira para 105.

Exemplo 4: O PERIGO DE OMITIR WHERE (NÃO EXECUTE EM DADOS IMPORTANTES SEM TESTAR!)
Se você executar o seguinte comando, TODOS os alunos terão seu email alterado para 'padrao@email.com':

-- NÃO FAÇA ISSO A MENOS QUE SEJA INTENCIONAL E TESTADO!
-- UPDATE Alunos
-- SET Email = 'padrao@email.com';

Considerações
A Cláusula WHERE é Fundamental: Sempre use uma cláusula WHERE específica para garantir que você está atualizando apenas as linhas desejadas. Teste sua condição WHERE com um SELECT primeiro para ter certeza de que ela seleciona as linhas corretas.

-- Antes de um UPDATE, faça um SELECT:
SELECT * FROM Alunos WHERE ID = 1;
-- Se o SELECT retornar as linhas corretas, então você pode fazer o UPDATE:
-- UPDATE Alunos SET Email = 'novo.email@example.com' WHERE ID = 1;

Tipos de Dados e Restrições: Os novos valores fornecidos na cláusula SET devem ser compatíveis com os tipos de dados das colunas e respeitar quaisquer restrições (ex: NOT NULL, UNIQUE).

Transações: Lembre-se de que UPDATE é um comando DML. Se você não estiver no modo autocommit, as alterações só serão permanentes após um COMMIT. Você pode usar ROLLBACK para desfazer as alterações se cometer um erro antes de comitar.
