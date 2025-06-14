
#  BigQuery vs MySQL SQL Cheat Sheet

| **Concept**                  | **MySQL Syntax**                                  | **BigQuery Syntax**                                             |  Notes |
|-----------------------------|---------------------------------------------------|------------------------------------------------------------------|---------|
| **Select Data**             | `SELECT * FROM table;`                           | `SELECT * FROM \`project.dataset.table\`;`                      | Use backticks and full path in BQ |
| **Limit Results**           | `LIMIT 10;`                                      | `LIMIT 10;`                                                     | Same |
| **Filter Rows**             | `WHERE column = 'value'`                         | `WHERE column = 'value'`                                       | Same |
| **Sorting**                 | `ORDER BY column DESC`                           | `ORDER BY column DESC`                                         | Same |
| **Aliases**                 | `SELECT col AS alias`                            | `SELECT col AS alias`                                          | Same |
| **Aggregate Functions**     | `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`    | Same                                                           | Same syntax |
| **GROUP BY**                | `GROUP BY column`                                | `GROUP BY column`                                              | Same |
| **HAVING**                  | `HAVING COUNT(*) > 5`                            | `HAVING COUNT(*) > 5`                                          | Same |
| **JOINs**                   | `INNER JOIN`, `LEFT JOIN`, etc.                  | Same                                                           | Fully supported |
| **Subqueries**              | `SELECT ... FROM (SELECT ...)`                   | Same                                                           | Same |
| **Aliases for tables**      | `FROM users AS u`                                | `FROM users AS u`                                              | Same |
| **String functions**        | `CONCAT(str1, str2)`, `LENGTH(col)`              | `CONCAT(str1, str2)`, `LENGTH(col)`                            | Mostly same |
| **Date functions**          | `NOW()`, `CURDATE()`, `YEAR(date_col)`           | `CURRENT_DATE()`, `EXTRACT(YEAR FROM date_col)`                | BQ uses `EXTRACT()` |
| **LIMIT + OFFSET**          | `LIMIT 10 OFFSET 20`                             | `LIMIT 10 OFFSET 20`                                           | Same |
| **IF/CASE**                 | `IF(expr, val1, val2)`, `CASE WHEN`              | `IF(expr, val1, val2)`, `CASE WHEN`                            | Same |
| **CREATE TABLE**            | `CREATE TABLE my_table ...`                      | Usually handled in UI or with `CREATE TABLE` DDL               | BQ also supports DDL |
| **Temporary tables**        | `CREATE TEMPORARY TABLE ...`                     | Use `WITH` CTE instead                                         | CTE preferred in BQ |
| **Auto-Increment IDs**      | `AUTO_INCREMENT`                                 |  Not supported directly                                       | Use `GENERATE_UUID()` or ROW_NUMBER() |
| **String matching**         | `LIKE '%word%'`                                  | `LIKE '%word%'`, `REGEXP_CONTAINS(column, 'regex')`            | BQ supports regex too |
| **Date Parsing**            | `STR_TO_DATE()`                                  | `PARSE_DATE()`, `PARSE_TIMESTAMP()`                            | Different function names |
| **Current Timestamp**       | `NOW()`                                          | `CURRENT_TIMESTAMP()`                                          | Different function name |
| **Window Functions**        |  Supported                                      |  Fully supported                                              | Identical usage |
| **Array functions**         |  Not supported                                 |  `ARRAY`, `UNNEST()`                                          | BigQuery special |
| **Structs/Nested Data**     |  Not supported                                 |  `STRUCT`, `RECORD` types                                     | BigQuery-only feature |

---

##  Example: Count of Movies by Country

**MySQL**
```sql
SELECT country, COUNT(*) 
FROM netflix_titles 
GROUP BY country 
ORDER BY COUNT(*) DESC;
```

**BigQuery**
```sql
SELECT country, COUNT(*) AS count
FROM `bigquery-public-data.netflix_titles.netflix_titles`
GROUP BY country
ORDER BY count DESC;
```

---

##  Special BigQuery Features

| Feature | Example | Description |
|--------|---------|-------------|
| `WITH` CTE | `WITH temp AS (...) SELECT * FROM temp` | Common Table Expressions |
| Arrays | `ARRAY_AGG(name)` | Combine values into array |
| Unnest | `FROM UNNEST(array_column)` | Explode arrays |
| Structs | `SELECT info.name FROM table` | Nested fields |
| Safe casting | `SAFE_CAST(value AS INT64)` | Avoid cast errors |
