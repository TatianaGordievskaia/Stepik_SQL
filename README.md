# Stepik-SQL
Интерактивный тренажер по SQL

# 1.1 Отношение (таблица)
1. Сформулируйте SQL запрос для создания таблицы book, занесите  его в окно кода (расположено ниже)  и отправьте на проверку (кнопка Отправить). Структура таблицы book:

| Поле | Тип, описание |
| ------ | ------ |
| book_id | INT PRIMARY KEY AUTO_INCREMENT |
| title | VARCHAR(50) |
| author | VARCHAR(30) |
| price | DECIMAL(8, 2) |
| amount | INT |
```sql
CREATE TABLE book (
book_id INT PRIMARY KEY AUTO_INCREMENT,
title VARCHAR(50),
author VARCHAR(30),
price DECIMAL(8, 2),              
amount INT);
```
2. Занесите новую строку в таблицу book (текстовые значения (тип VARCHAR) заключать либо в двойные, либо в одинарные кавычки):

| book_id | title | author | price | amount |
| ------ | ------ | ------ | ------ | ------ |
| INT PRIMARY KEY AUTO_INCREMENT | VARCHAR(50) | VARCHAR(30) | DECIMAL(8,2) | INT |
| 1 | Мастер и Маргарита | Булгаков М.А. | 670.99 | 3 |

```sql
INSERT INTO  book (book_id, title, author, price, amount) 
VALUES (1, 'Мастер и Маргарита', 'Булгаков М.А.', 670.99, 3);
```
3. Занесите три последние записи в таблицуbook,  первая запись уже добавлена на предыдущем шаге:

| book_id | title | author | price | amount |
| ------ | ------ | ------ | ------ | ------ |
| INT PRIMARY KEY AUTO_INCREMENT | VARCHAR(50) | VARCHAR(30) | DECIMAL(8,2) | INT |
| 1 | Мастер и Маргарита | Булгаков М.А. | 670.99 | 3 |
| 2 | Белая гвардия | Булгаков М.А. | 540.50 | 5 |
| 3 | Идиот  | Достоевский Ф.М. | 460.00 | 10 |
| 4 | Братья Карамазовы  | Достоевский Ф.М. | 799.01 | 2 |

