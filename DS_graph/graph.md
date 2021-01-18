# 一、 &#127800; 无向图 &#127800;
## 1.1 &#127800; 图的存储结构
- 首先图由两部分组成
	- 图所有的顶点
	- 图所有连接顶点的边

### 1.1.1 &#127800; 邻接矩阵
1. 使用一个V * V的二维数组int[V][V] adj, **把索引的值看做是顶点**
2. 如果**顶点v和顶点w相连**，我们只需要**将adj[v][w]和adj[w][v]的值设置为1**,否则设置为0即可。
3. &#10060; 缺点：邻接矩阵这种存储方式的空间复杂度是V^2的，如果我们处理的问题规模比较大的话，内存空间极有可能不够用。

![邻接矩阵](http://lc-dDwI9S44.cn-n1.lcfile.com/fec1c2f9ddeb80092e9c.png/%E9%82%BB%E6%8E%A5%E7%9F%A9%E9%98%B5.png)

### 1.1.2 &#127800; 邻接表
1. 使用一个大小为V的数组 Queue[V] adj，把索引看做是顶点
2. 每个索引处adj[v]存储了一个队列，该队列中存储的是所有与该顶点相邻的其他顶点
3. 邻接表的空间并不是是线性级别的，所以后面我们一直采用邻接表这种存储形式来表示图。

![邻接表](.../image/邻接表.png)
![](http://lc-dDwI9S44.cn-n1.lcfile.com/574b0a1778dee23f12e0.png/%E9%82%BB%E6%8E%A5%E8%A1%A8.png)

## 1.2 &#127800; 图的实现
### 1.2.1 &#127800; 图的API设计

<table>
	<tr>
		<th>类名</th>
		<th>graph</th>
	</tr>
	<tr>
		<td>构造方法</td>
		<td>Graph(int V)：创建一个包含V个顶点但不包含边的图</td>
	</tr>
	<tr>
		<td>成员方法</td>
		<td>
			1. public int V(): 获取图中顶点的数量 <br>
			2. public int E(): 获取图中边的数量 <br>
			3. public void addEdge(int v,int w): 向图中添加一条边 v-w <br>
			4. public Queue adj(int v)：获取和顶点v相邻的所有顶点 <br>
		</td>
	</tr>
	<tr>
		<td>成员变量</td>
		<td>
			1. private final int V:  记录顶点数
			2. private int E: 记录边数量
			3. private Queue[] adj: 邻接表
		</td>
	</tr>
</table>

<details>
<summary>&#127808; View The Codes &#127808;</summary>

```java

```
</details>










