<a  id="top" href="#top">:collision:ADO.NET连接层:collision: </a>


- [x] <a href="#01">`什么是ADO.NET`</a>:sunflower:
- [x] <a href="https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/%E6%96%87%E4%BB%B6/Integrated%20Security%3DSSPI%EF%BC%8Cture%EF%BC%8Cfalse%E7%9A%84%E8%AF%B4%E6%98%8E.md">`integrated security`<a>:sunflower:
- [x] <a href="#03">`ADO.NET SqlConnection类`</a>:sunflower:
- [x] <a href="#04">`ADO.NET SqlCommand类`</a>:sunflower:
- [x] <a href="#05">`ADO.NET SqlDataReader类`</a>:sunflower:
- [x] <a href="#06">`ADO.NET DataSet类`</a>:sunflower:
- [x] <a href="#07">`ADO.NET DataAdapter类`</a>:sunflower:
- [x] <a href="#08">`ADO.NET DataTable类`</a>:sunflower:

#### &nbsp;&nbsp;<a id="01">什么是ADO.NET</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `1.什么是ADO.NET`
>   ADO.NET是微软.NET平台的数据库操作技术。
>   ADO.NET提供了多个开发库，开发程序，实现多种数据库操作。
>   我们重点是讲解SQL Server开发应用程序库的使用。
>   因为ADO.NET操作SQL Server是最快，最方便，最安全的。
* `2.ADO.NET提供了两套开发库，来操作SQL Server`
>   (1)控件库
>>       通过拖拽控件方式，很方便的能够实现数据库操作(非专业人员使用)
>  （2）动态库
>>      通过这些类库，可以快速，敏捷的操作数据库，并且很方便的根据客户需求完成各种开发功能。
      `命名空间：System.Data.SqlClient;`
* `3.根据操作数据库的方式，又分类两类`
>   （1）非断开式操作
>>        Connection:一般是使用SqlConnection,主要实现数据库连接。
>>        Command:一般是使用SqlCommand,主要是发送各种数据库操作指令。
>>        DataReader:一般是使用SqlDataReader,主要是用来读取数据。
>   （2）断开式操作
>>        Connection:一般是使用SqlConnection,主要实现数据库连接。
>>        DataAdapter:一般是使用SqlDataAdapter，主要是断开式操作
>>        DataSet:内存小型数据库，用于临时存储数据。
>>        DataTable:数据表，用来临时存储数据，传送数据，操作数据。

* `连接数据库并创建一张表`
```csharp
using System;
using System.Data.SqlClient;

namespace Programe
{
    class Program
    {
        static void Main(string[] args)
        {
            new Program().CreateTable();
        }
        public void CreateTable()
        {
            SqlConnection con = null;
            try
            {
                // Creating Connection--关于integrated security设置的说明再相同目录下我有说明。  
                con = new SqlConnection("data source=.; database=student; integrated security=SSPI");
                // 创建一个sql表 ，有2个参实第一个参数是sql语句，第二个参数是应用的连接对象。 
                SqlCommand cm = new SqlCommand("create table student_info(id int not null,name varchar(100), email varchar(50), join_date date)", con);  
                // 打开连接  
                con.Open();
                // 执行sql查询  
                cm.ExecuteNonQuery();
                // 打印一条消息 
                Console.WriteLine("Table created Successfully");
            }
            catch (Exception e)
            {
                Console.WriteLine("OOPs, something went wrong." + e);
            }
            //关闭连接  
            finally
            {
                con.Close();
            }
        }
    }
}
```

* `插入一条数据`
```csharp
using System;
using System.Data.SqlClient;

namespace AdoNetConsoleApplication
{
    class AdoNetInsert
    {
        static void Main(string[] args)
        {
            new AdoNetInsert().InsertTable();
        }
        public void InsertTable()
        {
            SqlConnection con = null;
            try
            {
                // Creating Connection  
                con = new SqlConnection("data source=.; database=student; integrated security=SSPI");
                // writing sql query  
                String sql = "insert into student_info(id, name, email, join_date)values('101', 'Hiniu Su', 'hinew.su@example.com', '2017-11-18')";
                SqlCommand cm = new SqlCommand(sql, con); 
                // Opening Connection  
                con.Open();
                // Executing the SQL query  
                cm.ExecuteNonQuery();
                // Displaying a message  
                Console.WriteLine("插入数据记录成功~！");
            }
            catch (Exception e)
            {
                Console.WriteLine("OOPs, something went wrong." + e);
            }
            // Closing the connection  
            finally
            {
                con.Close();
            }
        }
    }
}
```

