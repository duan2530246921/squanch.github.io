# SQL UCASE() 函数
## UCASE() 函数
>UCASE() 函数把字段的值转换为大写。

## SQL UCASE() 语法
```
SELECT UCASE(column_name) FROM table_name;
```
## 用于 SQL Server 的语法
```
SELECT UPPER(column_name) FROM table_name;
```
---
## SQL UCASE() 实例
我们拥有下面这个 "Persons" 表：

Id|LastName|FirstName|Address|City
:--:|:--:|:--:|:--:|:--:
1|Adams|John|Oxford Street|London
2|Bush|George|Fifth Avenue|New York
3|Carter|Thomas|Changan Street|Beijing

现在，我们希望选取 "LastName" 和 "FirstName" 列的内容，然后把 "LastName" 列转换为大写。

我们使用如下 SQL 语句：
```
SELECT UCASE(LastName) as LastName,FirstName FROM Persons
```
结果集类似这样：

LastName|FirstName
:--:|:--:
ADAMS|John
BUSH|George
CARTER|Thomas