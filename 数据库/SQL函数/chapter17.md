# FORMAT() 函数
> FORMAT 函数用于对字段的显示进行格式化。

## SQL FORMAT() 语法
```
SELECT FORMAT(column_name,format) FROM table_name
```
参数|描述
:--:|:--:
column_name|必需。要格式化的字段。
format|必需。规定格式。

## SQL FORMAT() 实例
我们拥有下面这个 "Products" 表：

Prod_Id|ProductName|Unit|UnitPrice
:--:|:--:|:--:|:--:
1|gold|1000 g|32.35
2|silver|1000 g|11.56
3|copper|1000 g|6.85

现在，我们希望显示每天日期所对应的名称和价格（日期的显示格式是 "YYYY-MM-DD"）。

我们使用如下 SQL 语句：
```
SELECT ProductName, UnitPrice, FORMAT(Now(),'YYYY-MM-DD') as PerDate
FROM Products
```
结果集类似这样：

ProductName|UnitPrice|PerDate
:--:|:--:|:--:
gold|32.35|12/29/2008
silver|11.56|12/29/2008
copper|6.85|12/29/2008