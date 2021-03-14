# &#127800; JavaOOP 面试题 &#127800;

#### &#127800; 1、 Java 有哪些开发平台

<details>
<summary> &#127809; Answer &#127809; </summary>
  
1. JAVA SE：主要用在客户端开发
2. JAVA EE：主要用在web应用程序开发
3. JAVA ME：主要用在嵌入式应用程序开发
</details>

#### &#127800; 2、什么是JDK？什么是JRE?

<details>
<summary> &#127809; Answer &#127809; </summary>
  
1. JDK：java development kit：java开发工具包，是开发人员所需要安装的环境
2. JRE：java runtime environment：java运行环境，java程序运行所需要安装的环境
</details>

#### &#127800; 3、面向对象和面向过程的区别

<details>
<summary> &#127809; Answer &#127809; </summary>
  
1. 面向过程：
一种较早的编程思想，顾名思义就是该思想是**站着过程的角度思考问题**，**强调的就是功能行为**，功能的执行过程，即先后顺序，而每一个功能我们都使用函数（类似于方法）把这些步骤一步一步实现。使用的时候依次调用函数就可以了。
  
2. 面向对象： 
一种基于面向过程的新编程思想，顾名思义就是该思想是**站在对象的角度思考问题**，我们把多个功能合理放到不同对象里，强调的是具备某些功能的对象。
具备某种功能的实体，称为对象。面向对象最小的程序单元是：类。面向对象更加符合常规的思维方式，稳定性好，可重用性强，易于开发大型软件产品，有良好的可维护性。
在软件工程上，面向对象可以使工程更加模块化，实现更低的耦合和更高的内聚。
</details>

#### &#127800; 4、什么是OOP?

<details>
<summary> &#127809; Answer &#127809; </summary>
  
面向对象编程
</details>

#### &#127800; 5、Java中有几种数据类型

<details>
<summary> &#127809; Answer &#127809; </summary>
  
  整形：byte,short,int,long <br>
  浮点型：float,double <br>
  字符型：char 布尔型：boolean <br>
</details>

#### &#127800; 6、Char类型能不能转成int类型？能不能转化成string类型，能不能转成double类型

<details>
<summary> &#127809; Answer &#127809; </summary>
  

Char在java中也是比较特殊的类型，它的int值从1开始，一共有2的16次方个数据；<br>
```
Char < int < long < float < double
```
Char类型可以隐式转成int,double类型，但是不能隐式转换成string；<br>
如果char类型转成byte，short类型的时候，需要强转。<br>
</details>

#### &#127800; 7、什么是拆装箱？

<details>
<summary> &#127809; Answer &#127809; </summary>

1. 装箱就是自动将基本数据类型转换为包装器类型（int-->Integer）；调用方法：Integer的valueOf(int) 方法
2. 拆箱就是自动将包装器类型转换为基本数据类型（Integer-->int）。调用方法：Integer的intValue方法
  
在Java SE5之前，如果要生成一个数值为10的Integer对象，必须这样进行：
```java
Integer i = new Integer(10); 
```
而在从Java SE5开始就提供了自动装箱的特性，如果要生成一个数值为10的Integer对象，只需要
这样就可以了：
```java
  Integer i = 10;
```
  
面试题1： 以下代码会输出什么？
```java
  
```
boolean result = obj instanceof Class inti=0; 
  System.out.println(i instanceof Integer);//编译不通过i必须是引用类型，不能是基本类型 
  System.out.println(i instanceof Object);//编译不通过 
  Integer integer=newInteger(1); 
  System.out.println(integer instanceof Integer);
  //true 
  //false,在JavaSE规范中对instanceof运算符的规定就是：如果obj为null，那么将返回false。 System.out.println(nullinstanceofObject); 


  public class Main { public static void main(String[] args) { Integer i1 = 100; Integer i2 = 100; Integer i3 = 200; Integer i4 = 200; System.out.println(i1==i2); System.out.println(i3==i4); Java研发军团整理 https://www.ycbbs.vip
结果：
</details>

#### &#127800; 8、Java中的包装类都是那些？

<details>
<summary> &#127809; Answer &#127809; </summary>
  
  byte：Byte，<br>
  short：Short，<br>
  int：Integer，<br>
  long：Long，<br>
  float：Float，<br>
  double：Double，<br>
  char：Character ，<br>
  boolean：Boolean<br>
</details>

#### &#127800; 9、面向对象的特征有哪些方面?


<details>
<summary> &#127809; Answer &#127809; </summary>
  
抽象:
抽象是将一类对象的共同特征总结出来构造类的过程, 包括数据抽象和行为抽象两方面。
抽象只关注对象有哪些属性和行为,并不关注这些行为的细节是什么。
  
继承:
继承是从已有类得到继承信息创建新类的过程.提供继承信息的类被称为父类(超类、基类) ;得到继承信息的类被称为子类(派生类)。继承让变化中的软件系统有了一定的延续性 ,同时继承也是封装程序中可变因素的重要手段(如果不能理解请阅读阎宏博土的《Java 与模式》或《设计模式精解》中.关于桥梁模式的部分)。
  
封装：

通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口（可以想想普通洗衣机和全自动洗衣机的差别，明显全自动洗衣机封装更好因此操作起来更简单；我们现在使用的智能手机也是封装得足够好的，因为几个按键就搞定了所有的事情）。
  
多态性：
多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。如果将对象的方法视为对象向外界提供的服务，那么运行时的多态性可以解释为：当 A 系统访问 B 系统提供的服务时，B系统有多种提供服务的方式，但一切对 A 系统来说都是透明的（就像电动剃须刀是 A 系统，它的供电系统是 B 系统，B 系统可以使用电池供电或者用交流电，甚至还有可能是太阳能，A 系统只会通过 B 类对象调用供电的方法，但并不知道供电系统的底层实现是什么，究竟通过何种方式获得了动力）。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：1). 方法重写（子类继承父类并重写父类中已有的或抽象的方法）；2). 对象造型（用父类型引用引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。
</details>


#### &#127800; 10、重载和重写的区别
equals与==的区别
形参与实参区别
String StringBuffffer 和 StringBuilder 的区别是什么？
String类的常用方法有那些？
1. 返回信息
```
1. 返回信息
charAt：返回指定索引处的字符 
indexOf()：返回指定字符的索引 
length()：返回字符串长度
substring()：截取字符串

2. 变换信息
replace()：字符串替换 
trim()：去除字符串两端空白 
split()：分割字符串，返回一个分割后的字符串数组 
getBytes()：返回字符串的byte类型数组 
toLowerCase()：将字符串转成小写字母 
toUpperCase()：将字符串转成大写字符 
equals()：字符串比较
```
Super与this表示什么？
什么是接口？为什么需要接口？
接口有什么特点？
抽象类和接口的区别?
Java创建对象有几种方式？
什么时候用assert断言
数组有没有length()这个方法? String有没有length()这个方法
在 Java 中，如何跳出当前的多重嵌套循环？
