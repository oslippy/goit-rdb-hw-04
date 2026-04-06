---
# Домашнє завдання до Теми 4. DML та DDL команди. Складні SQL вирази

## 1. Створіть базу даних для керування бібліотекою книг згідно зі структурою, наведеною нижче. Використовуйте DDL-команди для створення необхідних таблиць та їх зв'язків.

**Структура БД**

a) Назва схеми — `LibraryManagement`

b) Таблиця `authors`:

* `author_id (INT, автоматично зростаючий PRIMARY KEY)`

* `author_name (VARCHAR)`

c) Таблиця `genres`:

* `genre_id (INT, автоматично зростаючий PRIMARY KEY)`

* `genre_name (VARCHAR)`

d) Таблиця `books`:

* `book_id (INT, автоматично зростаючий PRIMARY KEY)`

* `title (VARCHAR)`

* `publication_year (YEAR)`

* `author_id (INT, FOREIGN KEY зв'язок з "Authors")`

* `genre_id (INT, FOREIGN KEY зв'язок з "Genres")`

e) Таблиця `users`:

* `user_id (INT, автоматично зростаючий PRIMARY KEY)`

* `username (VARCHAR)`

* `email (VARCHAR)`

f) Таблиця `borrowed_books`:

* `borrow_id (INT, автоматично зростаючий PRIMARY KEY)`

* `book_id (INT, FOREIGN KEY зв'язок з "Books")`

* `user_id (INT, FOREIGN KEY зв'язок з "Users")`

* `borrow_date (DATE)`

* `return_date (DATE)`

~~~~sql
CREATE DATABASE IF NOT EXISTS LibraryManagement;

USE LibraryManagement;

CREATE TABLE authors (
    author_id INT NOT NULL AUTO_INCREMENT,
    author_name VARCHAR(150) NOT NULL,
    PRIMARY KEY (author_id)
);

CREATE TABLE genres (
    genre_id INT NOT NULL AUTO_INCREMENT,
    genre_name VARCHAR(100) NOT NULL,
    PRIMARY KEY (genre_id)
);

CREATE TABLE books (
    book_id INT NOT NULL AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    publication_year YEAR,
    author_id INT,
    genre_id INT,
    PRIMARY KEY (book_id),
    CONSTRAINT fk_books_author
        FOREIGN KEY (author_id) REFERENCES authors (author_id)
        ON UPDATE CASCADE ON DELETE SET NULL,
    CONSTRAINT fk_books_genre
        FOREIGN KEY (genre_id) REFERENCES genres (genre_id)
        ON UPDATE CASCADE ON DELETE SET NULL
);

CREATE TABLE users (
    user_id  INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) NOT NULL,
    email VARCHAR(150),
    PRIMARY KEY (user_id)
);

CREATE TABLE borrowed_books (
    borrow_id INT NOT NULL AUTO_INCREMENT,
    book_id INT NOT NULL,
    user_id INT NOT NULL,
    borrow_date DATE NOT NULL,
    return_date DATE,
    PRIMARY KEY (borrow_id),
    CONSTRAINT fk_bb_book
        FOREIGN KEY (book_id) REFERENCES books (book_id)
        ON UPDATE CASCADE ON DELETE RESTRICT,
    CONSTRAINT fk_bb_user
        FOREIGN KEY (user_id) REFERENCES users (user_id)
        ON UPDATE CASCADE ON DELETE CASCADE
);
~~~~

