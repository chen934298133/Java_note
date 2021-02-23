# &#127800; Java Reflection &#127800;
- Java反射就是在运行状态中，
  - **对于任意一个类，都能够知道这个类的所有属性和方法**；
  - **对于任意一个对象，都能够调用它的任意方法和属性；并且能改变它的属性。**
- 反射机制允许程序在运行时取得任何一个已知名称的`class`的内部信息，包括包括其`modifiers`(修饰符)，`fields`(属性)，`methods`(方法)等，并可于运行时改变 `fields` 内容或调用`methods`。
- 如此便可以更灵活的编写代码，代码可以在运行时装配，无需在组件之间进行源代码链接，降低代码的耦合度；还有动态代理的实现等等；但是需要注意的是反射使用不当会造成很高的资源消耗！

# &#127800; 1 Java反射机制提供的功能 &#127800;
- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法在运行时处理注解
- 生成动态代理（AOP）

# &#127800; 2 静态、动态语言与反射 &#127800;
##### Reflection(反射)是Java被视为动态语言的关键，反射机制允许程序在执行期借助于ReflectionAPI取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。 

**优点**：灵活使用反射能让代码更加灵活，比如: 
- JDBC原生代码注册驱动
- hibernate 的实体类
- Spring 的 AOP等等都有反射的实现。

**缺点**: 
- 反射也会消耗系统的性能，增加复杂性等，需要合理使用。
- 使用反射基本上是一种解释操作，可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于直接执行相同的操作。

## &#127800; 2.1 动态语言
- 一类在运行时可以改变其结构的语言:例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时，代码可以根据某些条件改变自身结构。
- 主要动态语言:Object-C、C#、JavaScript、PHP、Python等

## &#127800; 2.2 静态语言
- 与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C++、C。
- Java不是动态语言，但Java可以称之为“准动态语言 即Java有一定的动态性我们可以利用反射机制获得类似动态语言的特性。Java的动态性让编程的时候更加灵活!



# &#127800; 2.3 理解Class类并获取Class实例&#127800;

<details>
<summary>&#127808; Person &#127808;</summary>
  
```java
package Reflection;

public class Person {
    private String name = "Tom";
    public int age = 18;
    public Person(){

    }

    private void say(){
        System.out.println("private say()...");
    }

    private void work(){
        System.out.println("private work()...");
    }
}

```
</details>

<details>
<summary>&#127808; 得到 Class 的三种方式 &#127808;</summary>
  
```java
package Reflection;

public class Test {
    public static void main(String[] args) throws ClassNotFoundException {

        /**
         * 得到 Class 的三种方式
         */

        Person p1 = new Person();
        // 1、通过对象调用 getClass() 方法来获取,通常应用在：比如你传过来一个 Object 类型的对象，而我不知道你具体是什么类，用这种方法
        Class c1 = p1.getClass();
        // 2、直接通过 类名.class 的方式得到,该方法最为安全可靠，程序性能更高,这说明任何一个类都有一个隐含的静态成员变量 class
        Class c2 = Person.class;
        //3、通过 Class 对象的 forName() 静态方法来获取，用的最多，但可能抛出 ClassNotFoundException 异常
        Class c3 = Class.forName("Reflection.Person");

        System.out.println(c1.equals(c2));
        System.out.println(c1.equals(c3));
    }
}

```
</details>

<details>
<summary>&#127808; 通过 Class 类获取成员变量、成员方法、接口、超类、构造方法等 &#127808;</summary>
  
查阅 API 可以看到 Class 有很多方法：

1. getName()：获得类的完整名字。
2. getFields()：获得类的public类型的属性。
3. getDeclaredFields()：获得类的所有属性。包括private 声明的和继承类
4. getMethods()：获得类的public类型的方法。
5. getDeclaredMethods()：获得类的所有方法。包括private 声明的和继承类
6. getMethod(String name, Class[] parameterTypes)：获得类的特定方法，name参数指定方法的名字，parameterTypes 参数指定方法的参数类型。
7. getConstructors()：获得类的public类型的构造方法。
8. getConstructor(Class[] parameterTypes)：获得类的特定构造方法，parameterTypes 参数指定构造方法的参数类型。
9. newInstance()：通过类的不带参数的构造方法创建这个类的一个对象。
  