```sql
INSERT INTO  book (book_id, title, author, price, amount) 
VALUES (2, 'Белая гвардия', 'Булгаков М.А.', 540.50, 5);
INSERT INTO  book (book_id, title, author, price, amount) 
VALUES (3, 'Идиот', 'Достоевский Ф.М.', 460.00, 10);
INSERT INTO  book (book_id, title, author, price, amount) 
VALUES (4, 'Братья Карамазовы', 'Достоевский Ф.М.', 799.01, 2);
SELECT * FROM book;
```
# 1.2 Выборка данных
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
4. Для упаковки каждой книги требуется один лист бумаги, цена которого 1 рубль 65 копеек. Посчитать стоимость упаковки для каждой книги (сколько денег потребуется, чтобы упаковать все экземпляры книги). В запросе вывести название книги, ее количество и стоимость упаковки, последний столбец назвать pack. 
```sql
SELECT title, amount, 
1.65 * amount AS pack 
FROM book;
```
5. В конце года цену всех книг на складе пересчитывают – снижают ее на 30%. Написать SQL запрос, который из таблицы book выбирает названия, авторов, количества и вычисляет новые цены книг. Столбец с новой ценой назвать new_price, цену округлить до 2-х знаков после запятой.
```sql
SELECT title, author , amount,
ROUND(price*0.7,2) AS new_price 
FROM book;
```
6. При анализе продаж книг выяснилось, что наибольшей популярностью пользуются книги Михаила Булгакова, на втором месте книги Сергея Есенина. Исходя из этого решили поднять цену книг Булгакова на 10%, а цену книг Есенина - на 5%. Написать запрос, куда включить автора, название книги и новую цену, последний столбец назвать new_price. Значение округлить до двух знаков после запятой.
```sql
SELECT author, title, 
   ROUND(IF(author = 'Булгаков М.А.' , price*1.1, IF(author = 'Есенин С.А.', price*1.05, price)), 2) AS new_price
FROM book;
```
7. Вывести автора, название  и цены тех книг, количество которых меньше 10.
```sql
SELECT author, title, price 
FROM book
WHERE amount < 10;
```
8. Вывести название, автора,  цену  и количество всех книг, цена которых меньше 500 или больше 600, а стоимость всех экземпляров этих книг больше или равна 5000.
```sql
SELECT title, author, price, amount 
FROM book
WHERE (price > 600 OR price < 500) AND price*amount>=5000 ;
```
9. Вывести название и авторов тех книг, цены которых принадлежат интервалу от 540.50 до 800 (включая границы),  а количество или 2, или 3, или 5, или 7 .
```sql
SELECT title, author  
FROM book
WHERE amount IN (2, 3, 5, 7)
AND price BETWEEN 540.50 AND 800;
```
10. Вывести  автора и название  книг, количество которых принадлежит интервалу от 2 до 14 (включая границы). Информацию  отсортировать сначала по авторам (в обратном алфавитном порядке), а затем по названиям книг (по алфавиту).
```sql
SELECT author, title
FROM book
WHERE amount BETWEEN 2 AND 14
ORDER BY author DESC, title;
```
11. Вывести название и автора тех книг, название которых состоит из двух и более слов, а инициалы автора содержат букву «С». Считать, что в названии слова отделяются друг от друга пробелами и не содержат знаков препинания, между фамилией автора и инициалами обязателен пробел, инициалы записываются без пробела в формате: буква, точка, буква, точка. Информацию отсортировать по названию книги в алфавитном порядке.
```sql
SELECT title, author 
FROM book 
WHERE title LIKE "_% _%" 
AND author LIKE '%С.%'
ORDER BY title 
```
12. Придумайте один или несколько запросов к нашей таблице book. Проверьте, правильно ли они работают.
```sql
SELECT title, author  
FROM book
WHERE amount IN (3, 15)
```
# 1.3 Запросы, групповые операции
1. Отобрать различные (уникальные) элементы столбца amount таблицы book.
```sql
SELECT DISTINCT amount
FROM book;
```
2. Посчитать, количество различных книг и количество экземпляров книг каждого автора , хранящихся на складе.  Столбцы назвать Автор, Различных_книг и Количество_экземпляров соответственно.
```sql
SELECT author AS Автор, COUNT(title) AS Различных_книг, SUM(amount) AS Количество_экземпляров
FROM book
GROUP BY author;
```
3. Вывести фамилию и инициалы автора, минимальную, максимальную и среднюю цену книг каждого автора . Вычисляемые столбцы назвать Минимальная_цена, Максимальная_цена и Средняя_цена соответственно.
```sql
SELECT author, MIN(price) AS Минимальная_цена, MAX(price) AS Максимальная_цена, AVG(price) AS Средняя_цена
FROM book
GROUP BY author;
```
4. Для каждого автора вычислить суммарную стоимость книг S (имя столбца Стоимость), а также вычислить налог на добавленную стоимость  для полученных сумм (имя столбца НДС ) , который включен в стоимость и составляет k = 18%,  а также стоимость книг  (Стоимость_без_НДС) без него. Значения округлить до двух знаков после запятой. В запросе для расчета НДС(tax)  и Стоимости без НДС(S_without_tax) использовать следующие формулы:


$$tax = \frac { S * \frac { k }{ 100 }}{ 1 + \frac { k }{ 100 }}$$

$${S \text{without} \text{tax}} = \frac { S }{ 1 + \frac { k }{ 100 }}$$ 

