### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 30 篇题解，更多题解关注 Somnia1337@力扣。

终于来到了本次蓝桥杯 8 题的最后一题🙌

### H. 智力测试

#### 题目描述

一个 $n \times m$ 的棋盘，每行、每列都有一个正整数权值（分别记为数组 `row` 和 `col`），每次可从当前方格 `(x1, y1)` 跳到任意方格 `(x2, y2)`，但需满足：

- `x1 == x2 && y1 != y2 && col[y2] > col[y1]` 或 `x1 != x2 && y1 == y2 && row[x2] > row[x1]`（即只能在同行 / 同列跳动，且目标方格的对应权值大于起点方格）
- 对如上 2 种情况，分别不存在 `y3` 或 `x3`，使得 `col[y2] > col[y3] > col[y1]` 或 `row[x2] > row[x3] > row[x1]`（即不能跳过中间权值）

多次询问，每次给定起、止方格，回答合法的路径方案数（没有记为 `0`）对 `1000000007` 取余。

#### 输入输出示例

输入共 `3 + q` 行，第 1 行含三个正整数 `n`、`m`、`q`，第 2 行含 `n` 个正整数表示各行的权值，第 3 行含 `m` 个正整数表示各列的权值，随后 `q` 行各含四个正整数表示 `x1`、`y1`、`x2`、`y2`：

```text
4 4 2
4 2 3 1
2 1 2 1
4 4 1 1
2 2 2 4
```

范围：

- $1 \leqslant$ `n`，`m`，`T` $\leqslant 10^5$
- $1 \leqslant$ `R`，`C` $\leqslant 10^8$
- $1 \leqslant$ `x1`，`x2` $\leqslant$ `n`
- $1 \leqslant$ `y1`，`y2` $\leqslant$ `m`

输出 `q` 行，每行含一个正整数表示对该次询问的回答：

```text
4
0
```

#### 解题思路

x、y 两方向上的移动是独立的，记分别的移动步数为 `mx`、`my`，则最终答案为 $\large \rm C_{mx + my}^{mx}$。

假设列的权值为 `[1, 2, 3, 2, 2, 4, 3]`，

排序后为 `[1, 2, 2, 2, 3, 3, 4]`，

起点 `y == 0`（`col[y1] == 1`），终点 `y == 5`（`col[y2] == 4`），则 `my` 为：

- 从 `1` 走到 `2` 步数为 $1$，走到 `3` 步数为 $2$，走到 `4` 步数为 $3$。
- `2` 有 $2$ 次重复，`3` 有 $1$ 次重复，步数先减去 $2$，再乘以 $2^3 = 8$。

因此 `my == 8`，同理算得 `mx`，求组合数即可。

#### 答案

截至发文，dotcpp 上还未开放作答本题目。

```java
import java.util.*;
import java.io.*;
import java.math.BigInteger;

public class Main {
    private static final int M = 1000000007;
    private static final BigInteger BM = BigInteger.valueOf(M);
    
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        StringTokenizer st = new StringTokenizer(in.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int q = Integer.parseInt(st.nextToken());
        
        int[] row = new int[n];
        st = new StringTokenizer(in.readLine());
        for (int i = 0; i < n; i++) {
            row[i] = Integer.parseInt(st.nextToken());
        }
        int[] sortedRow = new int[n];
        System.arraycopy(row, 0, sortedRow, 0, n);
        Arrays.sort(sortedRow);
        
        int[] col = new int[m];
        st = new StringTokenizer(in.readLine());
        for (int j = 0; j < m; j++) {
            col[j] = Integer.parseInt(st.nextToken());
        }
        int[] sortedCol = new int[m];
        System.arraycopy(col, 0, sortedCol, 0, m);
        Arrays.sort(sortedCol);
        
        while (q-- > 0) {
            st = new StringTokenizer(in.readLine());
            int x1 = Integer.parseInt(st.nextToken()) - 1;
            int y1 = Integer.parseInt(st.nextToken()) - 1;
            int x2 = Integer.parseInt(st.nextToken()) - 1;
            int y2 = Integer.parseInt(st.nextToken()) - 1;
            
            boolean validX = row[x1] < row[x2] || row[x1] == row[x2] && x1 == x2;
            boolean validY = col[y1] < col[y2] || col[y1] == col[y2] && y1 == y2;
            if (!(validX && validY)) {
                out.println(0);
                continue;
            }
            
            int movX = moves(sortedRow, row[x1], row[x2]);
            int movY = moves(sortedCol, col[y1], col[y2]);
            out.println(comb(movX + movY, Math.min(movX, movY)));
        }
        
        out.flush();
        out.close();
    }
    
    private static int moves(int[] sortedA, int src, int dest) {
        long mov = 0;
        int pre = -1;
        int streak = 0;
        boolean minus = false;
        for (int p : sortedA) {
            if (p <= src) continue;
            if (p == pre) {
                if (!minus) {
                    mov--;
                    minus = true;
                }
                streak++;
            }
            else {
                minus = false;
                pre = p;
                mov++;
            }
            if (p == dest) break;
        }
        
        for (int k = 0; k < streak; k++) {
            mov = (mov << 1) % M;
        }
        return (int) mov;
    }
    
    private static BigInteger comb(int from, int choice) {
        BigInteger r = BigInteger.valueOf(1);
        
        for (int x = from; x > from - choice; x--) {
            r = r.multiply(BigInteger.valueOf(x));
        }
        for (int x = choice; x >= 2; x--) {
            r = r.divide(BigInteger.valueOf(x));
        }
        
        r = r.mod(BM);
        return r;
    }
}
```