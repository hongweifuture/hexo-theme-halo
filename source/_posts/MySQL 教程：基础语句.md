---
title: MySQL 教程：基础语句
author: hongwei
top: false
cover: false
coverImg: /medias/featureimages/3.jpg
toc: true
mathjax: false
summary: MySQL 基础语句，MySQL 是一种关系型数据库管理系统，其速度快，灵活性高，广泛应用在 WEB 方面。
categories:
  - 数据库
tags:
  - MySQL
  - databases
  - 数据库
  - web
  - sql基础
urlname: databases-mysql-base-sql
date: 2018-10-11 17:14:58
img:
password:
updated:
---


# 数据库基础操作
## 开启MySQL服务
```bash
net start mysql
```
## 关闭MySQL服务
```bash
net stop mysql
```
## 登陆MySQL
```bash
mysql -u帐号　-p 输入密码
mysql -uroot -p
```
## 查看所有数据库
```sql
show databases;
```
## 创建数据
```sql
 create database zhao charset utf8;
```
## 删除数据库
```sql
drop database ZHAO;
```

# 数据表
## 创建数据表
```
格式：CREATE TABLE 数据表名(

字段名1　　数据类型[列级别约束条件],

字段名2　　数据类型[列级别约束条件],

字段名3　　数据类型[列级别约束条件]);　　　　　
```
注意：格式不一定需要这样隔着写，完全可以全部写成一行。但是那样写可观性非常差。我这样写只是为了可以看的更清晰。

解释：1、[ ]中括号中的内容表示可以有可以没有，2、列级别这个“列”一定要搞清楚说的是什么，一张表中有行有列，列表示竖，行表示横　3、约束条件后面会讲到

## 创建没有约束的student表
```sql
CREATE TABLE student (id INT (11), NAME VARCHAR (12), age INT (11));
```
注释：SHOW TABLES 可以查询数据库底下的所有表。

## 创建有约束的student表　　　　　　　　　　　　
- 主键约束
- 外键约束
- 非空约束
- 唯一约束
- 默认约束
- 自动增加

### 主键约束

**PRIMARY KEY(primary key)**

独一无二(唯一)和不能为空(非空)，通俗的讲，就是在表中增加记录时，在该字段下的数据不能重复，不能为空，比如以上面创建的表为例子，在表中增加两条记录，如果id字段用了主键约束。则id不能一样，并且不能为空。一般每张表中度有一个字段为主键，唯一标识这条记录。以后需要找到该条记录也可以同这个主键来确认记录，因为主键是唯一的，并且非空，一张表中每个记录的主键度不一样，所以根据主键也就能找到对应的记录。而不是多条重复的记录。如果没有主键，那么表中就会存在很多重复的记录，那么即浪费存储空间，在查询时也消耗更多资源。

一般被主键约束了的字段度习惯性的称该字段为该表的主键

**单字段主键约束**

两种方式都可以
```sql
CREATE TABLE student (id INT(11) PRIMARY KEY,NAME VARCHAR(12),age  INT(11));　

CREATE TABLE student(id INT(11),NAME VARCHAR(12),age INT(11), PRIMARY KEY(id));
```

**多字段主键约束(复合主键)**

这个id和name都市主键，说明在以后增加的插入的记录中，id和name不能同时一样，比如说可以是这样。一条记录为id=1，name=yyy、另一条记录为：id=1，name=zzz。 这样是可以的。并不是你们所理解的两个字段分别度不可以相同。
```sql
CREATE TABLE student (id INT(11) PRIMARY KEY,NAME VARCHAR(12) PRIMARY KEY,age  INT(11));

CREATE TABLE student(id INT(11),NAME VARCHAR(12),age INT(11),PRIMARY KEY(id,NAME));
```

### 外键约束
什么是外键举个例子就清楚了，有两张表，一张表是emp(员工)表，另一张表是dept(部门)表，一个员工属于一个部门，那么如何通过员工能让我们自己他在哪个部门呢？那就只能在员工表中增加一个字段，能代表员工所在的部门，那该字段就只能是存储dept中的主键了(因为主键是唯一的，才能确实是哪个部门，进而代表员工所在的部门，如果是部门名称，有些部门的名称可能是同名。就不能区分了。)，像这样的字段，就符合外键的特点，就可以使用外键约束，使该字段只能够存储另一张表的主键。如果不被外键约束，那么该字段就无法保证存储进来的值就一定是另一张表的主键值。