```java
package Reflection;

import java.lang.Class;
import java.lang.reflect.Method;        // 代表类的方法
import java.lang.reflect.Field;         // 代表类成员
import java.lang.reflect.Constructor;   // 代表类构造器

/**
 * 查阅 API 可以看到 Class 有很多方法：
 *
 * 　　getName()：获得类的完整名字。
 * 　　getFields()：获得类的public类型的属性。
 * 　　getDeclaredFields()：获得类的所有属性。包括private 声明的和继承类
 * 　　getMethods()：获得类的public类型的方法。
 * 　　getDeclaredMethods()：获得类的所有方法。包括private 声明的和继承类
 * 　　getMethod(String name, Class[] parameterTypes)：获得类的特定方法，name参数指定方法的名字，parameterTypes 参数指定方法的参数类型。
 * 　　getConstructors()：获得类的public类型的构造方法。
 * 　　getConstructor(Class[] parameterTypes)：获得类的特定构造方法，parameterTypes 参数指定构造方法的参数类型。
 * 　　newInstance()：通过类的不带参数的构造方法创建这个类的一个对象。
 */
public class MethodsTest {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException, InstantiationException {

        Person p1 = new Person();
        // 1、通过对象调用 getClass() 方法来获取,通常应用在：比如你传过来一个 Object 类型的对象，而我不知道你具体是什么类，用这种方法
        Class c1 = p1.getClass();
        // 2、直接通过 类名.class 的方式得到,该方法最为安全可靠，程序性能更高,这说明任何一个类都有一个隐含的静态成员变量 class
        Class c2 = Person.class;
        // 3、通过 Class 对象的 forName() 静态方法来获取，用的最多，但可能抛出 ClassNotFoundException 异常
        try {
            Class c3 = Class.forName("Reflection.Person");
            System.out.println(c3);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        // 1. 获得类完整的名字
        String className = c2.getName();
        System.out.println(className);

        // 2. 获得类的public类型的属性。 //public int age
        Field[] fields = c2.getFields();
        for (Field f : fields) {
            System.out.println(f.getName());
        }

        // 获得类的所有属性。包括私有的   //private String name, public int age
        Field [] allFields = c2.getDeclaredFields();
        for (Field f : allFields ) {
            System.out.println(f.getName());
        }

        // 获得类的public类型的方法。这里包括 Object 类的一些方法
        Method[] methods = c2.getMethods();
        for (Method m : methods){
            System.out.println(m.getName());
        }

        // 获得类的所有方法。
        Method[] allMethods = c2.getDeclaredMethods();
        for (Method m : allMethods){
            System.out.println(m.getName());
        }

        // 获得指定的属性
        Field f1 = c2.getField("age");
        System.out.println(f1.getName());

        // 获得指定的私有属性
        Field f2 = c2.getDeclaredField("name");
        System.out.println(f2.getName());

        // 启用和禁用访问安全检查的开关，值为 true，则表示反射的对象在使用时应该取消 java 语言的访问检查；反之不取消
        f2.setAccessible(true);
        System.out.println(f2);

        //创建这个类的一个对象
        Object p2 = c2.newInstance();
        //将 p2 对象的  f2 属性赋值为 Bob，f2 属性即为 私有属性 name
        f2.set(p2, "Bob");
        //使用反射机制可以打破封装性，导致了java对象的属性不安全。
        System.out.println(f2.get(p2)); // Bob
        //获取构造方法
        Constructor[] constructors = c2.getConstructors();
        for (Constructor constructor : constructors){
            System.out.println(constructor.toString());
        }

    }
}

```
</details>

<details>
<summary>&#127808; 根据反射获取父类属性 &#127808;</summary>
  
```java
package Reflection.Parent_Son;

import java.lang.reflect.Field;

/**
 *
 * 4、根据反射获取父类属性
 *
 * 通过执行上述代码，我们获得了父类的私有属性值，
 * 这里要注意的是直接通过反射获取子类的对象是不能得到父类的属性值的，
 * 必须根据反射获得的子类 Class 对象在调用  getSuperclass() 方法获取父类对象，
 * 然后在通过父类对象去获取父类的属性值。
 *
 */
public class Test {
    public static void main(String[] args){

    }

    public void testGetParentField() throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        Class c1 = Class.forName("Reflection.Parent_Son.Son");
        System.out.println(getFieldValue(c1.newInstance(),"privateField"));
    }

    public static Field getDeclaredField(Object obj, String fieldName){
        Field field = null;
        Class c = obj.getClass();
        for (;c != Object.class; c = c.getSuperclass()){
            try {
                field = c.getDeclaredField(fieldName);
                field.setAccessible(true);
                return field;
            } catch (NoSuchFieldException e) {
//                e.printStackTrace();
                //这里什么都不要做！并且这里的异常必须这样写，不能抛出去。
                //如果这里的异常打印或者往外抛，则就不会执行c = c.getSuperclass(),最后就不会进入到父类中了
            }
        }
        return null;
    }

    public static Object getFieldValue(Object object, String fieldName) throws IllegalAccessException {
        Field field = getDeclaredField(object, fieldName);
        return field.get(object);
    }
}
```
</details>

