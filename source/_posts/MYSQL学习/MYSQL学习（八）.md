---
title: MYSQL学习（八）
date: 2021-11-07 21:15:51
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 关于分组查询
---

# 分组查询

## 分组数据

### 复杂的数据统计

```sql
SELECT 列名, AVG(xxx) FROM 表名 GROUP BY 列名;
```

### 带有 WHERE 子句的分组查询

1. 将记录进行过滤后分组

2. 分别对各个分组进行数据统计

```sql
SELECT 列名, AVG(xxx) FROM 表名 WHERE xxx 条件表达式 具体值 GROUP BY 列名；
```

### 作用于分组的过滤条件

```sql
SELECT 列名, AVG(xxx) FROM 表名 GROUP BY 列名 HAVING AVG(xxx) 条件表达式 具体值;
```

- 分组列

将作用于分组的列放到`HAVING`子句的条件中：

```sql
SELECT 列名, AVG(xxx) FROM 表名 GROUP BY 列名 HAVING 列名 条件表达式 具体值;
```

- 作用于分组的聚合函数

```sql
SELECT 列名, AVG(xxx) FROM 表名 GROUP BY 列名 HAVING MAXZ(xxx) 条件表达式 具体值;
```

### 分组和排序

需要为查询列表中有聚合函数的表达式添加`别名`：

```sql
SELECT 列名, AVG(xxx) AS avg_xxx FROM 表名 GROUP BY 列名 ORDER BY avg_xxx DESC;
```

### 嵌套分组

按照一种类型分成大组，然后根据大组分成小组，以此类推。

```sql
SELECT aaa, bbb, COUNT(*) FROM 表名 GROUP BY aaa, bbb;
```

聚合函数将作用于做后一个分组列（这里是 bbb）上。

### 使用分组注意事项

1. 如果分组列中含有 `NULL` 值，那么 `NULL` 也会作为一个独立的分组存在。

2. 如果存在多个分组列，也就是嵌套分组，聚集函数将作用在最后的那个分组列上。

3. 如果查询语句中存在 `WHERE` 子句和 `ORDER BY` 子句，那么 `GROUP BY` 子句必须出现在 `WHERE` 子句之后，`ORDER BY` 子句之前。

4. 非分组列不能单独出现在检索列表中(可以被放到聚集函数中)。

5. `GROUP BY` 子句后也可以跟随表达式(但不能是聚集函数)。

6. WHERE 子句和 HAVING 子句的区别。

`WHERE` 子句在分组前进行过滤，作用于每一条记录，`WHERE` 子句过滤掉的记录将不包括在分组中。而 `HAVING` 子句在数据分组后进行过滤，作用于整个分组。

## 简单查询语句中各子句的顺序

```sql
SELECT [DISTINCT] 查询列表
[FROM 表名]
[WHERE 布尔表达式]
[GROUP BY 分组列表 ]
[HAVING 分组过滤条件]
[ORDER BY 排序列表]
[LIMIT 开始行, 限制条数]
```

**其中中括号[]中的内容表示可以省略，我们在书写查询语句的时候各个子句必须严格遵守这个顺序，不然会报错的！**
