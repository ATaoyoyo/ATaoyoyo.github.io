---
title: MYSQL学习
date: 2021-10-27 20:49:37
tags:
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 攻克一下MYSQL...
---

# 数值类型

## 数据类型

1 字节等于 8 比特。

### 整数类型

- TINYINT
- SMALLINT
- MEDIUMINT
- INT(INTEGER)
- BIGINT

### 浮点数类型

- FLOAT 4 字节
- DOUBLE 8 字节

### 日期和时间类型

- YEAR 1 字节
- DATE 3 字节
- TIME 3 字节
- DATETIME 8 字节 日期加时间值
- TIMESTAMP 4 字节 时间戳

# 字符串类型

分为`可见字符`与`不可见字符`。

## 字符串

- CHAR 0~255
- VARCHAR
- TINYTEXT
- TEXT
- MEDIUMTEXT
- LONGTEXT

# 二进制类型

## BIT 类型

- BIT
- BINARY
- VARBINARY

# 数据库操作

## 创建数据库

```
CREATE DATABASE 数据库名;

CREATE DATABASE IF NOT EXISTS 数据库名;
```

## 切换数据库

```
USE 数据库名;
```

## 删除数据库

```
DROP DATABASE 数据库名;

DROP DATABASE IF EXISTS 数据库名;
```

## 查看当前使用的数据库

```
SELECT DATABASE();
```

# 表的基本操作

## 展示当前数据库的表

```
SHOW TABLES;
```

## 创建表

```
CREATE TABLE IF NOT EXISTS 表名(
  列名1 数据类型 [列的属性],
  列名2 数据类型 [列的属性],
  列名3 数据类型 [列的属性],
  ...
  列名n 数据类型 [列的属性],
) COMMENT 注释;
```

## 删除表

```
DROP TABLE IF EXISTS 表1, 表2, ..., 表n;
```

## 查看表结构

```
DESCRIBE 表名;
DESC 表名;
EXPLAIN 表名;
SHOW COLUMNS FROM 表名;
SHOW FIELDS FROM 表名;

SHOW CREATE TABLE 表名\G; 查看创建结构语句

SHOW TABLES FORM 数据库名.表名\G; 没有指定数据库时查看
```

## 修改表

### 修改表名

- 方式一

```
ALTER TABLE 旧表名 RENAME TO 新表名;
```

- 方式二

```
RENAME TABLE 旧表名1 TO 新表名1, 旧表名2 TO 新表名2, ... 旧表名n TO 新表名n;
```

将表转移到其他数据库下:

```
ALTER TABLE 表名 RENAME TO 数据库名.新表名;
```

### 增加列

#### 常规写法

```
ALTER TABLE 表名 ADD COLUMN 列名 数据类型 [列的属性];
```

#### 增加到第一列

```
ALTER TABLE 表名 ADD COLUMN 列名 列的类型 [列的属性] FIRST;
```

#### 增加到指定列后面

```
ALTER TABLE 表名 ADD COLUMN 列名 列的类型 [列的属性] AFTER 指定列名;
```

### 删除列

```
ALTER TABLE 表名 DROP COLUMN 列名;
```

### 修改列属性

**修改后的数据类型和属性一定要兼容表中现有的数据**

#### 更改列名及属性

- 方式一

```
ALTER TABLE 表名 MODIFY 列名 新数据类型 [新属性];
```

- 方式二

```
ALTER TABLE 表名 CHANGE 旧列名 新列名 新数据类型 [新属性];
```

#### 将列设为表的第一列

```
ALTER TABLE 表名 MODIFY 列名 列的类型 列的属性 FIRST;
```

#### 将列放到指定列的后边

```
ALTER TABLE 表名 MODIFY 列名 列的类型 列的属性 AFTER 指定列名;
```

#### 一条语句包含多个修改操作

```
ALTER TABLE 表名 操作1, 操作2, ..., 操作n;
```

## 列的属性

### 简单的查询和插入的语句

#### 简单的查询语句

```
SELECT * FROM 表名;
```

#### 简单的插入语句

```
INSERT INTO 表名(列1, 列2, ...) VALUES(列1的值，列2的值, ...);
```

#### 批量插入

```
INSERT INTO 表名(列1,列2, ...) VAULES(列1的值，列2的值, ...), (列1的值，列2的值, ...), (列1的值，列2的值, ...), ...;
```

### 列的属性

#### 默认值

