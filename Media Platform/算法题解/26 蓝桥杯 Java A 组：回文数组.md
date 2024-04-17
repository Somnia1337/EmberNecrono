### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 26 篇题解，更多题解关注 Somnia1337@力扣。

### D. 回文数组

#### 题目描述

输入一个长为 `n` 的数组，输出使其变得回文的最少操作次数。

在 1 次操作中，可以二选一：

- 选择一个元素，对其增减 $1$
- 选择相邻的两个元素，对其增减 $1$

#### 输入输出示例

输入共 2 行，第 1 行含一个正整数 `n`，第 2 行含 `n` 个正整数 `x`：

```text
4
1 2 3 4
```

范围：

- $1 \leqslant$ `n` $\leqslant 1 \times 10^5$
- $-1 \times 10^6 \leqslant$ `x` $\leqslant 1 \times 10^6$

输出一个整数表示答案：

```text
3
```

解释：

- 第 1 次操作后：`[2, 3, 3, 4]`
- 第 2 次操作后：`[3, 3, 3, 4]`
- 第 3 次操作后：`[4, 3, 3, 4]`，已经是回文

#### 解题思路

双指针 `i`、`j` 从两端向中间看，每次先用“操作 1”使 `arr[i] == arr[j]`，然后看 `arr[i+1]` 和 `arr[j-1]` 是否与它们同号，如果同号，可以将一部分操作改为“操作 2”，从而节约操作次数。

实现时，可以先一次遍历，更新 `arr[i] -= arr[j]`，再次遍历时仅比较 `arr[i]` 和 `arr[i+1]` 是否同号即可。

#### 答案

本答案已经在 dotcpp 取得 AC：

![[Snipaste_240416_184649.png|500]]

```java
import java.util.*;
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        int n = Integer.parseInt(in.readLine());
        int[] arr = new int[n];
        StringTokenizer st = new StringTokenizer(in.readLine());
        for (int i = 0; i < n; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }
        
        // 预处理
        for (int i = 0, j = n - 1; i < j; i++, j--) {
            arr[i] -= arr[j];
        }
        
        // 遍历前一半
        long ans = 0;
        for (int i = 0; i < n / 2; i++) {
            if (arr[i] == 0) continue;
            // 先用 "操作 1" 变 arr[i]
            ans += Math.abs(arr[i]);
            // 再看能否顺便变一下 arr[i+1]
            if (i + 1 < n / 2 && arr[i] > 0 == arr[i + 1] > 0) {
                arr[i + 1] = Math.abs(arr[i]) > Math.abs(arr[i + 1]) ? 0 : arr[i + 1] - arr[i];
            }
        }
        out.println(ans);
        
        out.flush();
        out.close();
    }
}
```