<details>
<summary>&#127808; View The Points &#127808;</summary>
  
```java
package Reflection;

import java.lang.annotation.ElementType;

public class AllTypeTest {
    public static void main(String[] args){
        System.out.println(Object.class);
        System.out.println(Comparable.class);
        System.out.println(String[].class);
        System.out.println(int[][].class);
        System.out.println(Override.class);
        System.out.println(ElementType.class);
        System.out.println(Integer.class);
        System.out.println(void.class);
        System.out.println(Class.class);
    }
}

```
```
class java.lang.Object
interface java.lang.Comparable
class [Ljava.lang.String;
class [[I
interface java.lang.Override
class java.lang.annotation.ElementType
class java.lang.Integer
void
class java.lang.Class
```
</details>

# &#127800; 3 类的加载 与 ClassLoader &#127800;

![](http://lc-dDwI9S44.cn-n1.lcfile.com/6f381c88c195f747f4fb.png/%E6%A0%88%E5%A0%86%E6%96%B9%E6%B3%95%E5%8C%BA1.png)

![](http://lc-dDwI9S44.cn-n1.lcfile.com/e3ec9643f148dc018498.png/%E6%A0%88%E5%A0%86%E6%96%B9%E6%B3%95%E5%8C%BA2.png)


## &#127800; 3.1 类的加载 与 ClassLoader的理解 

- **加载:** 将 `class` 文件字节码内容加载到**内存**中，并将这些**静态数据**转换成**方法区的运行时数据结构**然后生成一个代表这个类的 `java.lang.Class` 对象
- **链接:** 将Java类的**二进制代码**合并到 **JVM 的运行状态之中**的过程。
  - 验证: 确保加载的类信息符合 JVM 规范，没有安全方面的问题
  - 准备: 正式为类变量(static)**分配内存**并**设置类变量默认初始值**的阶段，这些内存都将在方法区中进行分配。解析:虚拟机常量池内的符号引用(常量名)替换为直接引用(地址)的的过程。
- **初始化:**
  - 执行类构造器 `<clinit>()` 方法的过程。类构造器 `<clinit>()` 方法是由**编译期自动收集类中所有类变量的赋值动作** 和 **静态代码块中的语句**合并产生的。(类构造器是构造类信息的，不是构造该类对象的构造器)。
  - 当初始化一个类的时候，如果发现其父类**还没有**进行初始化，则需要**先触发其父类的初始化**。
  - 虚拟机会保证一个类的 `<clinit>()`方法在多线程环境中被正确加锁和同步
  
<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

public class Test02 {
    public static void main(String[] args){
        A a = new A();
        System.out.println(A.m);
    }
}

class A{
    static {
        System.out.println("A类静态代码块初始化");
        m = 300;
    }

    static int m = 100;

    public A(){
        System.out.println("A类的无参构造初始化");
    }
}
```
</details>

## &#127800; 3.2 什么时候会发生类初始化
- 类的主动引用(一定会发生类的初始化)
  - 当虚拟机启动，先初始化 main 方法所在的类
  - new 一个类的对象
  - 调用类的静态成员(除了final常量)和静态方法
  - 使用`java.lang.reflect`包的方法对类进行反射调用
  - 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类

- 类的被动引用(不会发生类的初始化)
  - 当访问一个静态域时，只有真正声明这个域的类才会被初始化。如:当通过子类引用父类的静态变量，不会导致子类初始化
  - 通过数组定义类引用，不会触发此类的初始化
  - 引用常量不会触发此类的初始化(常量在链接阶段就存入调用类的常量池中了)

<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

public class Test03 {
    static {
        System.out.println("Main类被加载");
    }

    public static void main(String[] args) throws ClassNotFoundException {
        // 主动引用,会产生类引用
//        Son son = new Son();
        // 反射也会产生主动引用
//        Class.forName("Reflection.Son");
        // 不会产生类引用的方法
//        System.out.println(Son.b);

        System.out.println(Son.M);
    }
}

class Father {
    static int b = 2;

    static {
        System.out.println("父类被加载");
    }
}

class Son extends Father {
    static {
        System.out.println("子类被加载");
        m = 300;
    }
    static int m = 100;
    static final int M = 1;
}

```
</details>
  
## &#127800; 3.3 类加载器的作用
- **类加载的作用:** 将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后在堆中生成一个代表这个类的 `java.lang.Class` 对象，作为方法区中类数据的访问入口。

- **类缓存:** 标准的 JavaSE 类加载器可以按要求查找类，但一旦某个类被加载到类加载器中，它将维持加载(缓存)一段时间。不过 JVM 垃圾回收机制可以回收这些 Class 对象
  
![](http://lc-dDwI9S44.cn-n1.lcfile.com/84c5b116870c866afadc.png/%E7%B1%BB%E5%8A%A0%E8%BD%BD1.png)

![](http://lc-dDwI9S44.cn-n1.lcfile.com/4bf6b047a5f226d11cd7.png/%E4%B8%89%E7%A7%8D%E7%B1%BB%E5%8A%A0%E8%BD%BD.png)

  <details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

public class Test04 {
    public static void main(String[] args) throws ClassNotFoundException {
        // 获取系统类的加载器_systemClassLoader
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);

        // 获取系统类加载器的父类加载器 --> 扩展类加载器:_ExtClassLoader
        ClassLoader parent = systemClassLoader.getParent();
        System.out.println(parent);

        // 获取扩展类加载器的父类加载器 --> 根加载器_BootStarpClassLoader(用C++编写的，无法直接获取)
        ClassLoader parent1 = parent.getParent();
        System.out.println(parent1);

        // 测试当前类是哪个加载器加载的
        ClassLoader classLoader = Class.forName("Reflection.Test04").getClassLoader();
        System.out.println(classLoader);

        // 测试JDK内置的类是谁加载的
        classLoader = Class.forName("java.lang.Object").getClassLoader();
        System.out.println(classLoader);

        // 如何获得系统类加载器可以加载的路径
        System.out.println(System.getProperty("java.class.path"));
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\charsets.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\deploy.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\access-bridge-64.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\cldrdata.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\dnsns.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\jaccess.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\jfxrt.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\localedata.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\nashorn.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\sunec.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\sunjce_provider.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\sunmscapi.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\sunpkcs11.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\ext\zipfs.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\javaws.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\jce.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\jfr.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\jfxswt.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\jsse.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\management-agent.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\plugin.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\resources.jar;
        // C:\Program Files\Java\jdk1.8.0_91\jre\lib\rt.jar;
        // G:\DS_Test\out\production\DS_Test;
        // G:\DS_Test\src\Multithreading\Thread_1\lib\commons-io-1.4.jar;
        // G:\idea pre\IntelliJ IDEA Community Edition 2020.2.1\lib\idea_rt.jar
    }
}
 
```
</details>
  
# &#127800; 4 创建运行时类的对象 &#127800;

## &#127800; 4.1 获取运行时类的完整结构
  
<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

// 获得类的信息
public class Test05 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        Class c1 = Class.forName("Reflection.Person");

        // 获取类的名字
        System.out.println(c1.getName());       // 获得包名 + 类名
        System.out.println(c1.getSimpleName()); // 获得类名

        // 获得类的属性
        System.out.println("------------获得类的属性------------");
        Field [] fields = c1.getFields();       // 只能找到public属性

        fields = c1.getDeclaredFields();      // 可以找到全部的属性
        for (Field field : fields){
            System.out.println(field);
        }

        System.out.println("------------获得指定属性的值------------");
        // 获得指定属性的值
        Field name = c1.getDeclaredField("name");
        System.out.println(name);

        System.out.println("------------获取类的方法------------");
        // 获取类的方法
        Method[] methods = c1.getMethods();
        for (Method method : methods){
            System.out.println("getMethods: " + method);
        }

        methods = c1.getDeclaredMethods();
        for (Method method : methods){
            System.out.println("getDeclaredMethods: " + method);
        }

        System.out.println("------------获得指定方法------------");
        // 获得指定方法
        Method getName = c1.getMethod("getName",null);
        System.out.println(getName);
        Method setName = c1.getMethod("setName", String.class);
        System.out.println(setName);

        System.out.println("------------获取指定的构造器------------");
        // 获取指定的构造器
        Constructor[] constructors = c1.getConstructors();
        for (Constructor constructor : constructors){
            System.out.println(constructor);
        }
        constructors = c1.getDeclaredConstructors();
        for (Constructor constructor : constructors){
            System.out.println(constructor);
        }

        Constructor constructorDeclared = c1.getDeclaredConstructor(String.class, int.class, int.class);
        System.out.println("指定: " + constructorDeclared);
    }
}

```
</details>
  
## &#127800; 4.2 调用运行时类的指定结构

### 4.2.1 有了Class对象，能做什么
- 创建类的对象:调用Class对象的newInstance()方法 法
  1. 类必须有一个无参数的构造器。
  2. 类的构造器的访问权限需要足够
### 4.2.1 思考: 难道没有无参的构造器就不能创建对象了吗?
  - 只要在操作的时候明确的调用类中的构造器并将参数传递进去之后，才可以实例化操作

###### 步骤如下:
1. 通过 `Class` 类的 `getDeclaredConstructor(Class...parameterTypes)` 取得本类的指定形参类型的构造器
2. 向构造器的形参中传递一个对象数组进去，里面包含了构造器中所需的各个参数
3. 通过 `Constructor` 实例化对象

<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

// 动态地创建对象，通过反射
public class Test06 {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        Class c1 = Class.forName("Reflection.Person");

        // 构造一个对象
        Person p = (Person) c1.newInstance(); // 本质上调用无参构造器
        System.out.println(p);


        // 通过构造器创建对象
        Constructor declaredConstructor = c1.getDeclaredConstructor(String.class, int.class, int.class);
        Person person = (Person) declaredConstructor.newInstance("chen", 22, 180);
        System.out.println(person);

        // 通过反射调用普通方法
        Person person1 = (Person) c1.newInstance();
        // 通过反射获取一个方法
        Method setName = c1.getMethod("setName", String.class);
        // invoke : 激活
        setName.invoke(person1,"che");
        System.out.println(person1.getName());

        // 通过反射操作属性
        Person person2 = (Person) c1.newInstance();
        Field name = c1.getDeclaredField("name");
        // 由于 name 是 private 的，需要 SetAccessible 关闭安全检测
        name.setAccessible(true);
        name.set(person2,"c");
        System.out.println(person2.getName());
    }
}

```
</details>
  
### &#127800; 4.2.3 setAccessible

- Method和Field、Constructor对象都有setAccessible()方法
- setAccessible作用是启动和禁用访问安全检查的开关。
- 参数值为true则指示反射的对象在使用时应该取消Java语言访问检查。
  - 提高反射的效率。如果代码中必须用反射，而该句代码需要频繁的被调用，那么请设置为true
  - 使得原本无法访问的私有成员也可以访问
- 参数值为false则指示反射的对象应该实施Java语言访问检查
  
  
## &#127800; 4.2.4 性能测试
  
<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```
package Reflection;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

// 分析性能问题
public class Test10 {


    // 普通方法调用
    public static void test01(){
        Person person = new Person();
        long startTime = System.currentTimeMillis();

        for (int i = 0; i < 1000000; i++) {
            person.getName();
        }

        long endTime = System.currentTimeMillis();

        System.out.println("普通方法执行100万次的时间: " + (endTime - startTime) + "ms");
    }

    // 反射方式调用
    public static void test02() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        Person person = new Person();
        Class c2 = person.getClass();
        Method getName = c2.getDeclaredMethod("getName",null);
        long startTime = System.currentTimeMillis();

        for (int i = 0; i < 1000000; i++) {
            getName.invoke(person,null);
        }

        long endTime = System.currentTimeMillis();

        System.out.println("反射方式调用执行数次的时间: " + (endTime - startTime) + "ms");
    }

    // 反射方式调用，关闭检测
    public static void test03() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        Person person = new Person();
        Class c2 = person.getClass();
        Method getName = c2.getDeclaredMethod("getName",null);
        getName.setAccessible(true);
        long startTime = System.currentTimeMillis();

        for (int i = 0; i < 1000000; i++) {
            getName.invoke(person,null);
        }

        long endTime = System.currentTimeMillis();

        System.out.println("反射方式调用，关闭检测执行数次的时间: " + (endTime - startTime) + "ms");
    }

    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException {
        test01();
        test02();
        test03();
    }
}

