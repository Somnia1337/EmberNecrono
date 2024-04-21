### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 25 篇题解，更多题解关注 Somnia1337@力扣。

### C. 数字诗意

#### 题目描述

输入 `n` 个数字，输出其中无法被表示为连续多个（至少 2 个）正整数之和的数字个数。

#### 输入输出示例

输入共 2 行，第 1 行含一个正整数 `n`，第 2 行含 `n` 个正整数 `x`：

```text
3
3 6 8
```

范围：

- $1 \leq$ `n` $\leq 2 \times 10^5$
- $1 \leq$ `x` $\leq 1 \times 10^{16}$

输出一个整数表示答案：

```text
1
```

解释：

- `3 = 1 + 2`
- `6 = 1 + 2 + 3`
- `8` 无法表示为连续多个正整数之和

#### 解题思路

手写 / 暴力打一个小表，下划线 `_` 表示无法表出的数字：

```text
 _  _  3  _  5
 6  7  _  9 10
11 12 13 14 15
 _ 17 18 19 20
```

找规律，发现只有 $2^n$ 无法表出。

记 `long` 内最大的 $2^n$ 为 `POW`，对输入的每个数 `x`，`POW % x == 0` 时答案加 1。

#### 答案

```java
import java.util.*;
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        final long POW = 1L << 62;
        int n = Integer.parseInt(in.readLine());
        
        StringTokenizer st = new StringTokenizer(in.readLine());
        int ans = 0;
        while (n-- > 0) {
            if (POW % Long.parseLong(st.nextToken()) == 0) ans++;
        }
        out.println(ans);
        
        out.flush();
        out.close();
    }
}
```