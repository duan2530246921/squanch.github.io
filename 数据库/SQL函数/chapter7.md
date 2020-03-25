# SUM() 函数
>SUM 函数返回数值列的总数（总额）。

## SQL SUM() 语法
```
SELECT SUM(column_name) FROM table_name
```
---
## SQL SUM() 实例
"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

现在，我们希望查找 "OrderPrice" 字段的总数。

我们使用如下 SQL 语句：
```
SELECT SUM(OrderPrice) AS OrderTotal FROM Orders
```
结果集类似这样：

OrderTotal|
:--:|
5700|
