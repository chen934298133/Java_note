# BinaryTree

**哈希表最主要的目的就是将一个键和一个值联系起来，哈希表能够将存储的数据元素是一个键和一个值共同组成的键值对数据，我们可以根据键来查找对应的值。**

**哈希表的键具有唯一性。**


## 结点类API设计
<table>
	<tr>
		<th>类名</th>
		<th>TreeNode<Key,Value></th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>Node(Key key, Value value, Node left, Node right)：创建Node对象</td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>
1. public Node left:记录左子结点<br>
2. public Node right:记录右子结点<br>
3. public Key key:存储键<br>
4. public Value value:存储值<br></td>
	</tr>
</table>

## 二叉查找树API设计
<table>
	<tr>
		<th>类名</th>
		<th>BinaryTree<Key,Value></th>
	</tr>
	<tr>	
		<td>构造方法</td>
		<td>BinaryTree()：BinaryTree</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>
1. private Node root:记录根结点<br>
2. private int N:记录树中元素的个数<br></td>
	</tr>
  <tr>
		<td>成员变量</td>
		<td>
      1. public void put(Key key,Value value):向树中插入一个键值对<br>
      2.private Node put(Node x, Key key, Value val)：给指定树x上，添加键一个键值对，并返回添加后的新树<br>
      3.public Value get(Key key):根据key，从树中找出对应的值<br>
      4.private Value get(Node x, Key key):从指定的树x中，找出key对应的值<br>
      5.public void delete(Key key):根据key，删除树中对应的键值对<br>
      6.private Node delete(Node x, Key key):删除指定树x上的键为key的键值对，并返回删除后的新树<br>
      7.public int size():获取树中元素的个数<br></td>
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
<summary>&#127808; BinaryTree代码实现 &#127808;</summary>

```java
package algorithm.Tree;

public class BinaryTree<Key extends Comparable<Key>, Value> {

    private TreeNode root;
    private int N;

    public int size(){
        return N;
    }

    //向树中添加元素key-value
    public void put(Key key, Value value){
        root = put(root, key, value);
    }

    // 向指定的树x中添加key-value,并返回添加元素后新的树
    public TreeNode put(TreeNode x, Key key, Value value){
        // 递归出口
        // 如果树为空(或者依次寻找到叶子结点后，未找到也认作为空)
        // 则将此结点作为根节点使用
        if (x == null){
            N++;
            return new TreeNode(key, value, null, null);
        }

        // 如果x子树不为空,比较x结点的键和key的大小：
        int cmp = key.compareTo((Key) x.getKey());
        if (cmp > 0){
            // 如果key大于x结点的键，则继续找x结点的右子树
            x.right = put(x.right, key, value);
        }else if (cmp < 0){
            // 如果key小于x结点的键，则继续递归递归找x结点的左子树
            x.left = put(x.left,key,value);
        }else{
            // 如果key等于x结点的键，替换x结点的值为value即可
            x.setValue(value);
        }
        return x;
    }

    //查询树中指定key对应的value
    public Value get(Key key){
        return get(root, key);
    }

    //从指定的树x中，查找key对应的值
    public Value get(TreeNode x, Key key){
        // 与 put 思想相同
        if (x == null){
            return null;
        }

        int cmp = key.compareTo((Key) x.getKey());
        if (cmp > 0) {
            return get(x.right, key);
        }else if (cmp < 0){
            return get(x.left, key);
        }else {
            return (Value) x.getValue();
        }
    }

    //删除树中key对应的value
    public TreeNode delete(Key key){
        return delete(root, key);
    }

    //删除指定树x中的key对应的value，并返回删除后的新树
    // 删除方法有问题，等后面有空仔细改一下
    public TreeNode delete(TreeNode x, Key key){
        if (x == null){
            return null;
        }

        int cmp = key.compareTo((Key) x.getKey());
        if (cmp > 0){
            // 继续从下一个 x.right 寻找
            x.right = delete(x.right, key);
        }else if (cmp < 0){
            x.left = delete(x.left, key);
        }else {
            // 二叉树经典删除操作
            // 将需要删除的结点替换为左子树中最大的或者右子树中最小的

            N--;
            // 找右子树最小结点

            // 情形一: 右子树为空，返回左子树
            if (x.right==null){
                return x.left;
            }

            // 情形二： 左子树为空，返回右子树
            if (x.left==null){
                return x.right;
            }

            // 情形三： 左右子树都不为空
            // 就是在此处也可以实现寻找左子树中最大的结点，可以替换
            TreeNode minNode = x.right;     // 首先获取要删除节点的右子树的第一个结点
            while(minNode.left != null){
                // 循环找到右子树钟最小的结点
                minNode = minNode.left;
            }

            // 循环遍历删除最小的结点
            TreeNode node = x.right;
            while (node.left != null){
                if (node.left.left == null){
                    // 删除操作
                    node.left = node;
                }else {
                    node = node.left;
                }
            }

            // 更新刚刚找到的右子树的最小结点的左右子树 为 原来要删除的结点的左右子树

            minNode.left = x.left;
            minNode.right = x.right;
            // 原本父节点就是指向x的，所以直接更新x的信息即可
            x = minNode;
        }

        return x;
    }

    //查找整个树中最小的键
    public Key min(){
        return (Key) min(root).getKey();
    }

    //在指定树x中找出最小键所在的结点
    private TreeNode min(TreeNode x){

        //需要判断x还有没有左子结点，如果有，则继续向左找，如果没有，则x就是最小键所在的结点
        if (x.left!=null){
            return min(x.left);
        }else{
            return x;
        }
    }

    //在整个树中找到最大的键
    public Key max(){
        return (Key) max(root).getKey();
    }

    //在指定的树x中，找到最大的键所在的结点
    public TreeNode max(TreeNode x){
        //判断x还有没有右子结点，如果有，则继续向右查找，如果没有，则x就是最大键所在的结点
        if (x.right!=null){
            return max(x.right);
        }else{
            return x;
        }
    }
}

```
</details>
  
<details>
<summary>&#127808; 二叉树基础遍历 &#127808;</summary>

```java

```
</details>
  
<details>
<summary>&#127808; 二叉树层序遍历 &#127808;</summary>

```java

```
</details>
  
<details>
<summary>&#127808; 最大深度问题 &#127808;</summary>

```java

```
</details>
  

<details>
<summary>&#127808; View The Detail &#127808;</summary>

```java

```
</details>
