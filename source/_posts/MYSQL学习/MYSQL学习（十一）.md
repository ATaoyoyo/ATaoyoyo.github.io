---
title: MYSQL学习（十一）
date: 2021-11-16 16:54:07
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 组合查询
---

# 组合查询

将多条查询语句产生的结果集合并起来的查询方式称为`合并查询`或者`组合查询`。

## 设计单表的组合查询

最简单的方法，可以使用`OR`操作符连接搜索条件：

```sql
SELECT m1 FROM t1 WHERE m1 < 2 OR m1 > 2;
```

使用`UNION`连接查询语句：

```sql
SELECT m1 FROM t1 WHERE m1 < 2 UNION SELECT m1 FROM t1 WHERE m1 > 2;
```

**建议：使用 UNION 连接起来的各个查询语句的查询列表中位置相同的表达式的类型应该是相同的。**

## 涉及不同表的组合查询

```sql
SELECT m1, n1 FROM t1 WHERE m1 < 2 UNION SELECT m2, n2 FROM t2 WHERE m2 > 2;
```

## 包含或去除重复的行

默认情况下，使用`UNION`查询到的合并数据会自动去除重复的行；可以使用`UNION ALL`操作符保留记录。

```sql
SELECT m1, n1 FROM t1 UNION ALL SELECT m2, n2 FROM t2;
```

## 组合查询中的`ORDER BY`和`LIMIT`子句

在组合查询语句的末尾加上`ORDER BY`与`LIMIT`子句实现排序与条数筛选。

**由于最后的结果集展示的列名是第一个查询中给定的列名，所以 ORDER BY 子句中指定的排序列也必须是第一个查询中给定的列名（别名也可以）。**

```sql
SELECT m1, n1 FROM t1 UNION SELECT m2, n2 FROM t2 ORDER BY m1 DESC LIMIT 2;
```

加上小括号，使语句更加清晰：

```sql
(SELECT m1,n1 FROM t1) UNION (SELECT m2, n2 FROM t2) ORDER BY m1 DESC LIMIT 2;
```

单独在小括号中的查询语句中加入排序是没有效果的，但是可以和`LIMIT`一起使用：

```sql
(SELECT m1, n1 FROM t1 ORDER BY m1 DESC LIMIT 1) UNION (SELECT m2, n2 FROM t2 ORDER BY m2 DESC LIMIT 1);
```

也可以在小括号中的查询语句中使用`LIMIT`筛选：

```sql
(SELECT m1, n1 FROM t1 LIMIT 1) UNION (SELECT m2, n2 FROM t2 LIMIT 1);
```
