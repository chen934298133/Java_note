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
    
## &#127800; 1.2 最大优先队列
- 之前学习过堆，而 **堆** 这种结构是可以**方便的删除最大的值**，所以现在基于 **堆** 实现 **最大优先队列**。

### &#127800; 1.2.1 最大优先队列API设计

<table>
	<tr>
		<th>类名</th>
		<th>MaxPriorityQueue</th>
	</tr>
  	<tr>
		<td>构造方法</td>
		<td>
      MaxPriorityQueue(int capacity)：创建容量为capacity的MaxPriorityQueue对象
    </td>
	</tr>
  	<tr>
		<td>成员方法</td>
		<td>
      1. private boolean less(int i,int j)：判断堆中索引i处的元素是否小于索引j处的元素<br>
      2. private void exch(int i,int j): 交换堆中i索引和j索引处的值<br>
      3. public T delMax(): 删除队列中最大的元素,并返回这个最大元素<br>
      4. public void insert(T t)：往队列中插入一个元素<br>
      5. private void swim(int k): 使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置<br>
      6. private void sink(int k): 使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置<br>
      7. public int size(): 获取队列中元素的个数<br>
      8. public boolean isEmpty(): 判断队列是否为空<br>
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

### &#127800; 1.2.2 最大优先队列实现及测试

<details>
<summary>&#127808; 最大优先队列 &#127808;</summary>
  
```java
package DS.priority;

public class MaxPriorityQueue<T extends Comparable<T>> {
    //存储堆中的元素
    private T[] items;
    //记录堆中元素的个数
    private int N;

    public MaxPriorityQueue(int capacity) {
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

    //删除堆中最大的元素,并返回这个最大元素
    public T delMax() {
        T max = items[1];
        exch(1, N);
        N--;
        sink(1);
        return max;
    }

    //使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置
    private void swim(int k) {
        while (k > 1) {
            if (less(k / 2, k)) {
                exch(k / 2, k);
            }
            k = k / 2;
        }
    }

    //使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置
    private void sink(int k) {

        while (2 * k <= N) {
            int max;
            if (2 * k + 1 <= N) {
                if (less(2 * k, 2 * k + 1)) {
                    max = 2 * k + 1;
                } else {
                    max = 2 * k;
                }
            } else {
                max = 2 * k;
            }


            if (!less(k, max)) {
                break;
            }

            exch(k, max);
            k = max;
        }

    }

}

```
</details>

<details>
<summary>&#127808; Test &#127808;</summary>
  
```java
package DS.priority;

public class MaxTest {
    public static void main(String[] args) {

        MaxPriorityQueue<Integer> queue = new MaxPriorityQueue<>(10);

        queue.insert(1);
        queue.insert(2);
        queue.insert(3);
        queue.insert(4);
        queue.insert(5);
        queue.insert(6);

        System.out.print("MaxTest: ");
        while (!queue.isEmpty()) {
            System.out.print(queue.delMax() + " ");
        }
    }
}
```
</details>

```
MaxTest: 6 5 4 3 2 1 
```