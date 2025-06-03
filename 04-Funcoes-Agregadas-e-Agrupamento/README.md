# 04 - Funções Agregadas e Agrupamento

Funções agregadas realizam cálculos sobre um conjunto de valores (um grupo de linhas) e retornam um único valor resumido. Elas são frequentemente usadas com a cláusula `GROUP BY` para calcular métricas para diferentes subgrupos de dados.

## Tópicos Abordados Nesta Seção:

*   **Funções Agregadas Comuns (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`)**: As funções mais básicas para contagem, soma, média, mínimo e máximo.
    *   [Ver Detalhes e Exemplos](./COUNT_SUM_AVG_MIN_MAX.md)
*   **Cláusula `GROUP BY`**: Agrupa linhas que têm os mesmos valores em uma ou mais colunas, permitindo que funções agregadas sejam aplicadas a cada grupo.
    *   [Ver Detalhes e Exemplos](./GROUP_BY.md)
*   **Cláusula `HAVING`**: Filtra os resultados *após* a agregação e o agrupamento terem sido feitos (diferente da cláusula `WHERE`, que filtra linhas *antes* da agregação).
    *   [Ver Detalhes e Exemplos](./HAVING.md)

Essas ferramentas são essenciais para análise de dados, geração de relatórios e obtenção de insights a partir dos seus dados.

---
[Voltar para o Início](../../README.md) | [Próximo: Funções Agregadas Comuns](./COUNT_SUM_AVG_MIN_MAX.md)
