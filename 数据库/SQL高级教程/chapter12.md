# SQL UNION 操作符
> UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
请注意，UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。

---
## SQL UNION 语法
```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```
注释：默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

## SQL UNION ALL 语法
```
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```
注释：UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

---
Employees_China:

E_ID | E_Name
:--:|:--:
01|Zhang, Hua
02|Wang, Wei
03|Carter, Thomas
04|Yang, Ming

Employees_USA:

E_ID | E_Name
:--:|:--:
01|Adams, John
02|Bush, George
03|Carter, Thomas
04|Gates, Bill

---
## 使用 UNION 命令
列出所有在中国和美国的不同的雇员名：
```
SELECT E_Name FROM Employees_China
UNION
SELECT E_Name FROM Employees_USA
```
结果集：

E_NAME|
:--:|
Zhang, Hua|
Wang, Wei|
Carter, Thomas|
Yang, Ming|
Adams, John|
Bush, George|
Gates, Bill|

注释：这个命令无法列出在中国和美国的所有雇员。在上面的例子中，我们有两个名字相同的雇员，他们当中只有一个人被列出来了。UNION 命令只会选取不同的值。

---
## UNION ALL
UNION ALL 命令和 UNION 命令几乎是等效的，不过 UNION ALL 命令会列出所有的值。
```
SQL Statement 1
UNION ALL
SQL Statement 2
```
## 使用 UNION ALL 命令
列出在中国和美国的所有的雇员：
```
SELECT E_Name FROM Employees_China
UNION ALL
SELECT E_Name FROM Employees_USA
```
结果集：

E_NAME|
:--:|
Zhang, Hua|
Wang, Wei|
Carter, Thomas|
Yang, Ming|
Adams, John|
Bush, George|
Carter, Thomas|
Gates, Bill|