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
