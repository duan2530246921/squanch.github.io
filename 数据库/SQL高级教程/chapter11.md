# SQL FULL OUTER JOIN 关键字
> FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行. FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。
---

<img src="/_markdownimg/img_fulljoin.gif" alt="图片名称" align=center />

---
## SQL FULL OUTER JOIN 语法
```
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;
```
注释：在某些数据库中， FULL JOIN 称为 FULL OUTER JOIN。

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
## 全连接（FULL JOIN）实例
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
FULL JOIN Orders
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
Bush|George|
||34764

FULL JOIN 关键字会从左表 (Persons) 和右表 (Orders) 那里返回所有的行。如果 "Persons" 中的行在表 "Orders" 中没有匹配，或者如果 "Orders" 中的行在表 "Persons" 中没有匹配，这些行同样会列出。