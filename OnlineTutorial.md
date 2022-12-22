# SQL
SQL Academy Lessons

## Содержание
[Общая структура запроса](#Общая_структура_запроса)
[Условный оператор WHERE](#Условный_оператор_WHERE)

## <a name="Общая_структура_запроса">Общая структура запроса</a>

### 1. SELECT по одному столбцу
Выведите поле name из таблицы Passenger.
```SQL
SELECT name FROM Passenger
```
### 2. SELECT по нескольким столбцам
Выведите поля member_id, member_name и status из таблицы FamilyMembers.
```SQL
SELECT member_id, member_name, status FROM FamilyMembers
```
### 3. SELECT по всем столбцам
Выведите все столбцы из таблицы Payments.
```SQL
SELECT * FROM Payments
```
### 4. Получение уникальных записей
Выполните предложенный ниже запрос, чтобы увидеть список пассажиров. Если присмотреться, то можно увидеть, что пассажир с именем Bruce Willis присутствует здесь дважды (в начале и конце списка).
Ваша задача заключается в том, чтобы вывести только уникальные имена пассажиров, дополнив предложенный запрос.
```SQL
SELECT DISTINCT name FROM Passenger
```
____
#### <a name="Условный_оператор_WHERE">Условный оператор WHERE</a>

### 1. Простая фильтрация по числам
Выведите идентификаторы товаров (поле good) из таблицы Payments, стоимость которых больше 2000 единиц. Стоимость товара хранится в поле unit_price.
```SQL
SELECT good FROM Payments WHERE unit_price > 2000
```
### 2. Простая фильтрация по строкам
Выведите имена (поле member_name) членов семьи из таблицы FamilyMembers, чей статус (поле status) равен "father".
```SQL
SELECT member_name FROM FamilyMembers WHERE status  = 'father'
```
### 3. Логическое ИЛИ
Выведите имя (поле member_name) и дату рождения (поле birthday) членов семьи из таблицы FamilyMembers, чей статус (поле status) равен "father" или "mother".
```SQL
SELECT member_name, birthday FROM FamilyMembers
    WHERE status = 'father' OR status = 'mother'  
```
### 4. Логическое И
Необходимо получить все комнаты, в которых есть как кухня (поле has_kitchen), так и интернет (поле has_internet). Напишите запрос, удовлетворяющий вышеописанному условию, который выводит все поля из таблицы Rooms.
Наличие обозначается 1 или true, а отсутствие 0 или false.
```SQL
SELECT * FROM Rooms
    WHERE has_kitchen = true AND has_internet = true
```
### 5. WHERE BETWEEN
Выведите резервации комнат (поля room_id, start_date, end_date) из таблицы Reservations, у которых итоговая стоимость аренды (поле total) находится в промежутке от 500 до 1200 включительно.
```SQL
SELECT room_id ,start_date ,end_date FROM Reservations
    WHERE total BETWEEN 500 AND 1200 
```
### 6. Поиск по строковому шаблону
Выведите всех членов семьи с фамилией "Quincey".
```SQL
SELECT member_name FROM FamilyMembers
    WHERE member_name LIKE '% Quincey'
```
____
## Сортировка, оператор ORDER BY

###








## Многотабличные запросы

### 1. INNER JOIN
Объедините таблицы Class и Student_in_class с помощью внутреннего соединения по полям Class.id и Student_in_class.class. Выведите название класса (поле Class.name) и идентификатор ученика (поле Student_in_class.student).
```SQL
SELECT Class.name, Student_in_class.student FROM Class
    JOIN Student_in_class ON Student_in_class.class = Class.id 
```    
### 2.Многотабличный INNER JOIN
Дополните запрос из предыдущего задания, добавив ещё одно внутреннее соединение с таблицей Student. Объедините по полям Student_in_class.student и Student.id и вместо идентификатора ученика выведите его имя (поле first_name).
```SQL
SELECT Class.name, first_name
  FROM Class
    JOIN Student_in_class
      ON Student_in_class.class = Class.id 
    JOIN Student
      ON Student_in_class.student = Student.id 
```
### 3. Многотабличный INNER JOIN с фильтрацией строк
Выведите названия продуктов, которые покупал член семьи со статусом "son". Для получения выборки вам нужно объединить таблицу Payments с таблицей FamilyMembers по полям family_member и member_id, а также с таблицей Goods по полям good и good_id.
```SQL
SELECT good_name FROM Payments
    JOIN FamilyMembers
        ON Payments.family_member = FamilyMembers.member_id 
    JOIN Goods
        ON Payments.good = Goods.good_id 
    WHERE FamilyMembers.status  = 'son'
```
### 4. INNER JOIN с группировкой
Выведите идентификатор (поле room_id) и среднюю оценку комнаты (поле rating, для вывода используйте псевдоним avg_score), составленную на основании отзывов из таблицы Reviews.
```SQL
SELECT room_id, AVG(rating)  as avg_score
    FROM Reservations
        JOIN Reviews
            ON Reservations.id = Reviews.reservation_id 
    GROUP BY Reservations.room_id 
```
____
## Ограничение выборки

### 1. Ограничение записей с начала таблицы
Отсортируйте список компаний (таблица Company) по их названию в алфавитном порядке и выведите первые две записи.
```SQL
SELECT * FROM Company
    ORDER BY name LIMIT 2
```
### 2. Ограничение количества записей со смещением
Выведите начало (поле start_pair) и окончание (поле end_pair) второго и третьего занятия из таблицы Timepair.
```SQL
SELECT start_pair, end_pair FROM Timepair 
    ORDER BY id LIMIT 1, 2
```
