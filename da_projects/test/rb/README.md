## Источник данных - таблица TEST_SQL. 

**Поля таблицы:**
- **ST_ORD** - статус заявки
- **TYPE_PRODUCT_NAME** - тип заявки
- **PRODUCT_INFOSOURCE1** - источник
- **CREATE_DATE** - дата создания заявки
- **INDEX_LEAD** - индикатор заявки. Флаг 0/1 определяет регистрацию.
- **INDEX_ISSUE** - индикатор выдачи. Флаг 0/1 определяет наличие выдачи.
- **INDEX_SUM** - сумма по продукту

### Задание 1

Сгруппировать по месяцам количество заявок, сумму выдач, вычислить долю выдач.

```sql
SELECT
  monthname(create_date) AS months,
  COUNT(index_lead) AS amount,
  SUM(index_sum) AS total_sum,
  CONCAT(ROUND((SUM(index_sum) / (SELECT SUM(index_sum) FROM test_sql)) * 100.0, 2), '%') AS share_total_sum
FROM
  test_sql
GROUP BY
  months;
```

### Задание 2

Добавить индикатор, который будет выделять следующие значения:
- Если сумма по заявке больше 2.000.000 и дата создания заявки «март 2020» - 1
- Если сумма по заявке больше 1.000.000, но меньше 2.000.000 и дата создания заявки «март 2020» - 2 
- Все остальные заявки не должны попасть в результат выполнения запроса.

```sql
WITH ind (
  st_ord, type_product_name, product_infosource1,
  create_date, index_lead, index_issue,
  index_sum, indicator
) AS (
  SELECT
    *,
    CASE WHEN index_sum > 2000000
    AND MONTH(create_date) = 3
    AND YEAR(create_date) = 2020 THEN 1 WHEN index_sum < 2000000
    AND index_sum > 1000000
    AND MONTH(create_date) = 3
    AND YEAR(create_date) = 2020 THEN 2 ELSE -1 END
  FROM
    test_sql
);

SELECT * FROM ind WHERE indicator <> -1;
```


### Задание 3

Показать источник, через который пришло наибольшее число заявок.

```sql
SELECT
  t1.product_infosource1
FROM
  (
    SELECT
      product_infosource1,
      COUNT(*) AS total_amount
    FROM
      test_sql
    GROUP BY
      product_infosource1
  ) AS t1
ORDER BY
  total_amount DESC
LIMIT
  1;
```


### Задание 4

**tab_nums**
|numbers|
|:-:|
|1|
|-21|
|0|
|-100|
|7|
|5|
|0|
|10|
|-2|
|5|
|-2|

Напишите запрос, не используя предложение WHERE, который выводит только положительные числа.

```sql
SELECT
  t1.numbers
FROM
  table_num t1
  JOIN table_num t2 ON t1.numbers = t2.numbers
  AND t1.numbers > 0;
```

Напишите запрос, не используя предложение WHERE и явные операторы соединения JOIN, который выводит только положительные числа.

```sql
SELECT
  numbers
FROM
  table_num
GROUP BY
  numbers
HAVING
  numbers > 0;
```
