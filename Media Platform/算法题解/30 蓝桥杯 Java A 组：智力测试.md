### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 30 篇题解，更多题解关注 Somnia1337@力扣。

终于来到了本次蓝桥杯 8 题的最后一题🙌

### H. 智力测试

#### 题目描述



#### 输入输出示例

输入

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

输出

```text
4
0
```

解释：

#### 解题思路



#### 答案

本答案已经在 dotcpp 取得 AC：



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