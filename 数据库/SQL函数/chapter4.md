# LAST() 函数
> LAST() 函数返回指定的字段中最后一个记录的值。

**提示：可使用 ORDER BY 语句对记录进行排序。**

## SQL LAST() 语法
```
SELECT LAST(column_name) FROM table_name
```
---
## SQL LAST() 实例
"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

现在，我们希望查找 "OrderPrice" 列的最后一个值。

我们使用如下 SQL 语句：
```
SELECT LAST(OrderPrice) AS LastOrderPrice FROM Orders
```
结果集类似这样：

LastOrderPrice|
:--:|
100|