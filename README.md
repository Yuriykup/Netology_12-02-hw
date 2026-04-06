# Домашнее задание к занятию «Работа с данными (DDL/DML)»

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

``` Команды при установке MySQL.
kupriyanov@ntlg:~$sudo apt update
kupriyanov@ntlg:~$sudo apt install mysql-server -y
```
---
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.1

![Скриншот-1](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img1.png)

---

1.2. Создайте учётную запись sys_temp.

```sql
CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';
``` 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

---
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.3

![Скриншот-2](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img2.png)

---

1.4. Дайте все права для пользователя sys_temp. 

```sql
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
```

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

---
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.5

![Скриншот-3](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img3.png)

---
1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

---

1.7. Восстановите дамп в базу данных.

```sql
CREATE DATABASE IF NOT EXISTS sakila

SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
```

```
kupriyanov@ntlg:~/Downloads/sakila-db$ mysql -u sys_temp -p sakila < sakila-schema.sql
kupriyanov@ntlg:~/Downloads/sakila-db$ mysql -u sys_temp -p sakila < sakila-data.sql
```
---
1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*


```sql
USE sakila;
SHOW TABLES;
```
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.8

![Скриншот-4](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img4.png)

---



### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```
---
### ОТВЕТ НА ЗАДАНИЕ 2

```sql
SELECT TABLE_NAME, COLUMN_NAME  FROM information_schema.COLUMNS  WHERE TABLE_SCHEMA = 'sakila'    AND COLUMN_KEY = 'PRI'  ORDER BY TABLE_NAME;
```

### СКРИНШОТ ДЛЯ ЗАДАНИЯ 2

![Скриншот-5](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img5.png)

[Таблица](Sakila_table.xlsx)


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.
---
### ОТВЕТ НА ЗАДАНИЕ 3

##### Подключился к MySQL под пользователем sys_temp, но при первичном вводе команды для удаления прав на внесение, изменение и удаление данных из базы sakila отобразилась ОШИБКА:

```sql
REVOKE INSERT, UPDATE, DELETE ON sakila.* FROM 'sys_temp'@'localhost';
ERROR 1044 (42000): Access denied for user 'sys_temp'@'localhost' to database 'sakila'
```
Подозреваю, что проблема в правах пользователя `sys_temp`. Для изменения прав доступа необходимо подключиться к MySQL с правами пользователя `root`.
Подключаюсь с правами `root` и пробую изменить права для пользователя `sys_temp`.

```sql
REVOKE INSERT, UPDATE, DELETE ON sakila.* FROM 'sys_temp'@'localhost';
ERROR 1141 (42000): There is no such grant defined for user 'sys_temp' on host 'localhost'
```
Получаю ответ, что у пользователя `sys_temp` не найдено запрашиваемых к удалению прав.
Проверяю так ли это?

```sql
SHOW GRANTS FOR 'sys_temp'@'localhost';
```

### Скриншот запроса наличия прав у sys_temp.

![Скриншот-6](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img6.png)

##### На скриншоте видно, что пользователь `sys_temp` не имеет никих прав к базе 'sakila'.
Назначаю принудительно права для на внесения, изменения и удаления данных из базы sakila.

```sql
GRANT INSERT, UPDATE, DELETE ON sakila.* TO 'sys_temp'@'localhost';
Query OK, 0 rows affected (0.02 sec)
```
Проверяю присвоеные права.

### Скриншот запроса наличия прав у sys_temp после принудительного присвоения.

![Скриншот-6](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img6.png)

Терь пробую отобрать эти права:

```sql
REVOKE INSERT, UPDATE, DELETE ON sakila.* FROM 'sys_temp'@'localhost';
Query OK, 0 rows affected (0.03 sec)
```

Проверяю отсутвие прав.

### Скриншот запроса наличия прав у `sys_temp` после удаления прав на внесение, изменение и удаление данных из базы sakila.

![Скриншот-6](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img6.png)

##### Права у пользователя `sys_temp` удалены.
---

