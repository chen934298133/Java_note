# 队列
队列是一种基于先进先出(FIFO)的数据结构，是一种只能在一端进行插入,在另一端进行删除操作的特殊线性表，它按照先进先出的原则存储数据，先进入的数据，在读取数据时先读被读出来。


## 队列API设计
<table>
	<tr>
		<th>类名</th>
		<th>Queue</th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Queue()：创建Queue对象</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>1. public boolean isEmpty()：判断队列是否为空，是返回true，否返回false
      
2. public int size():获取队列中元素的个数
      
3. public T dequeue():从队列中拿出一个元素
      
4. public void enqueue(T t)：往队列中插入一个元素</td>
	</tr>
    <tr>
    <td>成员变量</td>
    <td>1. private Node head:记录首结点
      
2. private int N:当前栈的元素个数
      
3. private Node last:记录最后一个结点</td>
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

import javax.xml.soap.Node;
import java.util.Iterator;

public class Queue_Code<T> implements Iterable<T>{
    private Node_OneWay head;
    private Node_OneWay last;
    private  int N;

    private Queue_Code(){
        this.head = new Node_OneWay(null,null);
        this.last = null;
        this.N = 0;
    }

    public static Queue_Code queue(){
        return new Queue_Code();
    }

    public boolean isEmpty(){
        return N == 0;
    }

    public int size(){
        return N;
    }

    public void add(T t){
        if (last == null) {
            last = new Node_OneWay(t,null);
            head.next = last;
        }else {
            Node_OneWay newLast = new Node_OneWay(t, null);
            last.next = newLast;// 将原队尾结点与新队尾结点连接
            last = newLast; // 更新尾结点

//            Node_OneWay oldLast = last;             // 获取原队尾结点
//            last = new Node_OneWay(t, null);   // 更新尾结点
//            oldLast.next=last;                      // 将原队尾结点与新队尾结点连接
        }
        N++;
    }

    public T poll(){
        if (isEmpty()){
            return null;
        }
        Node_OneWay oldFirst = head.next;
        head.next = oldFirst.next;
        N--;

        if (isEmpty()){
            last = null;
        }

        return (T) oldFirst.getItem();
    }

    @Override
    public Iterator<T> iterator() {
        return new QIterator();
    }

    private class QIterator implements Iterator{

        private Node_OneWay n;

        public QIterator(){
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

import algorithm.Linear.Queue_Code;

public class Queue_Test {
    public static void main(String[] args){
        Queue_Code<Integer> queue1 = Queue_Code.queue();
        queue1.add(1);
        queue1.add(2);
        queue1.add(3);
        System.out.println(queue1.isEmpty());
        System.out.println("queue.size = " + queue1.size());
        System.out.print("queue.elements : ");
        for (Integer i : queue1){
            System.out.print(i + " ");
        }
        System.out.println();

        System.out.print(queue1.poll());
        System.out.print(queue1.poll());
        System.out.println(queue1.poll());
        System.out.println(queue1.size());

        Queue_Code<String> queue2 = Queue_Code.queue();
        queue2.add("a");
        queue2.add("b");
        queue2.add("c");
    }
}

```
</details>
  
  
<details>
<summary>&#127808; View The Detail &#127808;</summary>

```java
package algorithm.linear;

import java.util.Iterator;

public class Queue<T> implements Iterable<T>{
    //记录首结点
    private Node head;
    //记录最后一个结点
    private Node last;
    //记录队列中元素的个数
    private int N;


    private class Node{
        public T item;
        public Node next;

        public Node(T item, Node next) {
            this.item = item;
            this.next = next;
        }
    }
    public Queue() {
        this.head = new Node(null,null);
        this.last=null;
        this.N=0;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return N==0;
    }

    //返回队列中元素的个数
    public int size(){
        return N;
    }

    //向队列中插入元素t
    public void enqueue(T t){

        if (last==null){
            //当前尾结点last为null
            last= new Node(t,null);
            head.next=last;
        }else {
            //当前尾结点last不为null
            Node oldLast = last;
            last = new Node(t, null);
            oldLast.next=last;
        }

        //元素个数+1
        N++;
    }

    //从队列中拿出一个元素
    public T dequeue(){
        if (isEmpty()){
            return null;
        }

        Node oldFirst= head.next;
        head.next=oldFirst.next;
        N--;

        //因为出队列其实是在删除元素，因此如果队列中的元素被删除完了，需要重置last=null;

        if (isEmpty()){
            last=null;
        }
        return oldFirst.item;
    }


    @Override
    public Iterator<T> iterator() {
        return new QIterator();
    }

    private class QIterator implements Iterator{
        private Node n;

        public QIterator(){
            this.n=head;
        }
        @Override
        public boolean hasNext() {
            return n.next!=null;
        }

        @Override
        public Object next() {
            n = n.next;
            return n.item;
        }
    }


}


```
</details>

<details>
<summary>&#127808; View The LinkedList &#127808;</summary>

```java
package DS.Queue;

import java.util.Arrays;
import java.util.LinkedList;

public class Queue_LinkedList {
    public static void main(String[] args){
        LinkedList<Integer> queue = new LinkedList();
        queue.addLast(0);
        queue.addLast(1);
        queue.addLast(2);
        System.out.println(Arrays.toString(queue.toArray()));
        System.out.println(queue.pollFirst());
        System.out.println(Arrays.toString(queue.toArray()));
    }
}


```
</details>
