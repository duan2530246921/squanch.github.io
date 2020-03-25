# ROUND() 函数
>ROUND 函数用于把数值字段舍入为指定的小数位数。

## SQL ROUND() 语法
```
SELECT ROUND(column_name,decimals) FROM table_name
```
参数|描述
:--:|:--:
column_name|必需。要舍入的字段。
decimals|必需。规定要返回的小数位数。
---
## SQL ROUND() 实例
我们拥有下面这个 "Products" 表：

Prod_Id|ProductName|Unit|UnitPrice
:--:|:--:|:--:|:--:
1|gold|1000 g|32.35
2|silver|1000 g|11.56
3|copper|1000 g|6.85


现在，我们希望把名称和价格舍入为最接近的整数。

我们使用如下 SQL 语句：
```
SELECT ProductName, ROUND(UnitPrice,0) as UnitPrice FROM Products
```
结果集类似这样：

ProductName|UnitPrice
:--:|:--:
gold|2
silver|12
copper|7
