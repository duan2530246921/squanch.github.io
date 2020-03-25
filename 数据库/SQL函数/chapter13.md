# MID() 函数
> MID() 函数用于从文本字段中提取字符。

## SQL MID() 语法
```
SELECT MID(column_name,start[,length]) FROM table_name;
```
参数|描述
:--:|:--:  
column_name|必需。要提取字符的字段。
start|必需。规定开始位置（起始值是 1）。
length|可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。

---
SQL MID() 实例
我们拥有下面这个 "Persons" 表：

Id|LastName|FirstName|Address|City
:--:|:--:|:--:|:--:|:--:
1|Adams|John|Oxford Street|London
2|Bush|George|Fifth Avenue|New York
3|Carter|Thomas|Changan Street|Beijing

现在，我们希望从 "City" 列中提取前 3 个字符。

我们使用如下 SQL 语句：
```
SELECT MID(City,1,3) as SmallCity FROM Persons
```
结果集类似这样：

SmallCity|
:--:|
Lon|
New|
Bei|