---
title: MYSQL学习（十三）
date: 2021-11-19 10:41:17
tags: MYSQL
categories: MYSQL
cover: /img/article/mysql/mysql.PNG
top_img: /img/article/mysql/mysql.PNG
description: 数据的插入，删除与更新
---

# 视图

每次查询一条记录，就会写一条查询语句，如果一条查询语句非常长，敲完之后发现后续又要查一次，就又要敲一遍，非常麻烦。`视图`可以帮助我们复用这些查询语句。

```sql
CREATE VIEW 视图名 AS 查询语句;
```

可以将视图理解为查询语句的别名。
