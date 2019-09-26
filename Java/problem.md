#### java采用Unicode编码，可以将字符赋值汉字，同理，JavaScript、C#、php也一样。

#### 一图解java的程序运行：
![程序运行过程](https://github.com/swordboyASS/Java/blob/master/Picture/01.png)

#### 1.注意：枚举可以单独声明或者声明在类里面。方法、变量、构造函数也可以在枚举中定义。  
```java
  class FreshJuice {
    enum FreshJuiceSize{ SMALL, MEDIUM , LARGE }
    FreshJuiceSize size;
}

public class HelloWorld {
        public static void main(String[] args){
//            System.out.println("Heloworld");
            FreshJuice juice = new FreshJuice();
            juice.size = FreshJuice.FreshJuiceSize.MEDIUM;
            System.out.println(juice.size);

        }
}
```
#### 2.注释和C语言相同

#### 3.源文件声明规则：
* 一个源文件中只能有一个public类
* 一个源文件可以有多个非public类
* `源文件的名称应该和public类的类名保持一致`。例如：源文件中public类的类名是Employee，那么源文件应该命名为Employee.java。
* 如果一个类定义在某个包中，那么package语句应该在源文件的首行。
* 如果源文件包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面。
* import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。

#### Java包
包主要用来对类和接口进行分类。当开发Java程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。

#### Import语句
在Java中，如果给出一个完整的限定名，包括包名、类名，那么Java编译器就可以很容易地定位到源代码或者类。Import语句就是用来提供一个合理的路径，使得编译器可以找到某个类。   
例如，下面的命令行将会命令编译器载入java_installation/java/io路径下的所有类    
`import java.io.*;`

#### package和import
package 的作用就是 c++ 的 namespace 的作用，防止名字相同的类产生冲突。Java 编译器在编译时，直接根据 package 指定的信息直接将生成的 class 文件生成到对应目录下。如 package aaa.bbb.ccc 编译器就将该 .java 文件下的各个类生成到 ./aaa/bbb/ccc/ 这个目录。

import 是为了简化使用 package 之后的实例化的代码。假设 ./aaa/bbb/ccc/ 下的 A 类，假如没有 import，实例化A类为：new aaa.bbb.ccc.A()，使用 import aaa.bbb.ccc.A 后，就可以直接使用 new A() 了，也就是编译器匹配并扩展了 aaa.bbb.ccc. 这串字符串。


#### 布尔类型的定义
` boolean one = true;`


#### java 常量
常量在程序运行时是不能被修改的。在 Java 中使用 final 关键字来修饰常量，声明方式和变量类似：`final double PI = 3.1415927;`
虽然常量名也可以用小写，但为了便于识别，通常使用大写字母表示常量。



#### Java 局部变量
访问修饰符不能用于局部变量；(这一点C#相同)  
局部变量只在声明它的方法、构造方法或者语句块中可见；  
局部变量是在栈上分配的。  
局部变量`没有默认值`，所以局部变量被声明后，必须经过初始化，才可以使用。  

#### 实例变量
实例变量`具有默认值`。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；

#### 类变量（静态变量）
例如，对于下面的程序，无论创建多少个实例对象， 永远都只分配了一个 staticInt 变量(所有对象共享同一个静态变量)[与C#相同]，并且每创建一个实例对象，这个 staticInt 就会加 1；但是，每创建一个实例对象，就会分配一个 random， 即可能分配多个 random ，并且每个 random 的值都只自加了1次。
```java
public class StaticTest {
    private static int staticInt = 2;
    private int random = 2;

    public StaticTest() {
        staticInt++;
        random++;
        System.out.println("staticInt = "+staticInt+"  random = "+random);
    }

    public static void main(String[] args) {
        StaticTest test = new StaticTest();
        StaticTest test2 = new StaticTest();
    }
}
    //输出结果
    //staticInt = 3  random = 3
    //staticInt = 4  random = 3

  // DEPARTMENT是一个常量
  //public static final String DEPARTMENT = "开发人员";
```


#### 访问控制修饰符
Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

| | |
|:---|:---|
|default (即默认，什么也不写）: |在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。|
|private : 在同一类内可见。|使用对象：变量、方法。 注意：不能修饰类（外部类）|
|public : 对所有类可见。|使用对象：类、接口、变量、方法|
|protected : 对同一包内的类和所有子类可见。|使用对象：变量、方法。 注意：不能修饰类（外部类）|


#### final 修饰符
final 表示"最后的、最终的"含义，变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值。     
final 修饰符通常和 static 修饰符一起使用来创建类常量。

* final 方法
```java
父类中的 final 方法可以被子类继承，但是不能被之类重写。
声明 final 方法的主要目的是防止该方法的内容被修改。
如下所示，使用 final 修饰符声明方法。
public class Test{
    public final void changeName(){
       // 方法体
    }
}
```

* final 类
```
final 类不能被继承，没有类能够继承 final 类的任何特性。
实例
public final class Test {
   // 类体
}
```

#### abstract 修饰符
和C#一样

#### synchronized 修饰符
```java
synchronized 关键字声明的方法同一时间只能被一个线程访问。synchronized 修饰符可以应用于四个访问修饰符。
实例
public synchronized void showDetails(){
.......
}
```

#### transient 修饰符
```java
序列化的对象包含被 transient 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。

该修饰符包含在定义变量的语句中，用来预处理类和变量的数据类型。

实例
public transient int limit = 55;   // 不会持久化
public int b; // 持久化
```

#### volatile 修饰符
```java
volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。

一个 volatile 对象引用可能是 null。

实例
public class MyRunnable implements Runnable
{
    private volatile boolean active;
    public void run()
    {
        active = true;
        while (active) // 第一行
        {
            // 代码
        }
    }
    public void stop()
    {
        active = false; // 第二行
    }
}
通常情况下，在一个线程调用 run() 方法（在 Runnable 开启的线程），在另一个线程调用 stop() 方法。 如果 第一行 中缓冲区的 active 值被使用，那么在 第二行 的 active 值为 false 时循环不会停止。

但是以上代码中我们使用了 volatile 修饰 active，所以该循环会停止。
```

#### Java 增强 for 循环(和C#foreach（）方法相同)
Java5 引入了一种主要用于数组的增强型 for 循环。

Java 增强 for 循环语法格式如下:

for(声明语句 : 表达式)
{
   //代码句子
}

表达式：表达式是要访问的数组名，或者是返回值为数组的方法。

```java
public class Test {
   public static void main(String args[]){
      int [] numbers = {10, 20, 30, 40, 50};
 
      for(int x : numbers ){
         System.out.print( x );
         System.out.print(",");
      }
      System.out.print("\n");
      String [] names ={"James", "Larry", "Tom", "Lacy"};
      for( String name : names ) {
         System.out.print( name );
         System.out.print(",");
      }
   }
}
```
