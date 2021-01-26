# 栈

栈是一种基于先进后出(FILO)的数据结构，是一种只能在一端进行插入和删除操作的特殊线性表。它按照先进后出的原则存储数据，先进入的数据被压入栈底，最后的数据在栈顶，需要读数据的时候从栈顶开始弹出数据（最后一个数据被第一个读出来）。

我们称数据进入到栈的动作为压栈，数据从栈中出去的动作为弹栈。

一般用单向链表实现，也可用数组实现。


## 栈API设计
<table>
	<tr>
		<th>类名</th>
		<th>Stack</th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Stack)：创建Stack对象</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>1.public boolean isEmpty()：判断栈是否为空，是返回true，否返回false
      
      2.public int size():获取栈中元素的个数
      
      3.public T pop():弹出栈顶元素
      
      4.public void push(T t)：向栈中压入元素t</td>
	</tr>
    <tr>
    <td>成员变量</td>
    <td>1.private Node head:记录首结点
      
      2.private int N:当前栈的元素个数</td>
  </tr>
</table>

<details>
<summary>&#127808; View The Node_OneWay &#127808;</summary>

```java
package algorithm.Linear.List_OneWay;

public class Node_OneWay<T>{
    private T item;
    public Node_OneWay next;

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }

    public Node_OneWay(T item, Node_OneWay next){
        this.next = next;
        this.item = item;
    }
}

```
</details>

<details>
<summary>&#127808; 栈代码实现 &#127808;</summary>

```java
package algorithm.Linear;

import algorithm.Linear.List_OneWay.Node_OneWay;

import java.util.Iterator;


public class Stack_Code<T> implements Iterable{
    private Node_OneWay head;
    private int N;

//    private class Node{
//        public T item;
//        public Node next;
//
//        public Node(T item, Node next) {
//            this.item = item;
//            this.next = next;
//        }
//    }

    private Stack_Code(){
        this.head = new Node_OneWay(null, null);
        this.N = 0;
    }

    public static Stack_Code stack (){
        return new Stack_Code();
    }

    public boolean isEmpty(){
        return N == 0;
    }

    public int size(){
        return N;
    }

    public void push(T t){
        // 找到第一个结点
        Node_OneWay oldFirst = head.next;
        Node_OneWay newNode = new Node_OneWay(t, oldFirst);
        head.next = newNode;
        N++;
    }

    public T pop(){
        Node_OneWay oldFirst = head.next;
        if (oldFirst == null) {
            return null;
        }
        head.next = oldFirst.next;
        N--;
        return (T) oldFirst.getItem();
    }

    @Override
    public Iterator<T> iterator() {
        return new SIterable<>();
    }

    private class SIterable<T> implements Iterator{

        private Node_OneWay n;

        public SIterable(){
            this.n = head;
        }

        @Override
        public boolean hasNext() {
            return n.next != null;
        }

        @Override
        public T next() {
            n = n.next;
            return (T) n.getItem();
        }
    }
}

```
</details>
  
<details>
<summary>&#127808; View The Test &#127808;</summary>

```java
package algorithm.Linear.Test;

import algorithm.Linear.Stack_Code;

public class Stack_Test {
    public static void main(String[] args){
        Stack_Code<String> stack = Stack_Code.stack();
        stack.push("1");
        stack.push("2");
        stack.push("3");
        stack.push("4");

        {
            System.out.print("Stack.size = " + stack.size());
            System.out.print("     Element:");
            for (Object i : stack){
                System.out.print(i + " ");
            }
            System.out.println();
        }

        System.out.println("Stack.pop = " + stack.pop());

        {
            System.out.print("Stack.size = " + stack.size());
            System.out.print("     Element:");
            for (Object i : stack){
                System.out.print(i + " ");
            }
        }

    }
}

```
</details>
  
  
<details>
<summary>&#127808; View The Detail &#127808;</summary>

