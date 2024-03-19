#### 划线游戏

双人轮流划线的游戏：

```text
    | | |
  | | | | |
| | | | | | |
```

假设我方先手，记忆化搜索所有状态，找到能保证胜利的第一步划法。

输入：2 行，第 1 行输入 `n` 表示游戏的行数，第 2 行输入 `n` 个值表示各行的线条数。

输出：所有必胜方法，每行输出 `行号 划线数`（`行号` 从 `1` 开始）。如果不存在，输出 `-1`。

```text
3
3 5 7
```

```text
1 1
2 1
3 1
```

```java
class EnsureWinning {
    private int[] rows;
    private Map<String, Boolean> memo;
	
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(System.out);
        int n = Integer.parseInt(in.readLine());
        int[] arr = new int[n];
        StringTokenizer st = new StringTokenizer(in.readLine());
        for (int i = 0; i < n; i++) arr[i] = Integer.parseInt(st.nextToken());
        EnsureWinning ensureWinning = new EnsureWinning();
        List<int[]> res = ensureWinning.win(arr);
        if (res.isEmpty()) out.println(-1);
        else {
            for (int[] pair : res) {
                out.println((pair[0] + 1) + " " + pair[1]);
            }
        }
        out.flush();
        out.close();
    }
	
    private List<int[]> win(int[] rows) {
        this.rows = rows;
        memo = new HashMap<>();
        List<int[]> ans = new ArrayList<>();
        // 尝试在每行划线
        for (int i = 0; i < rows.length; i++) {
            // 尝试划掉所有条数
            for (int del = rows[i]; del >= 1; del--) {
                rows[i] -= del;
                // 必胜, 加入解集
                if (!turn()) ans.add(new int[]{i, del});
                rows[i] += del;
            }
        }
        return ans;
    }
	
    private boolean turn() {
        String key = hash();
        if (memo.containsKey(key)) return memo.get(key);
        int sum = 0;
        for (int r : rows) sum += r;
        // 只剩 1 条线
        if (sum == 1) {
            memo.put(key, false);
            return false;
        }
        // 尝试在每行划线
        for (int i = 0; i < rows.length; i++) {
            // 尝试划掉所有条数
            for (int del = rows[i]; del >= 1; del--) {
                rows[i] -= del;
                // 对手无法取胜, 即找到必胜的一手
                if (!turn()) {
                    memo.put(key, true);
                    rows[i] += del;
                    return true;
                }
                // 撤销本步
                rows[i] += del;
            }
        }
        memo.put(key, false);
        return false;
    }
	
    // 当前状态的哈希值
    private String hash() {
        // 复制一份, 排序后作为哈希值
        int[] h = new int[rows.length];
        System.arraycopy(rows, 0, h, 0, rows.length);
        Arrays.sort(h);
        return Arrays.toString(h);
    }
}
```