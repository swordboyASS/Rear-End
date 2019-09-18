<a  id="top" href="#top">:collision:高级用法:collision: </a>

* `SELECT TOP` : SELECT TOP 子句用于规定要返回的记录的数目。    

`SQL Server用法`: `SELECT TOP number|percent column_name(s) FROM table_name;`   
**示例** `SELECT TOP (1000) [ID] ,[age] ,[name] FROM [SQL-learing].[dbo].[Table_1]`*使用了完全限定名*  
`或者` `SELECT TOP (1000) * FROM [SQL-learing].[dbo].[Table_1]` *选中前1000行的所有数据*


* `LIKE` ： LIKE表示符合某个查询模式的模糊查询

`通用语法` `SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern;`   

**查询模式**

|pattern|作用|
|:--|:--|
'%a'    |//以a结尾的数据
'a%'    |//以a开头的数据
'%a%'    |//含有a的数据
'\_a\_'    |//三位且中间字母是a的
'_a'    |//两位且结尾字母是a的
'a_'   | //两位且开头字母是a的

**正则后面用到再补充**


- `别名`: 在下面的情况下使用别名

> 在查询中涉及超过一个表  
> 在查询中使用了函数  
> 列名称很长或者可读性差  
> 需要把两个列或者多个列结合在一起  

**具体使用方法后面再补充**
----


- `JOIN(连接)`: 连接结果像数学中的集合一样。下面介绍7种用法   
JOIN的作用就是将几张不同表的数据筛选后合并在一起1

#### `INNER JOIN` `注释:INNER JOIN和JOIN的作用是相同的` 
相当于取2个表的交集

`通用语法` `SELECT column_name(s) FROM table1 JOIN table2 ON table1.column_name=table2.column_name; `   
`示例:`  `SELECT * FROM Table_1 INNER JOIN Table_2 ON Table_1.ID=Table_2.ID ORDER BY Table_1.age;`

#### `LEFT JOIN` 
:LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。

`通用语法` `SELECT column_name(s) FROM table1 LEFT JOIN table2 ON table1.column_name=table2.column_name;`   
`示例:` `SELECT Table_1.age,Table_1.ID,Table_1.name  FROM Table_1 LEFT JOIN Table_2 ON  Table_1.age=Table_2.age;`

#### `RIGHT JOIN` 
: RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。   
用法和 前者相同

#### `FULL JOIN` 
:FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行.

* `UNION`  
```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。
```

* `SELECT INTO`
SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中。
```sql
SELECT *
INTO Table_new1
FROM Table_1;

//只复制一些列
SELECT name, url
INTO Table_new1
FROM Table_1;

//只复制姓名为’gaoju‘的列
SELECT *
INTO Table_new1
FROM Table_1
WHERE name = 'gaoju';

```

* `CREATE DATABASE`
CREATE DATABASE用于创建数据库
```csharp
CREATE DATABASE dbname;
```

* `CREATE TABLE`
样例: 
```sql
CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);
```

* `约束`
SQL 约束用于规定表中的数据规则。 如果存在违反约束的数据行为，行为会被约束终止。 约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。
```sql
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

##### 在 SQL 中, 有如下约束：
- NOT NULL - 指示某列不能存储 NULL 值。
- UNIQUE - 保证某列的每行必须有唯一的值。
- PRIMARY KEY - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- FOREIGN KEY - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- CHECK - 保证列中的值符合指定的条件。
- DEFAULT - 规定没有给列赋值时的默认值。


1. `NOT NULL约束`  
建表时设置约束
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

已有表添加约束
```sql
ALTER TABLE Persons
MODIFY Age int NOT NULL;
```

删除NOT NULL
```sql
ALTER TABLE Persons
MODIFY Age int NULL;
```

2. UNIQUE 约束
- UNIQUE 约束唯一标识数据库表中的每条记录。
- UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。
- PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。
- 请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束(只能有一个主键)。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL UNIQUE,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法(同时这个语法适合所有DBMS)：
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
)
```

为已有表添加UNIQUE约束
```sql
ALTER TABLE Persons
ADD UNIQUE (P_Id)
```
添加多列约束
```sql
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
```

撤销约束
```sql
ALTER TABLE Persons
DROP INDEX uc_PersonID
```

```sql
ALTER TABLE Persons
DROP CONSTRAINT uc_PersonID
```

3. PRIMARY KEY 约束(主键)
- PRIMARY KEY 约束唯一标识数据库表中的每条记录。
 主键必须包含唯一的值。
- 主键列不能包含 NULL 值。
- 每个表都应该有一个主键，并且每个表只能有一个主键。

