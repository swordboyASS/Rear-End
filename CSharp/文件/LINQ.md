<a  id="top" href="#top">:collision: LINQ-Language Integrated Query(语言集成查询)  :collision: </a>


- [x] <a href="#01">`LINQ特有的编程结构`</a>:sunflower:
- [x] <a href="#02">`LINQ的作用`</a>:sunflower:
- [x] <a href="#03">`LINQ查询用于原始数组`</a>:sunflower:
- [x] <a href="#04">`应用到集合对象`</a>:sunflower:
- [x] <a href="#05">`查询操作符`</a>:sunflower:
- [x] <a href="#06">`内部表示`</a>:sunflower:
- [x] <a href="#07">``</a>:sunflower:
- [x] <a href="#08">``</a>:sunflower:




#### &nbsp;&nbsp; <a id="01">LINQ特有的编程结构</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
> 隐士类型本地变量

> 对象/集合初始化语法

> Lambda表达式

> 扩展方法

> 匿名类型

#### &nbsp;&nbsp; <a id="02">LINQ的作用</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `LINQ应用场景如下：`
> LINQ to Object：针对数组和集合使用的LINQ查询。  
> LINQ to XML：使用LINQ来操纵和查询XML文档。  
> LINQ to DataSet：针对ADO.NET Dataset对象使用的LINQ查询。  
> LINQ to Entity：对ADO.NET Entity Framework（EF）APl使用的LINQ查询。  
> Parallel LINQ（PLINQ）：并行处理LINQ查询返回的数据。  
* `LINQ查询表达式是强类型的`必须保证表达式在语法上合法的。

#### &nbsp;&nbsp; <a id="03">LINQ查询用于原始数组</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `创建一个数组并进行LINQ操作`
```csharp
        static void QueryArray()
        {
            string[] currrentArray = { "person", "animal 2", "fallout 3" ,"fight"};

            IEnumerable<string> subset = from g in currrentArray
                                         where g.Contains(" ")
                                         orderby g
                                         select g;
            foreach (string s in subset)
            {
                Console.WriteLine($"Item:{s}");
            }
        }
```
使用了，from、in、where、orderby、select这几个LINQ查询符。 g只是一个变量名。
`有了LINQ查询可以大大减少对复杂逻辑的使用，是编程取数据子集变得更加方便`

##### `因为往往LINQ查询的返回值是实现了泛型IEnumerable<T>接口类型，写出来显得很笨重，所以就用匿名类型替代了`
```csharp
        static void QueryArray()
        {
            int[] array = { 6, 8, 78, 78, 78, 78, 783, 535,23, 2345 };
            //使用因式类型
            var subset = from i in array where i < 10 select i;
            //这里便也是隐式类型
            foreach (var s in subset)//实际上Array类并没有实现IEnumerable<T>接口但是通过扩展方法添加了该功能，所以我们间接的也使用了扩展方法
            {
                Console.WriteLine($"Item:{s}");
            }
        }
```
* `延迟执行`
```csharp
        static void QueryArray()
        {
            int[] array = { 6, 8, 78, 78, 78, 78, 783, 535,23, 2345 };
            var subset = from i in array where i < 10 select i;

            foreach (var s in subset)
            {
                Console.WriteLine($"{s}<10");
            }
            Console.WriteLine();

            array[2] = 5;
            foreach (var s2 in subset)
            {
                Console.WriteLine($"{s2}<10");
            }
        }
        //结果：
        6<10
        8<10

        6<10
        8<10
        5<10

        
```
* `操作数据在foreach之外`
```csharp
        static void QueryArray()
        {
            int[] array = { 6, 8, 78, 78, 78, 78, 783, 535,23, 2345 };
            //var subset = from i in array where i < 10 select i;

            int[] subsetIntArray = (from i in array where i < 10 select i).ToArray<int>();
            subsetIntArray[0] = 5;
            Console.WriteLine(subsetIntArray[0]); //5
            List<int> subsetListOfInts = (from i in array where i < 10 select i).ToList<int>();
        }
```

