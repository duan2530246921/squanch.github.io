# FIRST() 函数
> FIRST() 函数返回指定的字段中第一个记录的值。

**提示：可使用 ORDER BY 语句对记录  进行排序。**

## SQL FIRST() 语法
```
SELECT FIRST(column_name) FROM table_name
```
---
## SQL FIRST() 实例

"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

现在，我们希望查找 "OrderPrice" 列的第一个值。

我们使用如下 SQL 语句：
```
SELECT FIRST(OrderPrice) AS FirstOrderPrice FROM Orders
```
结果集类似这样：

FirstOrderPrice|
:--:|
1000|
