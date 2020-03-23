# SQL UPDATE 语句
> UPDATE 语句用于更新表中的记录。
---
## SQL UPDATE 语法

UPDATE 语句用于更新表中已存在的记录

---
```
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```
---
> **请注意 SQL UPDATE 语句中的 WHERE 子句！ WHERE 子句规定哪条记录或者哪些记录需要更新。如果您省略了 WHERE 子句，所有的记录都将被更新！**