```
列名 列的类型 DEFAULT 默认值
```

```
CREATE TABLE test_table (
  column1 INT,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

### NOT NULL 属性

```
CREATE TABLE test_table (
  column1 INT NOT NULL,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

### 主键

表中通过某个列或某些列确定的唯一的一条记录的列，可以称为候选键。一个表可以有多个候选键，可以选一个候选键作为主键，主键唯一且不能为空。

**方式一**

```
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

```
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

```
CREATE TABLE student_score (
  number INT,
  subject VARCHAR(30),
  score TINYINT,
  PRIMARY KEY (number, subject)
);
```

### `UNIQUE` 属性

通过 `UNIQUE` 属性，可以校验某个列或者某个列组合不可重复。

**方式一**

在该列后面直接声明`UNIQUE`属性：

```
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

```
UNIQUE [约束名称] (列名1, 列名2, ...)
```

或者

```
UNIQUE KEY [约束名称] (列名1, 列名2, ...)
```

```
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

### 主键和`UNIQUE`约束的区别

主键和 `UNIQUE` 约束都能保证某个列或者列组合的唯一性，但是：

一张表中只能定义一个主键，却可以定义多个 `UNIQUE` 约束！

规定：主键列不允许存放 `NULL`，而声明了 `UNIQUE` 属性的列可以存放 `NULL`，而且 `NULL` 可以重复地出现在多条记录中！

### 外键

```
CONSTRAINT [外键名称] FOREIGN KEY(列1, 列2, ...) REFERENCES 父表名(父列1, 父列2, ...);
```

```
CREATE TABLE student_score (
  number INT,
  subject VARCHAR(30),
  score TINYINT,
  PRIMARY KEY (number, subject),
  CONSTRAINT FOREIGN KEY(number) REFERENCES student_info(number)
);
```

### `AUTO_INCREMENT`属性

一个表中最多有一个具有 AUTO_INCREMENT 属性的列。

具有 `AUTO_INCREMENT` 属性的列必须建立索引。主键和具有 `UNIQUE` 属性的列会自动建立索引。

拥有 `AUTO_INCREMENT` 属性的列就不能再通过指定 `DEFAULT` 属性来指定默认值。

一般拥有 `AUTO_INCREMENT` 属性的列都是作为主键的属性，来自动生成唯一标识一条记录的主键值。

```
列名 列的类型 AUTO_INCREMENT
```

```
CREATE TABLE test_table (
  id INT UNSIGEND AUTO_INCREMENT PRIMARY KEY,
  column1 INT,
  column2 VARCHAR(100) DEFAULT 'abc'
);
```

### 列的注释

```
CREATE TABLE first_table (
  id int UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '自增主键',
  first_column INT COMMENT '第一列',
  second_column VARCHAR(100) DEFAULT 'abc' COMMENT '第二列'
) COMMENT '第一个表';
```

### 一个列同时具有多个属性

每个列可以同时具有多个属性，属性声明的顺序无所谓，各个属性之间用空白隔开

**有的属性是冲突的，一个列不能具有两个冲突的属性，。如一个列不能既声明为 `PRIMARY KEY`，又声明为 `UNIQUE KEY`，不能既声明为 `DEFAULT NULL`，又声明为 NOT NULL。大家在使用过程中需要注意这一点。**

### 标识符的命名

不建议通过以下方式命名：

- 名称中全都是数字
- 名称中有空白字符
- 名称使用了 MySQL 中的保留字

## 简单查询

### 查询单个列

```
SELECT 列名 FROM 表名;
```

#### 列的别名

```
SELECT 列名 [AS] 列的别名 FROM 表名;
```

### 查询多个列

```
SELECT 列名1, 列名2, ... 列名n FROM 表名;
```

### 查询所有列

```
SELECT * FROM 表名;
```

### 查询结果去重

#### 去除单列的重复结果

```
SELECT DISTINCT 列名 FROM 表名;
```

#### 去除多列的重复结果

```
SELECT DISTINCT 列名1, 列名2, ... 列名n  FROM 表名;
```

### 限制查询结果条数

```
LIMIT 开始行, 限制条数;
```

### 对查询结果排序

ASC: 由小到大，生序
DESC：由大到小，降序

```
ORDER BY 列名 ASC|DESC
```

#### 按照多个列的值进行排序

```
ORDER BY 列1 ASC|DESC, 列2 ASC|DESC ...
```
