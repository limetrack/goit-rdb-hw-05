# Вкладені запити
### 1. SQL запит, який буде відображати таблицю order_details та поле customer_id з таблиці orders відповідно для кожного поля запису з таблиці order_details:
```
SELECT 
    od.*,
    (SELECT o.customer_id 
     FROM orders o 
     WHERE o.id = od.order_id) AS customer_id
FROM 
    order_details od;
```
<img width="1010" alt="HW5_p1" src="https://github.com/user-attachments/assets/b38822da-a860-462c-a30c-08043924d9a2">

### 2. SQL запит, який буде відображати таблицю order_details. Відфільтруйте результати так, щоб відповідний запис із таблиці orders виконував умову shipper_id=3:
```
SELECT *
FROM order_details
WHERE order_id IN (
    SELECT order_id
    FROM orders
    WHERE shipper_id = 3
);
```
<img width="1009" alt="HW5_p2" src="https://github.com/user-attachments/assets/d3a08d86-730f-49f9-863f-3e8f29e1fcb2">

### 3. SQL запит, вкладений в операторі FROM, який буде обирати рядки з умовою quantity>10 з таблиці order_details. Для отриманих даних знайдіть середнє значення поля quantity — групувати слід за order_id:
```
SELECT 
    subquery.order_id,
    AVG(subquery.quantity) AS average_quantity
FROM (
    SELECT order_id, quantity
    FROM order_details
    WHERE quantity > 10
) AS subquery
GROUP BY subquery.order_id;
```
<img width="1008" alt="HW5_p3" src="https://github.com/user-attachments/assets/25a801a4-382d-4345-8233-352d6b2a1a27">

### 4. Розв’яжіть завдання 3, використовуючи оператор WITH для створення тимчасової таблиці temp:
```
WITH temp AS (
    SELECT order_id, quantity
    FROM order_details
    WHERE quantity > 10
)
SELECT 
    order_id,
    AVG(quantity) AS average_quantity
FROM temp
GROUP BY order_id;
```
<img width="1013" alt="HW5_p4" src="https://github.com/user-attachments/assets/11c68371-6aa9-4a4d-8093-696f09a81119">

### 5. Створіть функцію з двома параметрами, яка буде ділити перший параметр на другий:
```
DROP FUNCTION IF EXISTS divide_numbers;

DELIMITER //

CREATE FUNCTION divide_numbers(
    numerator FLOAT,
    denominator FLOAT
) RETURNS FLOAT
DETERMINISTIC
NO SQL
BEGIN
    IF denominator = 0 THEN
        RETURN NULL;
    END IF;
    RETURN numerator / denominator;
END //

DELIMITER ;

SELECT 
    order_id,
    quantity,
    divide_numbers(quantity, 5.0) AS divided_quantity
FROM 
    order_details;
```
<img width="1010" alt="HW5_p5" src="https://github.com/user-attachments/assets/23df3aa9-4cbe-4415-bac8-79d45616ae8f">

