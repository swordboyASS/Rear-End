<a  id="top" href="#top">:collision:  Razor :collision: </a>


- [x] <a href="#01">**``**</a>:sunflower:
- [x] <a href="#02">**``**</a>:sunflower:
- [x] <a href="#03">**``**</a>:sunflower:
- [x] <a href="#04">**``**</a>:sunflower:
- [x] <a href="#05">**``**</a>:sunflower:
- [x] <a href="#06">**``**</a>:sunflower:
- [x] <a href="#07">**``**</a>:sunflower:
- [x] <a href="#08">**``**</a>:sunflower:

#### &nbsp;&nbsp; <a id="01">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
Razor:'@' 符号 表示使用C#代码 ，还可以完成基本的运算 Razor 支持混编代码(C#和HTML混编)，`@i <br/>123` , <>尖括号之前的内容识别为C#代码，其后的内容识别为HTML代码 ，可喜的是在代码段中，有大括号的也会自动匹配
```razor
@foreach（var i=0;i<10;i++）
{
    if(i%2==0)
    {
        @i<br/>
    }

}
```

注释--和服务器端注释相同： @\*注释内容\*@ F12查看代码不会出现。
`<!--这是客户端注释，可以F12查看-->`

@using 引入名称空间

* **`HTMLHelper`**

1.链接
```razor
        <a href="/Home/Index">链接1</a> <br/> @*//传统的链接方式会受到路由规则的改变受影响，不安全*@
        <a href="@Url.Action("index","Home")">链接2</a><br />  @*稍微优化的方式，不至于被路由规则影响*@
        @Html.ActionLink("链接3","Index","Home") @*最简单的方式*@
```

2. Raw:页面中输出一行
```razor
@Html.Raw("666")
```

3. @Html.Encode(str); 对str这个字符串进行html编码
```razor
       @{ 
           string str = "<b>hhh</b>";
       }
       <br/>
        @Html.Encode(str);  //&lt;b&gt;hhh&lt;/b&gt;;
        
```


#### 


#### &nbsp;&nbsp; <a id="02">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="03">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="04">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="05">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="06">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="07">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="08">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:










