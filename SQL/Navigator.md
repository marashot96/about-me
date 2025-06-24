# <div align='center'> Навыки работы в SQL </div>

Добро пожаловать в раздел SQL моего портфолио! Здесь представлены практические задачи, которые демонстрируют мой уровень владения SQL при работе с реальными и учебными датасетами.

## 📌 Основные навыки:

- Уверенное владение **запросами SELECT, WHERE, GROUP BY, ORDER BY**
- Опыт написания **JOIN’ов всех типов (INNER, LEFT, FULL)**
- Умение использовать **вложенные запросы и CTE (WITH)**
- Работа с **агрегациями и оконными функциями (RANK, ROW_NUMBER, SUM OVER)**
- Подготовка аналитических выборок, временных таблиц, фильтрации и ранжирования данных

## ▶️ Пример моего запроса: последние 3 транзакции каждого клиента (с оконными функциями)

```sql
WITH ranked_transactions AS (
    SELECT
        c.customer_id,
        c.name,
        t.transaction_id,
        t.amount,
        t.transaction_date,
        COUNT(*) OVER (PARTITION BY c.customer_id) AS total_transactions,
        ROW_NUMBER() OVER (
            PARTITION BY c.customer_id
            ORDER BY t.transaction_date DESC
        ) AS txn_rank
    FROM customers c
    JOIN transactions t ON c.customer_id = t.customer_id
)
SELECT
    customer_id,
    name,
    transaction_id,
    amount,
    transaction_date,
    total_transactions,
    txn_rank
FROM ranked_transactions
WHERE txn_rank <= 3
ORDER BY customer_id, txn_rank;
```

## 🗂️ Структура раздела:
Здесь приведены различные запросы, которые я использовал в работе на протяжении 5 лет. Сделал основную сборку

| № | Файл | Содержание |
|---|------|------------|
| 1 | [`01_basic_select.sql`](./01_basic_select.sql) | Простые выборки: фильтрация, сортировка, группировка |
| 2 | [`02_joins_and_subqueries.sql`](./02_joins_and_subqueries.sql) | Соединения таблиц и подзапросы |
| 3 | [`03_agg_and_window.sql`](./03_agg_and_window.sql) | Агрегации и оконные функции |
| 4 | [`04_cte_and_temp_tables.sql`](./04_cte_and_temp_tables.sql) | CTE, временные таблицы |
| 5 | [`05_real_case_analysis.md`](./05_real_case_analysis.md) | Кейс: анализ клиентской активности в онлайн-банке |

---


## 🧩 Используемые СУБД:
- PostgreSQL (основная)
- SQLite (для быстрого прототипирования)
- MySQL (на этапе тестирования)

