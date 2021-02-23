# &#127800; 堆 &#127800;

**堆是计算机科学中一类特殊的数据结构的统称，堆通常可以被看做是一棵完全二叉树的数组对象。**

## &#127800; 1.1 堆的特性：
- 它是**完全二叉树**，除了树的最后一层结点不需要是满的，其它的**每一层从左到右都是满的**，如果最后一层结点不是满的，那么要求左满右不满。

![](http://lc-dDwI9S44.cn-n1.lcfile.com/9ce523da5f482294f063.png/%E5%AE%8C%E5%85%A8%E4%B8%8E%E4%B8%8D%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91.png)

- 它通常用**数组**来实现。
  - 具体方法就是将二叉树的结点按照**层级顺序**放入数组中，根结点在位置1，它的子结点在位置2和3，而子结点的子结点则分别在位置4,5,6和7，以此类推。
  
![](http://lc-dDwI9S44.cn-n1.lcfile.com/3010071a42666105bacf.png/%E5%A0%86%E7%9A%84%E6%95%B0%E7%BB%84%E5%AE%9E%E7%8E%B0.png)
# 重点处！！！！
- 如果一个结点的位置为k，则它的**父结点的位置为[k/2]**,而它的两个**子结点的位置则分别为2k和2k+1**。这样，在**不使用指针**的情况下，我们也可以**通过计算数组的索引**在树中**上下移动**：
  - &#127826; &#127826; &#127826; 从 `a[k]` 向上一层，就令 `k` 等于 `k/2` &#127826; &#127826; &#127826;
  - &#127826; &#127826; &#127826; 向下一层就令 `k` 等于 `2k` 或 `2k+1`。 &#127826; &#127826; &#127826;
- 每个结点都**大于等于它的两个子结点**。
  - 这里要注意堆中**仅仅规定了**每个结点大于等于它的两个子结点，但这两个子结点的**顺序并没有做规定**，跟我们之前学习的二叉查找树是**有区别的**。
  

## &#127800; 1.2 堆的API设计
<table>
	<tr>
		<th>类名</th>
		<th>Heap</th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Heap(int capacity)：创建容量为 capacity 的 Heap 对象</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>
1.private boolean less(int i,int j)：判断堆中索引i处的元素是否小于索引j处的元素<br>
      2.private void exch(int i,int j):交换堆中i索引和j索引处的值<br>
      3.public T delMax():删除堆中最大的元素,并返回这个最大元素<br>
      4.public void insert(T t)：往堆中插入一个元素<br>
      5.private void swim(int k):使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置<br>
      6.private void sink(int k):使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置<br>
    </td>
	</tr>
  <tr>
		<td>成员变量</td>
		<td>
      1.private T[] imtes : 用来存储元素的数组<br>
      2.private int N：记录堆中元素的个数<br>
    </td>
	</tr>
</table>

## &#127800; 1.3 insert插入方法的实现
- 堆是用数组完成数据元素的存储的，由于数组的底层是一串连续的内存地址，所以我们要往堆中插入数据，我们只能往数组中从索引0处开始，依次往后存放数据
- 但是堆中对元素的顺序是有要求的，每一个结点的数据要大于等于它的两个子结点的数据，所以每次插入一个元素，都会使得堆中的数据顺序变乱
- 这个时候我们就需要通过一些方法让刚才插入的这个数据放入到合适的位置
  - 上浮算法
  
![](http://lc-dDwI9S44.cn-n1.lcfile.com/32ba990e06b4568c368d.png/%E4%B8%8A%E6%B5%AE%E7%AE%97%E6%B3%95.png)

- 上浮时只需要不断比较**新结点 `a[k]`** 和它的**父结点 `a[k/2]`** 的大小，然后根据结果完成数据元素的**交换**，就可以完成堆的有序调整。

<details>
<summary>&#127808; 上浮算法思路 &#127808;</summary>

```java
    //使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置
    private void swim(int k){
        //通过循环，不断的比较 当前结点的值 和 其父结点的值，如果发现父结点的值比当前结点的值小，则交换位置
        while(k>1){
            //比较当前结点和其父结点
            if (less(k/2, k)){
                exch(k/2, k);
            }
            k = k/2;
        }
    }
```
</details>

## &#127800; 1.4 delMax删除最大元素方法的实现
- 由堆的特性我们可以知道，索引1处的元素，也就是根结点就是最大的元素
- 当我们把根结点的元素删除后，需要有一个新的根结点出现
  - 这时我们可以暂时把堆中最后一个元素放到索引1处，充当根结点，但是它有可能不满足堆的有序性需求
  - 这个时候我们就需要通过一些方法，让这个新的根结点放入到合适的位置。
    - 下沉算法
    
![](http://lc-dDwI9S44.cn-n1.lcfile.com/f143dca03d8c656a03e7.png/%E4%B8%8B%E6%B2%89%E7%AE%97%E6%B3%951.png)
![](http://lc-dDwI9S44.cn-n1.lcfile.com/ebb8abeae80d21b302c4.png/%E4%B8%8B%E6%B2%89%E7%AE%97%E6%B3%952.png)

- 只需要不断的比较**新结点 `a[k]`** 和它的**父结点 `a[k/2]`** 的大小，然后根据结果完成数据元素的**交换**，就可以完成堆的有序调整。

<details>
<summary>&#127808; 下沉算法思路 &#127808;</summary>

```java
//使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置
    private void sink(int k) {
        //通过循环不断的对比当前k结点和其左子结点2*k以及右子结点2k+1处中的较大值的元素大小，
        //如果当前结点小，则需要交换位置
        while (2 * k <= N) {
            //获取当前结点的子结点中的较大结点
            int max;//记录较大结点所在的索引
            if (2 * k + 1 <= N) {//如果存在右子节点
                if (less(2 * k, 2 * k + 1)) {//左子节点小于右子节点
                    max = 2 * k + 1;
                } else {
                    max = 2 * k;
                }
            } else {//若没有右子节点，即只有左子节点
                max = 2 * k;
            }

            //比较当前结点和较大结点的值
            if (!less(k, max)) {//如果k > max处的元素，结束重新排序
                break;
            }

            //交换k索引处的值和max索引处的值
            exch(k, max);

            //变换k的值，为下次循环做准备
            k = max;

        }


    }
```
</details>

## &#127800; 1.5 堆的实现

<details>
<summary>&#127808; 堆的实现 &#127808;</summary>

```java
package DS.Heap;

public class Heap<T extends Comparable<T>> {
    //存储堆中的元素
    private T[] items;
    //记录堆中元素的个数
    private int N;

    // 构造方法
    public Heap(int capacity) {
        this.items = (T[]) new Comparable[capacity + 1];
        this.N = 0;
    }

    //判断堆中索引i处的元素是否小于索引j处的元素
    private boolean less(int i, int j) {
        return items[i].compareTo(items[j]) < 0;
    }

    //交换堆中i索引和j索引处的值
    private void exch(int i, int j) {
        T temp = items[i];
        items[i] = items[j];
        items[j] = temp;
    }

    //往堆中插入一个元素
    public void insert(T t) {
        items[++N] = t;
        swim(N);
    }

    //使用上浮算法，使索引k处的元素能在堆中处于一个正确的位置
    private void swim(int k) {
        //通过循环，不断的比较当前结点的值和其父结点的值，如果发现父结点的值比当前结点的值小，则交换位置
        while (k > 1) {
            //比较当前结点和其父结点
            if (less(k / 2, k)) {
                exch(k / 2, k);
            }
            k = k / 2;
        }
    }

    //删除堆中最大的元素,并返回这个最大元素
    public T delMax() {
        T max = items[1];

        //交换索引1处的元素和最大索引处的元素，让完全二叉树中最右侧的元素变为临时根结点
        exch(1, N);
        //最大索引处的元素删除掉
        items[N] = null;
        //元素个数-1
        N--;
        //通过下沉调整堆，让堆重新有序
        sink(1);
        return max;
    }

    //使用下沉算法，使索引k处的元素能在堆中处于一个正确的位置
    private void sink(int k) {
        //通过循环不断的对比当前k结点和其左子结点2*k以及右子结点2k+1处中的较大值的元素大小，
        //如果当前结点小，则需要交换位置
        while (2 * k <= N) {
            //获取当前结点的子结点中的较大结点
            int max;//记录较大结点所在的索引
            if (2 * k + 1 <= N) {//如果存在右子节点
                if (less(2 * k, 2 * k + 1)) {//左子节点小于右子节点
                    max = 2 * k + 1;
                } else {
                    max = 2 * k;
                }
            } else {//若没有右子节点，即只有左子节点
                max = 2 * k;
            }

            //比较当前结点和较大结点的值
            if (!less(k, max)) {//如果k > max处的元素，结束重新排序
                break;
            }

            //交换k索引处的值和max索引处的值
            exch(k, max);

            //变换k的值，为下次循环做准备
            k = max;

        }


    }

    public static void main(String[] args) {
        Heap<String> heap = new Heap<String>(20);
        heap.insert("A");
        heap.insert("B");
        heap.insert("C");
        heap.insert("D");
        heap.insert("E");
        heap.insert("F");
        heap.insert("G");


        String del;
        while ((del = heap.delMax()) != null) {
            System.out.print(del + ", ");
        }

    }
}

```
</details>
