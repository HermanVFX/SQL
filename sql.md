
# SQL

## Основы SQL

SQL делится на два основных подраздела:

- DDL (Data Definition Language) - Создание структуры базы данных с помощью ключевых слов CREATE DROP ALTER

- DML (Data Manipulation Language) - Манипуляция над данными с помощью ключевых слов INSERT DELETE UPDATE SELECT

## DDL
### Создание новой базы данных
```SQL 
CREATE DATABASE my_database;
```
Так же мы можем создать новую схему (пакет) где будут храниться наши таблицы. Обратите внимание что после создания новой базы данных необходимо на нее переключиться.
```SQL
CREATE SCHEMA my_database_schema;
```
При использовании ключевого слова DROP мы можем удалить наши базы данных и схемы
```SQL
DROP DATABASE my_database;

DROP SCHEMA my_database_schema;
```
Для создания новой таблицы в скобках после названия через запятую обозначаем наши строки форматом имя_схемы.имя_таблицы(имя_колонки тип_данных, ...)
```SQL
CREATE TABLE my_database_schema.my_data (
    id INT,
    name VARCHAR(128),
    date DATE
);
```
Мы можем обозначить поля которые будут автоинкрементироваться через ключевое слово SERIAL. Указать что поле будет первичным ключом через ключевое слово PRIMARY KEY. Указать что поле будет уникальным с помощью UNIQUE и то что поле не будет NULL через словосочетание NOT NULL.
```SQL
CREATE TABLE my_database_schema.my_data
(
    id   INT,
    name VARCHAR(128) UNIQUE NOT NULL,
    date DATE                NOT NULL CHECK ( date > '1900-01-01' AND date < '2020-01-01' )
);
```
```SQL
CREATE TABLE my_employee
(
    id         SERIAL PRIMARY KEY,
    first_name VARCHAR(128) NOT NULL,
    last_name  VARCHAR(128) NOT NULL,
    salary     INT
);
```
## DML

Для заполнения созданой нами таблицы используем ключевое слово INSERT
```SQL
INSERT INTO my_data(id, name, date)
VALUES (01, 'Herman', '1996-11-04'),
       (02, 'Victor', '1997-05-28');
```

После названия таблицы мы указываем названия полей которые мы будем заполнять в виде имя_таблицы(имя_колонки, ...).
А сами значения указываються через запятую в скобках после ключевого слова VALUES

## Пример создания БД работников
Создадим три таблицы для компаний, департаментов и работников.

В таблице компаний укажем два поля где будут храниться уникальный идентефикатор и название компании. Идентефикатор будет генериться автоинкрементом.
```SQL
CREATE TABLE company
(
    id   SERIAL PRIMARY KEY,
    name VARCHAR(128) UNIQUE NOT NULL
);
```
В таблице департамента будут так же как и в предыдущей таблице имя и id, а также внешний ключ который свяжет наш департамент с соответствующей компанией.
```SQL
CREATE TABLE departments
(
    id         SERIAL PRIMARY KEY,
    name       VARCHAR(128) NOT NULL,
    id_company INT REFERENCES company (id)
);
```
Аналогичным образом создаем работников
```SQL
CREATE TABLE employee
(
    id            SERIAL PRIMARY KEY,
    f_name        VARCHAR(128) NOT NULL,
    l_name        VARCHAR(128) NOT NULL,
    salary        INT,
    id_department INT REFERENCES departments (id)
);
```
Заполняем их. Пример заполнения таблицы работников:
```SQL
INSERT INTO employee (id, f_name, l_name, salary, id_department)
VALUES (DEFAULT, 'Ольга', 'Лужкина', 27000, 11),
       (DEFAULT, 'Сергей', 'Фролов', 27500, 6),
       (DEFAULT, 'Ольга', 'Лужкина', 27000, 11),
       (DEFAULT, 'Виктор', 'Пашин', 38000, 12),
       (DEFAULT, 'Дмитрий', 'Лучкин', 35000, 12),
       (DEFAULT, 'Антон', 'Медведев', 36000, 13),
       (DEFAULT, 'Виктор', 'Орлов', 25400, 13),
       (DEFAULT, 'Алексей', 'Иванов', 22000, 14),
       (DEFAULT, 'Дмитрий', 'Фролов', 18600, 14),
       (DEFAULT, 'Катерина', 'Манина', 25400, 14),
       (DEFAULT, 'Виктор', 'Иванов', 48000, 12),
       (DEFAULT, 'Владимир', 'Пашин', 47500, 13),
       (DEFAULT, 'Алексей', 'Кузнецов', 34000, 11),
       (DEFAULT, 'Дмитрий', 'Попов', 30500, 10),
       (DEFAULT, 'Сергей', 'Петров', 29600, 10),
       (DEFAULT, 'Виктор', 'Романов', 17800, 8),
       (DEFAULT, 'Катерина', 'Лужкина', 18500, 7),
       (DEFAULT, 'Антон', 'Петров', 21000, 6),
       (DEFAULT, 'Дмитрий', 'Петренко', 35600, 5),
       (DEFAULT, 'Ольга', 'Кирова', 22500, 4),
       (DEFAULT, 'Катерина', 'Суванян', 21000, 3),
       (DEFAULT, 'Александра', 'Гудина', 38000, 2),
       (DEFAULT, 'Виктор', 'Пилавов', 45000, 2),
       (DEFAULT, 'Ева', 'Суворова', 31500, 2),
       (DEFAULT, 'Катерина', 'Иванова', 28000, 1),
       (DEFAULT, 'Дмитрий', 'Петров', 32000, 1),
       (DEFAULT, 'Виктор', 'Иванов', 25000, 1);
```
А теперь выведем работника и соответствующий департамент и компанию в которой он работает:
```SQL
SELECT concat(f_name, ' ', l_name) AS employee,
       departments.name            AS department,
       company.name                AS company
FROM employee
         JOIN departments ON departments.id = id_department
         JOIN company ON company.id = departments.id_company
ORDER BY company, department;
```
