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
<summary>&#127808; 生成链表测试 &#127808;</summary>

```java
package algorithm.Linear.Test;

import algorithm.Linear.List_TwoWay.Node_TwoWay;

public class NodeTwoWay_Test {
    public static void main(String[] args){
        Node_TwoWay n1 = new Node_TwoWay(1,null,null) ;
        Node_TwoWay n2 = new Node_TwoWay(2,n1,null) ;
        Node_TwoWay n3 = new Node_TwoWay(3,n2,null) ;
        Node_TwoWay n4 = new Node_TwoWay(4,n3,null) ;
        Node_TwoWay n5 = new Node_TwoWay(4,n4,null) ;

        n1.next = n2;
        n2.next = n3;
        n3.next = n4;
        n4.next = n5;
        System.out.println(n1.next.next.getItem());
        System.out.println(n5.pre.pre.getItem());
    }
}


```
</details>
