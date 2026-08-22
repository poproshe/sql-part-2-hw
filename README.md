# Домашнее задание к занятию «SQL. Часть 2»

**Ушаков Игорь Юрьевич**

## Задание 1

```sql
SELECT
    st.last_name,
    st.first_name,
    c.city,
    COUNT(cu.customer_id) AS customer_count
FROM store AS s
JOIN staff AS st ON st.staff_id = s.manager_staff_id
JOIN address AS a ON a.address_id = s.address_id
JOIN city AS c ON c.city_id = a.city_id
JOIN customer AS cu ON cu.store_id = s.store_id
GROUP BY st.staff_id, st.last_name, st.first_name, c.city
HAVING COUNT(cu.customer_id) > 300;
```

## Задание 2

```sql
SELECT COUNT(*) AS films_longer_than_average
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

## Задание 3

```sql
WITH monthly_payments AS (
    SELECT
        date_trunc('month', payment_date) AS payment_month,
        SUM(amount) AS total_amount
    FROM payment
    GROUP BY date_trunc('month', payment_date)
),
monthly_rentals AS (
    SELECT
        date_trunc('month', rental_date) AS rental_month,
        COUNT(*) AS rental_count
    FROM rental
    GROUP BY date_trunc('month', rental_date)
)
SELECT
    to_char(p.payment_month, 'YYYY-MM') AS payment_month,
    p.total_amount,
    COALESCE(r.rental_count, 0) AS rental_count
FROM monthly_payments AS p
LEFT JOIN monthly_rentals AS r ON r.rental_month = p.payment_month
ORDER BY p.total_amount DESC
LIMIT 1;
```

## Задание 4*

```sql
SELECT
    st.staff_id,
    st.last_name,
    st.first_name,
    COUNT(p.payment_id) AS sales_count,
    CASE
        WHEN COUNT(p.payment_id) > 8000 THEN 'Да'
        ELSE 'Нет'
    END AS "Премия"
FROM staff AS st
LEFT JOIN payment AS p ON p.staff_id = st.staff_id
GROUP BY st.staff_id, st.last_name, st.first_name
ORDER BY st.staff_id;
```

## Задание 5*

```sql
SELECT f.film_id, f.title
FROM film AS f
WHERE NOT EXISTS (
    SELECT 1
    FROM inventory AS i
    JOIN rental AS r ON r.inventory_id = i.inventory_id
    WHERE i.film_id = f.film_id
)
ORDER BY f.title;
```