```java
package cn.itcast.algorithm.linear;

import java.util.Iterator;

public class TowWayLinkList<T> implements Iterable<T> {
    //首结点
    private Node head;
    //最后一个结点
    private Node last;

    //链表的长度
    private int N;



    //结点类
    private class Node{
        public Node(T item, Node pre, Node next) {
            this.item = item;
            this.pre = pre;
            this.next = next;
        }

        //存储数据
        public T item;
        //指向上一个结点
        public Node pre;
        //指向下一个结点
        public Node next;
    }

    public TowWayLinkList() {
       //初始化头结点和尾结点
        this.head = new Node(null,null,null);
        this.last = null;
        //初始化元素个数
        this.N=0;
    }

    //清空链表
    public void clear(){
        this.head.next=null;
        this.head.pre=null;
        this.head.item=null;
        this.last=null;
        this.N=0;
    }

    //获取链表长度
    public int length(){
        return N;
    }

    //判断链表是否为空
    public boolean isEmpty(){
        return N==0;
    }

    //获取第一个元素
    public T getFirst(){
        if (isEmpty()){
            return null;
        }
        return head.next.item;
    }

    //获取最后一个元素
    public T getLast(){
        if (isEmpty()){
            return null;
        }
        return last.item;
    }

    //插入元素t
    public void insert(T t){

        if (isEmpty()){
            //如果链表为空：

            //创建新的结点
            Node newNode = new Node(t,head, null);
            //让新结点称为尾结点
            last=newNode;
            //让头结点指向尾结点
            head.next=last;
        }else {
            //如果链表不为空
            Node oldLast = last;

            //创建新的结点
            Node newNode = new Node(t, oldLast, null);

            //让当前的尾结点指向新结点
            oldLast.next=newNode;
            //让新结点称为尾结点
            last = newNode;
        }

        //元素个数+1
        N++;

    }

    //向指定位置i处插入元素t
    public void insert(int i,T t){
        //找到i位置的前一个结点
        Node pre = head;
        for(int index=0;index<i;index++){
            pre=pre.next;
        }
        //找到i位置的结点
        Node curr = pre.next;
        //创建新结点
        Node newNode = new Node(t, pre, curr);
        //让i位置的前一个结点的下一个结点变为新结点
        pre.next=newNode;
        //让i位置的前一个结点变为新结点
        curr.pre=newNode;
        //元素个数+1
        N++;
    }

    //获取指定位置i处的元素
    public T get(int i){
        Node n = head.next;
        for(int index=0;index<i;index++){
            n=n.next;
        }
        return n.item;
    }

    //找到元素t在链表中第一次出现的位置
    public int indexOf(T t){
        Node n = head;
        for(int i=0;n.next!=null;i++){
            n=n.next;
            if (n.next.equals(t)){
                return i;
            }
        }
        return -1;
    }

    //删除位置i处的元素，并返回该元素
    public T remove(int i){
        //找到i位置的前一个结点
        Node pre = head;
        for(int index=0;index<i;index++){
            pre=pre.next;
        }
        //找到i位置的结点
        Node curr = pre.next;
        //找到i位置的下一个结点
        Node nextNode= curr.next;
        //让i位置的前一个结点的下一个结点变为i位置的下一个结点
        pre.next=nextNode;
        //让i位置的下一个结点的上一个结点变为i位置的前一个结点
        nextNode.pre=pre;
        //元素的个数-1
        N--;
        return curr.item;
    }

    @Override
    public Iterator<T> iterator() {
        return new TIterator();
    }

    private class TIterator implements Iterator{
        private Node n;
        public TIterator(){
            this.n=head;
        }
        @Override
        public boolean hasNext() {
            return n.next!=null;
        }

        @Override
        public Object next() {
            n=n.next;
            return n.item;
        }
    }

}
```
</details>

<details>
<summary>&#127808; View The LinkedList &#127808;</summary>

```java
package DS.Stack;

import java.util.Arrays;
import java.util.LinkedList;

public class Stack_LinkedList {
    public static void main(String[] args){
        LinkedList<Integer> stack = new LinkedList<>();
        stack.addFirst(0);
        stack.addFirst(1);
        stack.addFirst(2);
        System.out.println(Arrays.toString(stack.toArray()));
        int tmp = stack.pollFirst();
        System.out.println(tmp);
        System.out.println(Arrays.toString(stack.toArray()));
    }
}

```
</details>
