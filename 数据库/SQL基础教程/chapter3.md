# SQL WHERE 子句
---
> WHERE 子句用于过滤记录。
WHERE 子句用于提取那些满足指定条件的记录。


---
## SQL WHERE 语法
```
SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;
```
---
## WHERE 子句中的运算符
下面的运算符可以在 WHERE 子句中使用：

运算符 | 描述
:--: | :--:
= | 等于
<> | 不等于。注释：在 SQL 的一些版本中，该操作符可被写成 !=
> | 大于
< | 小于
>= | 大于等于
<= | 小于等于
BETWEEN | 在某个范围内
LIKE | 搜索某种模式
IN | 指定针对某个列的多个可能值
