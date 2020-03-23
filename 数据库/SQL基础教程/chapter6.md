# SQL INSERT INTO 语句
> INSERT INTO 语句用于向表中插入新记录。
---
## SQL ORDER BY 语法

INSERT INTO 语句可以有两种编写形式。

第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：
```
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```
第二种形式需要指定列名及被插入的值：
```
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```
---