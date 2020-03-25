# SQL JOIN
> SQL join 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。
---

<img src="/_markdownimg/sql.png" alt="图片名称" align=center />

---
## Join 和 Key
有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。 数据库中的表可通过键将彼此联系起来。主键（Primary Key）是一个列，在这个列中的每一行的值都是唯一的。在表中，每个主键的值都是唯一的。这样做的目的是在不重复每个表中的所有数据的情况下，把表间的数据交叉捆绑在一起。

---
Persons表:

Id_P | LastName | FirstName | Address | City
:--:|:--:|:--:|:--:|:--:
1|Adams|Thomas|John|Oxford Street|London
2|Bush|George|Fifth Avenue|New York
3|Carter|Thomas|Changan Street|Beijing

---
Orders表:

Id_P | OrderNo | Id_P
:--:|:--:|:--:
1|77895|3
2|44678|3
3|22456|1
4|24562|1
1|34764|65

---
## 引用两个表
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons, Orders
WHERE Persons.Id_P = Orders.Id_P 
```
结果集:

LastName|FirstName|OrderNo
:--:|:--:|:--:
Adams|John|22456
Adams|John|24562
Carter|Thomas|77895
Carter|Thomas|44678

---
## SQL JOIN - 使用 Join
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
ORDER BY Persons.LastName
```
结果集:

LastName|FirstName|OrderNo
:--:|:--:|:--:
Adams|John|22456
Adams|John|24562
Carter|Thomas|77895
Carter|Thomas|44678

---
## 不同的 SQL JOIN
除了我们在上面的例子中使用的 INNER JOIN（内连接），我们还可以使用其他几种连接。
下面列出了您可以使用的 JOIN 类型，以及它们之间的差异。
* JOIN: 如果表中有至少一个匹配，则返回行
* LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行
* RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行
* FULL JOIN: 只要其中一个表中存在匹配，就返回行