```sql
SELECT author, 
    SUM(price*amount) AS Стоимость, 
    ROUND(SUM(price*amount)*0.18 / 1.18,2) AS НДС,
    ROUND(SUM(price*amount) / 1.18,2) AS Стоимость_без_НДС
FROM book
GROUP BY author;
```
5. Вывести  цену самой дешевой книги, цену самой дорогой и среднюю цену уникальных книг на складе. Названия столбцов Минимальная_цена, Максимальная_цена, Средняя_цена соответственно. Среднюю цену округлить до двух знаков после запятой.
```sql
SELECT MIN(price) AS Минимальная_цена, MAX(price) AS Максимальная_цена, ROUND(AVG(price),2) AS Средняя_цена 
FROM book;
```
6. Вычислить среднюю цену и суммарную стоимость тех книг, количество экземпляров которых принадлежит интервалу от 5 до 14, включительно. Столбцы назвать Средняя_цена и Стоимость, значения округлить до 2-х знаков после запятой.
```sql
SELECT ROUND(AVG(price),2) AS Средняя_цена, SUM(price*amount) AS Стоимость
FROM book
WHERE amount BETWEEN 5 AND 14;
```
7. Посчитать стоимость всех экземпляров каждого автора без учета книг «Идиот» и «Белая гвардия». В результат включить только тех авторов, у которых суммарная стоимость книг (без учета книг «Идиот» и «Белая гвардия») более 5000 руб. Вычисляемый столбец назвать Стоимость. Результат отсортировать по убыванию стоимости.
```sql
SELECT author, SUM(price*amount) AS Стоимость
FROM book
WHERE title NOT IN('Идиот', 'Белая гвардия')
GROUP BY author 
HAVING SUM(price*amount) > 5000
ORDER BY SUM(price*amount) DESC;
```
8. Придумайте один или несколько запросов к нашей таблице book, используя групповые функции. Проверьте, правильно ли они работают.
```sql
SELECT author
FROM book
WHERE price NOT IN(799.01, 	540.50) 
GROUP BY author;
```
# 1.4 Вложенные запросы
1. Вывести информацию (автора, название и цену) о  книгах, цены которых меньше или равны средней цене книг на складе. Информацию вывести в отсортированном по убыванию цены виде. Среднее вычислить как среднее по цене книги.
```sql
SELECT  author, title, price
FROM book
WHERE price <= (SELECT AVG(price) 
FROM book)
ORDER BY price DESC;
```
2. Вывести информацию (автора, название и цену) о тех книгах, цены которых превышают минимальную цену книги на складе не более чем на 150 рублей в отсортированном по возрастанию цены виде.
```sql
SELECT author, title, price
FROM book
WHERE price <= ABS((SELECT MIN(price) FROM book)+150) 
ORDER BY price
```
3. Вывести информацию (автора, книгу и количество) о тех книгах, количество экземпляров которых в таблице book не дублируется.
```sql
SELECT  author, title, amount
FROM book
WHERE amount IN( SELECT amount
FROM book
GROUP BY amount                
HAVING COUNT(amount)=1) 
```
4. Вывести информацию о книгах(автор, название, цена), цена которых меньше самой большой из минимальных цен, вычисленных для каждого автора.
```sql
SELECT  author, title, price
FROM book 
WHERE price < ANY( SELECT MIN(price) FROM book
GROUP BY author)
```
5. Посчитать сколько и каких экземпляров книг нужно заказать поставщикам, чтобы на складе стало одинаковое количество экземпляров каждой книги, равное значению самого большего количества экземпляров одной книги на складе. Вывести название книги, ее автора, текущее количество экземпляров на складе и количество заказываемых экземпляров книг. Последнему столбцу присвоить имя Заказ. В результат не включать книги, которые заказывать не нужно.
```sql
SELECT title, author, amount, (SELECT MAX(amount) FROM book)-amount AS Заказ
FROM book
WHERE amount<>(SELECT MAX(amount) FROM book)
```
6. Придумайте один или несколько запросов к нашей таблице book, используя вложенные запросы. Проверьте, правильно ли они работают.
```sql
SELECT title, amount, price 
FROM book
WHERE abs(1-(price/(SELECT AVG(price) FROM book)))<0.3
```
# 1.5 Запросы корректировки данных
1. Создать таблицу поставка (supply), которая имеет ту же структуру, что и таблиц book.

| Поле | Тип, описание |
| ------ | ------ |
| supply_id | INT PRIMARY KEY AUTO_INCREMENT |
| title | VARCHAR(50) |
| author | VARCHAR(30) |
| price | DECIMAL(8, 2) |
| amount | INT |
```sql
CREATE TABLE supply (
supply_id INT PRIMARY KEY AUTO_INCREMENT,
title VARCHAR(50),
author VARCHAR(30),
price DECIMAL(8, 2),              
amount INT);
```
2. Занесите в таблицу supply четыре записи, чтобы получилась следующая таблица:

| supply_id | title | author | price | amount |
| ------ | ------ | ------ | ------ | ------ | 
| 1 | Лирика | Пастернак Б.Л. | 518.99 | 2 |
| 2 | Черный человек | Есенин С.А. | 570.20 | 6 |
| 3 | Белая гвардия | Булгаков М.А. | 540.50 | 7 |
| 4 | Идиот | Достоевский Ф.М. | 360.80 | 3 |

