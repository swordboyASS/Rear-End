
<a  id="top" href="#top">:collision:HTML辅助方法:collision: </a>


- [x] <a href="#01">**`表单的使用`**</a>:sunflower:
- [x] <a href="#02">**`HTML辅助方法`**</a>:sunflower:
- [x] <a href="#03">**`其他输入辅助方法`**</a>:sunflower:
- [x] <a href="#04">**`渲染辅助方法`**</a>:sunflower:
- [x] <a href="#05">**``**</a>:sunflower:
- [x] <a href="#06">**``**</a>:sunflower:
- [x] <a href="#07">**``**</a>:sunflower:
- [x] <a href="#08">**``**</a>:sunflower:

#### &nbsp;&nbsp; <a id="01">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:

* **`action和method特性`**:+1:
1. action特性告知Web浏览器信息发往哪里--包含一个RUL
2. method特性告诉浏览器用POST还是GET方式发送HTTP请求。


4.表单控件  
这里面的方法会根据参数个数自动装配form各标签特性
* `文本框`@Html.TextBox("userName")
* `标签框`@Html.Label()
* `Hidden`@Html.Hidden()//同TextBox，但它是隐藏的   
* 没有按钮的封装，只能使用html的按钮标签
* `复选框` @Html.CheckBox()
* `单选框` @Html.RadioButton()
* `下拉列表`@Html.DropDownList()  DropDownList：在Action中向ViewData中传递一个List<SelectListltem>集合，在View中指向ViewData的参数，则会以下拉列表的形式展示数据

* 2种表单方式：
    
```razor
        @using (Html.BeginForm(actionName, controllerName))
        {...}
        @Html.BeginForm(actionName, controllerName))
        ...
        @Html.EndForm();
```

* `下拉列表演示`
```csharp
        public ActionResult HtmlTest()
        {
            //行为->传递数据->视图
            ViewBag.name = "GaoJu";
            List<SelectListItem> list = new List<SelectListItem>();
            list.Add(new SelectListItem() {
                Selected = false,
                Text = "xy",
                Value = "1"
            });
            list.Add(new SelectListItem()
            {
                Selected = false,
                Text = "hh",
                Value = "2"
            });
            list.Add(new SelectListItem()
            {
                Selected = false,
                Text = "aa",
                Value = "3"
            });
            list.Add(new SelectListItem()
            {
                Selected = false,
                Text = "bb",
                Value = "4"
            });
            ViewBag.ddlist = list;
            return View();
        }
```

```razor
        @Html.DropDownList("ddlist")
```




#### &nbsp;&nbsp; <a id="02">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="03">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="04">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="05">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="06">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="07">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="08">** **</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:




