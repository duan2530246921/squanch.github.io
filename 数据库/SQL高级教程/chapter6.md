# SQL 别名
> 通过使用 SQL，可以为表名称或列名称指定别名。基本上，创建别名是为了让列名称的可读性更强。
## 列的 SQL 别名语法
```
SELECT column_name AS alias_name
FROM table_name;
```
## 表的 SQL 别名语法
```
SELECT column_name(s)
FROM table_name AS alias_name;
```
---
## 在下面的情况下，使用别名很有用：
* 在查询中涉及超过一个表
* 在查询中使用了函数
* 列名称很长或者可读性差
* 需要把两个列或者多个列结合在一起