#### &nbsp;&nbsp; <a id="04">应用到集合对象</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `汽车类`
```csharp
    class Car
    {
        public string PetName { get; set; }
        public string Color { get; set; }
        public int Speed { get; set; }
        public string Make { get; set; }
    }
```
* `初始化--查询泛型集合`
```csharp
    class Program
    {
        
        static void GetFastCar(List<Car> car)
        {
            var fastCar = from i in car where i.Speed>90 && i.Make == "BMW" select i;
            
            foreach (var item in fastCar)
            {
                Console.WriteLine(item.ToString());
            }
        }
        static void Main(string[] args)
        {
            //对象初始化语法，生成一系列Car对象
            List<Car> myCars = new List<Car>
            {
            new Car {PetName="Henry",Color="Silver",Speed=100 ,Make ="BMW" },
            new Car { PetName = "Daisy", Color = "Tan", Speed = 90, Make ="BMW" },
            new Car { PetName = "Mary", Color = "Black", Speed =55 , Make ="VW" },
            new Car { PetName = "Clunker", Color = "Rust", Speed =5 , Make ="Yugo" },
            new Car { PetName = "Melvin", Color = "White", Speed =43 , Make ="Ford" }
            };
            GetFastCar(myCars);
        Console.ReadLine();
        }

    }
```
* `初始化--查询非泛型集合`  
延用了上述的Car类
```csharp
            //对象初始化语法，生成一系列Car对象
            ArrayList myCars = new ArrayList()
            {
            new Car {PetName="Henry",Color="Silver",Speed=100 ,Make ="BMW" },
            new Car { PetName = "Daisy", Color = "Tan", Speed = 90, Make ="BMW" },
            new Car { PetName = "Mary", Color = "Black", Speed =55 , Make ="VW" },
            new Car { PetName = "Clunker", Color = "Rust", Speed =5 , Make ="Yugo" },
            new Car { PetName = "Melvin", Color = "White", Speed =43 , Make ="Ford" }
            };
            //把 myCars转换成一个兼容IEnumberale<T>的类型
            var myCarsEnum = myCars.OfType<Car>();

            //建立兼容的LINQ表达式
            var fastCars = from i in myCarsEnum where i.Speed > 56 select i;
            foreach (var item in fastCars)
            {
                Console.WriteLine(item);
            }
```
* `使用OfType<T>()筛选数据`
        
```csharp
               //使用OfType<T>()筛选数据
            ArrayList arrayList = new ArrayList();
            arrayList.AddRange(new object[] { 10, 20, 50, false, new Car(), "string" });
            var choose = arrayList.OfType<int>();
            foreach (var item in choose)
            {
                Console.WriteLine(item);// 10, 20 ,50
            }     
```       

#### &nbsp;&nbsp; <a id="05">查询操作符</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `各种查询操作符`

|查询操作符|含义|
|:--|:--|
from、in|用于定义任何LINQ表达式的主干，允许从合适的容器中提取数据子集
where|用于定义从一个容器里取出哪些项的限制条件
select|用于从容器中选择一个序列
join、on、equals、into|基于指定的键来做关联操作。记住，这些“关联”不必与关系数据库的数据有什么关系
orderby、ascending、descending |允许结果子集按升序或降序排序
group、by|用特定的值来对数据分组后得到一个子集

`基本语法模板` `var resule = from matchingItem in container where BooleanExpression select machingItem`

* `投影新数据类型` 了解即可
```csharp
        //动态生成一个匿名类型，得到只有描述和名称的结果。
        var result = from p in products select new {p.Name,p.Speed}
        //使用数组转换方法,才能正确的使用
        result.ToArray();
        
```

* `获取总数`-Count()方法
* `反转结果集`-Reverse<T>()方法
        
* `对表达式进行排序`        
```csharp
        static void Main(string[] args)
        {
            //对象初始化语法，生成一系列Car对象
            List<Car> myCars = new List<Car>()
            {
            new Car {PetName="Henry",Color="Silver",Speed=100 ,Make ="BMW" },
            new Car { PetName = "Daisy", Color = "Tan", Speed = 90, Make ="BMW" },
            new Car { PetName = "Mary", Color = "Black", Speed =55 , Make ="VW" },
            new Car { PetName = "Clunker", Color = "Rust", Speed =5 , Make ="Yugo" },
            new Car { PetName = "Melvin", Color = "White", Speed =43 , Make ="Ford" }
            };
            var result = from i in myCars orderby i.PetName select i;
            foreach (var item in result)
            {
                Console.WriteLine(item);
                //按PetName字母顺序进行排序。正序，从小到大
                //PetName = Clunker,Color = Rust,Speed = 5,Company = Yugo
                //PetName = Daisy,Color = Tan,Speed = 90,Company = BMW
                //PetName = Henry,Color = Silver,Speed = 100,Company = BMW
                //PetName = Mary,Color = Black,Speed = 55,Company = VW
                //PetName = Melvin,Color = White,Speed = 43,Company = Ford
            }


        Console.ReadLine();
        }
```

* `韦恩图工具`

|方法|作用|
|:--|:--|
Except |对称差
Intersect |交集
Union |并集
Concat| 连接

```csharp
            List<string> person1 = new List<string> { "gaoju", "sichuan", "chengdu" };
            List<string> person2 = new List<string> { "chengdu", "sichuan", "BBC" };
            var result = (from i in person1 select i).Except(from i in person2 select i);
            var result2 = (from i in person1 select i).Intersect(from i in person2 select i);
            var result3 = (from i in person1 select i).Union(from i in person2 select i);
            var result4 = (from i in person1 select i).Concat(from i in person2 select i);
            foreach (var item in result4)
            {
                Console.WriteLine(item);
            }
```
`有时候通过Concat（）获得的有重复` `使用Distinct()方法去掉重复`

*  `LINQ聚合操作`

|方法|作用|
|:--|:--|
Count |元素个数
Average |平均值
Max |最大值
Min |最小值
Sum |求和




#### &nbsp;&nbsp; <a id="06">内部表示</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:
* `这部分相较于上述方法来说更为复杂`--有用到再来学

#### &nbsp;&nbsp; <a id="07"></a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="08"></a>:flags:<a href="#top">顶部</a>:arrow_upper_left:









