# 线性表——顺序表
## 分类
- 顺序存储 —— 顺序表(底层为数组)
- 链式存储 —— 链表(底层)
## 1.1 &#127800;顺序表实现
顺序表API设计：
- 重点在如何实现插入操作
<table>
  <tr>
    <th>类名</th>
    <th>SequenceList</th>
  </tr>
  <tr>
    <td>构造方法</td>
    <td>SequenceList(int capacity)：创建容量为capacity的SequenceList对象</td>
  </tr>
  <tr>
    <td>成员方法</td>
    <td>1. public void clear()：空置线性表<br>
      2. public boolean isEmpty()：判断线性表是否为空，是返回true，否返回false<br>
      3. public int length():获取线性表中元素的个数<br>
      4. public T get(int i):读取并返回线性表中的第i个元素的值<br><br>
      5. public void insert(int i,T t)：在线性表的第i个元素之前插入一个值为t的数据元素。<br>
      6. public void insert(T t):向线性表中添加一个元素t<br>
      7. public T remove(int i):删除并返回线性表中第i个数据元素。<br>
      8. public int indexOf(T t):返回线性表中首次出现的指定的数据元素的位序号，若不存在，则返回-1<br></td>
  </tr>
  <tr>
    <td>成员变量</td>
    <td>1. private T[] eles：存储元素的数组<br>
      2. private int N:当前线性表的长度</td>
  </tr>
</table>

<details>
<summary>&#127808; View The First Version &#127808;</summary>

```java
package algorithm.Linear;

import java.util.Iterator;

public class SequenceList_FirstVersion<T> implements Iterable{

    private T[] eles;
    private int N;

//    //使用构造函数构造
//    public SequenceList_FirstVersion(int capacity){
//        this.eles =(T[]) new Object[capacity];
//        this.N = 0;
//    }

    // 使用静态工厂，此处重点在于peivate
    private SequenceList_FirstVersion(int capacity){
        this.eles =(T[]) new Object[capacity];
        this.N = 0;
    }

    // 使用静态工厂构造，此处重点在于static
    public static SequenceList_FirstVersion sequenceList_firstVersion1(int capacity){
        return new SequenceList_FirstVersion(capacity);
    }

    public void clear(){
        this.N = 0;
    }
    public boolean idEmpty(){
        return N == 0;
    }
    public int length(){
        return N;
    }
    public T get(int i){
        return eles[i];
    }
    public void insert(T t){
        eles[N++] = t;
    }
    public void insert(int i, T t){//i为下标，不是顺序个数
        for(int index = N; index > i; index--){//倒序读取元素,N-1为最后一个元素下标
            eles[index] = eles[index-1];
        }
        eles[i] = t;
        N++;
    }
    public T remove(int i){
        T temp = eles[i];

        for(int index = i; index < N; index++){
            eles[index] = eles[index+1];
        }

        N--;
        return temp;
    };
    public int indexOf(T t){
        for(int i = 0; i < N; i++){
            if(eles[i].equals(t)) return i;
        }
        return -1;
    };
}

```

Test Code
```java
package algorithm.Linear.Test;

import algorithm.Linear.SequenceList_FirstVersion;

public class SequenceListTest {
    public static void main(String[] args){
//        SequenceList_FirstVersion<String> s1 = new SequenceList_FirstVersion<>(10);

        // 使用静态工厂新建对象
        SequenceList_FirstVersion<String> s1 = SequenceList_FirstVersion.sequenceList_firstVersion1(10);

        s1.insert("张1");
        s1.insert("张2");
        s1.insert("张3");
        s1.insert("张4");
        s1.insert(2,"张2.5");

        // test get
        System.out.println(s1.get(1));
        // test deleat
        System.out.println(s1.remove(0));


    }
}

```
</details>
  
## 1.1.2 &#127800;顺序表遍历
- 容器都要为外部提供遍历方式，Java中遍历集合的方式一般使用的是foreach循环。要使用foreach需要两步：
  1. 让SequenceList实现Iterable接口，重写iterator方法
  ```java
  public class SequenceList<T> implements Iterable<T>{
      .....
    public Iterator iterator(){... }
    private class SIterator implements Iterator{...}
  }
  ```
  2. 在SequenceList内部提供一个内部类SIterator,实现Iterator接口，重写hasNext方法和next方法；
  
<details>
<summary>&#127808; View The Codes &#127808;</summary>

```java
public class SequenceList<T> implements Iterable<T>{
     private class SIterator implements Iterator{

        private int temp;
        public SIterator(){
            this.temp = 0;
        }

        @Override
        public boolean hasNext() {
            return temp<N;
        }

        @Override
        public Object next() {
            return eles[temp++];
        }
    }
}
```
                           
Test Code
```
public class SequenceListTest {
    public static void main(String[] args){

        // 使用静态工厂新建对象
        SequenceList_FirstVersion<String> s1 = SequenceList_FirstVersion.sequenceList_firstVersion1(10);

        s1.insert("张1");
        s1.insert("张2");
        s1.insert("张3");
        s1.insert("张4");
        s1.insert(2,"张2.5");

        for (Object str : s1 ) {
            System.out.println(str);
        }
    }
}                     
```
</details>
  
## 1.1.3 &#127800; 顺序表容量可变
- 测试第一版无扩容机制的，会报异常
<details>
<summary>&#127808; First Edition Test &#127808;</summary>

