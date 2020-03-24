# SQL BETWEEN 操作符
> BETWEEN 操作符用于选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。
## SQL BETWEEN 语法
```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
---
> **请注意，在不同的数据库中，BETWEEN 操作符会产生不同的结果！在某些数据库中，BETWEEN 选取介于两个值之间但不包括两个测试值的字段。在某些数据库中，BETWEEN 选取介于两个值之间且包括两个测试值的字段。在某些数据库中，BETWEEN 选取介于两个值之间且包括第一个测试值但不包括最后一个测试值的字段。因此，请检查您的数据库是如何处理 BETWEEN 操作符！**