* `查询/检索记录`
```csharp
using System;
using System.Data.SqlClient;

namespace AdoNetConsoleApplication
{

    class AdoNetSelect
    {
        static void Main(string[] args)
        {
            new AdoNetSelect().SelectTable();
        }
        public void SelectTable()
        {
            SqlConnection con = null;
            try
            {
                // Creating Connection  
                con = new SqlConnection("data source=.; database=student; integrated security=SSPI");
                // writing sql query  
                SqlCommand cm = new SqlCommand("SELECT * FROM student_info", con);
                // Opening Connection  
                con.Open();
                // Executing the SQL query  
                SqlDataReader sdr = cm.ExecuteReader();
                Console.WriteLine("当前 student_info 表中的记录为：" );
                // Iterating Data  
                while (sdr.Read())
                {
                    Console.WriteLine(sdr["id"] + " " + sdr["name"] + " " + sdr["email"]); // Displaying Record  
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("OOPs, something went wrong.\n" + e);
            }
            // Closing the connection  
            finally
            {
                con.Close();
            }
        }
    }
}
```
      
* `删除记录`
```csharp
using System;
using System.Collections.Generic;
using System;
using System.Data.SqlClient;

namespace AdoNetConsoleApplication
{

    class AdoNetDelete
    {
        static void Main(string[] args)
        {
            new AdoNetDelete().DeleteFromTable();
        }
        public void DeleteFromTable()
        {
            SqlConnection con = null;
            try
            {
                // Creating Connection  
                con = new SqlConnection("data source=.; database=student; integrated security=SSPI");
                // writing sql query  
                SqlCommand cm = new SqlCommand("delete from student_info where id = '101'", con);
                // Opening Connection  
                con.Open();
                // Executing the SQL query  
                cm.ExecuteNonQuery();

                Console.WriteLine("已经成功地删除了编号为：101 的学生数据信息~！");

                // 重新查询数据库中的记录信息
                SqlCommand cm2 = new SqlCommand("SELECT * FROM student_info", con);
                // Executing the SQL query  
                SqlDataReader sdr = cm2.ExecuteReader();
                Console.WriteLine("当前 student_info 表中的记录为：");
                // Iterating Data  
                while (sdr.Read())
                {
                    Console.WriteLine(sdr["id"] + " " + sdr["name"] + " " + sdr["email"]); // Displaying Record  
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("OOPs, something went wrong.\n" + e);
            }
            // Closing the connection  
            finally
            {
                con.Close();
            }
        }
    }
}
```


#### &nbsp;&nbsp; <a id="02"> </a>:flags:<a href="#top">顶部</a>:arrow_upper_left:



#### &nbsp;&nbsp; <a id="03"> </a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* ``

#### &nbsp;&nbsp; <a id="04"> </a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="05"> </a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="06"> DataSet</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
`ADO.Net`的`DataSet`类包含数据的数据表集合。它用于在不与数据源交互的情况下获取数据，这就是为什么它也被称为断开数据访问方法。这是一个内存数据存储，可以同时容纳多个表。可以使用`DataRelation`对象来关联这些表。 `DataSet`也可以用来读写XML文档中的数据。   
ADO.NET提供了一个可用于创建DataSet对象的DataSet类。它包含执行数据相关操作的构造函数和方法。

* `DataSet类构造函数`

|构造函数|作用|
|:--|:--|
|DataSet()|它用于初始化DataSet类的新实例。|
DataSet(String)|它用于使用给定名称初始化DataSet类的新实例。
DataSet(SerializationInfo, StreamingContext)|它用于初始化具有给定序列化信息和上下文的DataSet类的新实例。
DataSet(SerializationInfo, StreamingContext, Boolean)|它用于初始化DataSet类的新实例。

