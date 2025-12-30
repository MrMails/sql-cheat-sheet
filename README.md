# Шпаргалка по синтаксису SQL

Краткое справочное руководство по синтаксису SQL, командам и функциям.

---

## Содержание

1. [Язык определения данных (DDL)](#1-язык-определения-данных-ddl)
   - [CREATE](#create)
   - [ALTER](#alter)
   - [DROP](#drop)
2. [Язык манипулирования данными (DML)](#2-язык-манипулирования-данными-dml)
   - [SELECT](#select)
   - [INSERT](#insert)
   - [UPDATE](#update)
   - [DELETE](#delete)
3. [Язык управления данными (DCL)](#3-язык-управления-данными-dcl)
   - [GRANT](#grant)
   - [REVOKE](#revoke)
4. [Язык управления транзакциями (TCL)](#4-язык-управления-транзакциями-tcl)
   - [COMMIT](#commit)
   - [ROLLBACK](#rollback)
   - [SAVEPOINT](#savepoint)
5. [Ограничения](#5-ограничения)
6. [Операторы](#6-операторы)
7. [Функции](#7-функции)
   - [Агрегатные функции](#агрегатные-функции)
   - [Строковые функции](#строковые-функции)
   - [Функции даты и времени](#функции-даты-и-времени)
8. [Объединения (Joins)](#8-объединения-joins)
   - [INNER JOIN](#inner-join)
   - [LEFT JOIN](#left-join)
   - [RIGHT JOIN](#right-join)
   - [FULL OUTER JOIN](#full-outer-join)
9. [Подзапросы](#9-подзапросы)
10. [Представления (Views)](#10-представления-views)
11. [Индексы](#11-индексы)
12. [Хранимые процедуры и функции](#12-хранимые-процедуры-и-функции)
13. [Общие предложения](#13-общие-предложения)
    - [WHERE](#where)
    - [GROUP BY](#group-by)
    - [HAVING](#having)
    - [ORDER BY](#order-by)
    - [LIMIT / TOP / FETCH](#limit--top--fetch)
14. [Типы данных](#14-типы-данных)
15. [Дополнительные ресурсы](#15-дополнительные-ресурсы)

---

## 1. Язык определения данных (DDL)

### CREATE

**Создать базу данных**

```sql
CREATE DATABASE database_name;
```

**Создать таблицу**

```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
);
```

*Пример:*

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    CreatedDate DATE DEFAULT CURRENT_DATE
);
```

### ALTER

**Добавить столбец**

```sql
ALTER TABLE table_name
ADD column_name datatype [constraints];
```

**Изменить столбец**

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name new_datatype;
```

**Удалить столбец**

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

### DROP

**Удалить таблицу**

```sql
DROP TABLE table_name;
```

**Удалить базу данных**

```sql
DROP DATABASE database_name;
```

---

## 2. Язык манипулирования данными (DML)

### SELECT

**Базовый синтаксис**

```sql
SELECT column1, column2, ...
FROM table_name
[WHERE condition]
[GROUP BY column1, column2, ...]
[HAVING condition]
[ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...];
```

*Пример:*

```sql
SELECT FirstName, LastName
FROM Customers
WHERE CreatedDate > '2023-01-01'
ORDER BY LastName ASC;
```

### INSERT

**Вставить во все столбцы**

```sql
INSERT INTO table_name
VALUES (value1, value2, ...);
```

**Вставить в конкретные столбцы**

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

*Пример:*

```sql
INSERT INTO Customers (FirstName, LastName, Email)
VALUES ('Alice', 'Smith', 'alice@example.com');
```

### UPDATE

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

*Пример:*

```sql
UPDATE Customers
SET Email = 'newemail@example.com'
WHERE CustomerID = 1;
```

### DELETE

```sql
DELETE FROM table_name
WHERE condition;
```

*Пример:*

```sql
DELETE FROM Customers
WHERE CustomerID = 1;
```

---

## 3. Язык управления данными (DCL)

### GRANT

```sql
GRANT privilege_type ON object_name TO user [WITH GRANT OPTION];
```

*Пример:*

```sql
GRANT SELECT, INSERT ON Customers TO 'db_user';
```

### REVOKE

```sql
REVOKE privilege_type ON object_name FROM user;
```

*Пример:*

```sql
REVOKE INSERT ON Customers FROM 'db_user';
```

---

## 4. Язык управления транзакциями (TCL)

### COMMIT

```sql
COMMIT;
```

### ROLLBACK

```sql
ROLLBACK;
```

### SAVEPOINT

```sql
SAVEPOINT savepoint_name;
```

**Откат к точке сохранения**

```sql
ROLLBACK TO SAVEPOINT savepoint_name;
```

---

## 5. Ограничения

- **NOT NULL**: Гарантирует, что столбец не может содержать значение NULL.
- **UNIQUE**: Гарантирует, что все значения в столбце уникальны.
- **PRIMARY KEY**: Уникально идентифицирует каждую запись в таблице.
- **FOREIGN KEY**: Обеспечивает ссылочную целостность между таблицами.
- **CHECK**: Гарантирует, что все значения в столбце удовлетворяют определенному условию.
- **DEFAULT**: Устанавливает значение по умолчанию для столбца, если оно не указано.

*Пример:*

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE NOT NULL,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

---

## 6. Операторы

- **Арифметические операторы**: `+`, `-`, `*`, `/`, `%`
- **Операторы сравнения**: `=`, `<>` или `!=`, `>`, `<`, `>=`, `<=`
- **Логические операторы**: `AND`, `OR`, `NOT`
- **Другие операторы**: `BETWEEN`, `IN`, `LIKE`, `IS NULL`, `EXISTS`

*Пример:*

```sql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 50;

SELECT * FROM Customers
WHERE Email LIKE '%@example.com';
```

---

## 7. Функции

### Агрегатные функции

- **COUNT()**: Подсчитывает количество строк.
- **SUM()**: Вычисляет сумму числового столбца.
- **AVG()**: Вычисляет среднее значение.
- **MIN()**: Находит минимальное значение.
- **MAX()**: Находит максимальное значение.

*Пример:*

```sql
SELECT COUNT(*) FROM Orders;

SELECT CustomerID, SUM(TotalAmount) as TotalSpent
FROM Orders
GROUP BY CustomerID;
```

### Строковые функции

- **UPPER(string)**: Преобразует в верхний регистр.
- **LOWER(string)**: Преобразует в нижний регистр.
- **SUBSTRING(string, start, length)**: Извлекает подстроку.
- **TRIM(string)**: Удаляет пробелы.
- **CONCAT(string1, string2, ...)**: Объединяет строки.

*Пример:*

```sql
SELECT UPPER(FirstName), LOWER(LastName)
FROM Customers;
```

### Функции даты и времени

- **NOW()**: Возвращает текущие дату и время.
- **CURDATE()**: Возвращает текущую дату.
- **DATE_ADD(date, INTERVAL value unit)**: Добавляет временной интервал к дате.
- **DATEDIFF(date1, date2)**: Возвращает разницу между двумя датами.

*Пример:*

```sql
SELECT NOW();

SELECT DATE_ADD(CURDATE(), INTERVAL 7 DAY);
```

---

## 8. Объединения (Joins)

### INNER JOIN

Возвращает записи, имеющие совпадающие значения в обеих таблицах.

```sql
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

*Пример:*

```sql
SELECT Orders.OrderID, Customers.FirstName, Customers.LastName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

### LEFT JOIN

Возвращает все записи из левой таблицы и совпадающие записи из правой таблицы.

```sql
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

### RIGHT JOIN

Возвращает все записи из правой таблицы и совпадающие записи из левой таблицы.

```sql
SELECT columns
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

### FULL OUTER JOIN

Возвращает все записи, если есть совпадение в левой или правой таблице.

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2 ON table1.column = table2.column;
```

---

## 9. Подзапросы

Запрос, вложенный в другой запрос.

```sql
SELECT column1
FROM table1
WHERE column2 = (SELECT column FROM table2 WHERE condition);
```

*Пример:*

```sql
SELECT FirstName, LastName
FROM Customers
WHERE CustomerID IN (SELECT CustomerID FROM Orders WHERE TotalAmount > 1000);
```

---

## 10. Представления (Views)

Виртуальная таблица, основанная на результирующем наборе SQL-запроса.

**Создать представление**

```sql
CREATE VIEW view_name AS
SELECT columns
FROM table
WHERE condition;
```

*Пример:*

```sql
CREATE VIEW HighValueOrders AS
SELECT OrderID, CustomerID, TotalAmount
FROM Orders
WHERE TotalAmount > 1000;
```

---

## 11. Индексы

Используются для ускорения поиска данных.

**Создать индекс**

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

**Создать уникальный индекс**

```sql
CREATE UNIQUE INDEX index_name
ON table_name (column);
```

---

## 12. Хранимые процедуры и функции

### Хранимая процедура

Набор SQL-операторов, которые могут быть сохранены и выполнены на сервере базы данных.

**Создать хранимую процедуру**

```sql
CREATE PROCEDURE procedure_name (parameters)
BEGIN
    -- SQL statements
END;
```

*Пример:*

```sql
CREATE PROCEDURE GetCustomerOrders(IN custID INT)
BEGIN
    SELECT * FROM Orders WHERE CustomerID = custID;
END;
```

### Пользовательская функция

Функция, которая возвращает одно значение.

**Создать функцию**

```sql
CREATE FUNCTION function_name (parameters)
RETURNS datatype
BEGIN
    DECLARE variable datatype;
    -- SQL statements
    RETURN variable;
END;
```

---

## 13. Общие предложения

### WHERE

Фильтрует записи, которые соответствуют определенным условиям.

```sql
SELECT * FROM table_name
WHERE condition;
```

### GROUP BY

Группирует строки, имеющие одинаковые значения в указанных столбцах.

```sql
SELECT column1, AGG_FUNC(column2)
FROM table_name
GROUP BY column1;
```

### HAVING

Фильтрует группы в соответствии с указанными условиями.

```sql
SELECT column1, AGG_FUNC(column2)
FROM table_name
GROUP BY column1
HAVING condition;
```

### ORDER BY

Сортирует результирующий набор.

```sql
SELECT * FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC];
```

### LIMIT / TOP / FETCH

Ограничивает количество возвращаемых записей.

- **MySQL, PostgreSQL, SQLite**

  ```sql
  SELECT * FROM table_name
  LIMIT number OFFSET offset;
  ```

- **SQL Server**

  ```sql
  SELECT TOP number * FROM table_name;
  ```

- **Oracle**

  ```sql
  SELECT * FROM table_name
  FETCH FIRST number ROWS ONLY;
  ```

---

## 14. Типы данных

### Числовые

- `INT`
- `DECIMAL(p, s)`
- `FLOAT`
- `DOUBLE`

### Строковые

- `CHAR(n)`
- `VARCHAR(n)`
- `TEXT`

### Дата и время

- `DATE`
- `TIME`
- `DATETIME`
- `TIMESTAMP`

### Двоичные

- `BINARY`
- `VARBINARY`
- `BLOB`

### Логические

- `BOOLEAN` или `BIT`


