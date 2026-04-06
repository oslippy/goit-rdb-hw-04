---
# Домашнє завдання до Теми 4. DML та DDL команди. Складні SQL вирази
---

## 1. Створіть базу даних для керування бібліотекою книг згідно зі структурою, наведеною нижче. Використовуйте DDL-команди для створення необхідних таблиць та їх зв'язків.

Структура БД

a) Назва схеми — “LibraryManagement”

b) Таблиця "authors":

author_id (INT, автоматично зростаючий PRIMARY KEY)
author_name (VARCHAR)
c) Таблиця "genres":

genre_id (INT, автоматично зростаючий PRIMARY KEY)
genre_name (VARCHAR)
d) Таблиця "books":

book_id (INT, автоматично зростаючий PRIMARY KEY)
title (VARCHAR)
publication_year (YEAR)
author_id (INT, FOREIGN KEY зв'язок з "Authors")
genre_id (INT, FOREIGN KEY зв'язок з "Genres")
e) Таблиця "users":

user_id (INT, автоматично зростаючий PRIMARY KEY)
username (VARCHAR)
email (VARCHAR)
f) Таблиця "borrowed_books":

borrow_id (INT, автоматично зростаючий PRIMARY KEY)
book_id (INT, FOREIGN KEY зв'язок з "Books")
user_id (INT, FOREIGN KEY зв'язок з "Users")
borrow_date (DATE)
return_date (DATE)
