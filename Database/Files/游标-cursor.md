### <a  id="top" href="#top">:closed_book:目录 </a>



- [x] <a href="#01">**`什么是游标?`**</a>
- [x] <a href="#02">**`游标的优点`**</a>
- [x] <a href="#03">**`游标的实现`**</a>
- [x] <a href="#04">**``**</a>
- [x] <a href="#05">**``**</a>
- [x] <a href="#06">**``**</a>
- [x] <a href="#07">**``**</a>
- [x] <a href="#08">**``**</a>

### &nbsp;&nbsp; <a id="01">什么是游标</a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

<a href="https://blog.csdn.net/wandong0917/article/details/76168610" target="blank">概念来源</a>
:star: 游标是什么？

游标实际上是一种能从包括多条数据记录的结果集中每次提取一条记录的机制。游标充当指针的作用。尽管游标能遍历结果中的所有行，但他一次只指向一行。

概括来讲，SQL的游标是一种临时的数据库对象，即可以用来存放在数据库表中的数据行副本，也可以指向存储在数据库中的数据行的指针。游标提供了在逐行的基础上操作表中数据的方法。

游标的一个常见用途就是保存查询结果，以便以后使用。游标的结果集是由SELECT语句产生，如果处理过程需要重复使用一个记录集，那么创建一次游标而重复使用若干次，比重复查询数据库要快的多。

大部分程序数据设计语言都能使用游标来检索SQL数据库中的数据，在程序中嵌入游标和在程序中嵌入SQL语句相同

---
### &nbsp;&nbsp; <a id="02">游标的优点</a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star: 优点：

在数据库中，游标是一个十分重要的概念。游标提供了一种对从表中检索出的数据进行操作的灵活手段，就本质而言，游标实际上是一种能从包括多条数据记录的结果集中每次提取一条记录的机制。游标总是与一条SQL 选择语句相关联因为游标由结果集（可以是零条、一条或由相关的选择语句检索出的多条记录）和结果集中指向特定记录的游标位置组成。当决定对结果集进行处理时，必须声明一个指向该结果集的游标。如果曾经用 C 语言写过对文件进行处理的程序，那么游标就像您打开文件所得到的文件句柄一样，只要文件打开成功，该文件句柄就可代表该文件。对于游标而言，其道理是相同的。可见游标能够实现按与传统程序读取平面文件类似的方式处理来自基础表的结果集，从而把表中数据以平面文件的形式呈现给程序。  

   &nbsp;&nbsp;我们知道关系数据库管理系统实质是面向集合的，在MS SQL SERVER 中并没有一种描述表中单一记录的表达形式，除非使用where 子句来限制只有一条记录被选中。因此我们必须借助于游标来进行面向单条记录的数据处理。由此可见，游标允许应用程序对查询语句select 返回的行结果集中每一行进行相同或不同的操作，而不是一次对整个结果集进行同一种操作；它还提供对基于游标位置而对表中数据进行删除或更新的能力；而且，正是游标把作为面向集合的数据库管理系统和面向行的程序设计两者联系起来，使两个数据处理方式能够进行沟通。

---
### &nbsp;&nbsp; <a id="03">游标的实现</a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star: 声明游标

1.ISO标准语法
语法如下：
```sql
DECLARE curSor_name[INSENSITIVE][SCROLL]CURSOR 
FOR select_statement 
FOR{READ ONLY|UPDATE[OF column_name[...n]]}]
```
参数说明如下。
- [x] DECLARE cursor_name：指定一个游标名称，其游标名称必须符合标识符规则。

- [x] INSENSITIVE：定义一个游标，以创建将由该游标使用的数据的临时复本。对游标的所有请求都从tempdb中的临时表中得到应答；因此，在对该游标进行提取操作时返回的数据中不反映对基表所做的修改，并且该游标不允许修改。使用SQL-92语法时，如果省略INSENSITIVE，（任何用户）对基表提交的删除和更新都反映在后面的提取中。

- [x] SCROLL：指定所有的提取选项（FIRST、LAST、PRIOR、NEXT、RELATIVE、ABSOLUTE）
均可用。
> FIRST：取第一行数据。  
> LAST：取最后一行数据。  
> PRIOR：取前一行数据。  
> NEXT：取后一行数据。  
> RELATIVE：按相对位置取数据。  
> ABSOLUTE：按绝对位置取数据。  

2. Transact-SQL 扩展语法
```sql
DECLARE curSor_name CURSOR
[LOCAL |GLOBAL]
[FORWARD_ONLY|ISCROLL]
[STATIC|IKEYSET|IDYNAMICIFAST_FORWARD]
[READ_ONLYI|SCROLL_LOCKS|IOPTIMISTIC]
[TYPE_WARNING]
FOR select statement
[FOR UPDATE[OF column_name [....n]]]
```

