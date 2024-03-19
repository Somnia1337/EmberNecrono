## D. Gargari and Permutations

#### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 18 篇题解，更多题解关注 Somnia1337@力扣。

#### 题目描述

本题目的在线练习：

```text
https://codeforces.com/contest/463/problem/D
```

「输入」

`n`、`k` 和 `k` 个 `[1:n]` 的排列。

`1 <= n <= 1000`，`2 <= k <= 5`。

「输出」

这 `k` 个排列的最长公共子序列(LCS)的长度。

「样例」

输入：

```text
4 3
1 4 2 3
4 1 2 3
1 2 4 3
```

输出：

```text
3
```

解释：LCS 为 `[1,2,3]`，输出其长度 `3`。

#### 解题思路

以最后一个排列（记作 `p`）为基准求 LCS。

`dp[i]` 表示以 `p[i]` 结尾的子序列的子问题答案，对每个 `i`，枚举 `dp[j]`（`j < i`）向 `dp[i]` 转移，其中 `j` 需满足在每个排列中 `p[j]` 都出现在 `p[i]` 的左侧，转移方程 `dp[i] = max(dp[j]) + 1`。

输出 `max(dp[i])`。

#### 复杂度

- 时间复杂度：$O(N^2)$
- 空间复杂度：$O(N)$

#### 代码

##### Java

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String... args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
		
        // 输入
        StringTokenizer st = new StringTokenizer(in.readLine());
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());
        int[] p = new int[n];
        int[][] index = new int[k][n + 1];
        for (int i = 0; i < k; i++) {
            st = new StringTokenizer(in.readLine());
            for (int j = 0; j < n; j++) {
                index[i][p[j] = Integer.parseInt(st.nextToken())] = j;
            }
        }
		
        // 处理
        int[] dp = new int[n];
        int ans = 0;
        for (int i = 0; i < n; i++) {
            m:
            for (int j = 0; j < i; j++) {
                for (int[] idx : index) {
                    if (idx[p[j]] > idx[p[i]]) continue m;
                }
                dp[i] = Math.max(dp[j], dp[i]);
            }
            ans = Math.max(++dp[i], ans);
        }
		
        // 输出
        out.println(ans);
		
        out.flush();
        out.close();
    }
}
```

#### 最后

更多系列「外源推文」&「生活分享」关注公众号：