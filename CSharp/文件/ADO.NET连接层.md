<a  id="top" href="#top">:collision:ADO.NET连接层:collision: </a>


- [x] <a href="#01">`什么是ADO.NET`</a>:sunflower:
- [x] <a href="https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/%E6%96%87%E4%BB%B6/Integrated%20Security%3DSSPI%EF%BC%8Cture%EF%BC%8Cfalse%E7%9A%84%E8%AF%B4%E6%98%8E.md">`integrated security`<a>:sunflower:
- [x] <a href="#03">``</a>:sunflower:
- [x] <a href="#04">``</a>:sunflower:
- [x] <a href="#05">``</a>:sunflower:
- [x] <a href="#06">``</a>:sunflower:
- [x] <a href="#07">``</a>:sunflower:
- [x] <a href="#08">``</a>:sunflower:

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

