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
