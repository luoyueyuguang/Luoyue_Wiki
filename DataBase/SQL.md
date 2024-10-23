*reference:《SQL基础教程》*

## postgresql配置
``` bash
#in ubuntu
sudo apt install postgresql

sudo su postgres
或
sudo -i
su postgres

psql

\q退出
```


## SQL语句分类
- DDL
DDL (Data Definition Language，数据定义语言) 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。DDL 包含以下几种指令。
CREATE ： 创建数据库和表等对象
DROP ： 删除数据库和表等对象
ALTER ： 修改数据库和表等对象的结构
- DML
DML (Data Manipulation Language，数据操纵语言) 用来查询或者变更表中的记录。DML 包含以下几种指令。
SELECT ：查询表中的数据
INSERT ：向表中插入新数据
UPDATE ：更新表中的数据
DELETE ：删除表中的数据
- DCL
DCL（Data Control Language，数据控制语言）   用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。DCL 包含以下几种指令。
COMMIT ： 确认对数据库中的数据进行的变更
ROLLBACK ： 取消对数据库中的数据进行的变更
GRANT ： 赋予用户操作权限
REVOKE ： 取消用户的操作权限

## SQL的基本书写规则
- SQL语句要以分号(;)结尾
- SQL 语句不区分大小写
	SQL 不区分关键字的大小写,但区分数据的大小写
- 常数的书写方式是固定的
	在 SQL 语句中直接书写的字符串、日期或者数字等称为常数,字符串使用单引号,日期需要单引号,数字不需要任何符号标识
- 单词需要用半角空格或者换行来分隔

## 建表
- 创建数据库
``` sql
CREATE DATABASE < 数据库名称 >;
```
- 创建表
``` sql
CREATE TABLE < 表名 >
（ < 列名 1> < 数据类型 > < 该列所需约束 > ，
< 列名 2> < 数据类型 > < 该列所需约束 > ，
< 列名 3> < 数据类型 > < 该列所需约束 > ，
< 列名 4> < 数据类型 > < 该列所需约束 > ，
.
.
.
< 该表的约束 1>， < 该表的约束 2> ，……）；
```

- 数据类型的指定
　数据类型表示数据的种类，包括数字型、字符型和日期型等
　- INTEGER 型
　  用来指定存储整数的列的数据类型（数字型），不能存储小数。
　 - CHAR 型
　   CHAR 是 CHARACTER（字符）的缩写，是用来指定存储字符串的列的数据类型（字符型）。
　  - VARCHAR 型