
<p id="title"></p>

## 目录

:arrow_down:<a href="#01">委托</a>

:arrow_down:<a href="#02">事件</a>

:arrow_down:<a href="#03">Lambda表达式</a>




<p id="01"></p>

:arrow_double_up:<a href="#title">返回目录</a>

### 委托
委托也是一个类
对回调函数的理解：回调函数就是一个通过函数指针调用的函数。如果你把函数的指针(地址)作为参数传递给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。

在.NET平台下，委托类型用来定义和响应应用程序中的回调。事实上，.NET委托类型是一个类型安全的对象，指向可以以后调用的其他方法。和传统的C++函数指针不同，.NET委托是内置支持多路广播和异步方法调用的对象。


委托是一个类型安全的对象，它指向程序中另一个以后会被调用的方法（或多个方法）。委托类型包含3个重要的信息：
1. 它所调用的方法的名称；
2. 该方法的参数（可选）；
3. 该方法的返回值类型（可选）。

#### 定义委托类型
使用delegate关键字定义委托类型
```cshrap
  //这个委托可以指向任何传入两个整数返回一个整数的方法
  public delegate int BinaryOp（int x，int y）；
```

#### System.MulticastDelegate与System.Delegate基类

|继承成员|作用|
|:--|:--|
Method|此属性返回System.Reflection.MethodInfo对象，用以表示委托维护的静态方法的详细信息
Target|如果方法调用是定义在对象级别的（而不是静态方法），Target返回表示委托维护的方法的对象。如果Target返回值为null，调用的方法是一个静态成员
Combine（）|此静态方法给委托维护的列表添加一个方法。在C#中，使用重载+=操作符作为简化符号调用此方法
CetInvocationList（）|此方法返回一个System.Delegate类型的数组，其中数组中的每个元素都表示一个可调用的特定方法
Remove（）,RemoveA11（）|这些静态方法从调用列表中移除一个（或所有）方法。在C#中，Remove（）方法可通过使用重载-=操作符来调用

#### 最简单的委托实例
```csharp
namespace GIthub
{
    //可以指向一个返回值为int，有2个int参数的方法。
    public delegate int BinaryOp(int x, int y);

    class SimpleMath
    {
        public static int Add(int x,int y) { return x + y; }
        public static int Subtract(int x, int y) { return x - y; }
        public int Show(int x, int y) { return x * y; }
    }

    class Program
    {
    static void Main(string[] args)
        {
            SimpleMath sm = new SimpleMath();
            BinaryOp b = new BinaryOp(SimpleMath.Add);
            BinaryOp c = new BinaryOp(sm.Show);

            Console.WriteLine(b(10, 10));//间接调用Add方法；
            Console.WriteLine(b.Invoke(10, 10));//这种做法和上面一样
            Console.WriteLine(b.Target);//因为b指向的是一个静态方法所以，对象并没有被引用。故不会有任何内容输出
            Console.WriteLine(c.Target);// 输出命名空间和所在的所指向的方法的类名。
            Console.WriteLine(b.Method);

            Console.ReadLine();
        }

    }
}
```


#### 使用委托发送对象状态通知
将某个委托定义在类里面，就要通过这个类去访问委托。
```csharp
         // 1) 定义委托类型
        public delegate void CarEngineHandler( string msgForCaller ); //这是一个委托类型

        // 2) 定义委托类型的成员变量
        private CarEngineHandler listOfHandlers; //这是一个委托变量

        // 3) 向调用者添加注册函数
        public void RegisterWithCarEngine( CarEngineHandler methodToCall )  //参数是一个委托类型，通过该方法注册
        {
            // listOfHandlers = methodToCall;
            // listOfHandlers += methodToCall; 
            // listOfHandlers += methodToCall; 
            if (listOfHandlers == null)
                listOfHandlers = methodToCall;
            else
                Delegate.Combine(listOfHandlers, methodToCall); //将第二个参数的方法注册到第一个参数-委托变量上
            // listOfHandlers += methodToCall;  这种方式是最简单的。
        }
        // 注销函数，注销某个方法。
        public void UnRegisterWithCarEngine( CarEngineHandler methodToCall )
        {
            listOfHandlers -= methodToCall;
        }     
        //main方法中。直接分配相关的委托对象。
        Car.CarEngineHandler handler2 = new Car.CarEngineHandler(OnCarEngineEvent2);
        
```


#### 方法组转换语法
不需要直接分配相关的委托对象
```csahrp

      //main方法
            Car c1 = new Car();

            // Register the simple method name.
            c1.RegisterWithCarEngine(CallMeHere);//不再创建委托对象了，而是直接调用注册函数，传入一个类型匹配的方法。
     //main方法       
             static void CallMeHere( string msg )
        {
            Console.WriteLine("=> Message from Car: {0}", msg);
        }       
      //
```


#### 泛型委托
泛型委托有着大量的应用。
```csharp
    //这个泛型委托可以调用任何返回void并接受单个参数的方法
    public delegate void MyGenericDelegate<T>(T arg);

    class GenericDelegate
    {
    }
```

#### Action<>和Function<>委托
1. Action<>委托指向返回值为void的方法
2. Func<>委托指向有返回值的方法，返回值为最后一个参数。
`Func<int,int,string>`这个function委托就指向有2个int参数返回值为string的方法。

委托列表
```csharp
            Delegate[] array = mydelegate.GetInvocationList();
            foreach (var item in array)
            {
                Console.WriteLine($"委托列表:{item}");
            }
```


<p id="02"></p>

:arrow_double_up:<a href="#title">返回目录</a>

### 事件
为了限制公共的委托成员对封装的冲突，引入了事件。

