# SQL GROUP BY 语句

> 合计函数 (比如 SUM) 常常需要添加 GROUP BY 语句。

## GROUP BY 语句
GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。

## SQL GROUP BY 语法
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
```
---
## SQL GROUP BY 实例
"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

现在，我们希望查找每个客户的总金额（总订单）。

我们想要使用 GROUP BY 语句对客户进行组合。

我们使用下列 SQL 语句：
```
SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer
```
结果集类似这样：

Customer|SUM(OrderPrice)
:--:|:--:
Bush|2000
Carter|1700
Adams|2000

让我们看一下如果省略 GROUP BY 会出现什么情况：
```
SELECT Customer,SUM(OrderPrice) FROM Orders
```
结果集类似这样：

Customer|SUM(OrderPrice)
:--:|:--:
Bush|5700
Carter|5700
Bush|5700
Bush|5700
Adams|5700
Carter|5700

上面的结果集不是我们需要的。

那么为什么不能使用上面这条 SELECT 语句呢？解释如下：上面的 SELECT 语句指定了两列（Customer 和 SUM(OrderPrice)）。"SUM(OrderPrice)" 返回一个单独的值（"OrderPrice" 列的总计），而 "Customer" 返回 6 个值（每个值对应 "Orders" 表中的每一行）。因此，我们得不到正确的结果。不过，您已经看到了，GROUP BY 语句解决了这个问题。

---
## GROUP BY 一个以上的列
我们也可以对一个以上的列应用 GROUP BY 语句，就像这样：
```
SELECT Customer,OrderDate,SUM(OrderPrice) FROM Orders
GROUP BY Customer,OrderDate
```