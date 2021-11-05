---
title: MYSQL学习（四）
date: 2021-11-01 18:16:50
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 了解表中列的属性以及配置语句
---

# 列的属性

## 简单的查询和插入的语句

### 简单的查询语句

```sql
SELECT * FROM 表名;
```

### 简单的插入语句

```sql
INSERT INTO 表名(列1, 列2, ...) VALUES(列1的值，列2的值, ...);
```

### 批量插入

```sql
INSERT INTO 表名(列1,列2, ...) VAULES(列1的值，列2的值, ...), (列1的值，列2的值, ...), (列1的值，列2的值, ...), ...;
```

## 列的属性

### 默认值

```sql
列名 列的类型 DEFAULT 默认值
```

```sql
CREATE TABLE test_table (
  column1 INT,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

## NOT NULL 属性

```sql
CREATE TABLE test_table (
  column1 INT NOT NULL,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

## 主键

表中通过某个列或某些列确定的唯一的一条记录的列，可以称为候选键。一个表可以有多个候选键，可以选一个候选键作为主键，主键唯一且不能为空。

**方式一**

```sql
CREATE TABLE student_info (
  number INT PRIMARY KEY,
  name varchar(5),
  set ENUM('男', '女'),
  id_number CHAR(18),
  department VARCHAR(30),
  major VARCHAR(30),
  enrollment_time DATE
);
```

**方式二**

```sql
CREATE TABLE student_info (
  number INT,
  name VARCHAR(5),
  sex ENUM('男', '女'),
  id_number CHAR(18),
  department VARCHAR(30),
  major VARCHAR(30),
  enrollment_time DATE,
  PRIMARY KEY (number)
);
```

**列组合作为主键**

```sql
CREATE TABLE student_score (
  number INT,
  subject VARCHAR(30),
  score TINYINT,
  PRIMARY KEY (number, subject)
);
```

## `UNIQUE` 属性

通过 `UNIQUE` 属性，可以校验某个列或者某个列组合不可重复。

**方式一**

在该列后面直接声明`UNIQUE`属性：

```sql
CREATE TABLE student_info (
  number INT PRIMARY KEY,
  name CHAR(5),
  sex ENUM('男', '女'),
  id_number CHAR(18) UNIQUE,
  department VARCHAR(30),
  major VARCHAR(30),
  enrollment_time DATE
);
```

**方式二**

单独声明`UNIQUE`属性：

```sql
UNIQUE [约束名称] (列名1, 列名2, ...)
```

或者

```sql
UNIQUE KEY [约束名称] (列名1, 列名2, ...)
```

```sql
CREATE TABLE student_info (
  number INT PRIMARY KEY,
  name VARCHAR(5),
  sex ENUM('男', '女'),
  id_number CHAR(18),
  department VARCHAR(30),
  major VARCHAR(30),
  enrollment_time DATE,
  UNIQUE KEY uk_id_number (id_number)
);
```

## 主键和`UNIQUE`约束的区别

主键和 `UNIQUE` 约束都能保证某个列或者列组合的唯一性，但是：

一张表中只能定义一个主键，却可以定义多个 `UNIQUE` 约束！

规定：主键列不允许存放 `NULL`，而声明了 `UNIQUE` 属性的列可以存放 `NULL`，而且 `NULL` 可以重复地出现在多条记录中！

## 外键

```sql
CONSTRAINT [外键名称] FOREIGN KEY(列1, 列2, ...) REFERENCES 父表名(父列1, 父列2, ...);
```

```sql
CREATE TABLE student_score (
  number INT,
  subject VARCHAR(30),
  score TINYINT,
  PRIMARY KEY (number, subject),
  CONSTRAINT FOREIGN KEY(number) REFERENCES student_info(number)
);
```

## `AUTO_INCREMENT`属性

一个表中最多有一个具有 AUTO_INCREMENT 属性的列。

具有 `AUTO_INCREMENT` 属性的列必须建立索引。主键和具有 `UNIQUE` 属性的列会自动建立索引。

拥有 `AUTO_INCREMENT` 属性的列就不能再通过指定 `DEFAULT` 属性来指定默认值。

一般拥有 `AUTO_INCREMENT` 属性的列都是作为主键的属性，来自动生成唯一标识一条记录的主键值。

```sql
列名 列的类型 AUTO_INCREMENT
```

```sql
CREATE TABLE test_table (
  id INT UNSIGEND AUTO_INCREMENT PRIMARY KEY,
  column1 INT,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

## 列的注释

```sql
CREATE TABLE first_table (
  id int UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '自增主键',
  first_column INT COMMENT '第一列',
  second_column VARCHAR(100) DEFAULT 'abc' COMMENT '第二列'
) COMMENT '第一个表';
```

## 一个列同时具有多个属性

每个列可以同时具有多个属性，属性声明的顺序无所谓，各个属性之间用空白隔开

**有的属性是冲突的，一个列不能具有两个冲突的属性，。如一个列不能既声明为 `PRIMARY KEY`，又声明为 `UNIQUE KEY`，不能既声明为 `DEFAULT NULL`，又声明为 NOT NULL。大家在使用过程中需要注意这一点。**

## 标识符的命名

不建议通过以下方式命名：

- 名称中全都是数字
- 名称中有空白字符
- 名称使用了 MySQL 中的保留字
