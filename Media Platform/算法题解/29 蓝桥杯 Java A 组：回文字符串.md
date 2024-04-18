### 前言

「算法题解」系列致力于分享有价值的题目、探讨更优秀的解法。

这是本系列的第 29 篇题解，更多题解关注 Somnia1337@力扣。

### G. 回文字符串

#### 题目描述



#### 输入输出示例

输入

```text

```

范围：

- 

输出

```text

```

解释：

#### 解题思路



#### 答案

本答案已经在 dotcpp 取得 AC：



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
            while (s[ed] == 'l' || s[ed] == 'q' || s[ed] == 'b') {
                if (s[0] == s[ed] && pal(s, ed)) {
                    v = true;
                    break;
                }
                ed--;
            }
            out.println(v || pal(s, ed) ? "YES" : "NO");
        }
        
        out.flush();
        out.close();
    }
    
    private static boolean pal(char[] s, int ed) {
        for (int i = 0, j = ed; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
}
```