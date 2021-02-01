#Hash

**哈希表最主要的目的就是将一个键和一个值联系起来，哈希表能够将存储的数据元素是一个键和一个值共同组成的键值对数据，我们可以根据键来查找对应的值。**

**哈希表的键具有唯一性。**


## 结点类API设计
<table>
	<tr>
		<th>类名</th>
		<th>Node<Key,Value></th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Node(Key key, Value value, Node left, Node right)：创建Node对象</td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>
1. public Key key:存储键<br>
2. public Value value:存储值<br>
3. public Node next:存储下一个结点<br></td>
	</tr>
</table>

## 哈希表API设计
<table>
	<tr>
		<th>类名</th>
		<th>HashTable<Key,Value></th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>HashTable()：创建SymbolTable对象</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>
1. public Value get(Key key)：根据键key，找对应的值<br>
2. public void put(Key key,Value val):向符号表中插入一个键值对<br> 
3. public void delete(Key key):删除键为key的键值对 <br>
4. public int size()：获取符号表的大小<br></td>
	</tr>
  <tr>
		<td>成员变量</td>
		<td>
1. private Node head:记录首结点<br>
2. private int N:记录符号表中键值对的个数</td>
	</tr>
</table>

<details>
<summary>&#127808; View The Node &#127808;</summary>

```java
package algorithm.Hash_2;

public class Node<Key, Value> {
    private Key key;
    private Value value;
    public Node next;

    public Node(Key key, Value value, Node next){
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public Key getKey() {
        return key;
    }

    public void setKey(Key key) {
        this.key = key;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }
}

```
</details>

<details>
<summary>&#127808; hashtable代码实现 &#127808;</summary>

```java
package algorithm.Hash_2;

import java.util.Iterator;

public class HashTable_Code<Key, Value> implements Iterable<Key>{
    private Node head;
    private int N;

    private HashTable_Code(){
        this.head = new Node(null, null, null);
        this.N = 0;
    }

    public static HashTable_Code hashTable_code(){
        return new HashTable_Code();
    }

    public int size(){
        return N;
    }

    public void put(Key key, Value value){
        Node temp = head;
        while( temp.next != null){
            temp = temp.next;
            if(temp.getKey().equals(key)){
                temp.setValue(value);
            }
        }
        Node newFirst = new Node(key, value,null);
        Node oldFirst = head.next;
        newFirst.next = oldFirst;
        head.next = newFirst;
        N++;
    }

    public void delete(Key key){
        // 此处耗费半小时理解了起始处head与head.next不同写法
        Node cur = head;
//        Node temp = head.next;
        while (cur.next != null){
            if (cur.next.getKey().equals(key)){
                cur.next = cur.next.next;
                N--;
                return;
            }
//            temp = temp.next;
            cur = cur.next;
        }
    }

    public Value get(Key key){
        Node temp = head;
        while (temp.next != null){
            temp = temp.next;
            if (temp.getKey().equals(key)){
                return (Value) temp.getValue();
            }
        }
        return null;
    }

    @Override
    public Iterator<Key> iterator() {
        return new HIterator();
    }

    private class HIterator implements Iterator{

        private Node curr;
        public HIterator(){
            this.curr = head;
        }
        @Override
        public boolean hasNext() {
            return curr.next != null;
        }

        @Override
        public Key next() {
            curr = curr.next;
            return (Key) curr.getKey();
        }
    }
}

```
</details>
  
<details>
<summary>&#127808; View The OrderHashTable代码实现（主要在于put方法的重写） &#127808;</summary>

