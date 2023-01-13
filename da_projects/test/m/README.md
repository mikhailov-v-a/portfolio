## Задание

Напишите следующие запросы к базе данных [prd_sbx_general](https://github.com/mikhailov-v-a/portfolio/tree/main/da_projects/test/m/db.md):

1. Количество абонентов в разрезе месяцев, регионов и тарифных групп.
2. Доля абонентов без начислений в разрезе месяцев, групп, регионов.
3. Количество абонентов, активированных в отчётный месяц 
в разрезе регионов.

Для регионов и тарифных групп в результирующей таблице 
должны быть их названия.

## Решение

### 1. Количество абонентов 

**В разрезе месяцев**
```sql
SELECT CASE strftime('%m', act_day)
           WHEN '01' THEN 'January'
           WHEN '02' THEN 'Febuary'
           WHEN '03' THEN 'March'
           WHEN '04' THEN 'April'
           WHEN '05' THEN 'May'
           WHEN '06' THEN 'June'
           WHEN '07' THEN 'July'
           WHEN '08' THEN 'August'
           WHEN '09' THEN 'September'
           WHEN '10' THEN 'October'
           WHEN '11' THEN 'November'
           WHEN '12' THEN 'December'
           ELSE ''
       END AS MONTH,
       COUNT(app_n) AS Amount
FROM pe_test_abnt
GROUP BY MONTH;
```

**В разрезе регионов**
```sql
SELECT ptr.region_name AS Regions,
       COUNT(pta.app_n) AS Amount
FROM pe_test_regions ptr
JOIN pe_test_abnt pta ON ptr.region = pta.region
GROUP BY Regions;
```

**В разрезе тарифных групп**
```sql
SELECT ptt.tp_group_name AS Tariff,
       COUNT(pta.app_n) AS Amount
FROM pe_test_tariffs ptt
JOIN pe_test_abnt pta ON ptt.tp_group = pta.tp_group
GROUP BY Tariff;
```

### 2. Доля абонентов без начислений 

**В разрезе месяцев**
```sql
WITH ptc (tbm, app_n, revenue, abon_rev) AS
  (SELECT *
   FROM pe_test_clc
   WHERE revenue = 0)
SELECT strftime('%m', pta.tbm) AS MONTH,
       COUNT(*) AS Amount
FROM pe_test_abnt pta
JOIN ptc ON pta.app_n = ptc.app_n
GROUP BY MONTH;
```

**В разрезе тарифных групп**
```sql
WITH ptc (tbm, app_n, revenue, abon_rev) AS
  (SELECT *
   FROM pe_test_clc
   WHERE revenue = 0)
SELECT ptt.tp_group_name AS Tariff,
       COUNT(*) AS Amount
FROM pe_test_abnt pta
JOIN ptc ON pta.app_n = ptc.app_n
LEFT JOIN pe_test_tariffs ptt ON pta.tp_group = ptt.tp_group
GROUP BY Tariff;
```

**В разрезе регионов**
```sql
WITH ptc (tbm, app_n, revenue, abon_rev) AS
  (SELECT *
   FROM pe_test_clc
   WHERE revenue = 0)
SELECT ptr.region_name AS Regions,
       COUNT(*) AS Amount
FROM pe_test_abnt pta
JOIN ptc ON pta.app_n = ptc.app_n
LEFT JOIN pe_test_regions ptr ON pta.region = ptr.region
GROUP BY Regions;
```

### 3. Количество абонентов, активированных в отчётный месяц в разрезе регионов

```sql
SELECT ptr.region_name AS Regions,
       COUNT(*) AS Amount
FROM pe_test_abnt pta
JOIN pe_test_clc ptc ON pta.app_n = ptc.app_n
JOIN pe_test_regions ptr ON pta.region = ptr.region
WHERE strftime('%m', pta.tbm) = strftime('%m', pta.act_day)
GROUP BY Regions;
```