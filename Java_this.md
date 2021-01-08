## this.属性名

普通方法访问其他方法时，有**局部变量**和**成员变量同名**，但程序又需要在该方法里访问这个**被覆盖的成员变量**，则须使用this。

```
//如常用的构造方法
public class Teacher {
    private String name;    // 教师名称
    private double salary;    // 工资
    private int age;    // 年龄
}

// 创建构造方法，为上面的3个属性赋初始值
// this后的变量为当前类的成员变量，非此方法的局部变量
public Teacher(String name,double salary,int age) {
    this.name = name;    // 设置教师名称
    this.salary = salary;    // 设置教师工资
    this.age = age;    // 设置教师年龄
}

```

## this.方法

让类中**一个方法**，访问该类里的**另一个方法或实例变量**。


```
public class Dog {
    public void jump() {
        System.out.println("正在执行jump方法");
    }
    
    public void run() {
        Dog d = new Dog();
        d.jump();
        System.out.println("正在执行 run 方法");
    }
}

public void run() {
    // 使用this引用调用run()方法的对象
    this.jump();	//this可省略
    System.out.println("正在执行run方法");
}

```

## this() 访问构造方法

this( ) 用来访问**本类的构造方法**

```
public class This_Test {
    private int age;
    private String name;
    
    public This_Test(int age, String name){
        this.age = age;
        this.name = name;
    }
   
    public This_Test(){
        this(1,"asd");
    }
}

```


[参考](http://c.biancheng.net/view/953.html)