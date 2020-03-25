# SQL RIGHT JOIN 关键字
> RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。
---

<img src="/_markdownimg/img_rightjoin.gif" alt="图片名称" align=center />

---
## SQL RIGHT JOIN 语法
```
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```
或
```
SELECT column_name(s)
FROM table1
RIGHT OUTER JOIN table2
ON table1.column_name=table2.column_name;
```
注释：在某些数据库中， LEFT JOIN 称为 LEFT OUTER JOIN。

---
"Persons" 表：

Id_P | LastName | FirstName | Address | City
:--:|:--:|:--:|:--:|:--:
1|Adams|Thomas|John|Oxford Street|London
2|Bush|George|Fifth Avenue|New York
3|Carter|Thomas|Changan Street|Beijing

"Orders" 表：

Id_P | OrderNo | Id_P
:--:|:--:|:--:
1|77895|3
2|44678|3
3|22456|1
4|24562|1
1|34764|65

---
## 右连接（RIGHT JOIN）实例
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
RIGHT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```
结果集：

LastName|FirstName|OrderNo
:--:|:--:|:--:
Adams|John|22456
Adams|John|24562
Carter|Thomas|77895
Carter|Thomas|44678
||34764

RIGHT JOIN 关键字会从右表 (Orders) 那里返回所有的行，即使在左表 (Persons) 中没有匹配的行。