---
title: MYSQL学习（十五）
date: 2021-11-25 14:33:59
date: 2021-11-22 09:52:24
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 存储函数和存储过程
---

# 存储函数和存储过程

前面说到`存储程序`分为`存储例程`，`触发器`以及`事件`。而`存储例程`有分为`存储函数`与`存储过程`。

## 存储函数

### 创建存储函数

存储函数其实就是一种函数，函数里可以执行`MySql`语句：

```sql
CREATE FUNCTION 存储函数名称([参数列表])
RETURNS 返回值类型
BEGIN
 函数体内容
END
```

函数体中可以写一条或者多条语句，每条语句需要用`;`结尾。

例子：

```sql
delimiter $
CREATE FUNCTION avg_score(s VARCHAR(100))
RETURNS DOUBLE
BEGIN
  RETURN (SELECT AVG(score) FROM student_score WHERE subject = s);
END $
```

定义了一个名为`avg_score`的函数，接收的参数`s`类型为`VARCHAR(100)`，返回值类型为`DOUBLE`；函数体中是一个`SELECT`语句，表明这个函数的返回值为这个查询语句的结果。

### 存储函数的调用

存储函数的调用与`MySql`内置函数的调用方式一样，函数名后面加小括号就表示函数调用，如果函数需要参数，括号内填入需要的参数即可。

```sql
SELECT avg_score('母猪的产后护理');
```

### 查看和删除存储函数

- 查看

```sql
SHOW FUNCTION STATUS [LIKE 需要匹配的函数名];
```

查看某个函数如何定义的：

```sql
SHOW CREATE FUNCTION 函数名;
```

- 删除

```sql
DROP FUNCTION 函数名;
```

### 函数体的定义

函数体中可以写多条语句，并且存储函数的函数体内支持一些特殊语法。

#### 在函数体中定义局部变量

```sql
DECLARE 变量名1, 变量名2, 变量名3, ... 数据类型 [DEFAULT 默认值];
```

在函数体中声明的变量不可以在函数外部访问，因此，这些变量也被称为`局部变量`（有学习编程内味了...)。并且，函数体中的局部变量不可以加`@`前缀，也必须在声明了局部变量之后才可以使用该变量。

函数体中的`DECLARE`语句也必须放在其他语句前面。

```sql
delimiter $
CREATE FUNCTION var_demo()
RETURNS INT
BEGIN
  DECLARE c INT;
  SET c = 5;
  RETURN c;
END $
```

#### 在函数体中使用自定义变量

```sql
delimiter $
CREATE FUNCTION use_defined_var_demo()
RETURNS INT
BEGIN
  SET @abc = 10;
  RETURN @abc;
END $
```

因为函数体中`@abc`是自定义变量，所以在函数调用完之后，仍然可以访问`@abc`，这与函数体中使用`DECLARE`声明的局部变量是有明显区别的。

#### 存储函数的参数

定义存储函数时，可以指定多个参数，每个参数都要指定对应的数据类型。

```sql
参数名 数据类型
```

参数名不可以与函数体中的变量名，别名等有冲突，并且只要指定了参数，在调用存储函数时，必须要一一对应。

#### 判断语句的编写

```sql
IF 表达式 THEN
    处理语句列表
[ELSEIF 表达式 THEN
    处理语句列表]
... # 这里可以有多个ELSEIF语句
[ELSE
    处理语句列表]
END IF;
```

例子：

```sql
delimiter $
CREATE FUNCTION condition_demo(i INT)
RETURNS VARCHAR(10)
BEGIN
    DECLARE result VARCHAR(10);
    IF i = 1 THEN
        SET result = '结果是1';
    ELSEIF i = 2 THEN
        SET result = '结果是2';
    ELSEIF i = 3 THEN
        SET result = '结果是3';
    ELSE
        SET result = '非法参数';
    END IF;
    RETURN result;
END $
```

例子：