## Створені таблиці
![created tables 1](https://github.com/oslippy/goit-rdb-hw-04/blob/main/Screenshot%202026-04-06%20at%2019.35.39.png)
![created tables 2](https://github.com/oslippy/goit-rdb-hw-04/blob/main/Screenshot%202026-04-06%20at%2019.36.23.png)

## 2. Заповніть таблиці простими видуманими тестовими даними. Достатньо одного-двох рядків у кожну таблицю.

~~~~sql
INSERT INTO authors (author_name) VALUES
    ('Іван Франко'),
    ('Ліна Костенко');

INSERT INTO genres (genre_name) VALUES
    ('Поезія'),
    ('Роман');

INSERT INTO books (title, publication_year, author_id, genre_id) VALUES
    ('Захар Беркут', 1882, 1, 2),
    ('Маруся Чурай', 1979, 2, 2);

INSERT INTO users (username, email) VALUES
    ('oleksandr_k', 'oleksandr@email.com'),
    ('maria_v',     'maria@email.com');

INSERT INTO borrowed_books (book_id, user_id, borrow_date, return_date) VALUES
    (1, 1, '2024-01-10', '2024-01-20'),
    (2, 2, '2024-02-05', NULL);
~~~~

## 3. Перейдіть до бази даних, з якою працювали у темі 3. Напишіть запит за допомогою операторів FROM та INNER JOIN, що об’єднує всі таблиці даних, які ми завантажили з файлів: order_details, orders, customers, products, categories, employees, shippers, suppliers. Для цього ви маєте знайти спільні ключі. Перевірте правильність виконання запиту.

~~~~sql
USE db_hw;

SELECT *
FROM order_details
    INNER JOIN orders ON order_details.order_id = orders.id
    INNER JOIN customers ON orders.customer_id = customers.id
    INNER JOIN employees ON orders.employee_id = employees.employee_id
    INNER JOIN shippers ON orders.shipper_id = shippers.id
    INNER JOIN products ON order_details.product_id = products.id
    INNER JOIN categories ON products.category_id = categories.id
    INNER JOIN suppliers ON products.supplier_id = suppliers.id;
~~~~

## [Результат запиту (just click)](https://github.com/oslippy/goit-rdb-hw-04/blob/main/joined_result.csv)

## 4. Виконайте запити, перелічені нижче.

* Визначте, скільки рядків ви отримали (за допомогою оператора COUNT).

~~~~sql
SELECT COUNT(*)
FROM order_details
    INNER JOIN orders ON order_details.order_id = orders.id
    INNER JOIN customers ON orders.customer_id = customers.id
    INNER JOIN employees ON orders.employee_id = employees.employee_id
    INNER JOIN shippers ON orders.shipper_id = shippers.id
    INNER JOIN products ON order_details.product_id = products.id
    INNER JOIN categories ON products.category_id = categories.id
    INNER JOIN suppliers ON products.supplier_id = suppliers.id;
~~~~

![count](https://github.com/oslippy/goit-rdb-hw-04/blob/main/Screenshot%202026-04-06%20at%2020.18.18.png)

* Змініть декілька операторів INNER на LEFT чи RIGHT. Визначте, що відбувається з кількістю рядків. Чому? Напишіть відповідь у текстовому файлі.
~~~~sql
SELECT COUNT(*)
FROM order_details
    INNER JOIN orders ON order_details.order_id = orders.id
    LEFT JOIN customers ON orders.customer_id = customers.id
    LEFT JOIN employees ON orders.employee_id = employees.employee_id
    INNER JOIN shippers ON orders.shipper_id = shippers.id
    INNER JOIN products ON order_details.product_id = products.id
    LEFT JOIN categories ON products.category_id = categories.id
    RIGHT JOIN suppliers ON products.supplier_id = suppliers.id;
~~~~

![count after changed type of join](https://github.com/oslippy/goit-rdb-hw-04/blob/main/Screenshot%202026-04-06%20at%2020.18.18.png)

Кількість рядків не змінилась бо дані в CSV (які ми завантажували у БД) чисті та узгоджені — кожен customer_id в orders має відповідний запис у customers, кожен supplier_id у products є в suppliers тощо. Кількість рядків змінилась би, якби, наприклад:

    * замовлення мало customer_id, якого немає в таблиці customers → INNER відкинув би його, LEFT залишив би з NULL
    
    * у suppliers був постачальник без жодного продукту → RIGHT JOIN suppliers додав би його рядок з NULL по всіх інших колонках

  
