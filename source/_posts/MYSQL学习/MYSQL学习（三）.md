---
title: MYSQL学习（三）
date: 
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 数据库中，表的基本操作
---


# 表的基本操作

## 展示当前数据库的表

```sql
SHOW TABLES;
```

## 创建表

```sql
CREATE TABLE IF NOT EXISTS 表名(
  列名1 数据类型 [列的属性],
  列名2 数据类型 [列的属性],
  列名3 数据类型 [列的属性],
  ...
  列名n 数据类型 [列的属性],
) COMMENT 注释;
```

## 删除表

```sql
DROP TABLE IF EXISTS 表1, 表2, ..., 表n;
```

## 查看表结构

```sql
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

```sql
ALTER TABLE 旧表名 RENAME TO 新表名;
```

- 方式二

```sql
RENAME TABLE 旧表名1 TO 新表名1, 旧表名2 TO 新表名2, ... 旧表名n TO 新表名n;
```

将表转移到其他数据库下:

```sql
ALTER TABLE 表名 RENAME TO 数据库名.新表名;
```

### 增加列

#### 常规写法

```sql
ALTER TABLE 表名 ADD COLUMN 列名 数据类型 [列的属性];
```

#### 增加到第一列

```sql
ALTER TABLE 表名 ADD COLUMN 列名 列的类型 [列的属性] FIRST;
```

#### 增加到指定列后面

```sql
ALTER TABLE 表名 ADD COLUMN 列名 列的类型 [列的属性] AFTER 指定列名;
```

### 删除列

```sql
ALTER TABLE 表名 DROP COLUMN 列名;
```

### 修改列属性

**修改后的数据类型和属性一定要兼容表中现有的数据**

#### 更改列名及属性

- 方式一

```sql
ALTER TABLE 表名 MODIFY 列名 新数据类型 [新属性];
```

- 方式二

```sql
ALTER TABLE 表名 CHANGE 旧列名 新列名 新数据类型 [新属性];
```

#### 将列设为表的第一列

```sql
ALTER TABLE 表名 MODIFY 列名 列的类型 列的属性 FIRST;
```

#### 将列放到指定列的后边

```sql
ALTER TABLE 表名 MODIFY 列名 列的类型 列的属性 AFTER 指定列名;
```

#### 一条语句包含多个修改操作

```sql
ALTER TABLE 表名 操作1, 操作2, ..., 操作n;
```