```java
public class SequenceListWrongCapacityTest {
    public static void main(String[] args){
        SequenceList_FirstEdition<String > s1 = SequenceList_FirstEdition.sequenceList_firstVersion1(2);
        s1.insert("z1");
        s1.insert("z2");
        s1.insert("z3");
    }
}
```
Result
```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 2
	at algorithm.Linear.SequenceList_FirstEdition.insert(SequenceList_FirstEdition.java:40)
	at algorithm.Linear.Test.SequenceListWrongCapacityTest.main(SequenceListWrongCapacityTest.java:10)
```
</details>
  
- 解决方案
  - ***添加元素时***，应该**检查当前数组的大小是否能容纳新的元素**，如果不能容纳，则需要**创建新的容量更大的数组**，可以创建一个是原数组**两倍容量**的新数组存储元素。
  - ***移除元素时***，应该**检查当前数组的大小是否太大**，如果发现数据元素的数量不足数组容量的1/4，则创建一个是原数组容量的1/2的新数组存储元素。
  
<details>
<summary>&#127808; View The Second Version &#127808;</summary>

```java
package algorithm.Linear;

import java.util.Iterator;

public class SequenceList_SecondEdition<T> implements Iterable{
    private int N;
    private T[] eles;

    private SequenceList_SecondEdition(int capacity){
        this.eles = (T[]) new Object[capacity];
        this.N = 0;
    }

    public static SequenceList_SecondEdition sequenceList_secondEdition(int capacity){
        return new SequenceList_SecondEdition(capacity);
    }

    public void clear(){
        this.N = 0;
    }

    public boolean isEmpty(){
        return this.N == 0;
    }

    public int length(){
        return N;
    }

    public T get(int i){
        return eles[i];
    }

    public void insert(T t){
    // ====================================================================================
        if (N == eles.length){
            resize(N * 2);
        }
        eles[N++] = t;
    }

    public void insert(int index, T t){
    // ====================================================================================
        if (N == eles.length){
            resize(N * 2);
        }
        for (int i = N; i > index; i--){
            eles[i] = eles[i-1];
        }

        eles[index] = t;
        N++;
    }
    public T remove(int index){
        T temp = eles[index];
        for (int i = index; i < N; i++){
            eles[i] = eles[i+1];
        }
        N--;
    // ====================================================================================

        if (N < (eles.length / 4)){
            resize(eles.length / 2);
        }
        return temp;
    }
    // ====================================================================================
    public void resize(int newSize){
        // temp array
        T[] temp = eles;
        // new array
        eles = (T[]) new Object[newSize];
        // copy elements to new array
        for (int i = 0; i < N; i++) {
            eles[i] = temp[i];
        }
    }

    @Override
    public Iterator iterator() {
        return null;
    }

    private class SIterator implements Iterator{

        private int temp;
        private SIterator(){
            this.temp = 0;
        }
        @Override
        public boolean hasNext() {
            return this.temp < N;
        }

        @Override
        public Object next() {
            return eles[temp++];
        }
    }
}

```
  
```java
package algorithm.Linear.Test;

import algorithm.Linear.SequenceList_FirstEdition;
import algorithm.Linear.SequenceList_SecondEdition;

public class SequenceListCapacityTest {
    public static void main(String[] args){
        SequenceList_FirstEdition<String > s1 = SequenceList_FirstEdition.sequenceList_firstVersion1(2);
        s1.insert("z1");
        s1.insert("z2");
        s1.insert("z3");

        SequenceList_SecondEdition<String> s2 = SequenceList_SecondEdition.sequenceList_secondEdition(2);
        s2.insert("s1");
        s2.insert("s2");
        s2.insert("s3");
        s2.insert("s4");
        System.out.println(s2.get(3));
    }
}

```
</details>
  
## 1.1.4 &#127800; ArrayList实现顺序表
- Java中的ArrayList与LinkedList可以实现顺序表。
  - ArrayList底层使用数组实现，特点：查询效率高，增删效率低，线程不安全。但由于效率高，我们一般使用它。
  - LinkedList底层用双向链表实现的存储。特点：查询效率低，增删效率高，线程不安全。
  
  
<details>
<summary>&#127808; View The ArrayListTest &#127808;</summary>

```java
package DS.List;

import java.util.ArrayList;
import java.util.Iterator;

public class ArrayList_Test {
    public static void main(String[] args){
        ArrayList<Integer> arrayList = new ArrayList<>();

        arrayList.add(1);
        arrayList.add(2);
        arrayList.add(3);
        arrayList.add(4);
        System.out.println(arrayList.contains(1));          // t
        System.out.println(arrayList.get(0));               // 1
        System.out.println(arrayList.indexOf(1));           // 0
        System.out.println(arrayList.lastIndexOf(1));    // 0
        System.out.println(arrayList.clone());              // [1, 2, 3, 4]
        System.out.println(arrayList.toString());           // [1, 2, 3, 4]
        System.out.println(arrayList.toArray());            // [Ljava.lang.Object;@4554617c
        System.out.println(arrayList.remove(3));      // 4
        System.out.println(arrayList.size());               // 3
        System.out.println(arrayList.isEmpty());            // false
        System.out.println(arrayList.set(0,9));            // 1(替换1为9，返回原来的元素值)

        for (int i :arrayList ) {
            System.out.print(i);                            // 923
        }
        System.out.println();

        Iterator<Integer> iterator = arrayList.iterator();

        while(iterator.hasNext()){
            System.out.print(iterator.next());              // 923
        }

    }
}

```
</details>