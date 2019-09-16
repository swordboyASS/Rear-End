解决方法：
即：Security Support Provider Interface
设置Integrated Security为 True 的时候，连接语句前面的 UserID, PW 是不起作用的，即采用windows身份验证模式。
只有设置为 False 或省略该项的时候，才按照 UserID, PW 来连接。
Integrated Security 可以设置为: True, false, yes, no ，这四个的意思很明白了，还可以设置为：sspi ，相当于 True，建议用这个代替 True。

initial catalog与database的区别是什么
        Initial Catalog: 
DataBase: 
两者没有任何区别只是名称不一样，就好像是人类的真实姓名与曾用名一样。。都可以叫你。

********************************************

Integrated Security=SSPI 这个表示以当前WINDOWS系统用户身去登录SQL SERVER服务器，如果SQL SERVER服务器不支持这种方式登录时，就会出错。 
你可以使用SQL SERVER的用户名和密码进行登录，如： 
"Provider=SQLOLEDB.1;Persist Security Info=False;Initial Catalog=数据库名;Data Source=192.168.0.1;User ID=sa;Password=密码"


***************************************************

Integrated  Security    -  或  -    Trusted_Connection  'false'  当为  false  时，将在连接中指定用户  ID  和密码。当为  true  时，将使用当前的  Windows  帐户凭据进行身份验证。  可识别的值为  true、false、yes、no  以及与  true  等效的  sspi（强烈推荐）。 


*************************************************

ADO.net  中数据库连接方式 
System.Data.SqlClient.SqlConnection 
常用的一些连接字符串(C#代码)：

SqlConnection  conn  =  new  SqlConnection(  “Server=(local);Integrated  Security=SSPI;database=Pubs“);

SqlConnection  conn  =  new  SqlConnection(“server=(local)\NetSDK;database=pubs;Integrated  Security=SSPI“);

SqlConnection  conn  =  new  SqlConnection(“Data  Source=localhost;Integrated  Security=SSPI;Initial  Catalog=Northwind;“);

SqlConnection  conn  =  new  SqlConnection(“  data  source=(local);initial  catalog=xr;integrated  security=SSPI; 
persist  security  info=False;workstation  id=XURUI;packet  size=4096;  “);

SqlConnection  myConn    =  new  System.Data.SqlClient.SqlConnection(“Persist  Security  Info=False;Integrated 
Security=SSPI;database=northwind;server=mySQLServer“);

SqlConnection  conn  =  new  SqlConnection(  “  uid=sa;pwd=passwords;initial  catalog=pubs;data  source=127.0.0.1;Connect  Timeout=900“);

在与 SQL Server 建立连接时出现与网络相关的或特定于实例的错误。未找到或无法访问服务器。请验证实例名称是否正确并且 SQL Server 已配置为允许远程连接。 (provider: 命名管道提供程序, error: 40 - 无法打开到 SQL Server 的连接)

如果你的机器装了sql2000 那Data Source=.肯定是不行的了 
因为实例名2000和2005的默认的是一样的 所以2005的实例肯定不能用Data Source=.表示 

查看sql2005的实例名 将Data Source=.\SQLEXPRESS 中的 SQLEXPRESS用你的新实例名替换掉。
![01](https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/Picture/01.png)
 



我装的是SQL Server 2005 EXPRESS 即VS2008自带的数据库,所以将Data Source写为:

Data Source=.\SQLEXPRESS即可.SQL2000之前用的.号不能在2005上使用.

今天还遇到一个问题,就是SQL 2005 EXPRESS 启用SA账号的问题.搞了半天不能用,尽管已经将SA启用,但是依然登陆不上,后来,将身份验证设置为SQL+Windows验证模式,才能在SQL Server Management Studio Express上登录.

VS2008其实已经自带了数据库,以及数据库驱动了,平时的开发调试完全可以用这个玩.只是没有数据库管理工具,所以无法建表,其实微软提供了免费的管理工具:

安装微软的SQL Server Management Studio Express就可以操作数据库了.

启用SA方法如下,开启MSE,用windows验证登陆,
![02](https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/Picture/02.png)
![03](https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/Picture/03.png)
![04](https://github.com/swordboyASS/Rear-end-Learing/blob/master/CSharp/Picture/04.png)
 





 

这样就能用SA来登录啦,当然可以自己修改密码.

数据库一打开,.NET能够连上数据库,进行正常的数据存取,那么之后的开发就容易多啦.
