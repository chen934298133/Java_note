/*
<details>
<summary></summary>
```java

```
</details>
*/
## Java中static关键字主要作用有四：
 - Static成员变量
 - Static成员方法
 - Static代码块
 - Static内部类

## 一、 &#127800;成员变量

- Java类分为两种static修饰的静态变量、无static修饰的实例变量。
- 静态变量可以用于引用所有对象的公共属性如：公司名称，所在大学名称。
	- 静态属性被共享给所有对象，成为全局变量，使用同一块内存，每次实例化都需加载一次。
	- 实例变量实例化时**仅加载一次**，分配内存后每个对象保留其副本，单独存储，所以每个对象实例变量的改变不会影响其他对象。
	- 静态变量可使程序存储器高效(即它**节省内存**)。
	- 静态变量属于类，在内存中只有**一个复制**，只要静态变量被加载，这个静态变量就会被分配空间。
<details>
<summary>**实例变量**</summary>

```java
class Counter {
    int count = 0;	//实例变量

    Counter() {
        count++;
        System.out.println(count);
    }

    public static void main(String args[]) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();
    }
}

```
结果如下：
```java
1
1 //此1不是没变，而是获取新的内存，从0 -> 1
1
// 即类加载时不分配内存，在实例化对象时，每个对象的实例变量会得到一个副本，获取新的内存。
```

</details>
<details>
<summary>**静态变量**</summary>

```java
class Counter2 {
    static int count = 0;// will get memory only once and retain its value

    Counter2() {
        count++;
        System.out.println(count);
    }

    public static void main(String args[]) {

        Counter2 c1 = new Counter2();
        Counter2 c2 = new Counter2();
        Counter2 c3 = new Counter2();

    }
}
```
结果如下：
```java
1
2 
3
// 即类加载时就会分配内存，在每次实例化对象时，都要访问一下该静态变量的唯一分配的内存。
```

</details>


- **静态变量**有两种引用方式
	- 类.静态变量
	- 对象.静态变量（**实例变量**仅此方式）

<details>
<summary>代码</summary>

```java
public class Counter {
    static int count = 0;
    int temp = 100;

    Counter(){
        count++;
        System.out.println(count);
    }

    public static void main(String[] args){
        Counter c1 = new Counter();
//        System.out.println(count);
        System.out.println(Counter.count);
        System.out.println(c1.count);
        System.out.println(c1.temp);

    }
}

```
</details>

## 二、 &#127800;静态成员 
- #### 1.Java中提供了static方法和非static方法。
	- static方法是**类的方法**，**不需要创建对象**就可以被调用。
	- 而非static方法是对象的方法，只有对象被**创建出来后**才可以被使用。
	- 静态方法可以访问静态数据成员，并**可以更改**静态数据成员的值。


- #### 2. static方法中**不能使用this和super关键字**，**不能调用非static方法**，<font color="0000cc">**只能访问所属类的静态成员变量和成员方法**</font>。
	- 因为当static方法被调用时，这个类的对象可能还没被创建，即使已经被创建了，也无法确定调用哪个对象的方法。
	- 同理，static方法也不能访问非static类型的变量。


- #### 3. static一个很重要的用途就是实现单例设计模式。
	- 单利模式的特点是该类只能有一个实例，为了实现这一功能，必须隐藏类的构造函数，即把构造函数声明为private，并提供一个创建对象的方法.
	- 由于构造对象被声明为private，外界无法直接创建这个类型的对象，只能通过该类提供的方法来获取类的对象，要达到这样的目的只能把创建对象的方法声明为static


 <details>
 <summary>程序如下</summary>

 ```java
     class Singleton{
    	private static Singleton instance=null;
    	private Singleton(){}
    	public static Singleton getInstance(){
    		if(instance==null){
    			instance=new Singleton();
    		}
    		return instance;
    	}
    }
 ```
 </details>

- 为什么java main方法是静态的？
	- 这是因为**对象**不需要调用静态方法，如果它是非静态方法，jvm首先要创建对象，然后调用main()方法，这将导致额外的内存分配的问题。


## 三、 &#127800;代码块

- #### static代码块在类中是独立于成员变量和成员函数的代码块的。
	- **注意： 这些static代码块只会被执行一次**

## 四、 &#127800;内部类
- &#127808; 定义在一个类内部的类即为内部类
	- 内部类可以声明public、protected、private等访问限制，可以声明为abstract的供其他内部类或外部类继承与扩展。
	- 声明为static、final的，也可以实现特定的接口。
	- 外部类按常规的类访问方式使用内部类，唯一的差别是外部类可以访问内部类的所有方法与属性，包括私有方法与属性。
- #### 1. 外部类如何调用静态内部类中的属性和方法
	- 外部类可以通过创建静态内部类实例的方法来调用静态内部类的非静态属性和方法
	- 外部类可以直接通过“ 外部类.内部类.属性（方法）” 的方式直接调用静态内部类中的静态属性和方法
- #### 2. 静态内部类如何调用外部类的属性和方法
	- 静态内部类如果要访问**外部的成员变量或者成员方法**，那么必须是**静态的**
	- 静态内部类可以直接调用外部类的静态属性和方法
	- 静态内部类可以通过创建外部类实例的方法调用外部类的非静态属性和方法

- #### 3. 如何创建静态内部类实例
	- 创建静态内部类的时候是不需要将静态内部类的实例对象绑定到外部类的实例对象上

<details>
<summary>&#127879; 查看代码&#127879;</summary>

```java

public class Static_Class {
    // 一个实例变量一个静态变量
    // An instance variable ，A static variable
    private int a;
    private static int b{}

    //一个静态方法，一个非静态方法
    //A static method, a non-static method
    public static void say(){}

    public void test(){
        // 在外部类中调用内部类的属性和方法
        Static_Class.Inner.c = 1; // 可以通过静态内部类的全类名来调用静态内部类的静态属性（外部类名.静态内部类名.属性）
        Static_Class.Inner.go(); // 可以通过静态内部类的全类名来调用静态内部类的静态方法（外部类名.静态内部类名.方法）
        // Outer.Inner.walk(); //不能通过类静态内部类的全类名来调用内部类的非静态属性和方法
        Inner inner = new Inner(); //可以通过创建内部类实例来调用静态内部类的非静态属性和方法
        inner.d = 1;
        inner.walk();
    }

    //静态内部类
    //Static internal class
    public static class Inner{
        static int c;
        int d;

        // 一个匿名代码块、一个静态代码块
        //An anonymous block of code, a static block of code
        {}
        static{}

        //A static method, a non-static method
        public static void go(){}
        public void walk(){
            // 在静态内部类中调用外部类的属性和方法
            int f = b; // 可以直接调用外部类的静态属性
            say(); // 可以直接调用外部类的静态方法
            // int e = a; 直接调用外部类的非静态属性出错编译出错
            // test(); 直接调用外部类的非静态方法时编译出错
            Static_Class outer = new Static_Class();
            int e = outer.a; // 可以通过创建外部类实例来调用外部类的非静态属性
            outer.test(); // 可以通过创建外部类实例来调用外部类的非静态方法
        }
    }
}

```
</details>