```sql
INSERT INTO supply (supply_id, title, author, price, amount) 
VALUES 
    (1, 'Лирика','Пастернак Б.Л.', 518.99, 2),
    (2, 'Черный человек','Есенин С.А.', 570.20, 6),
    (3, 'Белая гвардия','Булгаков М.А.', 540.50, 7),
    (4, 'Идиот', 'Достоевский Ф.М.', 360.80, 3);
SELECT * FROM supply;
```
3. Добавить из таблицы supply в таблицу book, все книги, кроме книг, написанных Булгаковым М.А. и Достоевским Ф.М.
```sql
INSERT INTO book (title, author, price, amount) 
SELECT title, author, price, amount 
FROM supply
WHERE author NOT IN('Булгаков М.А.', 'Достоевский Ф.М.');
SELECT * FROM book;
```
4. Занести из таблицы supply в таблицу book только те книги, авторов которых нет в  book.
```sql
INSERT INTO book (title, author, price, amount) 
SELECT title, author, price, amount 
FROM supply
WHERE author NOT IN (
        SELECT author 
        FROM book);
SELECT * FROM book;
```
5. Уменьшить на 10% цену тех книг в таблице book, количество которых принадлежит интервалу от 5 до 10, включая границы.
```sql
UPDATE book 
SET price = 0.9 * price 
WHERE amount BETWEEN 5 AND 10;
SELECT * FROM book;
```
6. В таблице book необходимо скорректировать значение для покупателя в столбце buy таким образом, чтобы оно не превышало количество экземпляров книг, указанных в столбце amount. А цену тех книг, которые покупатель не заказывал, снизить на 10%.
```sql
UPDATE book
SET buy = IF(buy > amount, amount, buy), price = IF(buy=0, 0.9*price, price);
SELECT * FROM book
```
7. Для тех книг в таблице book , которые есть в таблице supply, не только увеличить их количество в таблице book ( увеличить их количество на значение столбца amount таблицы supply), но и пересчитать их цену (для каждой книги найти сумму цен из таблиц book и supply и разделить на 2).
```sql
UPDATE book, supply 
SET book.amount = book.amount + supply.amount,
book.price = (book.price + supply.price)/2
WHERE book.title = supply.title AND book.author = supply.author;
SELECT * FROM book;
```
8. Удалить из таблицы supply книги тех авторов, общее количество экземпляров книг которых в таблице book превышает 10.
```sql
DELETE FROM supply 
WHERE author IN(
     SELECT author
     FROM book
     GROUP BY author
     HAVING SUM(amount) > 10);
SELECT * FROM supply;
```
9. Создать таблицу заказ (ordering), куда включить авторов и названия тех книг, количество экземпляров которых в таблице book меньше среднего количества экземпляров книг в таблице book. В таблицу включить столбец   amount, в котором для всех книг указать одинаковое значение - среднее количество экземпляров книг в таблице book.
```sql
CREATE TABLE ordering AS
SELECT author, title, 
(SELECT ROUND(AVG(amount)) 
FROM book) AS amount
FROM book
WHERE amount < (SELECT AVG(amount) 
FROM book);
SELECT * FROM ordering;
```
10. Придумайте один или несколько запросов корректировки данных к  таблицам book и  supply . Проверьте, правильно ли они работают.
```sql
SELECT author,title,amount,
ROUND(price*IF(amount = (SELECT MAX(amount) 
FROM book),0.95,1),2) as new_price
FROM book
```
# 1.6 Таблица "Командировки", запросы на выборку
1. Вывести из таблицы trip информацию о командировках тех сотрудников, фамилия которых заканчивается на букву «а», в отсортированном по убыванию даты последнего дня командировки виде. В результат включить столбцы name, city, per_diem, date_first, date_last.
```sql
SELECT name, city, per_diem, date_first, date_last
FROM trip
WHERE name LIKE '%а %'
ORDER BY date_last DESC;
```
2. Вывести в алфавитном порядке фамилии и инициалы тех сотрудников, которые были в командировке в Москве.
```sql
SELECT DISTINCT name
FROM trip
WHERE city = 'Москва'
ORDER BY name;
```
3. Для каждого города посчитать, сколько раз сотрудники в нем были.  Информацию вывести в отсортированном в алфавитном порядке по названию городов. Вычисляемый столбец назвать Количество. 
```sql
SELECT city, count(city) AS Количество
FROM trip
GROUP BY city
ORDER BY city;
```
4. Вывести два города, в которых чаще всего были в командировках сотрудники. Вычисляемый столбец назвать Количество.
```sql
SELECT city,  count(city) AS Количество
FROM trip
GROUP BY city
ORDER BY Количество DESC
LIMIT 2;
```
5. Вывести информацию о командировках во все города кроме Москвы и Санкт-Петербурга (фамилии и инициалы сотрудников, город ,  длительность командировки в днях, при этом первый и последний день относится к периоду командировки). Последний столбец назвать Длительность. Информацию вывести в упорядоченном по убыванию длительности поездки, а потом по убыванию названий городов (в обратном алфавитном порядке).
```sql
SELECT name, city, DATEDIFF(date_last, date_first)+1 AS Длительность
FROM trip
WHERE city NOT IN('Москва', 'Санкт-Петербург')
ORDER BY Длительность DESC, city DESC;
```
6. Вывести информацию о командировках сотрудника(ов), которые были самыми короткими по времени. В результат включить столбцы name, city, date_first, date_last.
```sql
SELECT name, city, date_first, date_last
FROM trip
WHERE DATEDIFF(date_last,date_first) = (SELECT MIN(DATEDIFF(date_last,date_first))
FROM trip );
```
7. Вывести информацию о командировках, начало и конец которых относятся к одному месяцу (год может быть любой). В результат включить столбцы name, city, date_first, date_last. Строки отсортировать сначала  в алфавитном порядке по названию города, а затем по фамилии сотрудника .
```sql
SELECT name, city, date_first, date_last
FROM trip
WHERE MONTH(date_first) = MONTH(date_last)
ORDER BY city, name;
```
8. Вывести название месяца и количество командировок для каждого месяца. Считаем, что командировка относится к некоторому месяцу, если она началась в этом месяце. Информацию вывести сначала в отсортированном по убыванию количества, а потом в алфавитном порядке по названию месяца виде. Название столбцов – Месяц и Количество.
```sql
SELECT MONTHNAME(date_first) AS Месяц , count(monthname(date_first)) AS  Количество
FROM trip
GROUP BY Месяц
ORDER BY Количество DESC, Месяц;
```
9. Вывести сумму суточных (произведение количества дней командировки и размера суточных) для командировок, первый день которых пришелся на февраль или март 2020 года. Значение суточных для каждой командировки занесено в столбец per_diem. Вывести фамилию и инициалы сотрудника, город, первый день командировки и сумму суточных. Последний столбец назвать Сумма. Информацию отсортировать сначала  в алфавитном порядке по фамилиям сотрудников, а затем по убыванию суммы суточных.
```sql
SELECT name, city, date_first, DATEDIFF(date_last+1, date_first)*per_diem as Сумма
FROM trip
WHERE YEAR(date_first)=2020 and MONTH(date_first)=3 OR MONTH(date_first)=2
ORDER BY name,Сумма DESC;
```
10. Вывести фамилию с инициалами и общую сумму суточных, полученных за все командировки для тех сотрудников, которые были в командировках больше чем 3 раза, в отсортированном по убыванию сумм суточных виде. Последний столбец назвать Сумма
```sql
SELECT name, SUM((datediff(date_last,date_first) + 1) * per_diem) AS Сумма
FROM trip
GROUP BY name
HAVING COUNT(name) > 3
ORDER BY 2 DESC;
```
# 1.7 Таблица "Нарушения ПДД", запросы корректировки
1. Создать таблицу fine следующей структуры:

