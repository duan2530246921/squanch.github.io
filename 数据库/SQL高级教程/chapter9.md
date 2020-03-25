# SQL LEFT JOIN 关键字
> LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。
---

<img src="/_markdownimg/img_leftjoin.gif" alt="图片名称" align=center />

---
## LEFT JOIN 关键字语法
```
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
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
## 左连接（LEFT JOIN）实例
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
LEFT JOIN Orders
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

LEFT JOIN 关键字会从左表 (Persons) 那里返回所有的行，即使在右表 (Orders) 中没有匹配的行。