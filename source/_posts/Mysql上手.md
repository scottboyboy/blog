---
title: Mysql上手
date: 2016-10-19 12:13:49
tags: [Mysql]
---
# Mysql上手操作
---
### 使用Mysql,打开 `相应的服务`。启动……
1. 打开命令窗口。此处有多种方法，我是在开始菜单（Mysql5.6 Command Line Client）打开的（简单）。
```
mysql -h localhost -u root -p   #开始菜单不需要此命令
```
2. 登陆,使用安装时设置的密码。
<!-- more -->
输入你的密码。
-------
3. 登陆成功就可以愉快地玩耍勒。
    * 先全局观望,发现有五六七八个左右的数据库。
```
mysql> show databases;  #看看本地有哪些数据库
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| test               |
| testdb             |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```
    * 厉害了我的world，我想看看哪个里面都有什么？
```
mysql> use world;       #使用world库
Database changed
mysql> show tables;     #看看库里有什么东东（表）
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
3 rows in set (0.00 sec)
```
    * 先猫一眼city的样子。
```
mysql> desc city;   #看看city模式（database schema）,包括五个属性（attribute）       
+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| ID          | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name        | char(35) | NO   |     |         |                |
| CountryCode | char(3)  | NO   |     |         |                |
| District    | char(20) | NO   |     |         |                |
| Population  | int(11)  | NO   |     | 0       |                |
+-------------+----------+------+-----+---------+----------------+
5 rows in set (0.09 sec)
```
    * 再看看city表（table）里面有数据吗？
```
| 4075 | Khan Yunis                           | PSE         | Khan Yun
is                   |     123175 |
| 4076 | Hebron                               | PSE         | Hebron
                     |     119401 |
| 4077 | Jabaliya                             | PSE         | North Ga
za                   |     113901 |
| 4078 | Nablus                               | PSE         | Nablus
                     |     100231 |
| 4079 | Rafah                                | PSE         | Rafah
                     |      92020 |
+------+--------------------------------------+-------------+---------
---------------------+------------+
4079 rows in set (0.06 sec)
```
* `发现有4079条记录（数据）`，太多了，缓冲区都不够用，我的命令都没了。如何是好？
```
mysql> select * from city limit 0,5;    #只要前5行，从第0行开始，列出总计5行OK
+----+----------------+-------------+---------------+------------+
| ID | Name           | CountryCode | District      | Population |
+----+----------------+-------------+---------------+------------+
|  1 | Kabul          | AFG         | Kabol         |    1780000 |
|  2 | Qandahar       | AFG         | Qandahar      |     237500 |
|  3 | Herat          | AFG         | Herat         |     186800 |
|  4 | Mazar-e-Sharif | AFG         | Balkh         |     127800 |
|  5 | Amsterdam      | NLD         | Noord-Holland |     731200 |
+----+----------------+-------------+---------------+------------+
5 rows in set (0.00 sec)
mysql> select * from city limit 4074,5;     #从4074开始，跟数组类似。只要5行
+------+------------+-------------+------------+------------+
| ID   | Name       | CountryCode | District   | Population |
+------+------------+-------------+------------+------------+
| 4075 | Khan Yunis | PSE         | Khan Yunis |     123175 |
| 4076 | Hebron     | PSE         | Hebron     |     119401 |
| 4077 | Jabaliya   | PSE         | North Gaza |     113901 |
| 4078 | Nablus     | PSE         | Nablus     |     100231 |
| 4079 | Rafah      | PSE         | Rafah      |      92020 |
+------+------------+-------------+------------+------------+
5 rows in set (0.00 sec)
```
> 一窥究竟，发现属性有ID,Name，CountryCode，District，Population。

--------------------------------------------------
4. 具体的操作：
    * 对于库的操作：
       > show databases;    #列出当前有哪些库【注意databases，还有分号】
       > use xxx;        #使用xx数据库，xxx表示数据库名
       > select database();      #当前使用的是哪个库【注意database后面没有s】
       > create database xxx;   #创建xxx数据库
       > drop database xxx;     #删除xxx数据库
    * 对于表的操作：
       > show tables;       #列出xxx库里面的表
       > create table stu(name char(20),id int,age int,address char(30),phone_num char(11),primary key (id));
       );       #创建一个stu表，并且id是主键
       ```
       mysql> create table stu (
            -> name char(20),
            -> id int,
             -> age int,
            -> address char(30),
            -> phone_num char(11),
            -> primary key (id)
            -> );
        Query OK, 0 rows affected (0.36 sec)
       ```
       > desc stu;  #stu表的详细属性
```
mysql> desc stu;
+-----------+----------+------+-----+---------+-------+
| Field     | Type     | Null | Key | Default | Extra |
+-----------+----------+------+-----+---------+-------+
| name      | char(20) | YES  |     | NULL    |       |
| id        | int(11)  | NO   | PRI | 0       |       |
| age       | int(11)  | YES  |     | NULL    |       |
| address   | char(30) | YES  |     | NULL    |       |
| phone_num | char(11) | YES  |     | NULL    |       |
+-----------+----------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```
    > insert into stu values ('tju',001,20,'8A',10086); #注意values，如果只有一条，-s 可选，但是多条数据 -s 必选
```
mysql> insert into stu values (
    -> 'tju',
    -> 001,
    -> 20,
    -> '8A',
    -> 10086
    -> );
Query OK, 1 row affected (0.08 sec)
mysql> insert into stu values (
    -> 'nku',
    -> 002,
    -> 20,
    -> '8b',
    -> 10010),
    -> (
    -> 'pku',
    -> 003,
    -> 21,
    -> 'bj',
    -> '110'
    -> ),
    -> ('tju',
    -> '004',
    -> 22,
    -> 'sh',
    -> '95188');
Query OK, 3 rows affected (0.08 sec)
Records: 3  Duplicates: 0  Warnings: 0
```
> 列出全部的记录（元组）
```
mysql> select * from stu;
+------+----+------+---------+-----------+
| name | id | age  | address | phone_num |
+------+----+------+---------+-----------+
| tju  |  1 |   20 | 8A      | 10086     |
| nku  |  2 |   20 | 8b      | 10010     |
| pku  |  3 |   21 | bj      | 110       |
| tju  |  4 |   22 | sh      | 95188     |
+------+----+------+---------+-----------+
4 rows in set (0.00 sec)
```
> 有选择的列出几个，条件是 age < 21
```
mysql> select name from stu where age < 21;
+------+
| name |
+------+
| tju  |
| nku  |
+------+
2 rows in set (0.00 sec)
```
> alter table stu add passwd varchar(12);   #增加属性passwd
> alter table stu add column passwd varchar(12);    #与上面效果相同
> delete from school where id =4;       #删除一条数据
> 删除passwd属性，同理会删除这一列的所有数据：
```
mysql> alter table stu drop column passwd;
Query OK, 0 rows affected (0.62 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
> rename table stu to school;   #修改表的名字
> alter table stu change phone_num num varchar(11);     #修改属性名（列名）
> update school set age = 23 where name ='nku';     #更新age为23

-------------------------------------------------------------
5. 退出
> exit
> quit  #两个命令都可以
----------------
## 编码
可以使用
```
mysql> show variables like 'character%'; --查看编码方式
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | gbk                        |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)

mysql> set character_set_client=utf8;	--更改编码
Query OK, 0 rows affected (0.00 sec)

mysql> show variables like 'character_set_client';	--查看client编码
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| character_set_client | utf8  |
+----------------------+-------+
1 row in set (0.01 sec)

mysql>


```

`欢迎讨论，未完待续`