| Поле | Описание |
| ------ | ------ |
| fine_id | ключевой столбец целого типа с автоматическим увеличением значения ключа на 1 |
| name | строка длиной 30 |
| number_plate | строка длиной 6 |
| violation | строка длиной 50 |
| sum_fine | вещественное число, максимальная длина 8, количество знаков после запятой 2 |
| date_violation | дата |
| date_payment | дата |

```sql
CREATE TABLE fine
(fine_id int primary key auto_increment,
    name varchar(30),
    number_plate varchar(6),
    violation varchar(30),
    sum_fine decimal(8,2),
    date_violation date,
    date_payment date);
```
2. В таблицу fine первые 5 строк уже занесены. Добавить в таблицу записи с ключевыми значениями 6, 7, 8.
```sql
INSERT INTO fine (fine_id, name, number_plate, violation, sum_fine,date_violation, date_payment)
VALUES (6, 'Баранов П.Е.', 'Р523ВТ', 'Превышение скорости(от 40 до 60)', null, '2020-02-14', null),
       (7, 'Абрамова К.А.', 'О111АВ', 'Проезд на запрещающий сигнал', null, '2020-02-23', null),
       (8, 'Яковлев Г.Р.', 'Т330ТТ', 'Проезд на запрещающий сигнал', null, '2020-03-03', null);
```
3. Занести в таблицу fine суммы штрафов, которые должен оплатить водитель, в соответствии с данными из таблицы traffic_violation. При этом суммы заносить только в пустые поля столбца  sum_fine.

Таблица traffic_violationсоздана и заполнена.
```sql
UPDATE fine, traffic_violation
SET fine.sum_fine = traffic_violation.sum_fine
WHERE fine.violation = traffic_violation.violation AND fine.sum_fine IS NULL;
SELECT * FROM fine;
```
4.
```sql

```
5.
```sql

```
6.
```sql

```
7.
```sql

```






