---

### `01-DDL-Data-Definition-Language/DROP_TABLE.md`


# DDL: DROP TABLE

O comando `DROP TABLE` é usado para remover completamente uma tabela existente do banco de dados.
Esta ação é **IRREVERSÍVEL** e remove a estrutura da tabela, todos os dados contidos nela, e quaisquer índices, gatilhos, restrições e permissões associadas à tabela.

## Sintaxe Básica

```sql
DROP TABLE nome_da_tabela;

Considerações e Advertências
PERMANENTE: Mais uma vez, a exclusão de uma tabela com DROP TABLE é geralmente permanente e não pode ser desfeita facilmente (a menos que você tenha backups).

Dependências: Se outras tabelas tiverem chaves estrangeiras (FOREIGN KEY) referenciando a tabela que você está tentando remover, o SGBD pode impedir a operação DROP TABLE para manter a integridade referencial. Você pode precisar remover essas restrições de chave estrangeira primeiro, ou usar opções como CASCADE (se suportado e se for realmente o que você quer, pois isso pode remover objetos dependentes).

-- Exemplo com CASCADE (USAR COM EXTREMO CUIDADO!)
-- A sintaxe exata e o comportamento do CASCADE podem variar entre SGBDs.
-- Em PostgreSQL, por exemplo:
-- DROP TABLE nome_da_tabela CASCADE;

Backup: Sempre tenha um backup recente do seu banco de dados antes de executar operações DROP TABLE em ambientes importantes, especialmente em produção.

Alternativa para Limpar Dados: Se você quer apenas remover todos os dados de uma tabela, mas manter sua estrutura,
considere usar DELETE FROM nome_da_tabela; (mais lento, logado, dispara gatilhos)
ou TRUNCATE TABLE nome_da_tabela; (mais rápido, geralmente menos logado, não dispara gatilhos de linha).