**外键约束的特点**
1. 外键约束可以描述任意一个字段(包括主键)，可以为空，并且一个表中可以有多个外键。但是外键字段中的值必须是另一张表中的主键。
2. 这样被外键关联的两种表的关系可以称为父子表或者主从表。子表(从表)拥有外键字段的表，父表(主表)被外键字段所指向的表。
3. 子表被外键约束修饰的字段必须和父表的主键字段的类型一样。

注意：一个表中有被外键修饰的字段，就称该表有外键(是“有外键”。而不是“是外键”)，并会给该表中的外键约束取一个名称，所以我们常说的这个表有没有外键，指的不是被外键约束修饰的字段名，而是指这个表是否有存在外键约束。也就是说，不能说这个表的外键是xxx(该表中被外键约束修饰的字段名)，这种说法是错误的，但是大多数人已经习惯了这样，虽然影响不大，但是在很多时候需要理解一个东西时，会造成一定的困扰。
```sql
格式：CONSTRAINT　　外键名称　　FOREIGN KEY(被外键约束的字段名称)　　REFERENCES  主表名(主键字段)

英文解释：CONSTRAINT:约束　　　　REFERENCES:参考
```

```sql
CREATE TABLE studentA (
  id INT (11),
  NAME VARCHAR (12),
  location VARCHAR (50),
  PRIMARY KEY (id)
);

CREATE TABLE studentB (
  id INT (11),
  NAME VARCHAR (12) NOT NULL,
  deptId INT (11),
  PRIMARY KEY (id),
  CONSTRAINT tableA_tableB_1 FOREIGH KEY (deptId) REFERENCES tableA (id)
);
```
解释：tableB中有一个名为tableA_tableB_1的外键关联了tableA和tableB两个表，被外键约束修饰的字段为tableB中的deptId，主键字段为tableA中的id　　　　　　　　　　　　　　　　　　　

### 非空约束
**NOT NULL**

被该约束修饰了的字段，就不能为空，主键约束中就包括了这个约束

```sql
CREATE TABLE studentA (
  id INT (11),
  NAME VARCHAR (12),
  location VARCHAR (50),
  PRIMARY KEY (id)
);
```

### 唯一约束
**UNIQUE**  

被唯一约束修饰了的字段，表示该字段中的值唯一，不能有相同的值，通俗点讲，就好比插入两条记录，这两条记录中处于该字段的值不能是一样的。

```sql
CREATE TABLE studentA (
  id INT (11),
  NAME VARCHAR (12) UNIQUE,
  location VARCHAR (50),
  PRIMARY KEY (id)
);
```
也就是说在插入的记录中，每条记录的name值不能是一样的。

### 默认约束
**Default**

指定这一列的默认值为多少，比如，男性同学比较多，性别就可以设置为默认男，如果插入一行记录时，性别没有填，那么就默认加上　

```sql
CREATE TABLE studentA (
  id INT (11) PRIMARY KEY,
  NAME VARCHAR (12) NOT NULL,
  eptId INT(11) DEFAULT 1111,
  salary FLOAT
);
```

### 自动增加　　
**AUTO_INCREMENT**

一个表只能一个字段使用AUTO_INCREMENT，并且使用这个约束的字段只能是整数类型(任意的整数类型     TINYINT,SMALLIN,INT,BIGINT)，默认值是1，也就是说从1开始增加的。一般就是给主键使用的，自动增加，使每个主键的值度不一样，并且不用我们自己管理，让主键自己自动生成

```sql
CREATE TABLE studentA (
  id INT(11) PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR (12) NOT NULL,
);
```

