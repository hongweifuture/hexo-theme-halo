---
title: MySQL 教程：单表查询
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/3.jpg
toc: true
mathjax: false
summary: MySQL的单表查询。MySQL是一种关系型数据库管理系统，其速度快，灵活性高，广泛应用在 WEB 方面。
categories:
  - 数据库
tags:
  - MySQL
  - databases
  - 数据库
  - web
  - sql单表查询
urlname: databases-mysql-singl-query
date: 2018-10-11 17:22:38
img:
password:
updated:
---

# 创建数据库并插入数据
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

```
# 单表查询
## 查询所有字段
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
## 查询指定字段
```sql
SELECT ID,NAME FROM fruits;

ID	NAME
1	apple
2	blackberry
3	orange
4	melon
5	banana
6	grape
7	coconut
8	cherry
9	apricot
10	lemon
11	berry
12	mango
```
## 查询指定条件的字段
```sql
SELECT * FROM fruits WHERE NAME = 'BANANA';

id	sid	name	price
5	102	banana	10.30
```
## 带IN关键字的查询
IN关键字：IN(xx，yy，...) 满足条件范围内的一个值即为匹配项，括号内的值，或的关系
```sql
SELECT * FROM fruits WHERE NAME IN ('BANANA','ORANGE');

id	sid	name	price
3	102	orange	11.20
5	102	banana	10.30
```

```sql
SELECT * FROM fruits WHERE ID NOT IN (3,8);

id	sid	name	price
1	101	apple	5.20
2	101	blackberry	10.20
4	105	melon	8.20
5	102	banana	10.30
6	102	grape	5.30
7	103	coconut	9.20
9	103	apricot	2.20
10	104	lemon	6.40
11	104	berry	7.60
12	106	mango	15.60
```
## 带BETWEEN AND 的范围查询
BETWEEN ... AND ... : 在...到...范围内的值即为匹配项
```sql
SELECT * FROM fruits WHERE ID BETWEEN 3 AND 9;

id	sid	name	price
3	102	orange	11.20
4	105	melon	8.20
5	102	banana	10.30
6	102	grape	5.30
7	103	coconut	9.20
8	101	cherry	3.20
9	103	apricot	2.20
```

```sql
SELECT * FROM fruits WHERE ID NOT BETWEEN 5 AND 11;

id	sid	name	price
1	101	apple	5.20
2	101	blackberry	10.20
3	102	orange	11.20
4	105	melon	8.20
12	106	mango	15.60
```
## 带LIKE的字符匹配查询
LIKE: 模糊查询，和LIKE一起使用的通配符有  "%"、"_"  

通配符|功能|
--|--|
"%" |作用是能匹配任意长度的字符。|
"_" |只能匹配任意一个字符|

```sql
SELECT * FROM fruits WHERE NAME LIKE 'black%';

id	sid	name	price
2	101	blackberry	10.20
```

```sql
SELECT * FROM fruits WHERE NAME LIKE 'b%y';

id	sid	name	price
2	101	blackberry	10.20
11	104	berry	7.60
```

```sql
SELECT * FROM fruits WHERE NAME LIKE '_ER_Y';

id	sid	name	price
11	104	berry	7.60
```
# 逻辑与之带AND的多条件查询
and：同时满足条件
```sql
SELECT
  *
FROM
  fruits
WHERE id IN (2, 4, 6, 8, 10)
  AND sid > 102;
  
id	sid	name	price
4	105	melon	8.20
10	104	lemon	6.40
```
# 逻辑或之OR的多条件查询
OR:有一个满足即可，类似in
```sql
SELECT
  *
FROM
  fruits
WHERE id IN (2, 4, 6, 8, 10)
  OR sid > 102;
  
id	sid	name	price
2	101	blackberry	10.20
4	105	melon	8.20
6	102	grape	5.30
7	103	coconut	9.20
8	101	cherry	3.20
9	103	apricot	2.20
10	104	lemon	6.40
11	104	berry	7.60
12	106	mango	15.60
```
# 关键字DISTINCT查询不重复的数据
```sql
SELECT
  DISTINCT SID
FROM
  fruits
WHERE id IN (2, 4, 6, 8, 10)
  OR sid > 102;
  
SID
101
105
102
103
104
106
```
# ORDER BY对查询的结果排序
## ORDER BY 字段 DESC 逆序排列
```sql
SELECT DISTINCT
  SID
FROM
  fruits
WHERE id IN (2, 4, 6, 8, 10)
  OR sid > 102
ORDER BY sid DESC;

SID
106
105
104
103
102
101
```
## ORDER BY 字段 ASC 正序排列，默认为正
```sql
SELECT DISTINCT
  SID
FROM
  fruits
WHERE id IN (2, 4, 6, 8, 10)
  OR sid > 102
ORDER BY sid ASC;

SID
101
102
103
104
105
106
```
# GROUP BY 对查询结果进行分组
## 不分组
```sql
SELECT SID FROM fruits;

SID
101
101
102
105
102
102
103
101
103
104
104
106
```
## 将相同的内容分到同一个组里面
> 分组之后，重复的都被分到一组

```sql
SELECT SID FROM fruits GROUP BY SID ;

SID
101
102
105
103
104
106
```
## GROUP_CONCAT查看分组后的数目和内容
查看分组中的各个字段内容 GROUP_CONCAT( )

```sql
SELECT SID,COUNT(NAME),GROUP_CONCAT(NAME) FROM fruits GROUP BY sid;

SID	count(name)	group_concat(name)
101	3	apple,blackberry,cherry
102	3	orange,banana,grape
103	2	coconut,apricot
104	2	lemon,berry
105	1	melon
106	1	mango
```
## HAVING条件过滤，相当于WHERE，只能分组用
```sql
SELECT SID,COUNT(NAME),GROUP_CONCAT(NAME) FROM fruits GROUP BY sid HAVING SID > 103;

SID	count(name)	group_concat(name)
104	2	lemon,berry
105	1	melon
106	1	mango
```
# LIMIT 限制查询结果的数量
> LIMIT 位置偏移量,行数 
> 默认位置偏移量为0，即第1行

通过LIMIT可以选择数据库表中的任意行数，也就是不用从第一条记录开始遍历，可以直接拿到 第5条到第10条的记录，也可以直接拿到第12到第15条的记录。
```sql
SELECT * FROM fruits WHERE ID LIMIT 0,5;

id	sid	name	price
1	101	apple	5.20
2	101	blackberry	10.20
3	102	orange	11.20
4	105	melon	8.20
5	102	banana	10.30

SELECT * FROM fruits WHERE ID LIMIT 5,9;

id	sid	name	price
6	102	grape	5.30
7	103	coconut	9.20
8	101	cherry	3.20
9	103	apricot	2.20
10	104	lemon	6.40
11	104	berry	7.60
12	106	mango	15.60
```