```
</details>
  
## &#127800; 5 反射操作泛型 &#127800; 
- Java采用泛型擦除的机制来引入泛型，Java中的泛型仅仅是给编译器javac使用的确保数据的安全性和免去强制类型转换问题，但是，一旦编译完成，所有和泛型有关的类型全部擦除
为了通过反射操作这些类型 Java 新增了 `ParameterizedType`，`GenericArrayType`
- `TypeVariable` 和 `WildcardType` 几种类型来代表不能被归一到 Class 类中的类型但是又和原始类型齐名的类型
- `ParameterizedType`: 表示一种参数化类型,比如 `Collection<String>`
- `GenericArrayType`:表示一种元素类型是参数化类型或者类型变量的数组类型 `TypeVariable`:是各种类型变量的公共父接口
- `WildcardType`:代表一种通配符类型表达式

<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

import java.lang.reflect.Method;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.util.Arrays;
import java.util.List;
import java.util.Map;

// 通过反射获取泛型
public class Test08 {
    public void test01(Map<String, Person> map, List<Person> list){
        System.out.println("test01");
    }

    public Map<String, Person> test02(){
        System.out.println("test02");
        return null;
    }

    public static void main(String[] args) throws NoSuchMethodException {

        
        Method method = Test08.class.getMethod("test01", Map.class, List.class);

        // 1. getGenericParameterTypes 获得泛型的参数类型
        Type[] genericParameterTypes = method.getGenericParameterTypes();

//        System.out.println(Arrays.toString(genericParameterTypes));

        for (Type genericParameterType : genericParameterTypes){
            System.out.println("# " + genericParameterType);
            // 判断是否是参数化类型
            if (genericParameterType instanceof ParameterizedType){
                // getActualTypeArguments 获得真实类型
                // 若是参数化类型，则先强转为参数化类型，以便使用 getActualTypeArguments 获得真实类型
                Type[] actualTypeArguments = ((ParameterizedType) genericParameterType).getActualTypeArguments();
                for (Type actualTypeArgument: actualTypeArguments){
                    System.out.println(actualTypeArgument);
                }
            }
        }

        System.out.println("----------------------------------------------------");

        method = Test08.class.getMethod("test02", null);
        // 2. getReturnType 获取返回值类型
        Type getGenericReturnType = method.getGenericReturnType();
        // 判断是否是参数化类型
        if (getGenericReturnType instanceof ParameterizedType){
            // getActualTypeArguments 获得真实类型
            Type[] actualTypeArguments = ((ParameterizedType) getGenericReturnType).getActualTypeArguments();
            for (Type actualTypeArgument: actualTypeArguments){
                System.out.println(actualTypeArgument);
            }
        }


    }
}

```
</details>
  
  
## &#127800; 利用注解和反射完成类和表结构的映射关系&#127800;
  
