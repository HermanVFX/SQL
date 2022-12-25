
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
