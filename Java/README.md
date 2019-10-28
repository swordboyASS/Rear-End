##  :golf: Java学习，启航！

#### :yum:[小问题](./problem.md)
#### :file_folder:[系统类](https://github.com/swordboyASS/Rear-end-Learing/blob/master/Java/%E4%B8%80%E4%BA%9B%E7%B1%BB.md#title)
#### :file_folder:[Object-Oriented](https://github.com/swordboyASS/Rear-End/blob/master/Java/Object-Oriented.md)
#### :file_folder:[Class-reuse](https://github.com/swordboyASS/Rear-End/blob/master/Java/%E7%B1%BB%E7%9A%84%E9%87%8D%E7%94%A8.md)


???为什么没输出
```java
package gaoju3.demo;


public class Hi {
    String text=" ";

    public Hi(String s) {
        String text = s;
//        System.out.println(text);
    }

    public static void main(String[] args) {
        Hi wrapper = new Hi("Hello");
        System.out.println(wrapper.text.toLowerCase());
    }
}
```