- getAnnotaions
- getAnnotaion

![](http://lc-dDwI9S44.cn-n1.lcfile.com/59acd558be0a8fe782c0.png/%E6%B3%A8%E8%A7%A3%E5%8F%8D%E5%B0%84%E6%98%A0%E5%B0%84.png)

<details>
<summary>&#127808; Check the details &#127808;</summary>
  
```java
package Reflection;

import java.lang.annotation.*;
import java.lang.reflect.Field;

// 练习反射操作注解
public class Test09 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class c1 = Class.forName("Reflection.Student1");

        System.out.println(c1.getAnnotations().length);
        // 通过反射获得注解
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }

        // 获取注解的value值
        TableChen tableChen = (TableChen) c1.getAnnotation(TableChen.class);
        String value = tableChen.value();
        System.out.println(value);

        // 获取类指定的注解
        Field f = c1.getDeclaredField("name");
        FieldChen annotation = f.getAnnotation(FieldChen.class);
        System.out.println(annotation.columnName());
        System.out.println(annotation.length());
        System.out.println(annotation.type());
    }
}

@TableChen("db_student")
class Student1 {

    @FieldChen(columnName = "db_name", type = "varchar", length = 3)
    private String name;
    @FieldChen(columnName = "db_id", type = "int", length = 10)
    private int id;
    @FieldChen(columnName = "db_age", type = "int", length = 10)
    private int age;

    public Student1(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public Student1() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

// 类名的注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface TableChen{
    String value();
}

// 属性的注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
@interface FieldChen{
    String columnName();
    String type();
    int length();
}
```
  </details>