### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 24 篇题解，更多题解关注 Somnia1337@力扣。

在上周六，终于参加了期盼已久的蓝桥杯（Java A 组），但感觉题目略水，有点失望😒

Anyway，本周每天都更新一道题（两道填空放在一起）。

### A. 拼正方形

#### 题目描述

有 `7385137888721` 个 $2 \times 2$ 的方块和 `10470245` 个 $1 \times 1$ 的方块，求能拼出的正方形的最大边长。

#### 解题思路

用 $1 \times 1$ 的方块给已经用 $2 \times 2$ 的方块拼成的大正方形“镶边”，每次使其边长加 $1$。

```java
public class Main {
    public static void main(String[] args) {
        final long SQ2 = 7385137888721L;
        final long SQ1 = 10470245L;
        
        // 用 2x2 的方块拼成一个大正方形
        long ans = (long) Math.sqrt(SQ2) * 2;
        
        // 用 1x1 的方块镶边
        long sq1 = SQ1;
        while (true) {
            if ((sq1 -= ans * 2 + 1) < 0) break;
            ans++;
        }
        
        System.out.println(ans);
    }
}
```

输出：

```text
5435122
```

#### 答案

`5435122`

### B. 召唤数学精灵

#### 题目描述

计算 $i$ 的数量，满足：

- `1` $\leq i \leq$ `2024041331404202`
- $A(i) - B(i) \space {\rm mod} \space 100 = 0$

其中，$A(x) = {\rm sum}(1,\space x)$，$B(x) = x!$。

#### 解题思路

观察到 $\forall i \geq 100$，$B(i) \space {\rm mod} \space 100 = 0$（赛后发现 $\forall i \geq 10$ 即可），那么算式 $A(i) - B(i) \space {\rm mod} \space 100 = 0$ 就化简为 $A(i) \space {\rm mod} \space 100 = 0$。

考虑 ${\rm sum}(1,\space 200) = 20100$，$20100 \space {\rm mod} \space 100 = 0$，可以以 $200$ 为一个周期，计算一个周期内 $A(i) \space {\rm mod} \space 100 = 0$ 的次数，与周期数相乘。

将 `1` ~ `2024041331404202` 拆成 `1` ~ `200` 和 `201` ~ `2024041331404202` 两部分：

- 第一部分的特殊之处在于 $i = 1$ 和 $i = 3$，它们都满足要求，而之后的例如 $i = 201$ 和 $i = 203$ 都不满足要求，因此额外加 $2$。
- 第二部分以每 $200$ 一个周期，算得周期内有 $4$ 个 $i$ 满足要求（代码略），乘以总周期数。

```java
public class Main {
    public static void main(String[] args) {
        final long ed = 2024041331404202L;
        
        // 分别计算两部分答案
        long ans1 = 4 + 2;
        long ans2 = (ed - 200) / 200 * 4;
        
        System.out.println(ans1 + ans2);
    }
}
```

输出：

```text
40480826628086
```

#### 答案

`40480826628086`