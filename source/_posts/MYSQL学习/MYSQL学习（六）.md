---
title: MYSQL学习（六）
date: 2021-11-01 20:16:50
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 数据库中，表的基本操作
---

# 带搜索条件多查询

## 简单搜索条件

```sql
SELECT * FROM 表名 WHERE xxx 比较操作符 xxx;
```

| 操作符        | 操作符                  | 描述           |
| ------------- | ----------------------- | -------------- |
| `=`           | `a = b`                 | a 等于 b       |
| `<>`或者`!=`  | `a <> b`                | a 不等于 b     |
| `<`           | `a < b`                 | a 小于 b       |
| `<=`          | `a <= b`                | a 小于等于 b   |
| `>`           | `a > b`                 | a 大于 b       |
| `>=`          | `a >= b`                | a 大于等于 b   |
| `BETWEEN`     | `a BETWEEN b AND c`     | 满足 a<=b<=c   |
| `NOT BETWEEN` | `a NOT BETWEEN b AND c` | 不满足 b<=a<=c |

## 匹配列表中的元素

匹配到列表中的某一项就算匹配成功。

```sql
SELECT * FROM 表名 WHERE xxx IN xxx;
```

| 操作符   | 示例                     | 描述                            |
| -------- | ------------------------ | ------------------------------- |
| `IN`     | `a IN (b1, b2, ...)`     | a 是 b1, b2, ... 中的某一个     |
| `NOT IN` | `a NOT IN (b1, b2, ...)` | a 不是 b1, b2, ... 中的任意一个 |

## 匹配`NULL`值

判断某一列是否没有值。

| 操作符        | 示例            | 描述            |
| ------------- | --------------- | --------------- |
| `IS NULL`     | `a IS NULL`     | `a的值是NULL`   |
| `IS NOT NULL` | `a IS NOT NULL` | `a的值不是NULL` |

## 多个搜索条件查询

### `AND`操作符

```sql
SELECT * FROM 表名 WHERE a 比较操作符 xxx AND b 比较操作符 xxx；
```

### `OR`操作符

只满足其中一个条件即可。

```sql
 SELECT * FROM 表名 WHERE a 比较操作符 OR xxx b 比较操作符 xxx;
```

### 条件复杂的组合搜索

`AND`的优先级高于`OR`，在查询是需要用小括号标记。

```sql
SELECT * FROM 表名 WHERE (xxx 比较操作符 OR xxx 比较操作符 xxx) AND xxx 比较操作符 xxx;
```

## 通配符

以下操作符支持`模糊查询`：

| 操作符     | 示例           | 描述       |
| ---------- | -------------- | ---------- |
| `LIKE`     | `a LIKE b`     | a 匹配 b   |
| `NOT LIKE` | `a NOT LIKE b` | a 不匹配 b |

以下为通配符：

- `%`：代表任意一个字符串

```sql
SELECT * FROM 表名 WHERE xxx LIKE 'x%';
SELECT * FROM 表名 WHERE xxx LIKE '%x%';
```

- `_`：代表任意一个字符

```sql
SELECT * FROM 表名 WHERE xxx LIKE '李_'
```

### 转义通配符

通过加`\`来区分转义通配符与通配符。

- `'\%'`代表普通字符`'%'`
- `'\_'`代表普通字符`'\_'`

```sql
SELECT * FROM 表名 WHERE xxx LIKE '李\%';
```
