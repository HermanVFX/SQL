# SQL
____
## Задание 1
Вывести имена всех людей, которые есть в базе данных авиакомпаний
```SQL
SELECT name
FROM Passenger
```
## Задание 2
Вывести названия всеx авиакомпаний
```SQL
SELECT name
FROM Company
```
## Задание 3
Вывести все рейсы, совершенные из Москвы
```SQL
SELECT *
FROM Trip
WHERE town_from = 'Moscow'
```
## Задание 4
Вывести имена людей, которые заканчиваются на "man"
```SQL
SELECT name
FROM Passenger
WHERE name LIKE '%man'
```
## Задание 5
Вывести количество рейсов, совершенных на TU-134
```SQL
SELECT COUNT(plane) as count
FROM Trip
WHERE plane = 'TU-134'
```
## Задание 6
Какие компании совершали перелеты на Boeing
```SQL
SELECT DISTINCT name
FROM Company
  JOIN Trip ON Company.id = Trip.company
WHERE plane = 'Boeing'
```
## Задание 7
Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
```SQL
SELECT DISTINCT plane
FROM Trip
WHERE town_to = 'Moscow'
```
## Задание 8
В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
```SQL
SELECT town_to,
  TIMEDIFF(time_in, time_out) AS flight_time
FROM Trip
WHERE town_from = 'Paris'
```
## Задание 9
Какие компании организуют перелеты с Владивостока (Vladivostok)?
```SQL
SELECT DISTINCT name
FROM Company
  JOIN Trip ON Company.id = Trip.company
WHERE town_from = 'Vladivostok'
```
## Задание 10
Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.
```SQL
SELECT *
FROM Trip
WHERE time_out >= '1900-01-01 10:00:00'
  AND time_out <= '1900-01-01 14:00:00'
```
## Задание 11
Вывести пассажиров с самым длинным именем
```SQL
SELECT name
FROM Passenger
WHERE LENGTH(name) = (
    SELECT MAX(LENGTH(name))
    FROM Passenger
  )
```
## Задание 12
Вывести id и количество пассажиров для всех прошедших полётов
```SQL
SELECT trip,
  COUNT(passenger) as count
FROM Pass_in_trip
GROUP BY trip
```
## Задание 13
Вывести имена людей, у которых есть полный тёзка среди пассажиров
```SQL
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(name) > 1
```
## Задание 14
В какие города летал Bruce Willis
```SQL
SELECT town_to
FROM Trip
  JOIN Pass_in_trip ON trip = Trip.id
  JOIN Passenger ON Passenger.id = passenger
WHERE Passenger.name = 'Bruce Willis'
```
## Задание 15
Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)
```SQL
SELECT time_in
FROM Trip
  JOIN Pass_in_trip ON trip = Trip.id
  JOIN Passenger ON Passenger.id = passenger
WHERE Passenger.name = 'Steve Martin'
  AND town_to = 'London'
```
## Задание 16
Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.
```SQL
SELECT name,
  COUNT(*) as count
FROM Pass_in_trip
  JOIN Passenger ON Passenger.id = Pass_in_trip.passenger
GROUP BY passenger
HAVING COUNT(trip) > 0
ORDER BY COUNT(trip) DESC,
  name
```
## Задание 17
Определить, сколько потратил в 2005 году каждый из членов семьи
```SQL
SELECT member_name,
  status,
  SUM(amount * unit_price) AS costs
FROM Payments
  JOIN FamilyMembers ON FamilyMembers.member_id = Payments.family_member
WHERE date LIKE '%2005%'
GROUP BY family_member
```
## Задание 18
Узнать, кто старше всех в семьe
```SQL
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
    SELECT MIN(birthday)
    FROM FamilyMembers
  )
```
## Задание 19
Определить, кто из членов семьи покупал картошку (potato)
```SQL
SELECT DISTINCT status
FROM FamilyMembers
  JOIN Payments ON Payments.family_member = member_id
  JOIN Goods ON Goods.good_id = Payments.good
WHERE good_name = 'potato'
```
## Задание 20
Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму
```SQL
SELECT status,
  member_name,
  Payments.amount * Payments.unit_price AS costs
FROM FamilyMembers
  JOIN Payments ON Payments.family_member = FamilyMembers.member_id
  JOIN Goods ON Goods.good_id = Payments.good
  JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE good_type_name = 'entertainment'
```
## Задание 21
Определить товары, которые покупали более 1 раза
```SQL
SELECT good_name
FROM Payments
  JOIN Goods ON Goods.good_id = Payments.good
GROUP BY good
HAVING COUNT(good) > 1
```
## Задание 22
Найти имена всех матерей (mother)
```SQL
SELECT member_name
From FamilyMembers
WHERE status = 'mother'
```
## Задание 23
Найдите самый дорогой деликатес (delicacies) и выведите его стоимость
```SQL
SELECT good_name,
  unit_price
FROM Payments
  JOIN Goods ON Goods.good_id = Payments.good
  JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1
```
## Задание 24
Определить кто и сколько потратил в июне 2005
```SQL
SELECT member_name,
  SUM(amount * unit_price) AS costs
FROM FamilyMembers
  JOIN Payments ON Payments.family_member = FamilyMembers.member_id
WHERE YEAR(date) = 2005
  AND MONTH(date) = 06
GROUP BY member_name
```
## Задание 25
Определить, какие товары не покупались в 2005 году
```SQL
SELECT good_name
FROM Goods
WHERE good_name NOT IN (
    SELECT DISTINCT good_name
    FROM Payments
      JOIN Goods ON Goods.good_id = Payments.good
    WHERE YEAR(date) = 2005
  )
```
## Задание 26
Определить группы товаров, которые не приобретались в 2005 году
```SQL
SELECT good_type_name
FROM GoodTypes
WHERE good_type_name NOT IN (
    SELECT DISTINCT good_type_name
    FROM Payments
      JOIN Goods ON Goods.good_id = Payments.good
      JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
    WHERE YEAR(date) = 2005
  )
```
## Задание 27
Узнать, сколько потрачено на каждую из групп товаров в 2005 году. Вывести название группы и сумму
```SQL
SELECT good_type_name,
  SUM(amount * unit_price) AS costs
FROM GoodTypes
  JOIN Goods ON Goods.type = GoodTypes.good_type_id
  JOIN Payments ON Payments.good = Goods.good_id
WHERE YEAR(date) = 2005
GROUP BY good_type_id
```
## Задание 28
Сколько рейсов совершили авиакомпании с Ростова (Rostov) в Москву (Moscow) ?
```SQL
SELECT COUNT(company) AS count
FROM Trip
WHERE town_from = 'Rostov'
  AND town_to = 'Moscow'
```
## Задание 29
Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134
```SQL
SELECT DISTINCT Passenger.name
FROM Trip
  JOIN Pass_in_trip ON trip = Trip.id
  JOIN Passenger ON Passenger.id = Pass_in_trip.passenger
WHERE town_to = 'Moscow'
  AND plane = 'TU-134'
```
## Задание 30
Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.
```SQL
SELECT trip,
  COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC
```
## Задание 31
Вывести всех членов семьи с фамилией Quincey.
```SQL
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '% Quincey'
```
## Задание 32
Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.
```SQL
-- SELECT ROUND(AVG(YEAR(TIMEDIFF(now(), birthday))) AS age FROM FamilyMembers
SELECT ROUND(AVG(TIMESTAMPDIFF(YEAR, birthday, now()))) AS age
FROM FamilyMembers
```
## Задание 33
Найдите среднюю стоимость икры. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar).
```SQL
SELECT AVG(unit_price) AS cost
FROM Payments
  JOIN Goods ON Goods.good_id = Payments.good
WHERE good_name = 'red caviar'
  OR good_name = 'black caviar'
```
## Задание 34
Сколько всего 10-ых классов
```SQL
SELECT COUNT(id) as count
FROM Class
WHERE name LIKE '10%'
```
## Задание 35
Сколько различных кабинетов школы использовались 2.09.2019 в образовательных целях ?
```SQL
SELECT COUNT(classroom) AS count
FROM Schedule
WHERE date = '2019-09-02'
```
## Задание 36
Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?
```SQL
SELECT *
FROM Student
WHERE address LIKE '%Pushkina%'
```
## Задание 37
Сколько лет самому молодому обучающемуся ?
```SQL
SELECT TIMESTAMPDIFF(YEAR, birthday, now()) AS year
FROM Student
ORDER BY year ASC
LIMIT 1
```
## Задание 38
Сколько Анн (Anna) учится в школе ?
```SQL
SELECT COUNT(first_name) as count
FROM Student
WHERE first_name LIKE 'Anna%'
```
## Задание 39
Сколько обучающихся в 10 B классе ?
```SQL
SELECT COUNT(student) AS count
FROM Student_in_class
  JOIN Class ON Class.id = Student_in_class.class
WHERE Class.name LIKE '10 B'
```
## Задание 40
Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.) ?
```SQL
SELECT Subject.name AS subjects
FROM Schedule
  JOIN Subject ON Subject.id = Schedule.subject
  JOIN Teacher ON Teacher.id = Schedule.teacher
WHERE last_name = 'Romashkin'
  AND first_name LIKE 'P%'
```
## Задание 41
Во сколько начинается 4-ый учебный предмет по расписанию ?
```SQL
SELECT DISTINCT start_pair
FROM Timepair
  JOIN Schedule ON Schedule.number_pair = Timepair.id
WHERE number_pair = 4
```
## Задание 42
Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет ?
```SQL
SELECT DISTINCT TIMEDIFF(
    (
      SELECT end_pair
      FROM Timepair
      WHERE id = 4
    ),
(
      SELECT start_pair
      FROM Timepair
      WHERE id = 2
    )
  ) AS time
FROM Timepair
```
## Задание 43
Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отcортируйте преподавателей по фамилии.
```SQL
SELECT last_name
FROM Teacher
  JOIN Schedule ON Schedule.teacher = Teacher.id
  JOIN Subject ON Subject.id = Schedule.subject
WHERE Subject.name = 'Physical Culture'
ORDER BY last_name
```
## Задание 44
Найдите максимальный возраст (колич. лет) среди обучающихся 10 классов ?
```SQL
SELECT MAX(TIMESTAMPDIFF(YEAR, birthday, now())) AS max_year
FROM Student
  JOIN Student_in_class ON Student_in_class.student = Student.id
  JOIN Class ON Class.id = Student_in_class.class
WHERE Class.name LIKE '10 %'
```
## Задание 45
Какой(ие) кабинет(ы) пользуются самым большим спросом?
```SQL
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
    SELECT COUNT(classroom) AS count
    FROM Schedule
    GROUP BY classroom
    ORDER BY count DESC
    LIMIT 1
  )
```
## Задание 46
В каких классах введет занятия преподаватель "Krauze" ?
```SQL
SELECT DISTINCT name
FROM Class
  JOIN Schedule ON Schedule.class = Class.id
  JOIN Teacher ON Teacher.id = Schedule.teacher
WHERE Teacher.last_name = 'Krauze'
```
## Задание 47
Сколько занятий провел Krauze 30 августа 2019 г.?
```SQL
SELECT COUNT(date) AS count
FROM Schedule
  JOIN Teacher ON Teacher.id = Schedule.teacher
WHERE last_name = 'Krauze'
  AND date LIKE '2019-08-30%'
GROUP BY date
```
## Задание 48
Выведите заполненность классов в порядке убывания
```SQL
SELECT name,
  COUNT(student) AS count
FROM Class
  JOIN Student_in_class ON Student_in_class.class = Class.id
GROUP BY name
ORDER BY count DESC
```
## Задание 49
Какой процент обучающихся учится в 10 A классе ?
```SQL
SELECT COUNT(student) / (
    SELECT COUNT(student)
    FROM Student_in_class
  ) * 100 AS percent
FROM Student_in_class
  JOIN Class ON Class.id = Student_in_class.class
WHERE name = '10 A'
```
## Задание 50
Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.
```SQL
SELECT ROUND(
    COUNT(student) / (
      SELECT COUNT(student)
      FROM Student_in_class
    ) * 100
  ) AS percent
FROM Student_in_class
  JOIN Student ON Student.id = Student_in_class.student
WHERE YEAR(birthday) = 2000
```
## Задание 51
Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).
```SQL
INSERT INTO Goods
SELECT COUNT(*) + 1,
  'Cheese',
  2
FROM Goods;
```
## Задание 52
Добавьте в список типов товаров (GoodTypes) новый тип "auto".
```SQL
INSERT INTO GoodTypes
SELECT COUNT(*) + 1,
  'auto'
FROM GoodTypes
```
## Задание 53
Измените имя "Andie Quincey" на новое "Andie Anthony".
```SQL
UPDATE FamilyMembers
SET member_name = "Andie Anthony"
WHERE member_name = "Andie Quincey"
```
## Задание 54
Удалить всех членов семьи с фамилией "Quincey".
```SQL
DELETE FROM FamilyMembers
WHERE member_name LIKE '% Quincey'
```
## Задание 55
Удалить компании, совершившие наименьшее количество рейсов.
```SQL
DELETE FROM Company
WHERE Company.id IN (
    SELECT company
    FROM Trip
    GROUP BY company
    HAVING COUNT(company) = (
        SELECT COUNT(company) AS count
        FROM Trip
        GROUP BY company
        LIMIT 1
      )
  )
```
## Задание 56
Удалить все перелеты, совершенные из Москвы (Moscow).
```SQL
DELETE FROM Trip
WHERE town_from = 'Moscow'
```
## Задание 57
Перенести расписание всех занятий на 30 мин. вперед.
```SQL
UPDATE Timepair
SET start_pair = DATE_ADD(start_pair, INTERVAL 30 MINUTE),
  end_pair = DATE_ADD(end_pair, INTERVAL 30 MINUTE)
```
## Задание 58
Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney"
```SQL
INSERT INTO Reviews
SELECT COUNT(*) + 1,
  (
    SELECT Reservations.id
    FROM Reservations
      JOIN Users ON Users.id = Reservations.user_id
      JOIN Rooms ON Rooms.id = Reservations.room_id
    WHERE address = '11218, Friel Place, New York'
      AND Users.name = 'George Clooney'
  ),
  5
FROM Reviews
```
## Задание 59
Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375.
```SQL
SELECT *
FROM Users
WHERE phone_number LIKE '+375 %'
```
## Задание 60
Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.
```SQL
SELECT teacher
FROM (
    SELECT teacher,
      COUNT(DISTINCT name) AS count
    FROM Schedule
      JOIN Class ON Class.id = Schedule.class
    WHERE Class.name LIKE '11%'
    GROUP BY teacher
  ) AS res
WHERE count > 1
```
## Задание 61
Выведите список комнат, которые были зарезервированы в течение 12 недели 2020 года.
```SQL
SELECT Rooms.*
FROM Reservations
  JOIN Rooms ON Rooms.id = Reservations.room_id
WHERE WEEK(start_date, 1) = 12
  AND YEAR(start_date) = 2020
```
## Задание 62
Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён.
```SQL
SELECT domain,
  COUNT(domain) AS count
FROM (
    SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain
    FROM Users
  ) AS asd
GROUP BY domain
ORDER BY count DESC,
  domain
```
## Задание 63
Выведите отсортированный список (по возрастанию) имен студентов в виде Фамилия.И.О.
```SQL
SELECT CONCAT(
    last_name,
    '.',
    LEFT(first_name, 1),
    '.',
    LEFT(middle_name, 1),
    '.'
  ) AS name
FROM Student
ORDER BY name
```
## Задание 64
Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат отсортируйте в порядке возрастания даты бронирования.
```SQL
SELECT YEAR(start_date) AS year,
  MONTH(start_date) AS month,
  COUNT(*) AS amount
FROM Reservations
GROUP BY year,
  month
ORDER BY year,
  month
```
## Задание 65
Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз.
```SQL
SELECT room_id,
  FLOOR(AVG(Reviews.rating)) AS rating
FROM Reservations
  JOIN Reviews ON Reviews.reservation_id = Reservations.id
GROUP BY room_id
```
## Задание 66
Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат.
```SQL
SELECT home_type,
  address,
  IFNULL(SUM(DATEDIFF(end_date, start_date)), 0) AS days,
  IFNULL(SUM(total), 0) AS total_fee
FROM Rooms
  LEFT JOIN Reservations ON Reservations.room_id = Rooms.id
WHERE has_tv = 1
  AND has_kitchen = 1
  AND has_internet = 1
  AND has_air_con = 1
GROUP BY Rooms.id
```
## Задание 67 
Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты с ведущим нулем, а день и месяц без.
```SQL
SELECT CONCAT(
    DATE_FORMAT(time_out, '%H:%i, %e.%c'),
    ' - ',
    DATE_FORMAT(time_in, '%H:%i, %e.%c')
  ) AS flight_time
FROM Trip
```
## Задание 68
Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату, когда он выехал
```SQL
SELECT asd.room_id,
  name,
  asd.end_date
FROM (
    SELECT room_id,
      MAX(end_date) AS end_date
    FROM Reservations
    GROUP BY room_id
  ) AS asd
  JOIN Reservations ON Reservations.room_id = asd.room_id
  AND asd.end_date = Reservations.end_date
  JOIN Users ON Reservations.user_id = Users.id
ORDER BY room_id
```
## Задание 69
Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они заработали
```SQL
SELECT owner_id,
  IFNULL(SUM(total), 0) AS total_earn
FROM Rooms
  LEFT JOIN Reservations ON Reservations.room_id = Rooms.id
GROUP BY owner_id
ORDER BY owner_id 
```
```SQL
SELECT owner_id,
     SUM(total_earn) AS total_earn
   FROM (
       SELECT room_id,
          IFNULL(SUM(total), 0) AS total_earn
       FROM Rooms
         LEFT JOIN Reservations ON Reservations.room_id = Rooms.id
       GROUP BY room_id
     ) AS asd
     JOIN Rooms ON asd.room_id = Rooms.id
   GROUP BY owner_id
   ORDER BY owner_id
```
## Задание 70
Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100, 100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в данную категорию
```SQL
SELECT 'economy' AS category,
  COUNT(id) AS count
FROM Rooms
WHERE price <= 100
UNION
SELECT 'comfort' AS category,
  COUNT(id) AS count
FROM Rooms
WHERE price > 100
  AND price < 200
UNION
SELECT 'premium' AS category,
  COUNT(id) AS count
FROM Rooms
WHERE price >= 200
```
## Задание 71
Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в аренду жилье. Результат округлите до сотых.
```SQL
WITH res AS (
  SELECT DISTINCT user_id
  FROM Reservations
  UNION
  SELECT DISTINCT owner_id
  FROM Rooms
    JOIN Reservations ON Reservations.room_id = Rooms.id
)
SELECT ROUND(
    (
      SELECT COUNT(*)
      FROM res
    ) / COUNT(id) * 100,
    2
  ) AS percent
FROM Users
```
