# Limitar Resultados: LIMIT / TOP / ROWNUM

Frequentemente, você não precisa de todas as linhas que uma consulta pode retornar, mas apenas de um subconjunto, como as "N primeiras" linhas, ou um "intervalo" de linhas (para paginação).
A sintaxe para limitar o número de resultados varia significativamente entre diferentes Sistemas de Gerenciamento de Banco de Dados (SGBDs).

## Sintaxe e Exemplos por SGBD

### 1. MySQL e PostgreSQL: `LIMIT` e `OFFSET`

*   **`LIMIT N`**: Retorna as primeiras N linhas.
*   **`LIMIT N OFFSET M`** (ou `LIMIT M, N` em MySQL): Pula as primeiras M linhas e então retorna as próximas N linhas. `OFFSET` é útil para paginação.

**Exemplo (MySQL/PostgreSQL): Selecionar os 3 produtos mais caros**
(Assumindo a tabela `Produtos` das seções anteriores)

```sql
SELECT Nome, Preco
FROM Produtos
ORDER BY Preco DESC
LIMIT 3;

Resultado:

Nome                  | Preco
----------------------|--------
Laptop XPTO           | 3500.00
Monitor 24"           | 1200.00
Cadeira de Escritório |  800.00

Exemplo (MySQL/PostgreSQL): Selecionar produtos da página 2, com 2 produtos por página
(Página 1: OFFSET 0, LIMIT 2; Página 2: OFFSET 2, LIMIT 2)

SELECT Nome, Preco
FROM Produtos
ORDER BY ID ASC -- Uma ordem consistente é importante para paginar
LIMIT 2 OFFSET 2; -- Pula os 2 primeiros, pega os próximos 2

Resultado (assumindo que os IDs 1 e 2 são os primeiros):

Nome               | Preco
-------------------|--------
Teclado Mecânico   |  300.00 -- Produto com ID 3
Monitor 24"        | 1200.00 -- Produto com ID 4

MySQL também suporta: LIMIT 2, 2; (OFFSET, COUNT)

2. SQL Server: TOP N e OFFSET/FETCH (SQL Server 2012+)
SELECT TOP N ...: Retorna as primeiras N linhas.

ORDER BY ... OFFSET M ROWS FETCH NEXT N ROWS ONLY: Pula M linhas e retorna as próximas N (requer ORDER BY).

Exemplo (SQL Server): Selecionar os 3 produtos mais caros

SELECT TOP 3 Nome, Preco
FROM Produtos
ORDER BY Preco DESC;

Exemplo (SQL Server 2012+): Paginação - página 2, 2 produtos por página

SELECT Nome, Preco
FROM Produtos
ORDER BY ID ASC -- ORDER BY é obrigatório para OFFSET/FETCH
OFFSET 2 ROWS       -- Pula 2 linhas
FETCH NEXT 2 ROWS ONLY; -- Pega as próximas 2 linhas

3. Oracle: ROWNUM e Subconsultas (ou FETCH FIRST em Oracle 12c+)
WHERE ROWNUM <= N: Usado em uma subconsulta após a ordenação para pegar as N primeiras linhas. ROWNUM é atribuído antes do ORDER BY se não estiver em uma subconsulta.

Oracle 12c+: OFFSET M ROWS FETCH NEXT N ROWS ONLY (similar ao SQL Server).

Exemplo (Oracle - método clássico com ROWNUM): Selecionar os 3 produtos mais caros

SELECT Nome, Preco
FROM (
    SELECT Nome, Preco
    FROM Produtos
    ORDER BY Preco DESC
)
WHERE ROWNUM <= 3;

Exemplo (Oracle 12c+): Selecionar os 3 produtos mais caros

SELECT Nome, Preco
FROM Produtos
ORDER BY Preco DESC
FETCH FIRST 3 ROWS ONLY;

Exemplo (Oracle 12c+): Paginação - página 2, 2 produtos por página

SELECT Nome, Preco
FROM Produtos
ORDER BY ID ASC
OFFSET 2 ROWS
FETCH NEXT 2 ROWS ONLY;

Considerações
ORDER BY é Essencial: Para que "as N primeiras" linhas tenham um significado consistente, você quase sempre precisará usar ORDER BY antes de aplicar a limitação. Sem ORDER BY, as "primeiras" linhas são arbitrárias.

Paginação: Para implementar paginação de resultados (mostrar resultados em várias páginas), as cláusulas OFFSET (ou equivalentes) são cruciais.

Performance: Limitar o número de resultados pode melhorar a performance, pois menos dados precisam ser transferidos e processados.
