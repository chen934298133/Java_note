# &#127800; 优先队列 &#127800;

- **普通的队列**是一种 先进先出 的数据结构，元素在队列尾追加，而从队列头删除。
- 在某些情况下，我们可能需要找出队列中的 **最大值或者最小值**，
  - 例如: 使用一个队列保存计算机的任务，一般情况下计算机的任务都是有优先级的，我们需要在这些计算机的任务中找出优先级最高的任务先执行，执行完毕后就需要把这个任务从队列中移除。
  - 普通的队列要完成这样的功能，需要每次**遍历队列中的所有元素**，比较并找出最大值，效率不是很高，这个时候，我们就可以使用一种特殊的队列来完成这种需求，**优先队列**。
## &#127800; 1.1 分类
- 按照其作用不同，可以分为以下两种：
  - 最大优先队列：
    - 可以获取并删除队列中最大的值
  - 最小优先队列：
    - 可以获取并删除队列中最小的值
    
## &#127800; 1.3 最小优先队列
- 最小优先队列实现起来也比较简单，我们同样也可以基于**堆**来完成**最小优先队列**。
- 堆中存放数据元素的数组要满足都满足如下特性：
  1. 最大的元素放在数组的索引1处。
  2. 每个结点的数据总是大于等于它的两个子结点的数据。
  
![最大堆](http://lc-dDwI9S44.cn-n1.lcfile.com/531ff5395ed973061049.png/%E6%9C%80%E5%A4%A7%E5%A0%86.png)

- 之前实现的堆可以把它叫做最大堆，可以用相反的思想实现最小堆，让堆中存放数据元素的数组满足如下特性：
  1. 最小的元素放在数组的索引1处。
  2. 每个结点的数据总是小于等于它的两个子结点的数据。

![最小堆](http://lc-dDwI9S44.cn-n1.lcfile.com/4fe126050f1e098ae301.png/%E6%9C%80%E5%B0%8F%E5%A0%86.png)
  
### &#127800; 1.3.2 最小优先队列API设计

<table>
	<tr>
		<th>类名</th>
		<th>MinPriorityQueue</th>
	</tr>
  	<tr>
		<td>构造方法</td>
		<td>
      MinPriorityQueue(int capacity)：创建容量为capacity的MinPriorityQueue对象
    </td>
	</tr>
  	<tr>
		<td>成员方法</td>
		<td>
      1. private boolean less(int i,int j)：判断堆中索引 i 处的元素是否小于索引 j 处的元素<br>
      2. private void exch(int i,int j): 交换堆中 i 索引和 j 索引处的值<br>
      3. public T delMin(): 删除队列中最小的元素,并返回这个最小元素<br>
      4. public void insert(T t)：往队列中插入一个元素<br>
      5. private void swim(int k):使用上浮算法，使索引 k 处的元素能在堆中处于一个正确的位置<br>
      6. private void sink(int k):使用下沉算法，使索引 k 处的元素能在堆中处于一个正确的位置<br>
      7. public int size():获取队列中元素的个数<br>
      8. public boolean isEmpty():判断队列是否为空<br>
    </td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>
      1. private T[] imtes : 用来存储元素的数组<br>
      2. private int N：记录堆中元素的个数<br>
    </td>
	</tr>
</table>

### &#127800; 1.3.2 最小优先队列实现与测试
  
<details>
<summary>&#127808; MinPriorityQueue &#127808;</summary>
  
```java
package DS.priority;

public class MinPriorityQueue<T extends Comparable<T>> {
    //存储堆中的元素
    private T[] items;
    //记录堆中元素的个数
    private int N;

    public MinPriorityQueue(int capacity) {
        this.items = (T[]) new Comparable[capacity + 1];
        this.N = 0;
    }

    //获取队列中元素的个数
    public int size() {
        return N;
    }

    //判断队列是否为空
    public boolean isEmpty() {
        return N == 0;
    }

    //判断堆中索引i处的元素是否小于索引j处的元素
    private boolean less(int i, int j) {
        return items[i].compareTo(items[j]) < 0;
    }

    //交换堆中i索引和j索引处的值
    private void exch(int i, int j) {
        T tmp = items[i];
        items[i] = items[j];
        items[j] = tmp;
    }

    //往堆中插入一个元素
    public void insert(T t) {
        items[++N] = t;
        swim(N);
    }

    //删除堆中最小的元素,并返回这个最小元素
    public T delMin() {
        T min = items[1];
        exch(1, N);
        N--;
        sink(1);
        return min;
    }

    //使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置
    private void swim(int k) {
        //通过循环比较当前结点和其父结点的大小

        while (k > 1) {

            if (less(k, k / 2)) {
                exch(k, k / 2);
            }

            k = k / 2;
        }
    }

    //使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置
    private void sink(int k) {
        //通过循环比较当前结点和其子结点中的较小值

        while (2 * k <= N) {
            //1.找到子结点中的较小值
            int min;
            if (2 * k + 1 <= N) {
                if (less(2 * k, 2 * k + 1)) {
                    min = 2 * k;
                } else {
                    min = 2 * k + 1;
                }
            } else {
                min = 2 * k;
            }

            //2.判断当前结点和较小值的大小

            if (less(k, min)) {
                break;
            }
            
            exch(k, min);
            k = min;
        }
    }

}

```
</details>
  
<details>
<summary>&#127808; MinTest &#127808;</summary>
  
```java
package DS.priority;

public class MinTest {
    public static void main(String[] args) {

        MinPriorityQueue<Integer> queue = new MinPriorityQueue<>(10);

        queue.insert(1);
        queue.insert(2);
        queue.insert(3);
        queue.insert(4);
        queue.insert(5);
        queue.insert(6);

        System.out.print("MinTest: ");
        while (!queue.isEmpty()) {
            System.out.print(queue.delMin() + " ");
        }
    }
}
```
</details>

```
MinTest: 1 2 3 4 5 6 
```
  