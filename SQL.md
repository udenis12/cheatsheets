# Common SQL queries
List of queries from [stepic course](https://stepik.org/course/63054/syllabus).
## Create new data

Create new table
```sql
CREATE TABLE book (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(50),
    author VARCHAR(30),
    price DECIMAL(8, 2),
    amount INT
);
```

Insert new rows
```sql
INSERT INTO book 
VALUES (NULL, "Белая гвардия", "Булгаков М.А.", 540.50, 5),
        (NULL, "Идиот", "Достоевский Ф.М.", 460.00, 10),
        (NULL, "Братья Карамазовы", "Достоевский Ф.М.", 799.01, 2);
```

## Select data

Select all data from table 
```sql
SELECT * 
FROM book;
```

Select columns
```sql
SELECT author, title, price 
FROM book;
```

Aliases (``AS`` is optional)
```sql
SELECT title AS Название, author AS Автор
FROM book;
```

Calculated columns
```sql
SELECT title, amount,
amount * 1.65 AS pack
FROM book
```

List of standart functions
|Function |	Description|
|:-------:|----------|
| CEILING(x) |	возвращает наименьшее целое число, большее или равное x (округляет до целого числа в большую сторону) |
|ROUND(x, k)|	округляет значение x до k знаков после запятой, если k не указано – x округляется до целого|
|FLOOR(x)|	возвращает наибольшее целое число, меньшее или равное x (округляет до  целого числа в меньшую сторону)	|
|POWER(x, y)|	возведение x в степень y|
|SQRT(x)|	квадратный корень из x|
|DEGREES(x)|	конвертирует значение x из радиан в градусы	|
|RADIANS(x)|	конвертирует значение x из градусов в радианы	|
|ABS(x)|	модуль числа x	|
|PI()|	pi = 3.1415926... |

Logical functions
```sql
SELECT author, title,
ROUND(IF(author = 'Булгаков М.А.', 
         price * 1.1, 
         IF(author = 'Есенин С.А.', 
            price * 1.05, 
            price)), 
      2) AS new_price
FROM book;
```

Selection by condition
```sql
SELECT author,title, price
FROM book
WHERE amount < 10;

```
Operator priorities
1. ( )
2. \*, /
3. \+, -
4. =, >, <, >=, <=, <>
5. NOT
6. AND
7. OR

``BETWEEN`` and ``IN`` operators
```sql
SELECT title, author
FROM book
WHERE price BETWEEN 540.50 AND 800 
    AND amount IN (2, 3, 5, 7);
```

Order by column
```sql
SELECT author, title
FROM book
WHERE amount BETWEEN 2 AND 14
ORDER BY author DESC, title
```

``LIKE`` operator
|Mask|Description|
|:---:|---|
| **%** | Zero or more characters |
| **_** | One character|
```sql
SELECT title, author
FROM book
WHERE (title LIKE "%_ %_") AND (author LIKE "% _.С." OR author LIKE "% С._.")
ORDER BY title
```
## Group selection
Select uniquie
```sql
SELECT DISTINCT amount
FROM book;
```

```sql
SELECT DISTINCT name
FROM trip
WHERE city = "Москва"
ORDER BY name
```

``COUNT(*)`` counts all rows, including NULL

``COUNT(column)`` counts all rows in collumn, excluding NULL
```sql
SELECT author AS Автор, 
    COUNT(DISTINCT title) AS Различных_книг, 
    SUM(amount) AS Количество_экземпляров
FROM book
GROUP BY author;
```

```sql
SELECT author,
    MIN(price) AS Минимальная_цена,
    MAX(price) AS Максимальная_цена,
    AVG(price) AS Средняя_цена
FROM book
GROUP BY author
```

Group selection for all table
```sql
SELECT MIN(price) AS "Минимальная_цена",
    MAX(price) AS "Максимальная_цена",
    ROUND(AVG(price), 2)AS "Средняя_цена"
FROM book;
```

Group selection by condition
```sql
SELECT ROUND(AVG(price), 2) AS "Средняя_цена", 
    SUM(price* amount) AS "Стоимость"
FROM book
WHERE amount BETWEEN 5 AND 14;
```

```sql
SELECT author,
    SUM(amount * price) AS "Стоимость"
FROM book
WHERE title <> "Идиот" AND title <> "Белая гвардия"
GROUP BY author
HAVING SUM(amount * price) > 5000
ORDER BY Стоимость DESC;
```

Group select by multiple collumns
```sql
SELECT name, number_plate, violation
FROM fine
GROUP BY name, number_plate, violation
HAVING COUNT(*) >= 2
ORDER BY name, number_plate, violation;
```

## Nested queries
```sql
SELECT author, title, price
FROM book
WHERE price < (
         SELECT AVG(price) 
         FROM book
      )
ORDER BY price DESC;

```

```sql
SELECT author, title, price
FROM book
WHERE (SELECT MIN(price)
      FROM book) + 150 >= price
ORDER BY price
```

```sql
SELECT author, title, amount
FROM book
WHERE amount IN (
    SELECT amount
    FROM book
    GROUP BY amount
    HAVING COUNT(amount) = 1
    );
```
``x > ANY()`` is equalent to ``x > MIN()``

``x > ALL()`` is equalent to ``x > MAX()``

```sql
SELECT author, title, price
FROM book
WHERE price < ANY (
    SELECT MIN(price)
    FROM book
    GROUP BY author);
```

```sql
SELECT title, author, amount,
(SELECT MAX(amount) FROM book) - amount AS Заказ
FROM book
WHERE amount < (SELECT MAX(amount) FROM book);
```

Data intervals
```sql
SELECT name, city, DATEDIFF(date_last, date_first) + 1 AS Длительность
FROM trip
WHERE city NOT IN ("Москва", "Санкт-Петербург")
ORDER BY Длительность DESC;
```

Specify num from date
```sql
SELECT name, city, date_first, date_last
FROM trip
WHERE MONTH(date_first) = MONTH(date_last)
ORDER BY city, name;
```

```sql
SELECT name, city, date_first,
    (DATEDIFF(date_last, date_first) + 1) * per_diem AS Сумма
FROM trip
WHERE MONTH(date_first) IN (2, 3) AND YEAR(date_first) = 2020
ORDER BY name, Сумма DESC;
```

## Data modify
Add values from another table
```sql
INSERT INTO book (title, author, price, amount)
SELECT title, author, price, amount
FROM supply
WHERE author NOT IN("Булгаков М.А.", "Достоевский Ф.М.");
```

```sql
INSERT INTO book (title, author, price, amount)
SELECT title, author, price, amount
FROM supply
WHERE author NOT IN (
    SELECT author
    FROM book
    );
```

Data modify by condition
```sql
UPDATE book
SET price = price * 0.9
WHERE amount BETWEEN 5 AND 10;
```

```sql
UPDATE book 
SET buy = IF(amount - buy < 0, amount, buy),
    price = IF(buy = 0, price * 0.9, price);
```

Data modify for multiple tables
```sql
UPDATE book, supply
SET 
    book.amount = book.amount + supply.amount,
    book.price = ROUND((book.price + supply.price) / 2, 2)
WHERE book.author = supply.author 
    AND book.title = supply.title;
```

```sql
UPDATE fine AS f,
    (SELECT name, number_plate, violation
     FROM fine
     GROUP BY name, number_plate, violation
     HAVING COUNT(*) >= 2
    ) AS q
SET f.sum_fine = f.sum_fine * 2
WHERE (f.name, f.number_plate, f.violation) = (q.name, q.number_plate, q.violation) AND f.date_payment IS NULL;
```

Delete data
```sql
DELETE FROM supply
WHERE author IN (
    SELECT author
    FROM book
    GROUP BY author
    HAVING SUM(amount) > 10
    );
```

```sql
DELETE FROM fine
WHERE date_violation < '2020-02-01';
```

Cleate table from another table
```sql
CREATE TABLE ordering AS
SELECT 
    author, 
    title,
    (SELECT FLOOR(AVG(amount))
     FROM book) AS amount
FROM book
WHERE amount < (SELECT AVG(amount)
                FROM book);
```

```sql
CREATE TABLE back_payment
SELECT name, number_plate, violation, sum_fine, date_violation
FROM fine
WHERE date_payment IS NULL;
```

Aliases
```sql
UPDATE fine AS f, traffic_violation AS tv
SET f.sum_fine = tv.sum_fine
WHERE f.sum_fine IS NULL AND f.violation = tv.violation;
```

```sql
UPDATE fine AS f, payment AS p
SET f.date_payment = p.date_payment, 
    f.sum_fine = IF(DATEDIFF(p.date_payment, p.date_violation) <= 20,
                   ROUND(f.sum_fine / 2, 2),
                   f.sum_fine)
WHERE (f.name, f.number_plate, f.violation, f.date_violation) 
    = (p.name, p.number_plate, p.violation, p.date_violation);
```


```sql
```


```sql
```


```sql
```


```sql
```


```sql
```


```sql
```


```sql
```


```sql
```







