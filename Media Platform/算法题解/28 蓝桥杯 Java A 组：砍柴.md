### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 28 篇题解，更多题解关注 Somnia1337@力扣。

### F. 砍柴

#### 题目描述

A、B 两人轮流砍柴，轮到某一方砍柴时，设原木剩余长度为 `x`：

- 如果 `x` 为 $0$ 或 $1$，本方无法继续砍柴，从而输掉比赛。
- 如果 `x` $\geqslant 2$，本方可选择一个长度 $2 \leqslant$ `p` $\leqslant$ `x`，砍掉长为 `p` 的一段。

共 `n` 段原木（`n` 局游戏），每局 A 先手，双方都采用最佳策略。

对每局游戏，如果 A 能够取胜，输出 `1`，否则输出 `0`。

#### 输入输出示例

输入共 `n + 1` 行，第 1 行含一个正整数 `n`，随后 `n` 行各含一个正整数 `x`：

```text
3
1
2
6
```

范围：

- $1 \leqslant$ `n` $\leqslant 10^4$
- $1 \leqslant$ `x` $\leqslant 10^5$

输出 `n` 行，每行含一个整数 `0` 或 `1`：

```text
0
1
1
```

解释：

- 对 `x` $= 1$，A 已无法砍柴，A 失败。
- 对 `x` $= 2$，A 砍掉 `p` $= 2$，轮到 B 时 `x` $= 0$，已无法砍柴，A 获胜。
- 对 `x` $= 6$，A 砍掉 `p` $= 5$，轮到 B 时 `x` $= 1$，已无法砍柴，A 获胜。

#### 解题思路

典型的博弈问题，预处理范围内的质数集，`boolean win(int x)` 用 DFS 判断当前剩余长度能否获胜，递归交换对手。

剪枝：对 `win(x)`，二分质数集查询 $\leqslant$ `x` 的最大位置，倒序遍历，更快接近 DFS 基态，否则 $10^5$ 容易栈溢出。

#### 答案

本答案已经在 dotcpp 取得 AC：

![[Snipaste_240417_093119.png|500]]

```java
import java.util.*;
import java.io.*;

public class Main {
    private static int[] primes;
    private static int[] memo;
    
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        int n = Integer.parseInt(in.readLine());
        int[] arr = new int[n];
        
        // 取输入的最大值
        int max = 0;
        for (int i = 0; i < n; i++) {
            arr[i] = Integer.parseInt(in.readLine());
            max = Math.max(arr[i], max);
        }
        
        // 预处理质数
        getPrimes(max);
        memo = new int[max + 1];
        for (int x : arr) {
            out.println(win(x) ? 1 : 0);
        }
        
        out.flush();
        out.close();
    }
    
    // 筛法预处理得到范围内所有质数
    private static void getPrimes(int max) {
        boolean[] v = new boolean[max + 1];
        Arrays.fill(v, true);
        for (int x = 2; x <= max; x++) {
            if (!v[x]) continue;
            for (int y = x * 2; y <= max; y += x) {
                v[y] = false;
            }
        }
        int n = 0;
        for (int x = 2; x <= max; x++) {
            if (v[x]) n++;
        }
        primes = new int[n];
        int i = 0;
        for (int x = 2; x <= max; x++) {
            if (v[x]) primes[i++] = x;
        }
    }
    
    // DFS 博弈
    private static boolean win(int x) {
        if (memo[x] != 0) return memo[x] == 1;
        if (x <= 1) return false;
        // 剪枝: 倒序遍历质数集, 更快接近 DFS 基态
        for (int i = bis(x); i >= 0; i--) {
            if (!win(x - primes[i])) {
                memo[x] = 1;
                return true;
            }
        }
        memo[x] = -1;
        return false;
    }
    
    // 二分, 从质数集中查找 <= x 的最大位置
    private static int bis(int x) {
        int l = 0;
        int r = primes.length - 1;
        while (l <= r) {
            int m = l + r >> 1;
            if (primes[m] <= x) l++;
            else r--;
        }
        return r;
    }
}
```