# 双向链表
双向链表也叫双向表，是链表的一种，它由多个结点组成，每个结点都由一个数据域和两个指针域组成，数据域用来存储数据，其中一个指针域用来指向其后继结点，另一个指针域用来指向前驱结点。链表的头结点的数据域不存储数据，指向前驱结点的指针域值为null，指向后继结点的指针域指向第一个真正存储数据的结点。


## 单向链表API设计
<table>
	<tr>
		<th>类名</th>
		<th>LinkList_TwoWay</th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>TowWayLinkList()：创建TowWayLinkList对象</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>1.public void clear()：空置线性表
      
2.publicboolean isEmpty()：判断线性表是否为空，是返回true，否返回false
      
3.public int length():获取线性表中元素的个数
      
4.public T get(int i):读取并返回线性表中的第i个元素的值
      
5.public void insert(T t)：往线性表中添加一个元素；
      
6.public void insert(int i,T t)：在线性表的第i个元素之前插入一个值为t的数据元素。
      
7.public T remove(int i):删除并返回线性表中第i个数据元素。
      
8.public int indexOf(T t):返回线性表中首次出现的指定的数据元素的位序号，若不存在，则返回-1。
      
9.public T getFirst():获取第一个元素
      
10.public T getLast():获取最后一个元素</td>
	</tr>
  <tr>
    <td>成员内部类（可有可无）</td>
    <td>private class Node:结点类</td>
  </tr>
    <tr>
    <td>成员变量</td>
    <td>1.private Node first:记录首结点
      
2.private Node last:记录尾结点2.private int N:记录链表的长度</td>
  </tr>
</table>

<details>
<summary>&#127808; View The Node_TwoWay &#127808;</summary>

```java
package algorithm.Linear.List_TwoWay;

public class Node_TwoWay<T> {
    private T item;
    public Node_TwoWay pre;
    public Node_TwoWay next;

    public Node_TwoWay(T item, Node_TwoWay pre, Node_TwoWay next){
        this.item = item;
        this.pre = pre;
        this.next = next;
    }

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }
}

```
</details>

<details>
<summary>&#127808; 双向链表代码实现 &#127808;</summary>

```java
package algorithm.Linear.List_TwoWay;



public class LinkList_TwoWay<T> {
    private Node_TwoWay head;
    private Node_TwoWay last;
    private int N;

    private LinkList_TwoWay() {
        this.head = new Node_TwoWay(null, null, null);
        this.last = null;
        this.N = 0;
    }
    // 使用静态工厂构造
    public static LinkList_TwoWay l1(){
        return new LinkList_TwoWay();
    }

    public void clear(){
        this.head.next = null;
        this.head.pre = null;
        this.head.setItem(null);
        this.last = null;
        this.N = 0;
    }

    public int length(){
        return N;
    }

    public boolean isEmpty(){
        return N == 0;
    }

    public void insert(T t){
        if (isEmpty()){
            Node_TwoWay newNode = new Node_TwoWay(t, head,null);

            head.next = newNode;
            last = newNode;
        }else {
            Node_TwoWay oldLast = last;
            Node_TwoWay newNode = new Node_TwoWay(t, oldLast,null);

            oldLast.next = newNode;
            last = newNode;
        }
        N++;
    }

    public void insert(int index, T t){
        Node_TwoWay pre = head;
        for (int i = 0; i < index; i++){
            pre= pre.next;
        }
        Node_TwoWay curr = pre.next;
        Node_TwoWay newCode= new Node_TwoWay(t, pre, curr);
        pre.next = newCode;
        curr.pre = newCode;
        N++;
    }

    public T get(int index){
        Node_TwoWay pre = head;
        for (int i = 0; i < index; i++){
            pre = pre.next;
        }
        return (T) pre.next.getItem();
    }

    public T remove(int index){
        Node_TwoWay pre = head;
        // find pre
        for (int i = 0; i < index; i++){
            pre = pre.next;
        }
        // find curr
        Node_TwoWay curr = pre.next;
        // find curr.next
        Node_TwoWay currNext = curr.next;

        /* remove curr*/
        // set pre.next = curr.next
        pre.next = currNext;
        // set curr.next.pre = pre
        currNext.pre = pre;

        N--;
        return (T) curr.getItem();
    }

    public T getFirst(){
        if (isEmpty()){
            return null;
        }
        return (T) head.next.getItem();
    }
    public T getLast(){
        if (isEmpty()){
            return null;
        }
        return (T) last.getItem();
    }
    
    public int indexOf(T t){
        Node_TwoWay pre = head;
        for (int i = 0; i < length(); i++){
            pre = pre.next;
            if (pre.getItem().equals(t)){
                return i;
            }
        }
        return -1;
    }


}

```
</details>
  
<details>
<summary>&#127808; View The Test &#127808;</summary>

```java
package algorithm.Linear.Test;

import algorithm.Linear.List_TwoWay.LinkList_TwoWay;

public class LinkList_TwoWayTest {
    public static void main(String[] args){
        LinkList_TwoWay l1 = LinkList_TwoWay.l1();
        l1.insert("1");
        l1.insert("2");
        l1.insert("32");
        l1.insert('s');
        l1.insert(2);
        l1.insert(2.1);
        l1.insert(2,"2.5");
        System.out.println(l1.getFirst());
        System.out.print(l1.get(0) + " ");
        System.out.print(l1.get(1) + " ");
        System.out.print(l1.get(2) + " ");
        System.out.println(l1.get(3));
        System.out.println(l1.remove(2));
        System.out.println(l1.getLast());
        System.out.println(l1.indexOf(2));
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
