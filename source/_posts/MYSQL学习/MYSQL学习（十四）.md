---
title: MYSQL学习（十四）
date: 2021-11-22 09:52:24
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 自定义变量和语句结束分隔符
---

# 自定义变量和语句结束分隔符

## 存储程序

使用`储存程序`可以封装一些语句，调用这个`储存程序`可以间接的执行这些语句，完成一个功能。

`储存程序`分为三种类型：

- 储存例程
- 触发器
- 事件

其中，`储存例程`又分为：

- 储存函数
- 储存过程

## 自定义变量简介

`MySql`中，可以使用`SET`语句来自定义一些变量，变量名必须使用`@`符号开头：

```sql
SET @a = 1;
```

可以使用`SELECT`语句查询：

```sql
SELECT @a;
```

自定义变量之间可以互相赋值：

```sql
SET @b = @a;
```

也可以将查询结果赋值给自定义变量，前提是查询出来的结果只有一个值：

```sql
SET @a = (SELECT m1 FROM t1 LIMIT 1);

SELECT n1 FROM t1 LIMIT 1 INTO @b;
```

## 语句结束分割符

在`MySql`命令行中，`;`代表这一条语句的结束，如果一次需要写多条查询语句，就必须将这些查询语句写在一行，非常麻烦。好在`MySql`中可以使用`delimiter`来定义语句结束分割符。

```sql
delimiter $;
```

这个时候，使用`;`就不会结束语句了。只有在输入了`$`符号时，才会结束语句。

```sql
delimiter $;
SELECT * FROM t1 LIMIT 1;
SELECT * FROM t2 LIMIT 1;
SELECT * FROM t3 LIMIT 1;
$
```

使用完自定义语句分割结束符之后，最好还是将其还原为系统默认的：

```sql
delimiter ;
```
