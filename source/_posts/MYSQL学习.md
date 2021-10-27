---
title: MYSQL学习
date: 2021-10-27 20:49:37
tags:
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