#### event关键字
为了简化自定义方法的构建来为委托调用列表增加和删除方法，C#提供了event关键字。在编译器处理event关键字的时候，它会自动提供注册和注销方法以及任何必要的委托类型成员变量。这些委托成员变量总是声明为私有的，因此不能直接从触发事件的对象访问它们。可以肯定的是，event关键字就像一块语法糖，只是节省了我们打字的时间。
```csharp
        // 1) 定义一个委托类型
        public delegate void CarEngineHandler( string msgForCaller );

        // 这个委托类型可以发送这些事件(订阅)
        public event CarEngineHandler Exploded;
        public event CarEngineHandler AboutToBlow;
        
        //调用
        Exploded("汽车爆炸了！");
        AboutToBlow("汽车快要爆炸了！");
        
        //以下是在main方法只的。
        1. 创建委托所在类的的对象，
        2. 通过该对象调用事件为其赋予一个或多个方法。
        
        //方法组转换语法
        c1.AboutToBlow += CarIsAlmostDoomed;（注册）
        
        public static void CarIsAlmostDoomed( string msg )
        { Console.WriteLine("=> Critical Message from Car: {0}", msg); }
        
        //调用事件(发布)
        AboutToBlow("Sorry, this car is dead...");

        

```

#### 事件的神秘面纱
C#事件事实上会扩展为两个隐藏的公共方法，一个带add前缀，另一个带remove_前缀。前缀后面是C#事件的名称。例如，Exploded事件产生两个隐藏方法，名为add_Exploded（）和remove_Exploded（）。


#### 泛型 EventHandler<T>委托
```csharp
  public event EventHandler<CarEventArgs> Exploded;  //限定事件类型。
  public event EventHandler<CarEventArgs> AboutToBlow;
```


<p id="03"></p>

:arrow_double_up:<a href="#title">返回目录</a>

### Lambda表达式

#### 匿名方法

```csharp
          //首先创建一个委托事件。
          public event Mydelegate Show;


            gd.Show += delegate
            {
                Console.WriteLine("匿名方法！");
            }; //注意这个分号是必须的
            
          //有参数的匿名方法，参数的类型必须和委托类型的参数类型相同。  
            gd.Show += delegate(int s1,int s2)
            {
                Console.WriteLine("有参数的匿名方法！");

            };
```

匿名方法的几个注意点:
* 匿名方法不能访问定义方法中的ref或out参数。
* 匿名方法中的本地变量不能与外部方法中的本地变量重名。
* 匿名方法可以访问外部类作用域中的实例变量（或静态变量）。
* 匿名方法内的本地变量可以与外部类的成员变量同名（本地变量的作用域不同，可以隐藏外部类的成员变量）。



#### Lambda表达式
Lambda表达式只是用更简单的方式来写`匿名方法`，彻底简化了对.NET委托类型的使用。  
一些例子：
1. 当你需要从一个集合中提取子集时，可以使用该方法，其原型如下：
```csharp
//System.Collections.Generic.List<T>类中的方法
public List<T> FindAll（Predicate<T>match）//唯一参数
```
如你所见，该方法返回新的List<T>;
  
2. 在调用FindA11（）时，List<T>中的每一项都将传入Predicate<T>对象所指向的方法。方法在实现时将执行一些计算，来判断传入的数据是否符合标准，并返回true或false。如果返回true，该项将被添加到表示子集的新List<T>中（明白了吗）。

```csharp
//FindA11（）方法使用该委托提取子集
public delegate bool Predicate<T>（T obj）；//该委托的参数是一个返回值为bool的排序方法；
```

一个完整的例子
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace GIthub
{
    class Program
    {
        static void Main(string[] args)
        {
            //TraditionShow();
            //AnonymousShow()
            LambdaShow();
            Console.ReadLine();
        }
        //传统方法
        static void  TraditionShow()
        {
            //创建整数列表
            int[] array = { 1, 2, 3 ,45,67,78,6,8,10};
            List<int> list = new List<int>();
            list.AddRange(array); //参数是一个集合

            //使用传统语法调用FindAll()
            //Predicate返
            Predicate<int> callback = new Predicate<int>(IsEvenNumber);//这个泛型委托需要一个返回值为bool型的比较函数作为参数，返回true则添加到callback里
            Console.WriteLine(callback);
            List<int> evenNumber = list.FindAll(callback);  //callback表示判断逻辑。
            Console.WriteLine($"evenNumber的类型是：{evenNumber}");
            Console.WriteLine("以下是偶数：");
            foreach (var item in evenNumber)
            {
                Console.WriteLine(item);
            }

            bool IsEvenNumber(int i)
            {
               return(i% 2 == 0) ;
            }
        }
        //匿名方法
        static void AnonymousShow()
        {
            int[] array = { 1, 2, 3, 45, 67, 78, 6, 8, 10 };
            List<int> list = new List<int>();
            list.AddRange(array); //参数是一个集合

            //使用匿名方法
            List<int> evenNumber = list.FindAll(delegate (int i) { return (i % 2 == 0); });
            foreach (var item in evenNumber)
            {
                Console.WriteLine(item);
            }
        }
        //Lambda方法
        static void LambdaShow()
        {
            int[] array = { 1, 2, 3, 45, 67, 6, 8, 10 };
            List<int> list = new List<int>();
            list.AddRange(array); //参数是一个集合

            //使用匿名方法
            List<int> evenNumber = list.FindAll(i => (i % 2) == 0);
            foreach (var item in evenNumber)
            {
                Console.WriteLine(item);
            }
        }
    }
}


```

#### 理解Lambda表达式
