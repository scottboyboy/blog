---
title: postgres export and import.md
date: 2017-06-12 17:28:35
tags: [postgres]
---
## Postgres数据库的导入和导出


-----
### 导出（Export）
1.连接数据库

> 这里使用了Git Client...，其实只要有个远程客户端都可以

**语法如下**

```sql
psql -U postgres -h 127.0.0.1 -p 5432 [-d databasename -W] -- 可以在此指定数据库
\c attendance		-- 或者连接之后再指定数据库
```

> 注意：如果上面指定了-W参数，会强制在连接每一个database时，输入密码，可以先不用

<!-- more -->

**操作如下**

```SQL
$ psql -U postgres -h 127.0.0.1 -p 5432
psql (9.5.6)
postgres=# \l
                                                         数据库列表
    名称    |  拥有者  | 字元编码 |            校对规则            |             Ctype              |       存取权限
------------+----------+----------+--------------------------------+--------------------------------+-----------------------
 attendance | postgres | UTF8     | Chinese (Simplified)_China.936 | Chinese (Simplified)_China.936 |
 postgres   | postgres | UTF8     | Chinese (Simplified)_China.936 | Chinese (Simplified)_China.936 |
 template0  | postgres | UTF8     | Chinese (Simplified)_China.936 | Chinese (Simplified)_China.936 | =c/postgres          +
            |          |          |                                |                                | postgres=CTc/postgres
 template1  | postgres | UTF8     | Chinese (Simplified)_China.936 | Chinese (Simplified)_China.936 | =c/postgres          +
            |          |          |                                |                                | postgres=CTc/postgres
 testim     | postgres | UTF8     | Chinese (Simplified)_China.936 | Chinese (Simplified)_China.936 |
(5 行记录)


postgres=# \c attendance;
attendance=#
```

2.设置编码
编码不同，可能在导入导出时遇到问题。明确指定了编码，Postgres会为你转换的。
**语法如下**

```sql
show client_encoding;	-- 查看使用的本地客户端的编码
show server_encoding;	-- psql服务器端的编码
\encoding UTF-8;		-- 这是一种方式，还有另外一种方式是SET client_encoding语法
RESET client_encoding;	-- 重置client端的编码
SET CLIENT_ENCODING TO 'UTF-8';	-- 再次更改为UTF-8
```

---

**操作如下**

```sql
attendance=# show client_encoding;
 client_encoding
-----------------
 GBK
(1 行记录)


attendance=# show server_encoding;
 server_encoding
-----------------
 UTF8
(1 行记录)


attendance=# \encoding UTF-8;
attendance=# show client_encoding;
 client_encoding
-----------------
 UTF8
(1 行记录)


attendance=# RESET client_encoding;
RESET
attendance=# show client_encoding;
 client_encoding
-----------------
 GBK
(1 行记录)


attendance=# SET CLIENT_ENCODING TO 'UTF-8';
SET
attendance=# show client_encoding;
 client_encoding
-----------------
 UTF8
(1 行记录)


attendance=#

```

3.正式导出

**语法如下**
使用copy命令导出到本地文件
```SQL
\copy student to 'd://psql/hey.csv' delimiter as '~' csv null '!';	-- 使用\copy的目的是psql可以访问本地文件，否则只能访问data目录;

\copy (select * from student where id > 50) to 'd://psql/tmp.csv' delimiter as '~' csv null '!'	-- 这个也可以，更多可能
```

> 注意：delimiter分割建议使用'~'，\copy可以不用';'，copy要用的。
>  csv null '!'很重要，

**操作如下**

```SQL
attendance=# \copy student to 'd://psql/hey.csv' delimiter as '~' csv null '!';
COPY 100

attendance=# \copy (select * from student where id > 50) to 'd://psql/tmp.csv' delimiter as '~' csv null '!'
COPY 59

```

### 导入（Import）

1.设置字段之间的分隔符
2.注意某些为NULL的字段，可能需要特殊处理
3.同导出一样设置编码


**语法如下**

*from-to* 的关系

```SQL
delete from student;	-- delete all data from student
 \copy student from 'd://psql/ss1.csv' delimiter as '~' csv null '!';	-- 空值用!代替，然后到postgres会有替换为NULL

```
**操作如下**
```SQL
attendance=# delete from student;
DELETE 100
attendance=# \copy student from 'd://psql/ss1.csv' delimiter as '~' csv null '!';
COPY 100
attendance=#
```


---
#### 参考文档
1. [postgres官方连接数据库](https://www.postgresql.org/docs/9.5/static/app-psql.html)
2. [postgres设置编码](https://www.postgresql.org/docs/9.5/static/multibyte.html)
3. [postgres中DELETE语法](https://www.postgresql.org/docs/8.2/static/dml-delete.html)