# 链表
虽然顺序表的查询很快，时间复杂度为O(1),但是**增删的效率是比较低的**，因为每一次增删操作都伴随着大量的数据元素移动。
可以使用另外一种存储结构实现线性表，链式存储结构。
链表是一种物理存储单元上**非连续、非顺序的存储结构**，其物理结构不能只管的表示数据元素的逻辑顺序，数据元素的逻辑顺序是**通过链表中的指针**链接次序实现的。
链表由一系列的结点（链表中的每一个元素称为结点）组成，结点可以在运行时动态生成。

## 分类
- 单向链表
- 双向链表

## 结点API设计
<table>
	<tr>
		<th>类名</th>
		<th>Node</th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Node(T t,Node next)：创建Node对象</td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>T item:存储数据</br>
		Node next：指向下一个结点</td>
	</tr>
</table>

<details>
<summary>&#127808; 结点类 &#127808;</summary>

```java
package algorithm.Linear;

public class Node<T>{
    private T item;
    public Node next;

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }

    public Node(T item, Node next){
        this.next = next;
        this.item = item;
    }
}

```
</details>

<details>
<summary>&#127808; 生成链表测试 &#127808;</summary>

```java
package algorithm.Linear.Test;

import algorithm.Linear.Node;

public class NodeTest {
    public static void main(String[] args){
        Node<Integer> n1 = new Node<Integer>(1,null);
        Node<Integer> n2 = new Node<>(2,null);
        Node<Integer> n3 = new Node<>(3,null);
        Node<Integer> n4 = new Node<>(4,null);

        n1.next = n2;
        n2.next = n3;
        n3.next = n4;
        n4.setItem(11);
        System.out.println(n3.next.getItem());
    }
}

```
</details>
