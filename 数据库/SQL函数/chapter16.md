# NOW() 函数
> NOW 函数返回当前的日期和时间。

**提示：如果您在使用 Sql Server 数据库，请使用 getdate() 函数来获得当前的日期时间。**

## SQL NOW() 语法
```
SELECT NOW() FROM table_name
```
## SQL NOW() 实例
我们拥有下面这个 "Products" 表：

Prod_Id|ProductName|Unit|UnitPrice
:--:|:--:|:--:|:--:
1|gold|1000 g|32.35
2|silver|1000 g|11.56
3|copper|1000 g|6.85

现在，我们希望显示当天的日期所对应的名称和价格。

我们使用如下 SQL 语句：
```
SELECT ProductName, UnitPrice, Now() as PerDate FROM Products
```
结果集类似这样：

ProductName|UnitPrice|PerDate
:--:|:--:|:--:
gold|32.35|12/29/2008 11:36:05 AM
silver|11.56|12/29/2008 11:36:05 AM
copper|6.85|12/29/2008 11:36:05 AM