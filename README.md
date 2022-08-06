# Stepik-SQL
Интерактивный тренажер по SQL


1. Вывести информацию о всех книгах, хранящихся на складе.
```sql
SELECT * FROM book;
```
2. Выбрать авторов, название книг и их цену из таблицы book.
```sql
SELECT author, title, price  FROM book;
```
3. Выбрать названия книг и авторов из таблицы book, для поля title задать имя(псевдоним) Название, для поля author –  Автор. 
```sql
SELECT title AS Название, author AS Автор
FROM book;
```
