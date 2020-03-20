# SQL SELECT DISTINCT 语句
---
> SELECT DISTINCT 语句用于返回唯一不同的值。
---
## SQL SELECT DISTINCT 语句
---
在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。
DISTINCT 关键词用于返回唯一不同的值。

---
## SQL SELECT DISTINCT 语法
---
```
SELECT DISTINCT column_name,column_name
FROM table_name;
```
---

## SQL中distinct的用法
---
表A：

 id | name 
 :----: | :----: 
 1 | a 
 2 | b 
 3 | c 
 4 | c 
 5 | b 
 <font color="red">1</font> | <font color="red">a</font> 

表B:

 xing | ming
:-----:|:-----:
rain|man
rainm|an

---

## 作用于单例
---
```
select distinct name from A
```
执行后结果如下：

name|
:--:|
a|
b|
c|

---

## 作用于多列
---
```
select distinct name, id from A
```
执行后结果如下：

name|id
:--:|:--:
a|1
b|2
b|5
c|3
c|4

实际上是根据name和id两个字段来去重的，这种方式Access和SQL Server同时支持。

```
select distinct xing, ming from B
```
返回如下结果：

 xing | ming
:-----:|:-----:
rain|man
rainm|an

返回的结果为两行，这说明distinct并非是对xing和ming两列“字符串拼接”后再去重的，而是分别作用于了xing和ming列。

---
## COUNT统计
---
```
select count(distinct name) from A;	  --表中name去重后的数目， SQL Server支持，而Access不支持
```
count是不能统计多个字段的，下面的SQL在SQL Server和Access中都无法运行。
```
select count(distinct name, id) from A;
```
若想使用，请使用嵌套查询，如下：
```
select count(*) from (select distinct xing, name from B) AS M;
```
---
## DISTINCT必须放在开头
---
```
select id, distinct name from A;   --会提示错误，因为distinct必须放在开头
```
---
## 其他
---
distinct语句中select显示的字段只能是distinct指定的字段，其他字段是不可能出现的。例如，假如表A有“备注”列，如果想获取distinc name，以及对应的“备注”字段，想直接通过distinct是不可能实现的。