### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 27 篇题解，更多题解关注 Somnia1337@力扣。

### E. 吊坠

#### 题目描述

输入 `n` 个长为 `m` 的字符串，作为图的结点，每两个结点间都有一条带权边，权值为两字符串的最长公共 **子串** 的长度。

注意，本题中的字符串都可以旋转，如 `"abba"` 可以旋转变换为 `"aabb"`，从而 `"abba"` 和 `"acca"` 之间的边权值为 `2`。

选出 **最少** 的边让所有结点连通，且成本 **最大**，输出这个成本。

#### 输入输出示例

输入共 `n + 1` 行，第 1 行含两个正整数 `n`、`m`，随后 `n` 行各含一个长为 `m` 的字符串：

```text
4 4
aabb
abba
acca
abcd
```

范围：

- $1 \leqslant$ `n` $\leqslant 200$
- $1 \leqslant$ `x` $\leqslant 50$

输出一个整数表示答案：

```text
8
```

解释：连接 `<1, 2>`，`<2, 3>`，`<2, 4>`，边权和为 $4 + 2 + 2 = 8$。

#### 解题思路

分 2 大步：

1. 求出所有边的边权，如：

```text
[0, 4, 2, 2]
[0, 0, 2, 2]
[0, 0, 0, 1]
[0, 0, 0, 0]
```

本题中的字符串可旋转，考虑在每个字符串后重复一次自身，如 `"abcd"` 变为 `"abcdabcd"`，这样遍历每个长为 `m` 的窗口就得到了自身的全部旋转结果。

用动态规划，对两个字符串滑一次，求得它们的最长公共子串长度，即边权。

2. 用 *kruskal 算法*——原本求最小生成树的算法——求 **最大生成树**。

所有边入堆，并查集判环，无环时加入边并 `union()`。

#### 答案

本答案已经在 dotcpp 取得 AC：

![[Snipaste_240417_082104.png|500]]

```java
import java.util.*;
import java.io.*;

public class Main {
    private static char[][] arr;
    
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        StringTokenizer st = new StringTokenizer(in.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        
        // 每个字符串重复自己
        arr = new char[n][m * 2];
        for (int i = 0; i < n; i++) {
            char[] s = in.readLine().toCharArray();
            for (int j = 0; j < m; j++) {
                arr[i][j] = arr[i][j + m] = s[j];
            }
        }
        
        // 求所有边权
        int[][] dist = new int[n][n];
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                dist[i][j] = lcs(i, j);
            }
        }
        
        // 所有边入堆
        Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt((int[] a) -> -a[2]));
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                pq.offer(new int[]{i, j, dist[i][j]});
            }
        }
        
        // Kruskal 算法
        int[] root = new int[n];
        Arrays.setAll(root, a -> a);
        int take = 0;
        int ans = 0;
        while (take < n - 1) {
            int[] p = pq.poll();
            if (find(root, p[0]) != find(root, p[1])) {
                union(root, p[0], p[1]);
                take++;
                ans += p[2];
            }
        }
        out.println(ans);
        
        out.flush();
        out.close();
    }
    
    // 求两旋转字符串的最长公共子串长度
    private static int lcs(int x, int y) {
        int m = arr[0].length;
        int max = 0;
        int[][] dp = new int[m + 1][m + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= m; j++) {
                if (arr[x][i - 1] == arr[y][j - 1]) {
                    dp[i][j] = Math.max(dp[i - 1][j - 1] + 1, dp[i][j]);
                    max = Math.max(dp[i][j], max);
                }
            }
        }
        return Math.min(max, m / 2);
    }
    
    // 并查集
    private static int find(int[] root, int i) {
        return root[i] == i ? i : (root[i] = find(root, root[i]));
    }
    
    // 并查集
    private static void union(int[] root, int x, int y) {
        root[find(root, x)] = find(root, y);
    }
}
```