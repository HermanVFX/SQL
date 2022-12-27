# SQL
____
## Задание 14
В какие города летал Bruce Willis
```SQL
SELECT town_to
FROM Trip
  JOIN Pass_in_trip ON trip = Trip.id
  JOIN Passenger ON Passenger.id = passenger
WHERE Passenger.name = 'Bruce Willis'
```
##Задание 15
Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)
```SQL
SELECT time_in FROM Trip
    JOIN Pass_in_trip ON trip = Trip.id
    JOIN Passenger ON Passenger.id = passenger 
WHERE Passenger.name = 'Steve Martin' AND town_to = 'London'
```
## Задание 19
Определить, кто из членов семьи покупал картошку (potato)
```SQL
SELECT DISTINCT  status FROM FamilyMembers
    JOIN Payments ON Payments.family_member = member_id
    JOIN Goods ON Goods.good_id = Payments.good 
WHERE good_name = 'potato'
```
## Задание 22
Найти имена всех матерей (mother)
```SQL
SELECT member_name From FamilyMembers
WHERE status = 'mother'
```
