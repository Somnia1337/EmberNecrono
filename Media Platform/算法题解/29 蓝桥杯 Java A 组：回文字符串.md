### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 29 篇题解，更多题解关注 Somnia1337@力扣。

### G. 回文字符串

#### 题目描述

输入 `n` 个字符串，对每个串，如果可以通过在字符串 **首部** 添加任意数量的 `'l'` / `'q'` / `'b'` 使其变为回文串，输出 `"Yes"`，否则输出 `"No"`。

#### 输入输出示例

输入共 `n + 1` 行，第 1 行含一个正整数 `n`，随后 `n` 行各含一个字符串 `s`：

```text
3
gmgqlq
pdlbll
aaa
```

范围：

- $1 \leqslant$ `n` $\leqslant 10$
- $1 \leqslant$ `sum(s.length())` $\leqslant 10^6$

输出

```text
Yes  
No  
Yes
```

解释：

- `"gmgqlq"` 变为回文串 `"qlqgmgqlq"`
- `"pdlbll"` 无法变为回文串
- `"aaa"` 已经是回文串

#### 解题思路

对于最终的回文串，在首部添加字符 $\Leftrightarrow$ 抵消尾部的字符。

对每个串，当其以 `'l'` / `'q'` / `'b'` 结尾时（能被首部添加的字符抵消），判断其是否回文：

- 是，输出 `"Yes"` 并结束计算
- 否，左移尾指针，重复以上步骤，直到结尾字符不再能被抵消，额外判断一次

#### 答案

截至发文，dotcpp 上还未开放作答本题目。

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        
        int n = Integer.parseInt(in.readLine());
        while (n-- > 0) {
            char[] s = in.readLine().toCharArray();
            int ed = s.length - 1;
            boolean v = false;
            // 持续抵消尾部字符, 并判断回文
            while (s[ed] == 'l' || s[ed] == 'q' || s[ed] == 'b') {
                if (isPal(s, ed)) {
                    v = true;
                    break;
                }
                ed--;
            }
            // 最后判断一次
            out.println(v || isPal(s, ed) ? "Yes" : "No");
        }
        
        out.flush();
        out.close();
    }
    
    private static boolean isPal(char[] s, int ed) {
        for (int i = 0, j = ed; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
}
```