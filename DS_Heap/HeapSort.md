# &#127800; 堆排序 &#127800;

#### &#127800; 1.1 例子：
**给定一个数组： String[] arr = {"S","O","R","T","E","X","A","M","P","L","E"}**<br>
**请对数组中的字符按从小到大排序。**

#####  实现步骤
- **建堆**<br>
  1. 构造堆；<br>
- **调整堆**<br>
  2. 得到**堆顶元素**，这个值就是**最大值**；<br>
  3. 交换 **堆顶元素** 和 **数组中的最后一个元素** ，此时所有元素中的最大元素已经放到合适的位置；<br>
  4. 对堆进行**调整**，重新让除了最后一个元素的剩余元素中的**最大值放到堆顶**；<br>
- 重复2~4这个步骤，直到堆中剩一个元素为止。<br>

## &#127800; 1.2 API设计：
<table>
	<tr>
		<th>类名</th>
		<th>HeapSort</th>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>
      1.public static  void sort(Comparable[] source)：对source数组中的数据从小到大排序<br>
      2.private static void createHeap(Comparable[] source, Comparable[] heap):根据原数组source，构造出堆heap<br>
      3.private static  boolean less(Comparable[] heap, int i, int j)：判断heap堆中索引i处的元素是否小于索引j处的元素<br>
      4.private static  void exch(Comparable[] heap, int i, int j):交换heap堆中i索引和j索引处的值<br>
      5.private static void sink(Comparable[] heap, int target, int range):在heap堆中，对target处的元素做下沉，范围是0~range。<br>
    </td>
	</tr>
</table>

## &#127800; 1.3 建堆过程
- 堆的构造，最直观的想法就是另外再创建一个和新数组数组
- 然后从左往右遍历原数组，每得到一个元素后，添加到新数组中
- 通过上浮，对堆进行调整，最后新的数组就是一个堆。
- 上述的方式虽然很直观，也很简单，但是我们可以用更聪明一点的办法完成它。
  - 创建一个新数组，把原数组 `0~length-1` 的数据拷贝到新数组的 `1~length` 处，
  - 再从新数组长度的 **一半** 处开始往 **1索引** 处扫描（从右往左），
  - 然后对扫描到的每一个元素做下沉调整即可。
  
<details>
<summary>&#127808; 建堆过程图解 &#127808;</summary>
  
![](http://lc-dDwI9S44.cn-n1.lcfile.com/d82d608598187a3ddd00.png/%E5%A0%86%E6%8E%92%E5%BA%8F1.png)
![构造完毕，堆有序](http://lc-dDwI9S44.cn-n1.lcfile.com/dd5c104fda2a2b4ec2b2.png/%E5%A0%86%E6%8E%92%E5%BA%8F2.png)
</details>


## &#127800; 1.4 堆调整

- 对构造好的堆，我们只需要做类似于堆的删除操作，就可以完成排序。
  1.将堆顶元素和堆中最后一个元素交换位置；
  2.通过对堆顶元素下沉调整堆，把最大的元素放到堆顶(此时最后一个元素不参与堆的调整，因为最大的数据已经到了数组的最右边)
  3.重复1~2步骤，直到堆中剩最后一个元素。
    
<details>
<summary>&#127808; 建堆过程图解 &#127808;</summary>
  
![](http://lc-dDwI9S44.cn-n1.lcfile.com/1c090e0baf0da1d445c8.png/%E8%B0%83%E6%95%B4%E5%A0%861.png)
![](http://lc-dDwI9S44.cn-n1.lcfile.com/152d7331b682e8f5e92c.png/%E8%B0%83%E6%95%B4%E5%A0%862.png)
![](http://lc-dDwI9S44.cn-n1.lcfile.com/72e81f9e8943c5347bc3.png/%E8%B0%83%E6%95%B4%E5%A0%863.png)
![](http://lc-dDwI9S44.cn-n1.lcfile.com/3493b0d586e4600ab3d4.png/%E8%B0%83%E6%95%B4%E5%A0%864.png)

</details>

## &#127800; 1.5 代码实现

<details>
<summary>&#127808; 堆排序代码 &#127808;</summary>

```java
package DS.Heap;

public class HeapSort {

    //判断heap堆中索引i处的元素是否小于索引j处的元素
    private static boolean less(Comparable[] heap, int i, int j) {
        return heap[i].compareTo(heap[j]) < 0;
    }

    //交换heap堆中i索引和j索引处的值
    private static void exch(Comparable[] heap, int i, int j) {
        Comparable tmp = heap[i];
        heap[i] = heap[j];
        heap[j] = tmp;
    }


    //根据原数组source，构造出堆heap
    private static void createHeap(Comparable[] source, Comparable[] heap) {
        //把source中的元素拷贝到heap中，heap中的元素就形成一个无序的堆
        System.arraycopy(source, 0, heap, 1, source.length);

        //对堆中的元素做下沉调整(从长度的一半处开始，往索引1处扫描)
        for (int i = (heap.length) / 2; i > 0; i--) {
            sink(heap, i, heap.length - 1);
        }

    }


    //对source数组中的数据从小到大排序
    public static void sort(Comparable[] source) {
        //构建堆
        Comparable[] heap = new Comparable[source.length + 1];
        createHeap(source, heap);
        //定义一个变量，记录未排序的元素中最大的索引
        int N = heap.length - 1;
        //通过循环，交换1索引处的元素和排序的元素中最大的索引处的元素
        while (N != 1) {
            //交换元素
            exch(heap, 1, N);
            //排序交换后最大元素所在的索引，让它不要参与堆的下沉调整
            N--;
            //需要对索引1处的元素进行对的下沉调整
            sink(heap, 1, N);
        }

        //把heap中的数据复制到原数组source中
        System.arraycopy(heap, 1, source, 0, source.length);

    }


    //在heap堆中，对target处的元素做下沉，范围是0~range
    private static void sink(Comparable[] heap, int target, int range) {

        while (2 * target <= range) {
            //1.找出当前结点的较大的子结点
            int max;
            if (2 * target + 1 <= range) {
                if (less(heap, 2 * target, 2 * target + 1)) {
                    max = 2 * target + 1;
                } else {
                    max = 2 * target;
                }
            } else {
                max = 2 * target;
            }

            //2.比较当前结点的值和较大子结点的值
            if (!less(heap, target, max)) {
                break;
            }

            exch(heap, target, max);

            target = max;
        }
    }
}
```
</details>