参数说明如下:

|参数|说明|
|:--|:--|
DECLARE_cursor_name |指定一个游标名称，其游标名称必须符合标识符规则
LOCAL|定义游标的作用域仅限在其所在的批处理、存储过程或触发器中。当建立游标在存储过程执行结束后，游标会被自动释放
GLOBAL|指定该游标的作用域对连接是全局的。在由连接执行的任何存储过程或批处理中，都可以引用该游标名称。该游标仅在脱接时隐性释放
FORWARD_ONLY |指定游标只能从第一行滚动到最后一行。FETCHNEXT是唯一受支持的提取选项非指定,STATIC、KEYSET 或DYNAMIC 关键字，否则默认为FORWARD ONLY.STATIC、KEYSET 和DYNAMIC游标默认为SCROLL。与ODBC和ADO这类数据库AP1不同，STATIC、KEYSET 和DYNAMIC Transact-SQL 游标支持 FORWARD ONLY.FAST FORWARD 和FORWARD ONLY是互斥的；如果指定一个，则不能指定另一个
STATIC|定义一个游标，以创建将由该游标使用的数据的临时复本。对游标的所有请求都从tempdb 中的该临时表中得到应答：因此，在对该游标进行提取操作时返回的数据中不反映对基表所做的修改，并且该游标不允许修改
KEYSET|指定当游标打开时，游标中行的成员资格和顺序已经固定。对行进行唯一标识的键集内置在tempdb内一个称为keyset的表中。对基表中的非键值所做的更改（由游标所有者更改或由其他用户提交）在用户滚动游标时是可视的。其他用户进行的插入是不可视的（不能通过Transact-SOL服务器游标进行插入）。如果某行已删除，则对该行的提取操作将返回。@@FETCH_STATUS值-2。从游标外更新键值类似于删除旧行后接着插入新行的操作。含有新值的行不可视，对含有旧值的行的提取操作将返回QQFETCH_STATUS值-2。如果通过指定WHERE CURRENTOF子句用游标完成更新，则新值可视
DYNAMIC|定义一个游标，以反映在滚动游标时对结果集内的行所做的所有数据的更改。行的数据值、 顺序和成员在每次提取时都会更改。动态游标不支持ABSOLUTE提取选项
FAST FORWARD |指明一个FORWARD ONLY、READ ONLY 型游标
SCROLL_LOCKS |指定确保通过游标完成的定位更新或定位删除可以成功。将行读入游标以确保它们可用于以后的修改时，SQL Sever会锁定这些行。如果还指定了FAST_FORWARD，则不能指定SCROLL_LOCKS
OPTIMISTIC|指明在数据被读入游标后，如果游标中某行数据已发生变化，那么对游标数据进行更新或删除可能会导致失败
TYPE_WARNING|指定如果游标从所请求的类型隐性转换为另一种类型，则给客户端发送警告消息

#### 创建一个名为Cur_emp的标准游标
```sql
Use database_name
DEClARE Cur_emp CURSOR 
FOR Select * from Employee
GO
```

#### 创建一个名为Cur_emp_01的只读游标
```sql
Use database_name
DEClARE Cur_emp_01 CURSOR 
FOR Select * from Employee
FOR READ ONLY --只读游标
GO
```
#### 创建一个名为Cur_emp_02的更新游标
```sql
Use database_name
DEClARE Cur_emp_01 CURSOR 
FOR Select Name,age,gender from Employee
FOR UPDATE --更新游标
GO
```


:star: 打开游标

语法格式如下
```sql
OPEN{{[GLOBAL]cursor_name}|cursor_variable_name}
```
参数说明如下:   
- [x] GOLBAL:指定cursor_name为全局游标
- [x] cursor_name:已声明的游标名称，如果全局游标和局部游标都使用cursor_nane作为其名称，那么如果指定了GLOBAL,cursor_name指的是全局游标，否则，指的是局部游标
- [x] cursor_variable_name:游标变量的名称，该名称引用一个游标。

首先声明一个名为emp_01的游标，然后使用open命令打开该游标
```sql
use database
declare emp_01 cursor -- 声明游标
for select * from Employee
where ID='1'
open emp_01       --打开游标
GO
```


:star: 读取游标中的信息

语法如下:
```sql
Fetch
  [[NEXT|Prior|FIRST|Last
      |ABSOLUTE{n|@nvar}
      |RELATIVE{n|@nvar}
    ]
  FROM
  ]
  {{[GLOBAL] cursor_name}|@cursor_variable_name}
  [INTO @variable_name[,...n]]
```

参数说明:

