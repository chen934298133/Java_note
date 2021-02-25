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
  
## &#127800; 1.4 索引优先队列

- 在之前实现的**最大优先队列**和**最小优先队列**，他们可以分别快速访问到队列中最大元素和最小元素
- 但是他们有一个缺点，就是没有办法**通过索引**访问已存在于优先队列中的对象，并更新它们。
- 为了实现这个目的，在**优先队列的基础上**，学习一种新的数据结构，索引优先队列。
  - 接下来我们以最小索引优先队列举列。
  
### &#127800; 1.4.1 索引优先队列实现思路

1. 步骤一: 存储数据时，给每一个数据元素关联一个整数，例如`insert(int k,T t)`,我们可以看做`k`是`t`关联的整数，那么我们的实现需要通过`k`这个值，快速获取到队列中`t`这个元素，此时有个`k`这个值需要具有唯一性。最直观的想法就是我们可以用一个`T[] items`数组来保存数据元素，在`insert(int k,T t)`完成插入时，可以把`k`看做是`items`数组的索引，把`t`元素放到`items`数组的索引`k`处，这样我们再根据`k`获取元素`t`时就很方便了，直接就可以拿到`items[k]`即可。

![](http://lc-dDwI9S44.cn-n1.lcfile.com/daadb98ca528114c0670.png/%E7%B4%A2%E5%BC%95%E9%98%9F%E5%88%971.png)

2. 步骤二: 步骤一完成后的结果，虽然我们给每个元素关联了一个整数，并且可以使用这个整数快速的获取到该元素，但是，`items`数组中的元素顺序是随机的，并不是堆有序的，所以，为了完成这个需求，我们可以增加一个数组`int[]pq`,来保存每个元素在`items`数组中的索引，`pq`数组需要堆有序，也就是说，`pq[1]`对应的数据元素`items[pq[1]]`要小于等于`pq[2]`和`pq[3]`对应的数据元素`items[pq[2]]`和`items[pq[3]]`。

![](http://lc-dDwI9S44.cn-n1.lcfile.com/bd7ff4e5b3989437753d.png/%E7%B4%A2%E5%BC%95%E9%98%9F%E5%88%972.png)

3. 步骤三: 通过步骤二的分析，我们可以发现，其实我们通过上浮和下沉做堆调整的时候，其实调整的是pq数组。如果需要对`items`中的元素进行修改，比如让`items[0]="H"`,那么很显然，我们需要对`pq`中的数据做堆调整，而且是调整`pq[9]`中元素的位置。但现在就会遇到一个问题，我们修改的是`items`数组中0索引处的值，如何才能快速的知道需要挑中`pq[9]`中元素的位置呢？最直观的想法就是遍历`pq`数组，拿出每一个元素和0做比较，如果当前元素是0，那么调整该索引处的元素即可，但是效率很低。我们可以另外增加一个数组，`int[] qp`,用来存储`pq`的逆序。
  - 例如：在`pq`数组中：`pq[1]=6;`那么在qp数组中，把6作为索引，1作为值，结果是：`qp[6]=1`

![](http://lc-dDwI9S44.cn-n1.lcfile.com/b0e3a0f1adc907f1aa2e.png/%E7%B4%A2%E5%BC%95%E9%98%9F%E5%88%973.png)

4. 步骤四: 当有了`pq`数组后，如果我们修改`items[0]="H"`，那么就可以先通过索引0，在`qp`数组中找到`qp`的索引：`qp[0]=9`,那么直接调整`pq[9]`即可。

### &#127800; 1.4.1 索引优先队列API设计
  
<table>
	<tr>
		<th>类名</th>
		<th>IndexMinPriorityQueue</th>
	</tr>
  	<tr>
		<td>构造方法</td>
		<td>
      IndexMinPriorityQueue(int capacity)：创建容量为capacity的IndexMinPriorityQueue对象
    </td>
	</tr>
  	<tr>
		<td>成员方法</td>
		<td>
      1. private boolean less(int i,int j)：判断堆中索引i处的元素是否小于索引j处的元素<br>
      2. private void exch(int i,int j): 交换堆中i索引和j索引处的值<br>
      3. public int delMin(): 删除队列中最小的元素,并返回该元素关联的索引<br>
      4. public void insert(int i,T t)：往队列中插入一个元素,并关联索引i<br>
      5. private void swim(int k): 使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置<br>
      6. private void sink(int k): 使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置<br>
      7. public int size(): 获取队列中元素的个数<br>
      8. public boolean isEmpty(): 判断队列是否为空<br>
      9. public boolean contains(int k): 判断k对应的元素是否存在<br>
      10. public void changeItem(int i, T t): 把与索引i关联的元素修改为为t<br>
      11. public int minIndex(): 最小元素关联的索引<br>
      12. public void delete(int i): 删除索引i关联的元素<br>
    </td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>
      1. private T[] imtes : 用来存储元素的数组<br>
      2. private int[] pq:保存每个元素在items数组中的索引，pq数组需要堆有序<br>
      3. private int [] qp:保存qp的逆序，pq的值作为索引，pq的索引作为值<br>
      4. private int N：记录堆中元素的个数<br>
    </td>
	</tr>
</table>
  
  
### &#127800; 1.4.2 索引优先队列实现及测试

<details>
<summary>&#127808; IndexMinPriorityQueue &#127808;</summary>
  
```java
package DS.priority;

public class IndexMinPriorityQueue<T extends Comparable<T>> {
    //存储堆中的元素
    private T[] items;
    //保存每个元素在items数组中的索引，pq数组需要堆有序
    private int[] pq;
    //保存qp的逆序，pq的值作为索引，pq的索引作为值
    private int[] qp;
    //记录堆中元素的个数
    private int N;

    public IndexMinPriorityQueue(int capacity) {
        this.items = (T[]) new Comparable[capacity + 1];
        this.pq = new int[capacity + 1];
        this.qp = new int[capacity + 1];
        this.N = 0;

        //默认情况下，队列中没有存储任何数据，让qp中的元素都为-1；
        for (int i = 0; i < qp.length; i++) {
            qp[i] = -1;
        }

    }

    // 1. 获取队列中元素的个数
    public int size() {
        return N;
    }

    // 2. 判断队列是否为空
    public boolean isEmpty() {
        return N == 0;
    }

    // 3. 判断k对应的元素是否存在
    public boolean contains(int k) {
        return qp[k] != -1;
    }

    // 4. 最小元素关联的索引
    public int minIndex() {

        return pq[1];
    }

    // 5. 往队列中插入一个元素,并关联索引i
    public void insert(int i, T t) {
        //判断i是否已经被关联，如果已经被关联，则不让插入

        if (contains(i)) {
            return;
        }
        //元素个数+1
        N++;
        //把数据存储到items对应的i位置处
        items[i] = t;
        //把i存储到pq中
        pq[N] = i;
        //通过qp来记录pq中的i
        qp[i] = N;

        //通过堆上浮完成堆的调整

        swim(N);

    }

    // 6. 删除队列中最小的元素,并返回该元素关联的索引
    public int delMin() {
        //获取最小元素关联的索引
        int minIndex = pq[1];

        //交换pq中索引1处和最大索引处的元素
        exch(1, N);
        //删除qp中对应的内容
        qp[pq[N]] = -1;
        //删除pq最大索引处的内容
        pq[N] = -1;
        //删除items中对应的内容
        items[minIndex] = null;
        //元素个数-1
        N--;
        //下沉调整
        sink(1);

        return minIndex;
    }

    // 7. 删除索引i关联的元素
    public void delete(int i) throws Exception{
 
        //找到i在pq中的索引
        int k = qp[i];

        //交换pq中索引k处的值和索引N处的值
        exch(k, N);
        //删除qp中的内容
        qp[pq[N]] = -1;
        //删除pq中的内容
        pq[N] = -1;
        //删除items中的内容
        items[k] = null;
        //元素的数量-1
        N--;
        //堆的调整
        sink(k);
        swim(k);


    }

    // 8. 把与索引i关联的元素修改为为t
    public void changeItem(int i, T t) {
        //修改items数组中i位置的元素为t
        items[i] = t;
        //找到i在pq中出现的位置
        int k = qp[i];
        //堆调整
        sink(k);
        swim(k);
    }

    public T[] allToString() {
        T[] clone = items.clone();
        return clone;
    }

    // 判断堆中索引i处的元素是否小于索引j处的元素
    private boolean less(int i, int j) {

        return items[pq[i]].compareTo(items[pq[j]]) < 0;
    }

    // 交换堆中i索引和j索引处的值
    private void exch(int i, int j) {
        //交换pq中的数据
        int tmp = pq[i];
        pq[i] = pq[j];
        pq[j] = tmp;


        //更新qp中的数据
        qp[pq[i]] = i;
        qp[pq[j]] = j;

    }

    // 使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置
    private void swim(int k) {
        while (k > 1) {
            if (less(k, k / 2)) {
                exch(k, k / 2);
            }

            k = k / 2;
        }
    }

    // 使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置
    private void sink(int k) {
        while (2 * k <= N) {
            //找到子结点中的较小值
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
            //比较当前结点和较小值
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
<summary>&#127808; ClassLoader_Test &#127808;</summary>
  
```java
package DS.priority;

import java.util.Arrays;

public class IndexTest {
    public static void main(String[] args) throws Exception {
        IndexMinPriorityQueue<Character> queue = new IndexMinPriorityQueue<>(10);

        queue.insert(0,'1');
        queue.insert(1,'a');
        queue.insert(2,'b');
        queue.insert(3,'c');
        queue.insert(4,'d');
        queue.insert(5,'f');

        System.out.println(Arrays.toString(queue.allToString()));
        System.out.println("queue.size(): " + queue.size());
        System.out.println("queue.isEmpty(): " + queue.isEmpty());
        System.out.println("queue.contains(1): " + queue.contains(1));
        System.out.println("queue.minIndex(): " + queue.minIndex());
        System.out.println("queue.delMin(): " + queue.delMin());
//        System.out.println(queue.contains(5));
//        queue.changeItem(5,'z');
        queue.delete(3);
        System.out.println("delete success");
        queue.changeItem(5,'z');
        System.out.println("change success");
        System.out.println(Arrays.toString(queue.allToString()));

    }
}

```
</details>
  
```
[1, a, b, c, d, f, null, null, null, null, null]
queue.size(): 6
queue.isEmpty(): false
queue.contains(1): true
queue.minIndex(): 0
queue.delMin(): 0
delete success
change success
[null, a, null, c, d, z, null, null, null, null, null]
```