
---
---

### `02-DML-Data-Manipulation-Language/README.md`

# 02 - DML (Data Manipulation Language)

A DML, ou Linguagem de Manipulação de Dados, é um subconjunto de comandos SQL usado para gerenciar (inserir, consultar, atualizar e excluir) os dados dentro das tabelas do banco de dados.
Diferentemente da DDL, que lida com a estrutura, a DML lida com o conteúdo.

## Comandos DML Principais Abordados Nesta Seção:

*   **`INSERT INTO`**: Usado para adicionar novas linhas (registros) a uma tabela.
    *   [Ver Detalhes e Exemplos](./INSERT_INTO.md)
*   **`SELECT FROM`**: Usado para recuperar (consultar) dados de uma ou mais tabelas. Este é o comando SQL mais utilizado.
    *   [Ver Detalhes e Exemplos](./SELECT_FROM.md)
*   **`UPDATE SET`**: Usado para modificar dados existentes em uma ou mais linhas de uma tabela.
    *   [Ver Detalhes e Exemplos](./UPDATE_SET.md)
*   **`DELETE FROM`**: Usado para remover linhas existentes de uma tabela.
    *   [Ver Detalhes e Exemplos](./DELETE_FROM.md)

## Transações

Comandos DML (especialmente `INSERT`, `UPDATE`, `DELETE`) geralmente operam dentro do contexto de uma **transação**. Uma transação é uma sequência de uma ou mais operações SQL executadas como uma única unidade de trabalho.

*   **`COMMIT`**: Salva permanentemente todas as alterações feitas na transação atual.
*   **`ROLLBACK`**: Desfaz todas as alterações feitas na transação atual, revertendo o banco de dados ao estado anterior ao início da transação.
*   Muitos SGBDs têm um modo "autocommit" que pode ser ativado ou desativado. Se estiver ativado, cada comando DML é tratado como uma transação individual e é automaticamente comitado. Se desativado, você precisa explicitamente usar `COMMIT` para salvar as alterações.

Entender transações é crucial para garantir a consistência e integridade dos dados.

---
[Voltar para o Início](../../README.md) | [Próximo: INSERT INTO](./INSERT_INTO.md)
