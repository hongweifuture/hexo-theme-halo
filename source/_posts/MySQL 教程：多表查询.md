---
title: MySQL 教程：多表查询
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/3.jpg
toc: true
mathjax: false
summary: MySQL的多表查询。MySQL是一种关系型数据库管理系统，其速度快，灵活性高，广泛应用在 WEB 方面。
categories:
  - 数据库
tags:
  - MySQL
  - databases
  - 数据库
  - web
  - sql多表查询
urlname: databases-mysql-many-query
date: 2018-10-10 17:23:50
img:
password:
updated:
---

# 创建数据库并插入数据
## 表一，fruits，水果
```sql
# 创建表，数据类型请自行查询
CREATE TABLE fruits (
  id INT NOT NULL,
  sid INT NOT NULL,
  NAME CHAR(255) NOT NULL,
  price DECIMAL (8, 2) NOT NULL,
  PRIMARY KEY (id)
);

# 表中插入数据
INSERT INTO fruits
VALUES
  ('1', 101, 'apple', 5.2),
  ('2', 101, 'blackberry', 10.2),
  ('3', 102, 'orange', 11.2),
  ('4', 105, 'melon', 8.2),
  ('5', 102, 'banana', 10.3),
  ('6', 102, 'grape', 5.3),
  ('7', 103, 'coconut', 9.2),
  ('8', 101, 'cherry', 3.2),
  ('9', 103, 'apricot', 2.2),
  ('10', 104, 'lemon', 6.4),
  ('11', 104, 'berry', 7.6),
  ('12', 106, 'mango', 15.6);
  ('13', 110, 'HHHHHH', 12.6);

```
## 表二，suppliers，供应商
```sql
CREATE TABLE suppliers (
  sid INT NOT NULL,
  sName CHAR(50) NOT NULL,
  city CHAR(50) NULL,
  zip CHAR(10) NULL,
  scall CHAR(50) NOT NULL,
  PRIMARY KEY (sid)
);

INSERT INTO suppliers
VALUES
  (
    101,
    'Supplies A',
    'Tianjin',
    '400000',
    '18075'
  ),
  (
    102,
    'Supplies B',
    'Chongqing',
    '400000',
    '44333'
  ),
  (
    103,
    'Supplies C',
    'Shanghai',
    '400000',
    '90046'
  ),
  (
    104,
    'Supplies D',
    'Zhongshan',
    '400000',
    '11111'
  ),
  (
    105,
    'Supplies E',
    'Taiyuang',
    '400000',
    '22222'
  ),
  (
    106,
    'Supplies F',
    'Beijing',
    '400000',
    '45678'
  ),
  (
    107,
    'Supplies G',
    'Zhengzhou',
    '400000',
    '33332'
  );
```
## 表3，顾客
```sql
CREATE TABLE people (
  id INT NOT NULL,
  NAME VARCHAR(30),
  num INT NOT NULL,
  CITY VARCHAR(50),
  PRIMARY KEY (id)
);

INSERT INTO people
VALUES
  ('1', 'A', 23, 'Shanghai'),
  ('2', 'B', 5, 'TIANJIN'),
  ('3', 'C', 3, 'BEIJING'),
  ('4', 'D', 11, 'NANCHANG'),
  ('5', 'E', 56, 'NANJING'),
  ('6', 'F', 15, 'JIANGXI'),
  ('7', 'G', 23, 'SHENZHEN'),
  ('8', 'H', 56, 'WUHAN'),
  ('9', 'I', 76, 'GUANZHOU'),
  ('10', 'G', 34, 'LIAONING');

```

# 普通双表查询
查询水果的供应商编码、名字即对应的水果名称和价格
```sql
SELECT f.SID,S.SNAME,F.name,F.price FROM fruits AS f ,suppliers AS S WHERE F.SID =S.SID;

SID	SNAME	name	price
101	Supplies A	apple	5.20
101	Supplies A	blackberry	10.20
102	Supplies B	orange	11.20
105	Supplies E	melon	8.20
102	Supplies B	banana	10.30
102	Supplies B	grape	5.30
103	Supplies C	coconut	9.20
101	Supplies A	cherry	3.20
103	Supplies C	apricot	2.20
104	Supplies D	lemon	6.40
104	Supplies D	berry	7.60
106	Supplies F	mango	15.60
```
# 内连接，两个表的公共部分
格式：表名 (INNER) JOIN 表名 ON 连接条件

