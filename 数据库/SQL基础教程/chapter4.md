# SQL AND, OR and NOT（与，或不是运算符）
> AND 和 OR 运算符用于基于一个以上的条件对记录进行过滤。
---
## AND 和 OR 运算符

---
AND&OR运算符用于根据一个以上的条件过滤记录，即用于组合多个条件以缩小SQL语句中的数据。WHERE子句可以与AND，OR和NOT运算符结合使用。AND和OR运算符用于根据多个条件筛选记录：

* 如果由AND分隔的所有条件为TRUE，则AND运算符显示记录。

* 如果使用AND运算符组合N个条件。对于SQL语句执行的操作(无论是事务还是查询)，所有由AND分隔的条件都必须为TRUE。

* 如果由OR分隔的任何条件为真，则OR运算符显示记录。

* 如果使用OR运算符组合N个条件。对于SQL语句执行的操作(无论是事务还是查询)，OR分隔的任何一个条件都必须为TRUE。

如果条件不为TRUE，则NOT运算符显示记录。

---
## AND语法
---
```
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;
```
---
## OR语法
---
```
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```
---
## NOT语法
---
```
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```
---
## 实例
---
以下是"Customers"表中的数据：

CustomerID|CustomerName|ContactName|Address|City|PostalCode|Country
:--:|:--:|:--:|:--:|:--:|:--:|:--:
1|Alfreds Futterkiste|Maria Anders|Obere Str. 57|Berlin|12209|Germany
2|Ana Trujillo Emparedados y helados|Ana Trujillo|Avda. de la Constitución 2222|México D.F.|05021|Mexico
3|Antonio Moreno Taquería|Antonio Moreno|Mataderos 2312|México D.F.|05023|Mexico
4|Around the Horn|Thomas Hardy|120 Hanover Sq.|London|WA1 1DP|UK
5|Berglunds snabbköp|Christina Berglund|Berguvsvägen 8|Luleå|S-958 22|Sweden

### AND运算符实例
---
以下SQL语句从 "Customers" 表中选择其国家为 "Germany" 、其城市为"Berlin" 的所有客户：
```
SELECT * FROM Customers
WHERE Country='Germany'
AND City='Berlin';
```

### OR 运算符实例
---
以下SQL语句选择城市为“Berlin”或“München”的“Customers”的所有字段：
```
SELECT * FROM Customers
WHERE City='Berlin' OR City='München';
```

### NOT 运算符实例
---
以下SQL语句选择国家不是 "Germany"的"Customers"的所有字段：
```
ELECT * FROM Customers
WHERE NOT Country='Germany';
```

### 结合AND & OR
---
您还可以组合AND和OR（使用括号来组成成复杂的表达式）。
以下SQL语句从国家 "Germany" 且城市为"Berlin" 或"München"的"Customers" 表中选择所有客户：
```
SELECT * FROM Customers
WHERE Country='Germany' AND (City='Berlin' OR City='München');
```
以下SQL语句选择来自"Customers" 的国家不是 "Germany" 且不是 "USA"的所有字段：
```
SELECT * FROM Customers
WHERE NOT Country='Germany' AND NOT Country='USA';
```
