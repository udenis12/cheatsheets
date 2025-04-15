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


```sql
```


```sql
```


```sql
```
