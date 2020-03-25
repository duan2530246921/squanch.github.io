# SQL INNER JOIN 关键字
> 在表中存在至少一个匹配时，INNER JOIN 关键字返回行。
---

<img src="/_markdownimg/img_innerjoin.gif" alt="图片名称" align=center />

---
## INNER JOIN 关键字语法
```
SELECT column_name(s)
FROM table_name1
INNER JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```
注释：INNER JOIN 与 JOIN 是相同的。

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
## 内连接（INNER JOIN）实例
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
INNER JOIN Orders
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

---
INNER JOIN 关键字在表中存在至少一个匹配时返回行。如果 "Persons" 中的行在 "Orders" 中没有匹配，就不会列出这些行。

## 在使用 join 时，on 和 where 条件的区别如下：
* on 条件是在生成临时表时使用的条件，它不管 on 中的条件是否为真，都会返回左边表中的记录。
* where 条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有 left join 的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。