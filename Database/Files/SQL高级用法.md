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
`建表时设置约束`
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

`已有表添加约束`
```sql
ALTER TABLE Persons
MODIFY Age int NOT NULL;
```

`删除NOT NULL`
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

`为已有表添加UNIQUE约束`
```sql
ALTER TABLE Persons
ADD UNIQUE (P_Id)
```
`添加多列约束`
```sql
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
```

`撤销约束`
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

`建表并添加主键`
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

如需命名 PRIMARY KEY 约束，并定义多个列的 PRIMARY KEY 约束，请使用下面的 SQL 语法：
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
)
```
**特别说明** `在上面的实例中，只有一个主键 PRIMARY KEY（pk_PersonID）。然而，pk_PersonID 的值是由两个列（P_Id 和 LastName）组成的。`

`已有表添加主键`
```sql
ALTER TABLE Persons
ADD PRIMARY KEY (P_Id)
```
```sql
ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
```
如果使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）。

`撤销 PRIMARY KEY 约束` 
```sql
ALTER TABLE Persons
DROP CONSTRAINT pk_PersonID
```

4.FOREIGN KEY 约束(外键约束)

一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。
让我们通过一个实例来解释外键。请看下面两个表：

`"Persons" 表：`

|P_Id|LastName|FirstName|Address|City|
|:--|:--|:--|:--|:--|
1	|Hansen	|Ola	|Timoteivn| 10	|Sandnes
2	|Svendson	|Tove	|Borgvn 23	|Sandnes
3	|Pettersen	|Kari	|Storgt 20	|Stavanger

`"Orders" 表：`

|O_Id|OrderNo|P_Id|
|:--|:--|:--|
1	|77895	|3
2	|44678	|3
3	|22456	|2
4	|24562	|1

* 请注意，"Orders" 表中的 "P_Id" 列指向 "Persons" 表中的 "P_Id" 列。
* "Persons" 表中的 "P_Id" 列是 "Persons" 表中的 PRIMARY KEY。
* "Orders" 表中的 "P_Id" 列是 "Orders" 表中的 FOREIGN KEY。
* FOREIGN KEY 约束用于预防破坏表之间连接的行为。
* FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

`建表时添加外键`
```sql
CREATE TABLE Orders
(
O_Id int NOT NULL PRIMARY KEY,
OrderNo int NOT NULL,
P_Id int FOREIGN KEY REFERENCES Persons(P_Id)
)
```

如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：
```sql
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
)
```

`已有表添加FOREIGN KEY 约束`
```sql
ALTER TABLE Orders
ADD CONSTRAINT fk_PerOrders
FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
```

`撤销FOREIGN KEY 约束`
```sql
ALTER TABLE Orders
DROP CONSTRAINT fk_PerOrders
```

5. CHECK 约束
- CHECK 约束用于`限制`列中的值的范围。
- 如果对单个列定义 CHECK 约束，那么该列只允许特定的值。
- 如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 CHECK 约束。CHECK 约束规定 "P_Id" 列必须只包含大于 0 的整数。
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL CHECK (P_Id>0),
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
)
```

`表已被创建时添加约束`
```sql
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```

`撤销 CHECK 约束`
```sql
ALTER TABLE Persons
DROP CONSTRAINT chk_Person
```

6. DEFAULT 约束
DEFAULT 约束用于向列中插入默认值。
如果没有规定其他的值，那么会将默认值添加到所有的新记录。

`建表时添加约束`
```sql
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
```
`已有表时添加约束`
```sql
ALTER TABLE Persons
ADD CONSTRAINT ab_c DEFAULT 'SANDNES' for City
```

`撤销 DEFAULT 约束`
```sql
ALTER TABLE Persons
ALTER COLUMN City DROP DEFAULT
```

* CREATE INDEX 语句
CREATE INDEX 语句用于在表中创建索引。   
在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。  

#### 索引的作用
您可以在表中创建索引，以便更加快速高效地查询数据， 用户无法看到索引，它们只能被用来加速搜索/查询。   但同时也存在着缺点    
更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。


`允许重复的索引`
```sql
CREATE INDEX index_name
ON table_name (column_name)
```
`不允许重复的索引`
```sql
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

如果希望索引不止一个列，可以在括号中列出这些列的名称，用逗号隔开：
```sql
CREATE INDEX PIndex
ON Persons (LastName, FirstName)
```


`SQL 撤销索引、撤销表以及撤销数据库`
1. 撤销索引
```sql
DROP INDEX table_name.index_name
```
2. 撤销表
```sql
DROP TABLE table_name
```
3. 撤销数据库
```sql
DROP DATABASE database_name
```
4. 删除表数据但不撤销表
```sql
TRUNCATE TABLE table_name
```

* ALTER 语句
ALTER TABLE 语句用于在已有的表中添加、删除或修改列。

如需在表中添加列，请使用下面的语法:
```sql
ALTER TABLE table_name
ADD column_name datatype
```
如需删除表中的列，请使用下面的语法（请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：
```sql
ALTER TABLE table_name
DROP COLUMN column_name
```

要改变表中列的数据类型，请使用下面的语法：
```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

为某个表添加一列
```sql
ALTER TABLE Table_1 ADD DateOfBirth date;
```

删除表的某列
```sql
ALTER TABLE Table_1 DROP COLUMN DateOfBirth;
```


- **`视图`**
视图是可视化的表。`视图的作用：`

1. 视图隐藏了底层的表结构，简化了数据访问操作，客户端不再需要知道底层表的结构及其之间的关系。
2. 视图提供了一个统一访问数据的接口。（即可以允许用户通过视图访问数据的安全机制，而不授予用户直接访问底层表的权限）
3. 从而加强了安全性，使用户只能看到视图所显示的数据。
4. 视图还可以被嵌套，一个视图中可以嵌套另一个视图。

1. `CREATE VIEW 语句`
在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。
您可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。

`通用语法`
```sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```
###### 视图总是显示最新的数据！每当用户查询视图时，数据库引擎通过使用视图的 SQL 语句重建数据。

 
`更新视图`
```sql
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

`撤销视图`
```sql
DROP VIEW view_name
```

* **`日期`**







