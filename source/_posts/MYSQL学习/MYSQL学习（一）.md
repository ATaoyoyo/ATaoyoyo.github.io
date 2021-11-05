---
title: MYSQL学习（一）
date: 2021-10-31 17:16:50
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 了解数值类型，字符串类型以及二进制类型
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