```sql
delimiter $
CREATE FUNCTION condition_demo(i INT)
RETURNS VARCHAR(10)
BEGIN
    DECLARE result VARCHAR(10);
    IF i = 1 THEN
        SET result = '结果是1';
    ELSEIF i = 2 THEN
        SET result = '结果是2';
    ELSEIF i = 3 THEN
        SET result = '结果是3';
    ELSE
        SET result = '非法参数';
    END IF;
    RETURN result;
END $
```

#### 循环语句的编写

`MySql`支持三种形式的循环语句编写：

- `WHILE`

```sql
WHILE 表达式 DO
    处理语句列表
END WHILE;
```

- `REPEAT`

```sql
REPEAT
    处理语句列表
UNTIL 表达式 END REPEAT;
```

先执行处理语句，再判断表达式是否成立，如果成立则退出循环，否则继续执行处理语句。

- `LOOP`

```sql
LOOP
    处理语句列表
END LOOP;
```

`LOOP`循环没有判断终止循环的条件，需要在处理语句列表中使用加入循环终止的条件，然后使用`RETURN`关键字来停止循环。

## 存储过程

### 创建存储过程

```sql
CREATE PROCEDURE 存储过程名称([参数列表])
BEGIN
  需要执行的语句
END
```

存储过程与存储函数最明显的区别是不需要声明返回值类型。

例子：

```sql
delimiter $
CREATE PROCEDURE t1_operation(m1_value INT, n1_value CHAR(1))
BEGIN
  SELECT * FROM t1;
  INSERT INTO t1(m1, n1) VALUES(m1_value, n1_value);
  SELECT * FROM t1;
END $
```

在例子中，`t1_operation`接收两个参数，并且做了三件事：

- 查询`t1`表
- 在`t1`表中插入数据
- 再次查询`t1`表

### 存储过程的调用

使用`CALL`关键字调用存储过程：

```sql
CALL 存储过程([参数列表])；
```

### 查看和删除存储过程

- 查看

```sql
SHOW PROCEDURE STATUS [LIKE 需要匹配的存储过程名称];
```

查看如何定义：

```sql
SHOW CREATE PROCEDURE 存储过程名称;
```

- 删除

```sql
DROP PROCEDURE 存储过程名称;
```

### 存储过程中的语句

与存储函数中使用到的各种语句，变量，判断，循环等的规则一样。

### 存储过程的参数前缀

```sql
参数类型 [IN | OUT | INOUT] 参数名 数据类型
```

| 前缀    | 实际参数是否必须是变量 | 描述                                                                                                          |
| ------- | ---------------------- | ------------------------------------------------------------------------------------------------------------- |
| `IN`    | 否                     | 用于调用者向存储过程传递数据，如果 IN 参数在过程中被修改，调用者不可见。                                      |
| `OUT`   | 是                     | 用于把存储过程运行过程中产生的数据赋值给 OUT 参数，存储过程执行结束后，调用者可以访问到 OUT 参数。            |
| `INOUT` | 是                     | 综合 IN 和 OUT 的特点，既可以用于调用者向存储过程传递数据，也可以用于存放存储过程中产生的数据以供调用者使用。 |

## 存储过程和存储函数的不同点

存储过程和存储函数非常类似，我们列举几个它们的不同点以加深大家的对这两者区别的印象：

- 存储函数在定义时需要显式用 RETURNS 语句标明返回的数据类型，而且在函数体中必须使用 RETURN 语句来显式指定返回的值，存储过程不需要。

- 存储函数只支持 IN 参数，而存储过程支持 IN 参数、OUT 参数、和 INOUT 参数。

- 存储函数只能返回一个值，而存储过程可以通过设置多个 OUT 参数或者 INOUT 参数来返回多个结果。

- 存储函数执行过程中产生的结果集并不会被显示到客户端，而存储过程执行过程中产生的结果集会被显示到客户端。

- 存储函数直接在表达式中调用，而存储过程只能通过 CALL 语句来显式调用。