## 查看表结构
### 查看表基本结构语句
```sql
格式1：DESCRIBE 表名

DESCRIBE　student;

格式2：DESC 表名

DESC student;
```
### 查看创建表的语句　
```sql
格式1：SHOW CREATE TABLE 表名

SHOW CREATE TABLE student;

格式2：SHOW CREATE TABLE 表名\G

SHOW CREATE TABLE student\G;
```
## 修改数据表　　　　　　
修改数据表包括：对表中字段的增加、删除、修改。  在这个里面用的关键字为 ALTER
### 修改表名
```sql
格式：ALTER TABLE<旧表名> RENAME[TO]<新表名>;

将student表名改为student1(改完后在改回来)

ALTER TABLE student RENAME TO student1;
```
### 修改表中的字段名
```sql
格式：ALTER TABLE<表名> CHANGE<旧字段名><新字段名><新数据类型>

将student表中的name字段名改为 username

ALTER TABLE student CHANGE name username VARCHAR(30);
```
解释：这个不仅能改变字段名，还能将字段的数据类型一并修改，也就是说，你可以单纯的只修改字段名，也可以单纯的只修改数据类型，也可以同时一起修改

### 修改表中的数据类型
```sql　　　　　　
格式：ALTER TABLE<表名> MODIFY<字段名><数据类型>　　　　　　　　　　　　　

ALTER TABLE student MODIFY username VARCHAR(20);
```
解释：只能修改字段名的数据类型，但是其原理跟上面change做的事情一样，这里也有修改字段名的过程，只不过修改后的字段名和修改前的字段名相同，但是数据类型不一样。
### 修改字段的排列位置
```sql
方式1：ALTER TABLE<表名> MODIFY<字段1><数据类型> FIRST|AFTER<字段2>

ALTER TABLE student MODIFY username VARCHAR(20) AFTER age;
```
解释：将字段1的位置放到第一，或者放到指定字段2的后面
```sql
方式2：ALTER TABLE<表名> CHANGE<字段1><字段2><数据类型> FIRST|AFTER<字段3>　

ALTER TABLE student CHANGE username username VARCHAR(20) AFTER age;
```
解释：其实是一样的，将是字段2覆盖字段1，然后在进行排序

**总结**

CHANGE和MODIFY的区别？

原理都是一样的，MODIFY只能修改数据类型，但是CHANGE能够修改数据类型和字段名，也就是说MODIFY是CHANGE的更具体化的一个操作。可能觉得用CHANGE只改变一个数据类型不太爽，就增加了一个能直接改数据类型的使用关键字MODIFY来操作。
## 添加字段
```sql
格式：ALTER TABLE<表名称> ADD<新字段名><数据类型>[约束条件][FIRST|AFTER<已存在的表名>]

解释：在一个特定位置增加一个新的字段，如果不指定位置，默认是最后一个。

ALTER TABLE student ADD sex VARCHAR(11);
```
## 删除字段
```sql
格式：ALTER TABLE<表名称> DROP<字段名>;

ALTER TABLE student DROP sex;
```
## 删除表的外键约束
```　sql
格式：ALTER TABLE<表名称> DROP FOREIGN KEY<外键约束名>
```
注意：外键约束名 指的不是被外键约束修饰的字段名，切记，而是我们在创建外键约束关系时取的名字。

## 更改表的存储引擎
```sql
格式：ALTER TABLE<表名> ENGINE=<更改后的存储引擎名>
```
这个存储引擎目前我自己也不太清楚，虽然知道有哪几种引擎，但是稍微深入一点就不清楚了，所以打算留到日后在说。
# 删除表
## 删除无关联表
```　sql
格式：DROP TABLE<表名>；

DROP TABLE student;
```
## 删除被其他表关联的主表
这个是比较重要的一点，在有外键关联关系的两张表中，如果删除主表，那么是删不掉的，并且会报错。因为有张表依赖于他。那怎么办呢？针对这种情况，总共有两种方法
1. 先删除你子表，然后在删除父表，这样就达到了删除父表的目的，但是子表也要被删除
2. 先解除外键关系，然后在删除父表，这样也能达到目的，并且保留了子表，只删除我们不需要的父表。在3.7中就讲解了如何删除外键关系。

# 总结
讲了这么多，但实际中，用到的并不是很多，特别是对表结构的修改的操作，在实际开发中，一般数据库表被定义下来了，就不会在修改了，发现数据库表设计的不好，也是将表全部删除，然后在重新创建过新表。但是在我们学习的过程中，这些操作还是很重要的，因为需要这些基础来学习后面更深入的东西，不可能因为实际中不用，就不学这不学那，要相信，不管做什么，那肯定是有意义的事情，可能那意义并不大，但是日后肯定会对我们有所帮助。

　　　　　　　　　　　　　　　　　　　　　　　　　　