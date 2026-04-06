# Домашнее задание к занятию «Работа с данными (DDL/DML)»

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

``` #### Команды при установке MySQL.
kupriyanov@ntlg:~$sudo apt update
kupriyanov@ntlg:~$sudo apt install mysql-server -y
```
---
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.1

![Скриншот-1](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img1.png)

---

1.2. Создайте учётную запись sys_temp.

```sql
mysql> CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';
``` 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

---
### СКРИНШОТ ДЛЯ ЗАДАНИЯ 1.3

![Скриншот-2](https://github.com/Yuriykup/Netology_12-02-hw/blob/main/img/img2.png)

---

1.4. Дайте все права для пользователя sys_temp. 

```sql
mysql> GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
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

```sql
mysql> ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

```

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.
1.7. Восстановите дамп в базу данных.

```sql
mysql> CREATE DATABASE IF NOT EXISTS sakila

mysql> SHOW DATABASES;
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
kupriyanov@ntlg:~/Downloads/sakila-db$ mysql -u sys_temp -p sakila < sakila-schema.sql
kupriyanov@ntlg:~/Downloads/sakila-db$ mysql -u sys_temp -p sakila < sakila-data.sql
```

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*


### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*