* `DataSet类属性`

|属性|作用|
|:--|:--|
CaseSensitive|它用于检查DataTable对象是否区分大小写。
DataSetName|它用于获取或设置当前DataSet的名称。
DefaultViewManager|它用于获取DataSet中包含的数据的自定义视图，以允许过滤和搜索。
HasErrors|它用于检查此DataSet中的任何DataTable对象中是否有错误。
IsInitialized|它用于检查DataSet是否被初始化。
Locale|它用于获取或设置用于比较表中字符串的语言环境信息。
Namespace|它用于获取或设置DataSet的名称空间。
Site|它用于获取或设置DataSet的ISite。
Tables|它用于获取DataSet中包含的表的集合。

|方法|作用|
|:--|:--|
BeginInit()|它用于在窗体上使用的DataSet的初始化。
Clear()|它用于通过删除所有表中的所有行来清除任何DataSet中的数据。
Clone()|它用于复制DataSet的结构。
Copy()|它用于复制此DataSet的结构和数据。
CreateDataReader(DataTable[])|它将为每个DataTable返回一个带有一个结果集的DataTableReader。
CreateDataReader()|它将为每个DataTable返回一个带有一个结果集的DataTableReader。
EndInit()|它结束在窗体上使用的DataSet的初始化。
GetXml()|它返回存储在DataSet中的数据的XML表示形式。
GetXmlSchema()|它返回存储在DataSet中的数据的XML表示的XML Schema。
Load(IDataReader, LoadOption, DataTable[])|它用于使用提供的IDataReader从数据源填充数据集。
Merge(DataSet)|它用于将指定的DataSet及其模式合并到当前的DataSet中。
Merge(DataTable)|它用于将指定的DataTable及其模式合并到当前的DataSet中。
ReadXml(XmlReader, XmlReadMode)|它用于使用指定的XmlReader和XmlReadMode将XML模式和数据读入DataSet。
Reset()|它用于清除所有表，并从DataSet中删除所有关系，外部约束和表。
WriteXml(XmlWriter, XmlWriteMode)|它用于使用指定的XmlWriter和XmlWriteMode编写DataSet的当前数据和可选的模式。


#### &nbsp;&nbsp; <a id="07">DataAdapter</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
DataAdapter类作为DataSet和数据源之间的桥梁来检索数据。 DataAdapter是一个代表一组SQL命令和一个数据库连接的类。它可以用来填充数据集并更新数据源。
* `DataAdapter构造函数`

|构造函数|作用|
DataAdapter()|它用于初始化DataAdapter类的新实例。
DataAdapter(DataAdapter)|它用于从相同类型的现有对象初始化DataAdapter类的新实例。


|方法|作用|
|:--|:--|
CloneInternals()|它用于创建DataAdapter的这个实例的副本。
Dispose(Boolean)|它用于释放DataAdapter使用的非托管资源。
FillSchema(DataSet, SchemaType, String, IDataReader)|它用于将DataTable添加到指定的DataSet。
GetFillParameters()|它用于在执行SQL SELECT语句时获取用户设置的参数。
ResetFillLoadOption()|它用于将FillLoadOption重置为默认状态。
ShouldSerializeAcceptChangesDuringFill()|它确定是否应该保持AcceptChangesDuringFill属性。
ShouldSerializeFillLoadOption()|它确定是否应该保持FillLoadOption属性。
ShouldSerializeTableMappings()|它确定是否存在一个或多个DataTableMapping对象。
Update(DataSet)|它用于调用相应的INSERT，UPDATE或DELETE语句。

#### &nbsp;&nbsp; <a id="08">DataTable</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
DataTable类将关系数据表示为表格形式。ADO.NET提供了一个DataTable类来独立创建和使用数据表。它也可以和DataSet一起使用。 最初，当创建DataTable时，它没有表模式。我们可以通过向表中添加列和约束来创建表模式。在定义表模式之后，可以向表中添加行。

