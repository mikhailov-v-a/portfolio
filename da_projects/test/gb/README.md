## 1. Реализовать работу джоинов (INNER, LEFT, RIGHT, FULL) для таблиц A и B, вывести результаты.

**A** 
 
|  a   |  
|:----:|
|  7   |
|  1   |
|  2   |
|  3   |
|  4   |
|  1   |
| NULL |

**B** 

|  b   |
|:----:|
|  1   |
|  3   |
|  5   |
|  7   |
|  7   |
| NULL |
| NULL |

**INNER JOIN**:

```sql
SELECT * FROM A JOIN B ON A.a = B.b;
```

|  a  |  b  | 
|:---:|:---:|
|  7  |  7  |
|  7  |  7  |
|  1  |  1  |
|  3  |  3  |
|  1  |  1  |

**LEFT JOIN**:

```sql
SELECT * FROM A LEFT JOIN B ON A.a = B.b;
```

|  a   |  b   | 
|:----:|:----:|
|  7   |  7   |
|  7   |  7   |
|  1   |  1   |
|  2   | NULL |
|  3   |  3   |
|  4   | NULL |
|  1   |  1   |
| NULL | NULL |

**RIGHT JOIN**:

```sql
SELECT * FROM A RIGHT JOIN B ON A.a = B.b;
```

|  a   |  b   | 
|:----:|:----:|
|  1   |  1   |
|  1   |  1   |
|  3   |  3   |
| NULL |  5   |
|  7   |  7   |
|  7   |  7   |
| NULL | NULL |
| NULL | NULL |

**FULL JOIN**:

```sql
SELECT * FROM A FULL JOIN B ON A.a = B.b;
```

|  a   |  b   | 
|:----:|:----:|
|  7   |  7   |
|  7   |  7   |
|  1   |  1   |
|  2   | NULL |
|  3   |  3   |
|  4   | NULL |
| NULL |  5   |
| NULL | NULL |
| NULL | NULL |

## 2. Написать запрос, который выводит количество пятёрок и фамилии тех учеников, у которых больше трёх двоек.

Вид таблицы:

| name    | mark | subj         |
|---------|------|--------------|
| Иванов  | 4    | История      |
| Иванов  | 5    | Физика       |
| Петров  | 2    | История      |
| Сидоров | 5    | История      |
| Петров  | 5    | Физика       |
| Иванов  | 2    | История      |
| Сидоров | 5    | История      |

```sql
SELECT COUNT(mark),
       name
FROM TABLE
WHERE mark = 5
  AND name IN
    (SELECT name
     FROM TABLE
     WHERE mark = 2
     GROUP BY name
     HAVING COUNT(mark) > 3)
GROUP BY name;
```

## 3. Создать базу данных аптеки и ответить на вопросы

**[Схема БД](https://github.com/mikhailov-v-a/portfolio/tree/main/da_projects/test/gb/gb_db_erd.png)**

**На какую сумму было продано арбидола за январь 2020?**

```sql
SELECT SUM(t.sum)
FROM transact t
JOIN drugs d ON t.id = d.id
WHERE d.name = 'Арбидол'
  AND strftime('%Y-%m', t.tr_date) = '2020-01';
```

**Вывести названия товаров, сумму продаж по этим товарам из TRANSACT, у которых количествово товаров из таблицы AVAILABILITY на 31.01.2020 было больше десяти, а цена по этим товарам в таблице PRICE в течение 2020 года была не менее 100 рублей.**

```sql
SELECT drugs.name,
       SUM(transact.sum) AS Сумма
FROM transact
JOIN drugs ON transact.id = drugs.id
JOIN availability ON transact.id = availability.id
JOIN price ON transact.id = price.id
WHERE (availability.amount > 1
       AND (strftime('%Y-%m-%d', availability.eff_date_from) >= '2020-12-31'
            AND strftime('%Y-%m-%d', availability.eff_date_to) <= '2020-12-31')
       AND (price.price > 100
            AND strftime('%Y', availability.eff_date_from) = '2020'
            AND strftime('%Y', availability.eff_date_to) = '2020'));
```