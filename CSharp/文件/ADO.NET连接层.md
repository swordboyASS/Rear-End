<a  id="top" href="#top">:collision:ADO.NET连接层:collision: </a>


- [x] <a href="#01">`什么是ADO.NET`</a>:sunflower:
- [x] <a href="#02">``</a>:sunflower:
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
>>       通过拖拽控件方式，很方便的能够实现数据库操作
>  （2）动态库
>>      通过这些类库，可以快速，敏捷的操作数据库，并且很方便的根据客户需求完成各种开发功能。
      `命名空间：System.Data.SqlClient;`
* `3.根据操作数据库的方式，又分类两类`
>   （1）非断开式操作
>>        Connection:一般是使用SqlConnection,主要实现数据库连接。
>>       Command:一般是使用SqlCommand,主要是发送各种数据库操作指令。
>>        DataReader:一般是使用SqlDataReader,主要是用来读取数据。
>   （2）断开式操作
>>        Connection:一般是使用SqlConnection,主要实现数据库连接。
>>       DataAdapter:一般是使用SqlDataAdapter，主要是断开式操作
>>       DataSet:内存小型数据库，用于临时存储数据。
>>        DataTable:数据表，用来临时存储数据，传送数据，操作数据。






