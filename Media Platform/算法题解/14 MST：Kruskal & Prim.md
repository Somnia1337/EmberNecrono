## 1135. 最低成本连通所有城市

#### 前言

「力扣题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 14 篇题解，更多题解关注 Somnia1337@力扣。

#### 题目描述

> 难度：🟡中等
>
> 标签：\#并查集 \#图 \#最小生成树 \#堆

想象你是城市基建规划者，地图上有 `n` 座城市，它们的编号为 `1` 到 `n`。

给你整数 `n` 和一个数组 `conections`，其中 `connections[i] = [xi,yi,costi]` 表示将城市 `xi` 和城市 `yi` 连接的花费为 `costi`（**连接是双向的**）。

返回连接所有城市的 **最低成本**，每对城市之间 **至少** 有一条路径。如果无法连接所有 `n` 个城市，返回 `-1`。

该 **最小成本** 应该是所用全部连接成本的总和。

![[@2024.01.05-题目描述.png]]

```text
输入: n=3, conections=[[1,2,5],[1,3,6],[2,3,1]]
输出: 6
解释: 选出任意 2 条边都可以连接所有城市, 我们从中选取成本最小的 2 条
```

#### 解题思路

这是一个求 MST（Minimum Spanning Tree，最小生成树）的裸题，两个经典算法是 *Kruskal 算法* 和 *Prim 算法*。

简单的 $n$ 阶连通图的 MST 包含 $n - 1$ 条边。

##### Kruskal 算法

「数据结构与算法分析」P407 / 「离散数学」P150

思想：按权从小到大加入边（贪心）。

过程：

1. 将所有边按权重从小到大排列。
2. 按序遍历这些边，对每条边 $e$ 进行判断，如果在图中加入 $e$ 不会生成环，则加入 $e$，否则跳过 $e$。
3. 重复 2，直到选出 $n - 1$ 条边。

过程中，判环的方法是使用并查集维护结点之间的连通性，如果两个结点 $u$，$v$ 已经连通，则加入边 $(u, v)$ 将导致环。

并查集的使用 👇

[并查集：“图”与“等价类”是如何结合的](https://mp.weixin.qq.com/s/Wv-5jt9t1Zo1CZhTiX9R7g)

```java
class Solution {
    public int minimumCost(int n, int[][] connections) {
        // 按权排序
        Arrays.sort(connections, Comparator.comparingInt(a -> a[2]));
        // 每个结点所属支的根 (用于并查集)
        int[] root = new int[n + 1];
        for (int i = 1; i <= n; i++) root[i] = i;
        int cnt = 0, ans = 0; // cnt: 已选择边数
        for (int[] c : connections) {
            // 会成环, 跳过
            if (find(root, c[0]) == find(root, c[1])) continue;
            union(root, c[0], c[1]); // 合并
            ans += c[2];
            if (++cnt == n - 1) break; // 选够了
        }
        return cnt == n - 1 ? ans : -1; // 没选够, 返回 -1
    }
	
    // 并查集
    private int find(int[] root, int i) {
        while (root[i] != i) {
            root[i] = root[root[i]];
            i = root[i];
        }
        return i;
    }
	
    private void union(int[] root, int x, int y) {
        root[find(root, x)] = find(root, y);
    }
}
```

##### Prim 算法

「数据结构与算法分析」P404

思想：从一个结点开始，每次加入离已连通部分最近的点，并更新剩余点的最短距离。

过程：

1. 选择一个起始结点，将其标记为已访问。
2. 用一个最小堆维护已连通部分到未访问结点的最短距离，将起始结点的所有邻边入堆。
3. 将权最小的边出堆（该边的两个端点分别属于已连通和未访问集合）。
4. 加入该未访问结点，并将其所有邻边入堆。
5. 重复 3、4，直到选出 $n - 1$ 条边。

```java
class Solution {
    public int minimumCost(int n, int[][] connections) {
        // 邻接链表, 存储 {相邻结点, 距离}
        List<Integer[]>[] dist = new List[n + 1];
        Arrays.setAll(dist, e -> new ArrayList<>());
        for (int[] c : connections) {
            dist[c[0]].add(new Integer[]{c[1], c[2]});
            dist[c[1]].add(new Integer[]{c[0], c[2]});
        }
		
        // 记录每个结点是否已经选择
        boolean[] vis = new boolean[n + 1];
        Queue<Integer[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        pq.add(new Integer[]{1, 0});
		
        int cnt = 0, ans = 0; // cnt: 已选择结点数
        while (!pq.isEmpty() && cnt < n) {
            Integer[] p = pq.poll();
            if (!vis[p[0]]) {
                vis[p[0]] = true;
                cnt++;
                ans += p[1];
                pq.addAll(dist[p[0]]); // 更新最短距离
            }
        }
        return cnt == n ? ans : -1;
    }
}
```

#### 相关题目

##### 1584. 连接所有点的最小费用

\#并查集 \#数组 \#图 \#最小生成树

#### 最后

更多系列「外源推文」&「生活分享」关注公众号：