---
title: MYSQL学习（十三）
date: 2021-11-19 10:41:17
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 视图
---

# 视图

每次查询一条记录，就会写一条查询语句，如果一条查询语句非常长，敲完之后发现后续又要查一次，就又要敲一遍，非常麻烦。`视图`可以帮助我们复用这些查询语句。

```sql
CREATE VIEW 视图名 AS 查询语句;
```

可以将视图理解为查询语句的别名。

```sql
CREATE VIEW male_student_view AS SELECT s1.number, s1.name, s1.major FROM student_info AS s1 INNER JOIN student_score AS s2 WHERE s1.number = s2.number AND s1.sex = '男';
```

## 使用视图

`视图`也称为`虚拟表`，可以在`视图`上进行增删改查操作，只不过建立在`视图`的查询语句上。

```sql
SELECT * FROM male_student_view;
```

### 利用视图来创建新视图

```sql
CREATE VIEW by_view AS SELECT number, name FORM male_student_view;
```

### 创建视图时指定自定义别名

在视图名使用小括号扩起自定义列名，需要与查询语句中的列名一一对应。

```sql
CREATE VIEW student_info_view(no, n, m) AS SELECT number, name, major FROM student_info;
```

## 查看和删除视图

### 查看有哪些视图

与查看表的命令一样：

```sql
SHOW TABLES;
```

因为视图是一张`虚拟表`，所以新创建的视图的名称也不能和当前数据库中的其他视图或者表的名称有冲突。

### 查看视图的定义

```sql
SHOW CREATE VIEW 视图名;
```

## 可更新的视图

对视图执行 `INSERT、DELETE、UPDATE` 语句的本质上是对该视图对应的底层表中的数据进行增、删、改操作。

也就是说，改变了视图中的数据，实际上是改变的视图对应的表中的数据。

不过并不是可以在所有的视图上执行更新语句的，在生成视图的时候使用了下边这些语句的都不能进行更新：

- 聚集函数（比如 SUM(), MIN(), MAX(), COUNT()等等）
- DISTINCT
- GROUP BY
- HAVING
- UNION 或者 UNION ALL
- 某些子查询
- 某些连接查询
- 等等等等

### 删除视图

```sql
DROP VIEW 视图名;
```
