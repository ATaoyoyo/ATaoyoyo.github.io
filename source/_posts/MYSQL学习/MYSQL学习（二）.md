---
title: MYSQL学习（二）
date: 
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 常用的数据库操作
---


# 数据库操作

## 创建数据库

````sql
CREATE DATABASE 数据库名;

CREATE DATABASE IF NOT EXISTS 数据库名

## 切换数据库

```sql
USE 数据库名;
````

## 删除数据库

```sql
DROP DATABASE 数据库名;

DROP DATABASE IF EXISTS 数据库名;
```

## 查看当前使用的数据库

```sql
SELECT DATABASE();
```
