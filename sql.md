
# SQL

## Основы SQL

SQL делится на два основных подраздела:

- DDL (Data Definition Language) - Создание структуры базы данных с помощью ключевых слов CREATE DROP ALTER

- DML (Data Manipulation Language) - Манипуляция над данными с помощью ключевых слов INSERT DELETE UPDATE SELECT

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
