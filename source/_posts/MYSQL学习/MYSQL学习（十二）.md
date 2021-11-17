---
title: MYSQL学习（十二）
date: 2021-11-17 16:34:52
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 数据的插入，删除与更新
---

# 数据的插入，删除与更新

## 插入数据

### 插入完整的记录

```sql
INSERT INTO 表名 VALUES(列1的值, 列2的值, ..., 列n的值);
```

如果不知道某个列具体填什么值，并且没有使用`NOT NULL`声明，可以使用`NULL`代替：

```sql
INSERT INTO 表名 VALUES(列1的值, NULL, ..., 列n的值)
```

可以在表名后面定义插入顺序：

```sql
INSERT INTO 表名(列1, 列2, ..., 列n) VALUES(列1的值, 列2的值, ..., 列n的值);
```

### 插入记录的一部分

插入记录的时候，某些列的值可以被省略，但是这个列必须满足下边列出的某个条件之一：

- 该列允许存储 NULL 值
- 该列有 DEFAULT 属性，给出了默认值

**`INSERT`语句中指定的列顺序可以改变，但是一定要和 `VALUES` 列表中的值一一对应起来。**

### 批量插入记录

如果需要插入多组数据，在`VALUES`后面用小括号`()`将每组数据括起来即可：

```sql
INSERT INTO 表名(列1, 列2, ..., 列n) VALUES(列1的值, 列2的值,..., 列n的值), (列1的值, 列2的值,..., 列n的值), (列1的值, 列2的值,..., 列n的值), ..., (列1的值, 列2的值,..., 列n的值);
```

### 将某个查询的结果集插入表中

```sql
INSERT INTO 表1(表1.列1, 表1.列2) SELECT 表2.列1, 表2.列2 FROM 表2 WHERE 表2.列1 运算符 具体值;
```

例子：

```sql
INSERT INTO second_table(s, i) SELECT second_column, first_column FROM first_table WHERE first_column < 5;
```

具体过程是，先执行查询语句，然后将查询的结果集插入到指定的表中。**需要插入的列，应该与查询列表中的表达式一一对应。比如说`s, i`，对应的就是`seconde_column, first_column`。**

### INSERT IGNORE

对于一些是主键或者具有 UNIQUE 约束的列或者列组合来说，它们不允许重复值的出现。可以使用`INSERT IGNORE`忽略这种束缚。

```sql
INSERT IGNORE INTO first_table(first_column, second_column) VALUES(1, '哇哈哈') ;
```

如果列中已经存在已插入的值，那么这条插入记录会被自动忽略。

### INSERT ON DUPLICATE KEY UPDATE

插入记录时，表中具有`UNIQUE`属性或者`主键`属性的列与该记录不重复时，可以直接插入，否则更新已存在的记录。

语法：

```sql
INSERT ... ON DUPLICATE KEY UPDATE ...
```

举个例子：

```sql
INSERT INTO first_table (first_column, second_column) VALUES(1, '哇哈哈') ON DUPLICATE KEY UPDATE second_column = '雪碧';
```

如果存在`first_column`为`1`的记录，就将该记录中的`second_column`改为`雪碧`。

插入新记录时，对于具有`主键`或者`UNIQUE`属性的列与该记录有重复时，可以使用`VALUES(列名)`来引用新记录中对应列的值。

```sql
INSERT INTO first_table (first_column, second_column) VALUES(1, '哇哈哈') ON DUPLICATE KEY UPDATE second_column = VALUES(second_column);
```

`VALUES(second_column)`代表待插入记录中`second_column`的值，乍一看，跟上面的效果没啥区别，但是插入多条记录时就非常有用了。

```sql
INSERT INTO first_table (first_column, second_column) VALUES(1, '哇哈哈'), (2, '喜之郎') ON DUPLICATE KEY UPDATE second_column = VALUES(second_column);
```

## 删除数据

```sql
DELETE FROM 表名 [WHERE 表达式];
```

**如果不加`WHERE`条件会将所有记录都删除。**

可以使用`LIMIT`子句限制删除记录的数量，使用`ORDER BY`子句指定符合条件的记录的删除顺序。

```sql
DELETE FROM first_table ORDER BY first_column DESC LIMIT 1;
```

## 更新数据

```sql
UPDATE 表名 SET 列1=值1, 列2=值2, ...,  列n=值n [WHERE 布尔表达式];
```

**如果不加`WHERE`条件会更新所有记录。**

同样可以使用`LIMIT`子句限制更新记录的数量，用`ORDER BY`子句指定符合条件的记录的更新顺序。

```sql
UPDATE first_table SET second_column = '爽歪歪' ORDER BY first_column DESC LIMIT 1;
```
