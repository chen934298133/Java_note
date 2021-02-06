## &#127800; 再次认识Lambda &#127800;
#### 注意
- 接口必须时函数式接口，即只需要实现一个方法
<br>

<details>
<summary>&#127808; Lambda未出现时 &#127808;</summary>
  
```java
package DS.lambda;

public class NoLambda {
    // 优化版本1： 静态内部类
    static class ILambda_1 implements ILambda{
        @Override
        public void lambda() {
            System.out.println("I am Lambda1!");
        }
    }
    public static void main(String[] args) {
        // 原始繁琐版本1
        ILambda iLambda = new ILambda_0();
        iLambda.lambda();

        // 优化版本1：静态内部类
        iLambda = new ILambda_1();
        iLambda.lambda();

        // 优化版本2：局部内部类
        class ILambda_2 implements ILambda{
            @Override
            public void lambda() {
                System.out.println("I am Lambda2!");
            }
        }
        iLambda = new ILambda_2();
        iLambda.lambda();

        // 优化版本3： 匿名内部类
        iLambda = new ILambda() {
            @Override
            public void lambda() {
                System.out.println("I am lambda3!");
            }
        };
        iLambda.lambda();

        // 优化版本4：Lambda
        iLambda = () -> {
            System.out.println("I am lambda4!");
        };
        iLambda.lambda();
    }
}

interface ILambda {
    void lambda();
}

class ILambda_0 implements ILambda {
    @Override
    public void lambda() {
        System.out.println("I am Lambda0!");
    }
}

```
</details>

<details>
<summary>&#127808; Lambda使用 &#127808;</summary>
  
```java
package DS.lambda;

/**
 * lambda使用注意点
 * 接口必须时函数式接口，即只需要实现一个方法
 */
public class MyLambda {
    public static void main(String[] args){
        Lambda lambda ;

        // lambda
        lambda = (int num) -> System.out.println("Lambda1: " + num);
        lambda.lambda(1);

        // 简化： 参数只有一个可省略括号
        lambda = num -> System.out.println("Lambda2: " + num);
        lambda.lambda(2);
    }
}

interface Lambda{
    void lambda(int a);
//    void lambda1();
}
```
</details>