```java
package algorithm.Hash_2;

// 有序只需要改put方法
public class OrderHashTable_Code<Key extends Comparable, Value> {
    private Node head;
    private int N;

    private OrderHashTable_Code(){
        this.head = new Node(null, null, null);
        this.N = 0;
    }

    public static OrderHashTable_Code hashTable_code(){
        return new OrderHashTable_Code();
    }

    public int size(){
        return N;
    }

    // 根据键保持顺序，需要再泛型声明时继承comparable,以便调用compareTo()
    public void put(Key key, Value value){
        Node pre = head;
        Node curr = head.next;
        while (curr != null && key.compareTo(curr.getKey()) > 0){
            pre = curr;
            curr = curr.next;
        }

        if (curr != null && key.compareTo(curr.getKey()) == 0){
            curr.setValue(value);
            return;
        }

        Node newNode = new Node(key, value, curr);
        pre.next = newNode;

        N++;
    }

    public void delete(Key key){
        Node temp = head;
        while (temp.next != null){
            if (temp.next.getKey().equals(key)){
                temp.next = temp.next.next;
                N--;
                return;
            }
            temp = temp.next;
        }
    }

    public Value get(Key key){
        Node temp = head;
        while (temp.next != null){
            temp = temp.next;
            if (temp.getKey().equals(key)){
                return (Value) temp.getValue();
            }
        }
        return null;
    }
}
```
</details>
  
<details>
<summary>&#127808; View The Test &#127808;</summary>

```java
package algorithm.Hash_2;

public class Test{
    public static void main(String[] args){
        HashTable_Code<Integer, String> hashtable = HashTable_Code.hashTable_code();
        hashtable.put(1,"one");
        hashtable.put(2,"two");
        hashtable.put(3,"three");
        hashtable.put(4,"four");

        System.out.println(hashtable.size());
        System.out.println(hashtable.get(2));
        // 要想在此处使用Integer、String等重点在于继承Iterable时也要带上泛型
        for (Integer key: hashtable) {
            System.out.println(key);
        }

        hashtable.delete(3);

        // 要想在此处使用Integer、String等重点在于继承Iterable时也要带上泛型
        for (Integer key: hashtable) {
            System.out.println(key);
        }
        System.out.println(hashtable.size());
        System.out.println(hashtable.get(10));


        // ========================================================

        OrderHashTable_Code h1 = OrderHashTable_Code.hashTable_code();
        h1.put(1,"1");
        h1.put(2,"2");
        h1.put(3,"3");
        System.out.println(h1.size());
        h1.delete(1);
        System.out.println(h1.size());
        System.out.println(h1.get(1));
    }
}


```
</details>
  
  
<details>
<summary>&#127808; View The Detail &#127808;</summary>

```java
package cn.itcast.algorithm.symbol;

public class OrderSymbolTable<Key extends Comparable<Key>,Value> {
    //记录首结点
    private Node head;
    //记录符号表中元素的个数
    private int N;

    private class Node{
        //键
        public Key key;
        //值
        public Value value;
        //下一个结点
        public Node next;

        public Node(Key key, Value value, Node next) {
            this.key = key;
            this.value = value;
            this.next = next;
        }
    }

    public OrderSymbolTable() {
        this.head = new Node(null,null,null);
        this.N=0;
    }

    //获取符号表中键值对的个数
    public int size(){
        return N;
    }

    //往符号表中插入键值对
    public void put(Key key,Value value){
        //定义两个Node变量，分别记录当前结点和当前结点的上一个结点

        Node curr = head.next;
        Node pre = head;
        while(curr!=null && key.compareTo(curr.key)>0){

            //变换当前结点和前一个结点即可
            pre = curr;
            curr = curr.next;
        }

        //如果当前结点curr的键和要插入的key一样，则替换
        if (curr!=null && key.compareTo(curr.key)==0){
            curr.value = value;
            return;
        }

        //如果当前结点curr的键和要插入的key不一样，把新的结点插入到curr之前
        Node newNode = new Node(key, value, curr);
        pre.next = newNode;

        //元素的个数+1；
        N++;

    }

    //删除符号表中键为key的键值对
    public void delete(Key key){
        //找到键为key的结点，把该结点从链表中删除

        Node n = head;
        while(n.next!=null){
            //判断n结点的下一个结点的键是否为key，如果是，就删除该结点
            if (n.next.key.equals(key)){
                n.next = n.next.next;
                N--;
                return;
            }


            //变换n
            n = n.next;
        }
    }

    //从符号表中获取key对应的值
    public Value get(Key key){
        //找到键为key的结点
        Node n = head;
        while(n.next!=null){
            //变换n
            n = n.next;
            if (n.key.equals(key)){
                return n.value;
            }
        }
        return null;
    }


}

```
</details>