## 双表内连接查询
查询水果的供应商编码、名字、城市即对应的水果名称和价格
```sql
SELECT f.SID,S.SNAME,F.name,F.price,s.city FROM fruits AS f INNER JOIN suppliers AS S ON F.SID =S.SID;

SELECT f.SID,S.SNAME,F.name,F.price,s.city FROM fruits AS f JOIN suppliers AS S ON F.SID =S.SID;

SID	SNAME	name	price	city
101	Supplies A	apple	5.20	Tianjin
101	Supplies A	blackberry	10.20	Tianjin
102	Supplies B	orange	11.20	Chongqing
105	Supplies E	melon	8.20	Taiyuang
102	Supplies B	banana	10.30	Chongqing
102	Supplies B	grape	5.30	Chongqing
103	Supplies C	coconut	9.20	Shanghai
101	Supplies A	cherry	3.20	Tianjin
103	Supplies C	apricot	2.20	Shanghai
104	Supplies D	lemon	6.40	Zhongshan
104	Supplies D	berry	7.60	Zhongshan
106	Supplies F	mango	15.60	Beijing
```
发现和上面普通查询一样

## 自连接查询，即双表是同一张表
查询供应id为2的水果供应商提供的其他水果名称
```sql
SELECT f2.id,F2.name,f2.sid FROM fruits AS f1 JOIN fruits AS F2 ON f1.sid = f2.sid AND f1.id = 2;

id	name	sid
1	apple	101
2	blackberry	101
8	cherry	101
```
查询条件为表1，查询字段为表2

## 通过子句查询进行内链接
```sql
SELECT fruits.id,fruits.name,fruits.sid FROM fruits WHERE fruits.SID = (SELECT fruits.SID FROM fruits WHERE fruits.ID = 2);

id	name	sid
1	apple	101
2	blackberry	101
8	cherry	101
```

# 外连接
## 左外连接，左表全部和左右表公共部分集合
格式： 表名 LEFT (OUTER) JOIN 表名 ON 条件

```sql
# SUPPLIERS 为左表，显示全部内容
# FRUITS 为右表，显示与左表公共部分
# 右表其他内容显示为空，NULL

SELECT s.sid,s.sname,f.id,f.name FROM suppliers AS s LEFT JOIN fruits AS f ON f.sid = s.sid ;

sid	sname	id	name
101	Supplies A	1	apple
101	Supplies A	2	blackberry
102	Supplies B	3	orange
105	Supplies E	4	melon
102	Supplies B	5	banana
102	Supplies B	6	grape
103	Supplies C	7	coconut
101	Supplies A	8	cherry
103	Supplies C	9	apricot
104	Supplies D	10	lemon
104	Supplies D	11	berry
106	Supplies F	12	mango
107	Supplies G	NULL  NULL
```
## 右外连接，右表全部和左右表公共部分集合
> 与左外连接相似,就是全部显示右表及公共部分

格式： 表名 RIGHT (OUTER) JOIN 表名 ON 条件

```sql
# SUPPLIERS 为左表，显示与右表公共部分
# FRUITS 为右表，显示全部内容
# 左表其他内容显示为空，NULL

SELECT s.sid,s.sname,f.id,f.name FROM suppliers AS s RIGHT JOIN fruits AS f ON f.sid = s.sid ;

sid	sname	id	name
101	Supplies A	1	apple
101	Supplies A	2	blackberry
102	Supplies B	3	orange
105	Supplies E	4	melon
102	Supplies B	5	banana
102	Supplies B	6	grape
103	Supplies C	7	coconut
101	Supplies A	8	cherry
103	Supplies C	9	apricot
104	Supplies D	10	lemon
104	Supplies D	11	berry
106	Supplies F	12	mango
NULL  NULL	13	HHHHHH
```

# 三表查询
- 表一和表二`sid`关联
- 表一和表三`id`关联

查询供应商Supplies B供应的水果顾客购买量
```sql
SELECT
  s.city,
  s.sName,
  f.name,
  p.NAME,
  p.num
FROM
  fruits AS F
  JOIN suppliers AS s
    ON f.sid = s.sid
    AND s.sName = 'Supplies B'
  LEFT JOIN people AS p
    ON f.id = p.id
ORDER BY p.num DESC;

city	sName	name	NAME	num
Chongqing	Supplies B	banana	E	56
Chongqing	Supplies B	grape	F	15
Chongqing	Supplies B	orange	C	3
```


