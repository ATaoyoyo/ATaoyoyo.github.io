---
title: MYSQL学习（九）
date: 2021-11-08 16:43:34
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 关于子查询
---

# 自查询

## 多表查询的需求

前面的学习都是在单表中完成查询，但有时候会从多个表中查询数据。比如在学生信息表中查找某个同学的学号，然后根据学号在学生成绩表中查询该同学的所有成绩。使用单表查询就得写两条语句，如果有更多的查询，就非常的麻烦。

## 标量子查询

```sql
SELECT * FROM 表名1 WHERE xxx 条件表达式 (SELECT xxx FROM 表名2 WHERE yyy 条件表达式 具体值);
```

小括号中的查询语句称为`子查询`或者`内层查询`，使用`内层查询`的结果作为搜索条件的操作数的查询称为`外层查询`。**所有的子查询都必须用小括号扩起来**

若子查询的结果只有一个值，那么这种自查询叫做`标量自查询`，`标量自查询`只单纯的代表一个值。

```sql
SELECT (SELECT xxx FROM 表名 WHERE yyy 条件表达式 具体值) AS alias;
```

## 列子查询

列子查询的结果为多个值。

```sql
SELECT * FROM 表名1 WHERE xxx IN (SELECT xxx FROM 表名2 WHERE bbb 条件表达式 具体值);
```

## 行子查询

子查询的结果集中最多只包含一条记录，而且这条记录中超过一个列的数据，这个子查询称为`行子查询`。

```sql
SELECT * FROM 表名1 WHERE (aaa, bbb) = (SELECT aaa, 具体值 FROM 表名2 LIMIT 1);
```

## 表子查询

子查询结果集中包含多行多列，这个子查询称为`表子查询`。

```sql
SELECT * FROM 表名1 WHERE (aaa, bbb) IN (SELECT aaa, 具体值1 FROM 表名2 WHERE ccc = 具体值2);
```

## EXISTS 和 NOT EXISTS 子查询

查看子查询是否为空集。

| 操作符       | 示例                     | 描述                               |
| ------------ | ------------------------ | ---------------------------------- |
| `EXISTS`     | `EXISTS (SELECT ...)`    | 当子查询结果集不是空集时表达式为真 |
| `NOT EXISTS` | `NOT EXISTS(SELECT ...)` | 当子查询结果集是空集时表达式为真   |

## 不相关子查询和相关子查询

### 不相关子查询

子查询单独运行并产生结果之后，作为外层查询的条件去执行外层查询，这种子查询称为`不相关子查询`。

### 相关子查询

子查询的语句中需要引用到外层查询的值，这种子查询称为`相关子查询`。

```sql
SELECT * FROM 表名1 WHERE EXISTS (SELECT * FROM 表名1 WHERE 表名1.xxx = 表名2.xxx);
```
