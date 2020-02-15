---
title: MySQL 教程：增删改查
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/3.jpg
toc: true
mathjax: false
summary: MySQL的增删改查。MySQL是一种关系型数据库管理系统，其速度快，灵活性高，广泛应用在 WEB 方面。
categories:
  - 数据库
tags:
  - MySQL
  - databases
  - 数据库
  - web
  - sql增删改查
urlname: databases-mysql-normal-operate
date: 2018-10-11 17:26:19
img:
password:
updated:
---

# 创建表
```sql
CREATE TABLE JOHNNY (
  id INT NOT NULL,
  sid INT NOT NULL DEFAULT 000,
  NAME CHAR(255) NOT NULL DEFAULT 111,
  price DECIMAL (8, 2) NOT NULL  DEFAULT 222,
  PRIMARY KEY (id)
);

```
# 增，添加数据

## 语法1：指定所有字段名
```sql
INSERT INTO  表名（字段名1，字段名2，…）VALUES（值1，值2，…）；

INSERT INTO JOHNNY (ID, SID, NAME, PRICE)
VALUES
  ('1', 110, 'J', 12.6);
  
id	sid	name	price
1	110	J	12.60 
```

## 语法2：不指定字段名，添加的值的顺序应和字段在表中的顺序完全一致
```sql
INSERT INTO  表名 VALUES（值1，值2，…）；

INSERT INTO JOHNNY
VALUES
  ('2', 120, 'H', 12.9);
  
id	sid	name	price
1	110	J	12.60
2	120	H	12.90
```
## 语法3：指定字段添加值，其他字段为默认值
```sql
INSERT INTO  表名（字段名1）VALUES（值1）；

INSERT INTO JOHNNY (ID)
VALUES
  ('3');
  
id	sid	name	price
1	110	J	12.60
2	120	H	12.90
3   0  111 222.00
```
## 语法4：set写法
```sql
INSERT INTO 表名 SET 字段名1=值1[,字段名2=值2，…]

INSERT INTO JOHNNY SET ID = 4,
sid = 140,
NAME = 'Y',
PRICE = 15;

id	sid	name	price
1	110	J	12.60
2	120	H	12.90
3   0  111 222.00
4  140  Y  15.00
```
# 删，删除数据
语法：DELETE FROM 表名 WHERE 条件表达式
## 删除指定数据
```sql
DELETE FROM JOHNNY WHERE ID = 4;

id	sid	name	price
1	110	J	12.60
2	120	H	12.90
3   0  111 222.00
```
## 删除全部数据 DELETE
```sql
DELETE FROM JOHNNY;
```
> 原表有3行数据，其中id为自增字段，如id为3，全删除后重新插入数据，会从id为4的那一行开始添加，相当于追加

## 删除全部数据 TRUNCATE
```sql
TRUNCATE TABLE JOHNNY;
```
> 原表有3行数据，其中id为自增字段，如id为3，全删除后重新插入数据，会从id为0的哪一行开始添加，相当于从头开始

# 改，修改更新数据
语法：UPDATE 表名 SET 内容 WHERE 条件表达式
## 修改指定内容
```sql
UPDATE
  JOHNNY
SET
  SID = 555
WHERE ID = 3;

id	sid	name	price
1	110	J	12.60
2	120	H	12.90
3   555  111 222.00
```
## 修改全部内容
```sql
UPDATE
  JOHNNY
SET
  NAME = 'zhao';
  
id	sid	name	price
1	110	zhao	12.60
2	120	zhao	12.90
3   555 zhao    222.00
```
# 查，查询数据
## 单表查询
[跳转 单表查询](https://www.zhwei.cn/databases-mysql-singlQuery/)
## 多表查询
[跳转 多表查询](https://www.zhwei.cn/databases-mysql-manyQuery/)