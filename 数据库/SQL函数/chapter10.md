# SQL EXISTS 运算符
## EXISTS 运算符
> XISTS 运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回 True，否则返回 False。

## SQL EXISTS 语法
```
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```