# SQL LEN() 函数
## LEN() 函数
>LEN() 函数返回文本字段中值的长度。

## SQL LEN() 语法
```
SELECT LEN(column_name) FROM table_name;
```
MySQL 中函数为 LENGTH():
```
SELECT LENGTH(column_name) FROM table_name;
```
---
## SQL LEN() 实例
我们拥有下面这个 "Persons" 表：

Id|LastName|FirstName|Address|City
:--:|:--:|:--:|:--:|:--:
1|Adams|John|Oxford Street|London
2|Bush|George|Fifth Avenue|New York
3|Carter|Thomas|Changan Street|Beijing

现在，我们希望取得 "City" 列中值的长度。

我们使用如下 SQL 语句：
```
SELECT LEN(City) as LengthOfCity FROM Persons
```
结果集类似这样：

LengthOfCity|
:--:|
6|
8|
7|
