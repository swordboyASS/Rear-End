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
* 汽车类
```csharp
    class Car
    {
        public string PetName { get; set; }
        public string Color { get; set; }
        public int Speed { get; set; }
        public string Make { get; set; }
    }
```
* 初始化
```csharp
    class Program
    {
        //查询泛型集合
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


#### &nbsp;&nbsp; <a id="05">查询操作符</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="06">内部表示</a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="07"></a>:flags:<a href="#top">顶部</a>:arrow_upper_left:


#### &nbsp;&nbsp; <a id="08"></a>:flags:<a href="#top">顶部</a>:arrow_upper_left:









