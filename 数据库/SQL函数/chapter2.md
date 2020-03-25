# SQL COUNT() 函数
> COUNT() 函数返回匹配指定条件的行数。
## SQL COUNT(column_name) 语法
COUNT(column_name) 函数返回指定列的值的数目（NULL 不计入）：
```
SELECT COUNT(column_name) FROM table_name;
```
## SQL COUNT(*) 语法
COUNT(*) 函数返回表中的记录数：
```
SELECT COUNT(*) FROM table_name;
```
## SQL COUNT(DISTINCT column_name) 语法
COUNT(DISTINCT column_name) 函数返回指定列的不同值的数目：
```
SELECT COUNT(DISTINCT column_name) FROM table_name;
```
> 注释：COUNT(DISTINCT) 适用于 ORACLE 和 Microsoft SQL Server，但是无法用于 Microsoft Access。
---
## SQL COUNT(column_name) 实例
"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

计算客户 "Carter" 的订单数。
```
SELECT COUNT(Customer) AS CustomerNilsen FROM Orders
WHERE Customer='Carter'
```
以上 SQL 语句的结果是 2，因为客户 Carter 共有 2 个订单：

CustomerNilsen|
:--:|
2|

---
SQL COUNT(*) 实例
如果我们省略 WHERE 子句，比如这样：
```
SELECT COUNT(*) AS NumberOfOrders FROM Orders
```
结果集：

NumberOfOrders|
:--:|
6|

这是表中的总行数

---
## SQL COUNT(DISTINCT column_name) 实例
计算 "Orders" 表中不同客户的数目。
```
SELECT COUNT(DISTINCT Customer) AS NumberOfCustomers FROM Orders
```
结果集：

NumberOfCustomers|
:--:|
3|

这是 "Orders" 表中不同客户（Bush, Carter 和 Adams）的数目。


