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
　   CHAR 是 CHARACTER（字符）的缩写，是用来指定存储字符串的列的数据类型（字符型）。以定长字符串的形式存储
　  - VARCHAR 型
　  　用来指定存储字符串的列的数据类型 (字符串类型)，也可以通过括号内的数字来指定字符串的长度(最大长度)。但该类型的列是以可变字符串的形式来保存字符串的。
　  - DATE型
　  　用来指定存储日期（年月日）的列的数据类型（日期型）。
- 约束是除了数据类型之外，对列中存储的数据进行限制或者追加条件的功能。
　可以设置`NOT NULL`约束，代表无空白。
　也可以设置主键约束，即`PRIMARY KEY`
## 表的删除和更新
``` SQL
DROP TABLE < 表名 >；
```

``` SQL
ALTER TABLE < 表名 > ADD COLUMN < 列的定义 > ；
```
``` SQL
ALTER TABLE < 表名 > DROP COLUMN < 列名 > ；
# 某些DB中可以这样写
ALTER TABLE <表名> DROP < 列名 > ；
# 某些可以这样写
ALTER TABLE <表名> DROP （ < 列名 >，<列名 > ，……）；
```

``` SQL
-- DML ：插入数据
BEGIN TRANSACTION;
INSERT INTO Product VALUES ('0001', 'T恤衫 ', ' 衣服 ',
1000, 500, '2009-09-20');
INSERT INTO Product VALUES ('0002', ' 打孔器 ', ' 办公用品 ',
500, 320, '2009-09-11');
INSERT INTO Product VALUES ('0003', ' 运动T恤 ', ' 衣服 ',
4000, 2800, NULL);
INSERT INTO Product VALUES ('0004', ' 菜刀 ',
 ' 厨房用具 ',
3000, 2800, '2009-09-20');
INSERT INTO Product VALUES ('0005', ' 高压锅', ' 厨房用具 ',
6800, 5000, '2009-01-15');
INSERT INTO Product VALUES ('0006', ' 叉子',
 ' 厨房用具 ',
500, NULL, '2009-09-20');
INSERT INTO Product VALUES ('0007', ' 擦菜板 ', ' 厨房用具 ',
880, 790, '2008-04-28');
INSERT INTO Product VALUES ('0008', ' 圆珠笔', ' 办公用品 ',
100, NULL,'2009-11-11');
COMMIT;
```

## SELECT语句
``` SQL
SELECT < 列名 > ，……
FROM < 表名 > ；
```