---
title: MySQL 教程：聚合函数
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/3.jpg
toc: true
mathjax: false
summary: MySQL的聚合函数。MySQL是一种关系型数据库管理系统，其速度快，灵活性高，广泛应用在 WEB 方面。
categories:
  - 数据库
tags:
  - MySQL
  - databases
  - 数据库
  - web
  - sql聚合函数
urlname: databases-mysql-aggregate-function
date: 2018-10-09 17:27:16
img:
password:
updated:
---

# 聚合函数特点
1. 组函数，将字段当作一个组进行统计，可结合分组`GROUP BY`联合使用
2. 不用`GROUP BY`，中间结果集中的所有行自动形成一组，然后计算组合数
3. 每个组函数接收一个参数（字段名或者表达式） 统计结果中默认忽略字段为`NULL`的记录，不参与计算
4. 要想列值为`NULL`的行也参与组函数的计算，必须使用`IFNULL`函数对`NULL`值做转换。
5. 不允许出现嵌套 比如`sum(max(...))`

# 内容
```sql
SELECT * FROM fruits;

id	sid	name	price
1	101	apple	5.20
2	101	blackberry	10.20
3	102	orange	11.20
4	105	melon	8.20
5	102	banana	10.30
6	102	grape	5.30
7	103	coconut	9.20
8	101	cherry	3.20
9	103	apricot	2.20
10	104	lemon	6.40
11	104	berry	7.60
12	106	mango	15.60
```

# COUNT(*) 统计行的数量。非NULL值
## 统计id和sid 的行数
```sql
SELECT COUNT(id),COUNT(sid) FROM fruits;

count(id)	count(sid)
12	12
```
## 统计当sid为103时id的行数
```sql
SELECT COUNT(id) FROM fruits WHERE SID = 103;

count(id)
2
```
## DISTINCT 过滤掉重复行
```sql
SELECT COUNT(Sid) AS SID FROM fruits;

SID
12

SELECT COUNT(DISTINCT Sid) AS SID FROM fruits;

SID
6
```

# MAX(*) 最大值
```sql
SELECT MAX(id) FROM fruits;

max(id)
12
```
# MIN(*) 最小值
```sql
SELECT MIN(id) FROM fruits;

min(id)
1
```
# AVG(*) 平均值
```sql
SELECT AVG(price) FROM fruits WHERE ID IN (1,2,3);

AVG(price)
8.866667
```
# SUM(*) 累加和
```sql
SELECT SUM(price) FROM fruits WHERE SID = 102;

SUM(price)
26.80
```
# GROUP BY 组合使用
```sql
SELECT
  MAX(ID),
  MIN(SID),
  COUNT(SID),
  GROUP_CONCAT(NAME),
  AVG(PRICE),
  SUM(PRICE)
FROM
  fruits
GROUP BY SID;

MAX(ID)	MIN(SID)	COUNT(SID)	group_concat(NAME)	AVG(PRICE)	SUM(PRICE)
8	101	3	apple,blackberry,cherry	6.200000	18.60
6	102	3	orange,banana,grape	8.933333	26.80
9	103	2	coconut,apricot	5.700000	11.40
11	104	2	lemon,berry	7.000000	14.00
4	105	1	melon	8.200000	8.20
12	106	1	mango	15.600000	15.60
```