# SQL
SQL Academy Lessons
Практические задачки из интерактивного учебника по SQL
## Содержание
- [Общая структура запроса](#Общая_структура_запроса)
- [Условный оператор WHERE](#Условный_оператор_WHERE)
- [Сортировка, оператор ORDER BY](#Сортировка_оператор_ORDER_BY)
- [Группировка данных и агрегатные функции](#Группировка_данных_и_агрегатные_функции)
- [Многотабличные запросы](#Многотабличные_запросы)
- [Ограничение выборки](#Ограничение_выборки)
- [Вложенные SQL запросы](#Вложенные_SQL_запросы)
- [Обобщенное табличное выражение, оператор WITH](#Обобщенное_табличное_выражение_оператор_WITH)
- [Условный оператор WHERE](#Условный_оператор_WHERE)
- [Условный оператор WHERE](#Условный_оператор_WHERE)
____
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
## <a name="Сортировка_оператор_ORDER_BY">Сортировка, оператор ORDER BY</a>

### 1. Сортировка по убыванию
Для каждого отдельного платежа выведите идентификатор товара и сумму, потраченную на него, в отсортированном по убыванию этой суммы виде. Список платежей находится в таблице Payments.
Для вывода суммы используйте псевдоним sum.
```SQL
SELECT good, unit_price * amount as sum
    FROM Payments ORDER BY sum DESC
```
### 2. Сортировка по нескольким столбцам
Выведите список членов семьи с фамилией Quincey, в отсортированном по возрастанию столбцам status и member_name виде.
```SQL
SELECT * FROM FamilyMembers
    WHERE member_name LIKE '% Quincey'
        ORDER BY status, member_name
```
____
## <a name="Группировка_данных_и_агрегатные_функции">Группировка данных и агрегатные функции</a>

### 1. Группировка и сортировка
Подсчитайте количество учеников в каждом классе, а также отсортируйте их по убыванию количества учеников. Принадлежность ученика к конкретному классу вы можете получить из таблицы Student_in_class. В качестве результата необходимо вывести идентификатор класса (поле class) и количество учеников в этом классе.
Для вывода количества учеников используйте псевдоним count.
```SQL
SELECT class, COUNT(student ) AS count
    FROM Student_in_class
        GROUP BY class
        ORDER BY count DESC
```
### 2. Агрегатные функции MIN и MAX
Найдите самых старших членов семьи (используйте поле birthday) среди всех существующих семей на основании их статуса (поле status). Выведите статус и дату рождения.
Для вывода даты рождения используйте псевдоним birthday.
```SQL
SELECT status ,MIN(birthday) AS birthday FROM FamilyMembers
    GROUP BY status 
```
### 3. Агрегатная функция AVG
Получите среднее время полётов, совершённых на каждой из моделей самолёта. Выведите поле plane и среднее время полёта в секундах.
Для вывода времени используйте псевдоним time.
Используйте функцию TIMESTAMPDIFF(second, time_out, time_in), чтобы получить разницу во времени в секундах между двумя датами.
```SQL
SELECT plane ,AVG(TIMESTAMPDIFF(second, time_out, time_in)) AS time
    FROM Trip
        GROUP BY plane
```
### 4. Выборка с использованием нескольких агрегатных функций
Выведите идентификатор комнаты (поле room_id), среднюю стоимость за один день аренды (поле price, для вывода используйте псевдоним avg_price), а также количество резерваций этой комнаты (используйте псевдоним count). Полученный результат отсортируйте в порядке убывания сначала по количеству резерваций, а потом по средней стоимости.
```SQL
SELECT room_id, AVG(price) AS avg_price, COUNT(total) AS count
    FROM Reservations GROUP BY room_id 
        ORDER BY count DESC, avg_price DESC
```
### 5. HAVING
Дополните запрос из предыдущего задания, оставив в выборке только те комнаты, чья средняя стоимость аренды превышает 150 ед.
```SQL
SELECT room_id, AVG(price) AS avg_price, COUNT(total) AS count
    FROM Reservations GROUP BY room_id 
        HAVING avg_price > 150
        ORDER BY count DESC, avg_price DESC
```
____
## <a name="Многотабличные_запросы">Многотабличные запросы</a>

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
## <a name="Ограничение_выборки">Ограничение выборки</a>

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
____
## <a name="Вложенные_SQL_запросы">Вложенные SQL запросы</a>

### 1. Скалярные подзапросы
Выведите количество полётов каждого пассажира, представленного в таблице Passenger. Список полётов находится в таблице Pass_in_trip.
В качестве результата выведите количество полётов (используйте псевдоним count) и имя пассажира.
```SQL
SELECT  (SELECT COUNT(trip) FROM Pass_in_trip WHERE passenger  = Passenger.id) as count,
        name
FROM Passenger
```
### 2. Столбцовые подзапросы с выражением IN
Выведите названия товаров из таблицы Goods (поле good_name), которые ещё ни разу не покупались ни одним из членов семьи (таблица Payments).
```SQL
SELECT good_name FROM Goods 
WHERE good_id NOT IN (SELECT good FROM Payments)
```
### 3. Строковые подзапросы
Выведите список комнат (все поля, таблица Rooms), которые по своим удобствам (has_tv, has_internet, has_kitchen, has_air_con) совпадают с комнатой с идентификатором "11".
```SQL
SELECT * FROM Rooms
WHERE (has_tv, has_internet, has_kitchen, has_air_con) =
(SELECT has_tv,
        has_internet,
        has_kitchen,
        has_air_con
FROM Rooms WHERE id = 11)
```
____
## <a name="Обобщенное_табличное_выражение_оператор_WITH">Обобщенное табличное выражение, оператор WITH</a>
___
## <a name="Обобщенное_табличное_выражение_оператор_WITH">Обобщенное табличное выражение, оператор WITH</a>
### 1. Отчёт по тратам
Создать отчет затрат семьи Quincey за 3 квартал 2005 года. Отсортировать по возрастанию member_name, затем по убыванию поля costs.
Последней строкой вывести итог по всей семье. Для этого необходимо под колонкой good_name вывести слово "Total:", а под costs - общую сумму всех затрат, оставив первые два поля пустыми.
```SQL
WITH report 
    AS (
            SELECT  member_name,
                    status,
                    good_name,
                    amount * unit_price AS costs
            FROM FamilyMembers
                JOIN Payments ON Payments.family_member = member_id 
                JOIN Goods ON Goods.good_id = Payments.good 
            WHERE member_name LIKE '%Quincey'
                AND date LIKE "2005-07%" 
                    OR date LIKE "2005-08%"
                    OR date LIKE "2005-09%"
        )
SELECT * FROM report 
UNION 
SELECT NULL, NULL,"Total:",SUM(costs)
FROM report 
ORDER BY CASE WHEN member_name IS NULL THEN 1 ELSE 0 END,member_name, costs DESC

```


