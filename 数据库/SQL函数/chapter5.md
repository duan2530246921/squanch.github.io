# MAX() 函数
>MAX 函数返回一列中的最大值。NULL 值不包括在计算中。

## SQL MAX() 语法
```
SELECT MAX(column_name) FROM table_name
```
**注释：MIN 和 MAX 也可用于文本列，以获得按字母顺序排列的最高或最低值。**

---
## SQL MAX() 实例
"Orders" 表：

O_Id|OrderDate|OrderPrice|Customer
:--:|:--:|:--:|:--:
1|2008/12/29|1000|Bush
2|2008/11/23|1600|Carter
3|2008/10/05|700|Bush
4|2008/09/28|300|Bush
5|2008/08/06|2000|Adams
6|2008/07/21|100|Carter

现在，我们希望查找 "OrderPrice" 列的最大值。

我们使用如下 SQL 语句：
```
SELECT MAX(OrderPrice) AS LargestOrderPrice FROM Orders
```
结果集类似这样：

LargestOrderPrice|
:--:|
2000|