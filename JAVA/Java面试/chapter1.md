# Java 基础
## 1.JDK 和 JRE 有什么区别？
---
JDK：java development kit（java开发工具）
1. 主要面向开发人员。开发人员在软件开发时使用的SDK（Software Development Kit 一般指软件开发包），它提供了Java的开发环境和运行环境。
2. 如果你电脑安装了JDK，那么你不仅可以开发Java程序，也同时拥有了运行Java程序的平台。
3. 是整个Java开发的核心，包括了Java运行环境，Java工具和Java基础类库。
JRE：java runtime environment（java运行时环境）
1. 主要面向程序使用者。
2. 如果你电脑安装了JRE，那么你的电脑只能运行Java程序，不能从事Java开发。
3. 包含JVM标准实现及Java核心类库。它包括Java虚拟机、Java平台核心类和支持文件。它不包含开发工具(编译器、调试器等)。同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：console，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。
---
## 2.== 和 equals 的区别是什么？
---
 + ==：比较的是两个字符串内存地址（堆内存）的数值是否相等，属于数值比较；
 + equals()：比较的是两个字符串的内容，属于内容比较
---
## 3.两个对象的 hashCode()相同，则 equals()也一定为 true，对吗？
---
1. hashCode()返回该对象的哈希码值；equals()返回两个对象是否相等。
2. HashCode 用于在散列的存储结构中确定对象的存储地址。
3. 如果两个对象equals()相等，那么两个对象的hashCode()方法返回的结果也必然相等。
4. 如果两个对象的 hashCode()相同，则 equals()却不一定相等。
5. 如果重写equals()方法，必须重写hashCode()方法，以保证equals方法相等时两个对象hashcode返回相同的值。（API上有标注：请注意，通常需要在重写此方法时覆盖hashCode方法，以便维护hashCode方法的常规协定，该方法声明相等的对象必须具有相等的哈希代码。）
---
## 4.final 在 java 中有什么作用？
---
final 语义是不可改变的。
+ 被 final 修饰的类，不能够被继承。
+ 被 final 修饰的成员变量必须要初始化，赋初值后不能再重新赋值(可以调用对象方法修改属性值)。对基本类型来说是其值不可变；对引用变量来说其引用不可变，即不能再指向其他的对象。
+ 被 final 修饰的方法代表不能重写。
---
## 5.java 中的 Math.round(-1.5) 等于多少？
---
Math.round(-1.5)的返回值是-1。 四舍五入的原理是在参数上加0.5然后做向下取整。

---
## 6.String 属于基础的数据类型吗？
---
Sring不是基本数据类型，而是一个类（class），是Java编程语言中的字符串。String对象是char的有序集合，并且该值是不可变的。因为java.lang.String类是final类型的，因此不可以继承这个类、不能修改这个类。为了提高效率节省空间，我们应该用StringBuffer类。
```
Java的8大基本数据类型分别是：

整数类 byte, short, int, long

浮点类 double, float

逻辑类 boolean

文本类 char
```
---
## 7.java 中操作字符串都有哪些类？它们之间有什么区别？
---
String、StringBuffer、StringBuilder
+ String : final修饰，String类的方法都是返回new String。即对String对象的任何改变都不影响到原对象，对字符串的修改操作都会生成新的对象。
+ StringBuffer : 对字符串的操作的方法都加了synchronized，保证线程安全。
+ StringBuilder : 不保证线程安全，在方法体内需要进行字符串的修改操作，可以new StringBuilder对象，调用StringBuilder对象的append、replace、delete等方法修改字符串。
---
## 8.String str="i"与 String str=new String(“i”)一样吗？
---
+ String s=“i”;这句话的意思是把“i”这个值在内存中的地址赋给s,下面的String s2=“i”；这句话的操作也是把“i”这个值在内存中的地址赋给s2,这两个引用的是同一个地址值，他们两个共享同一个内存。那么他们两个用==比较，结果当然是true。
+ String s1 = new String(“i”);String类型重写了 equals()方法，判断值是否相等，明显相等，因此 equals 是相等的，但是用==比较的结果就是fasle
---
## 9.如何将字符串反转？
---
```
public static String reverse2(String s) {
    int length = s.length();
    String reverse = "";
    for (int i = 0; i < length; i++){
        reverse = s.charAt(i) + reverse;
    }
    return reverse;
}
public static String reverse4(String s) {
    return new StringBuffer(s).reverse().toString();
}
```
---
## 10.String 类的常用方法都有那些？
---
+ indexOf()：返回指定字符的索引。
+ charAt()：返回指定索引处的字符。
+ replace()：字符串替换。
+ trim()：去除字符串两端空白。
+ split()：分割字符串，返回一个分割后的字符串数组。
+ getBytes()：返回字符串的 byte 类型数组。
+ length()：返回字符串长度。
+ toLowerCase()：将字符串转成小写字母。
+ toUpperCase()：将字符串转成大写字符。
+ substring()：截取字符串。
+ equals()：字符串比较。

---
## 11.抽象类必须要有抽象方法吗？
---
不需要，抽象类不一定非要有抽象方法。

---
## 12.普通类和抽象类有哪些区别？
---
+ 普通类不能包含抽象方法，抽象类可以包含抽象方法。
+ 抽象类不能直接实例化，普通类可以直接实例化。
---
## 13.抽象类能使用 final 修饰吗？
---
不能，定义抽象类就是让其他类继承的，如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾，所以 final 不能修饰抽象类

---
## 14.接口和抽象类有什么区别？
---
+ 实现：抽象类的子类使用 extends 来继承；接口必须使用 implements 来实现接口。
+ 构造函数：抽象类可以有构造函数；接口不能有。
+ main 方法：抽象类可以有 main 方法，并且我们能运行它；接口不能有 main 方法。
+ 实现数量：类可以实现很多个接口；但是只能继承一个抽象类。
+ 访问修饰符：接口中的方法默认使用 public 修饰；抽象类中的方法可以是任意访问修饰符
---
## 15.java 中 IO 流分为几种？
---
按功能来分：输入流（input）、输出流（output）。

按类型来分：字节流和字符流。

字节流和字符流的区别是：字节流按 8 位传输以字节为单位输入输出数据，字符流按 16 位传输以字符为单位输入输出数据。

---
## 16.BIO、NIO、AIO 有什么区别？
---
+ BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
+ NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
+ AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。
---
## 17.Files的常用方法都有哪些？
+ Files.exists()：检测文件路径是否存在。
+ Files.createFile()：创建文件。
+ Files.createDirectory()：创建文件夹。
+ Files.delete()：删除一个文件或目录。
+ Files.copy()：复制文件。
+ Files.move()：移动文件。
+ Files.size()：查看文件个数。
+ Files.read()：读取文件。
+ Files.write()：写入文件
---