|参数|描述|
|:--|:--|
NEXT|返回紧跟当前行之后的结果行，并且当前行递增为结果行。如果FETCH为对游标的第一次提取操作，则返回结果集中的第一行。NEXT为默认的游标提取选项
PRIOR|返回紧临当前行前面的结果行，并且当前行递减为结果行。如果FETCH PRIOR为对游标的第一次提取操作，则没有行返回并且游标置于第一行之前
FIRST|回游标中的第一行并将其作为当前行
LAST |返回游标中的最后一行并将其作为当前行
ABSOLUTE{n|@nvar}|如果n或@nvar为正数，返回从游标头开始的第n行，并将返回的行变成新的当前行。如果n或@nvar为负数，返回游标尾之前的第n行，并将返回的行变成新的当前行。如果n或anvar为0，则没有行返回
RELATIVE{n|@nvar}|如果n或@nvar为正数，返回当前行之后的第n行，并将返回的行变成新的当前行。如果n
|或@nvar为负数，返回当前行之前的第n行，并将返回的行变成新的当前行。如果n或@nvar为0，返回当前行。如果对游标的第一次提取操作时将FETCHRELATIVE的n或@nvar指定为负数或0，则没有行返回。n必须为整型常量且@nvar 必须为smallint、tinyint 或int 
GLOBAL|指定cursor_name为全局游标
cursor_name|要从中进行提取的开放游标的名称。如果同时有以作为名称的全局和局部游标cursor_name |存在，若指定为GLOBAL，则cursor_name对应于全局游标，未指定GLOBAL，则对应于局部游标
@cursor_variable_name |游标变量名，引用要进行提取操作的打开的游标
INTO @variable name.…n]|允许将提取操作的列数据放到局部变量中。列表中的各个变量从左到右与游标结果集中的相应列相关联。各变量的数据类型必须与相应的结果列的数据类型匹配或是结果列数据类型所支持的隐性转换。变量的数目必须与游标选择列表中的列的数目一致
@@FETCH_STATUS|返回上次执行FETCH命令的状态。在每次用FETCH从游标中读取数据时，都应检查该变量，以确定上次FETCH操作是否成功，决定如何进行下一步处理。@@FETCH-STATUS 变量有3个不同的返回值，说明如下：（1）返回值为0，FETCH语句成功；（2）返回值为-1，FETCH语句失败或此行不在结果集中：（3）返回值为-2，被提取的行不存在


用@@FEACH_STATUS控制一个while循环中的游标活动，SQL语句入下：
```sql
use database
declare ReadCursor cursor 
for select * from  student
open ReadCursor 
fetch next from ReadCursor
WHILE @@FETCH_STATUS=0
BEGIN
  FETCH NEXT FROM ReadCursor
END
```

:star:关闭游标  
使用CLOSE语句关闭游标，但不释放游标占用的系统资源

语法如下:
```sql
CLOSE {{[GOLBAL]cursor_name}|cursor_variable_name}
```
参数说明如下:  
- [x] GLOBAL:指定cursor_name为全局游标
- [x] cursor_name:开放游标的名称。如果全局游标和局部游标都使用cursor_nane作为其名称，那么如果指定了GLOBAL,cursor_name指的是全局游标，否则，指的是局部游标
- [x] cursor_variable_name:与开放游标关联的游标变量名称

SQL语句如下：
```sql
use database 
declare CloseCursor cursor 
for select * from student 
for read only 
open closecursor
close CloseCursor
```

:star:释放游标  
使用deallocate 命令删除游标引用，当释放最后的游标引用时，组成该游标的数据结构有SQL Server释放

语法如下:
```sql
deallocate {{[GLOBAL]cursor_name}|@cursor_variable_name}
```
参数说明如下:   
- [x]  cursor_name: 已声明游标的名称。如果全局游标和局部游标都使用cursor_nane作为其名称，那么如果指定了GLOBAL,cursor_name指的是全局游标，否则，指的是局部游标
- [x] @cursor_variable_name: cursor 变量的名称。@cursor_variable_name必须为cursor类型
---

当使用deallocate @cursor_variable_name 来删除游标时，游标变量并不会被删除，除非超过使用游标的存储过程和触发器范围。

使用deallocate命令释放名为FreeCursor的游标,SQL语句如下
```sql
use database
declare FreeCursor cursor
FOR select * from student
open FreeCursor
Close FreeCursor
deallocate FreeCursor
```

### &nbsp;&nbsp; <a id="04"></a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star: 

---
### &nbsp;&nbsp; <a id="05"></a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star: 

---
### &nbsp;&nbsp; <a id="06"></a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star: 

---
### &nbsp;&nbsp; <a id="07"></a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star:

### &nbsp;&nbsp; <a id="08"></a>&nbsp;&nbsp;<a href="#top">:blue_book:</a>

:star:

---











