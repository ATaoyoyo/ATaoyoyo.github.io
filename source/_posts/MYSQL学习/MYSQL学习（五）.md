---
title: MYSQL学习（五）
date: 2021-11-01 19:16:50
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 对于表结构的简单查询语句
---

# 简单查询

## 查询单个列

```sql
SELECT 列名 FROM 表名;
```

### 列的别名

```sql
SELECT 列名 [AS] 列的别名 FROM 表名;
```

## 查询多个列

```sql
SELECT 列名1, 列名2, ... 列名n FROM 表名;
```

## 查询所有列

```sql
SELECT * FROM 表名;
```

## 查询结果去重

### 去除单列的重复结果

```sql
SELECT DISTINCT 列名 FROM 表名;
```

### 去除多列的重复结果

```sql
SELECT DISTINCT 列名1, 列名2, ... 列名n  FROM 表名;
```

## 限制查询结果条数

```sql
LIMIT 开始行, 限制条数;
```

## 对查询结果排序

ASC: 由小到大，生序
DESC：由大到小，降序

```sql
ORDER BY 列名 ASC|DESC
```

### 按照多个列的值进行排序

```sql
ORDER BY 列1 ASC|DESC, 列2 ASC|DESC ...
```
