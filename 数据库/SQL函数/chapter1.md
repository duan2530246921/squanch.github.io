# SQL AVG() 函数
## AVG() 函数
> AVG() 函数返回数值列的平均值。

### SQL AVG() 语法
```
SELECT AVG(column_name) FROM table_name
```
---
## SQL AVG() 实例

"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

计算 "OrderPrice" 字段的平均值。
```
SELECT AVG(OrderPrice) AS OrderAverage FROM Orders
```
结果集:

OrderAverage|
:--:|
950|

找到 OrderPrice 值高于 OrderPrice 平均值的客户
```
SELECT Customer FROM Orders
WHERE OrderPrice>(SELECT AVG(OrderPrice) FROM Orders)
```
结果集:

Customer|
:--:|
Bush|
Carter|
Adams|