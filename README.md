# PayrollServiceSQL
UC1

mysql> create database payroll_service;

mysql>  show databases;
+--------------------+
| Database           |
+--------------------+
| address_book       |
| information_schema |
| mysql              |
| payroll_service    |
| performance_schema |
+--------------------+


mysql> USE payroll_service;
Database changed
mysql> select database();
+-----------------+
| database()      |
+-----------------+
| payroll_service |
+-----------------+

___________________________________________________________________________________________________________________________________________________________________

UC2

mysql> create table employee_payroll
    -> (
    -> Id        INT unsigned NOT NULL AUTO_INCREMENT,
    -> name      VARCHAR(150) NOT NULL,
    -> salary    Double NOT NULL,
    -> start     DATE NOT NULL,
    -> PRIMARY KEY (id)
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> DESCRIBE employee_payroll;
+--------+--------------+------+-----+---------+----------------+
| Field  | Type         | Null | Key | Default | Extra          |
+--------+--------------+------+-----+---------+----------------+
| Id     | int unsigned | NO   | PRI | NULL    | auto_increment |
| name   | varchar(150) | NO   |     | NULL    |                |
| salary | double       | NO   |     | NULL    |                |
| start  | date         | NO   |     | NULL    |                |
+--------+--------------+------+-----+---------+----------------+

_______________________________________________________________________________________________________________________________________________________________

UC3

mysql> INSERT INTO employee_payroll ( name, salary, start) VALUES
    -> ( 'Bill', 1000000.00, '2018-01-03' ),
    -> ( 'Terisa', 2000000.00, '2019-11-13' ),
    -> ( 'Charlie', 3000000.00, '2020-05-21' );
Query OK, 3 rows affected (0.01 sec)

_________________________________________________________________________________________________________________________________________________________________

UC4

mysql> select * from employee_payroll;
+----+---------+---------+------------+
| Id | name    | salary  | start      |
+----+---------+---------+------------+
|  1 | Bill    | 1000000 | 2018-01-03 |
|  2 | Terisa  | 2000000 | 2019-11-13 |
|  3 | Charlie | 3000000 | 2020-05-21 |
+----+---------+---------+------------+

____________________________________________________________________________________________________________________________________________________________________

UC5

mysql> select salary from employee_payroll where name = 'Bill';
+---------+
| salary  |
+---------+
| 1000000 |
+---------+

mysql> select * from employee_payroll
    -> where start between cast('2018-01-01' as date) and date(now());
+----+---------+---------+------------+
| Id | name    | salary  | start      |
+----+---------+---------+------------+
|  1 | Bill    | 1000000 | 2018-01-03 |
|  2 | Terisa  | 2000000 | 2019-11-13 |
|  3 | Charlie | 3000000 | 2020-05-21 |
+----+---------+---------+------------+


mysql> select * from employee_payroll
    -> where start between cast('2019-01-01' as date) and date(now());
+----+---------+---------+------------+
| Id | name    | salary  | start      |
+----+---------+---------+------------+
|  2 | Terisa  | 2000000 | 2019-11-13 |
|  3 | Charlie | 3000000 | 2020-05-21 |
+----+---------+---------+------------+

_______________________________________________________________________________________________________________________________________________________________

UC6

mysql> alter table employee_payroll ADD gender CHAR(1) AFTER name;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> describe employee_payroll;
+--------+--------------+------+-----+---------+----------------+
| Field  | Type         | Null | Key | Default | Extra          |
+--------+--------------+------+-----+---------+----------------+
| Id     | int unsigned | NO   | PRI | NULL    | auto_increment |
| name   | varchar(150) | NO   |     | NULL    |                |
| gender | char(1)      | YES  |     | NULL    |                |
| salary | double       | NO   |     | NULL    |                |
| start  | date         | NO   |     | NULL    |                |
+--------+--------------+------+-----+---------+----------------+

mysql> update employee_payroll set gender = 'F' where name = 'Terisa';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from employee_payroll;
+----+---------+--------+---------+------------+
| Id | name    | gender | salary  | start      |
+----+---------+--------+---------+------------+
|  1 | Bill    | NULL   | 1000000 | 2018-01-03 |
|  2 | Terisa  | F      | 2000000 | 2019-11-13 |
|  3 | Charlie | NULL   | 3000000 | 2020-05-21 |
+----+---------+--------+---------+------------+

mysql> update employee_payroll set gender = 'M' where name = 'Bill' or name = 'Charlie';
Query OK, 2 rows affected (0.01 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> select * from employee_payroll;
+----+---------+--------+---------+------------+
| Id | name    | gender | salary  | start      |
+----+---------+--------+---------+------------+
|  1 | Bill    | M      | 1000000 | 2018-01-03 |
|  2 | Terisa  | F      | 2000000 | 2019-11-13 |
|  3 | Charlie | M      | 3000000 | 2020-05-21 |
+----+---------+--------+---------+------------+

mysql> update employee_payroll set salary = 3000000.00 where name = 'Terisa';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from employee_payroll;
+----+---------+--------+---------+------------+
| Id | name    | gender | salary  | start      |
+----+---------+--------+---------+------------+
|  1 | Bill    | M      | 1000000 | 2018-01-03 |
|  2 | Terisa  | F      | 3000000 | 2019-11-13 |
|  3 | Charlie | M      | 3000000 | 2020-05-21 |
+----+---------+--------+---------+------------+

___________________________________________________________________________________________________________________________________________________________________


UC7


mysql> SELECT AVG(salary) from employee_payroll where gender = 'M' GROUP BY gender;
+-------------+
| AVG(salary) |
+-------------+
|     2000000 |
+-------------+


mysql> select gender, AVG(salary) from employee_payroll group by gender;
+--------+-------------+
| gender | AVG(salary) |
+--------+-------------+
| M      |     2000000 |
| F      |     3000000 |
+--------+-------------+
2 rows in set (0.00 sec)

mysql> select gender, COUNT(name) FROM employee_payroll GROUP BY gender;
+--------+-------------+
| gender | COUNT(name) |
+--------+-------------+
| M      |           2 |
| F      |           1 |
+--------+-------------+
2 rows in set (0.00 sec)

mysql> select gender, SUM(name) FROM employee_payroll GROUP BY gender;
+--------+-----------+
| gender | SUM(name) |
+--------+-----------+
| M      |         0 |
| F      |         0 |
+--------+-----------+
2 rows in set, 2 warnings (0.00 sec)

mysql> select gender, SUM(salary) FROM employee_payroll GROUP BY gender;
+--------+-------------+
| gender | SUM(salary) |
+--------+-------------+
| M      |     4000000 |
| F      |     3000000 |
+--------+-------------+
2 rows in set (0.00 sec)

