# Applet

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

# File Processing

### DFS 文件夹

```java
private void dfs(File folder) {
	if (folder.isDirectory()) { // 为文件夹
		File[] files = folder.listFiles(); // 获取子文件 / 子文件夹列表
		if (files != null) { // 判空
			for (File file : files) {
				if (file.isFile()) { // 为文件, 处理
					if (file.getName().endsWith(".md")) { // 条件
						// 处理
					}
				}
				else if (file.isDirectory()) dfs(file); // 为文件夹, 递归
			}
		}
	}
}
```

### 读取文本文件

```java
private List<String> read(File file) {
	List<String> ret = new ArrayList<>();
	try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
		String line; // while 外置定义 line
		while ((line = reader.readLine()) != null) // 非 null
		{
			// 处理
		}
	}
	catch (IOException e) {
		e.printStackTrace();
	}
	return ret;
}
```

# Methods

### 小方法

#### `isSubseq()`

返回 `sub` 是否为 `ori` 的子序列。

```java
private boolean isSubseq(String ori, String sub) {
	char[] chs1 = ori.toCharArray(), chs2 = sub.toCharArray();
	int i = 0, j = 0;
	while (i < chs1.length && j < chs2.length) {
		if (chs1[i] == chs2[j]) j++;
		i++;
	}
	return j == chs2.length;
}
```

#### `isPal()`

返回整型 `x` 是否为回文数。

```java
private boolean isPal(int x) {
	return x == rev(x);
}

private int rev(int x) {
	int ret = 0;
	while (x > 0) {
		ret = 10 * ret + (x % 10);
		x /= 10;
	}
	return ret;
}
```

#### `getPals()`

对长为 `n` 的字符串 `s`，返回一个 `boolean[][] ret`，查询 `ret[i][j]` 即为 `s[i..=j]` 是否为回文串。

双闭区间，例如 `s = "abc"`，`ret[0][1]` 对应 `"ab"`。

```java
private boolean[][] getPals(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	boolean[][] ret = new boolean[n][n];
	for (int r = 0; r < n; r++) {
		for (int l = 0; l <= r; l++) {
			ret[l][r] = chs[l] == chs[r] && (r - l + 1 <= 2 || ret[l + 1][r - 1]);
		}
	}
	return ret;
}
```

#### `countCeilSubarrays()`

返回求和 `<= ceil` 的子数组数量

```java
private int countCeilSubarrays(int[] arr, int ceil) {
	int l = 0, r = 0, sum = 0, ret = 0;
	while (r < arr.length) {
		sum += arr[r++];
		while (sum > ceil) sum -= arr[l++];
		ret += r - l;
	}
	return ret;
}
```

### 数学

#### 快速幂

##### 带模幂

```java
private int pow(long x, int p, int MOD) {
	long ret = 1;
	while (p > 0) {
		if (p % 2 == 1) ret = ((ret % MOD) * (x % MOD)) % MOD;
		x = ((x % MOD) * (x % MOD)) % MOD;
		p >>= 1;
	}
	return (int) ret;
}
```

##### 广泛幂

```java
private double pow(double x, int p) {
	if (p < 0) {
		x = 1 / x;
		p = -p;
	}
	double ret = 1;
	while (p > 0) {
		if (p % 2 == 1) ret *= x;
		x *= x;
		p /= 2;
	}
	return ret;
}
```

#### 最大公约数

```java
private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### 最小公倍数

```java
private int lcp(int a, int b) {
	return a * b / gcd(a, b);
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

### 二分查找

#### `biL()`

```java
private int biL(int[] arr, int tar) {
	int l = 0, r = arr.length;
	while (l < r) {
		int m = l + r >> 1;
		if (arr[m] <= tar) l = m + 1;
		else r = m;
	}
	return r;
}
```

#### `biR()`

```java
private int biR(int[] arr, int tar) {
	int l = 0, r = arr.length;
	while (l < r) {
		int m = l + r >> 1;
		if (arr[m] >= tar) r = m;
		else l = m + 1;
	}
	return l;
}
```

### 搜索

#### 转向

- 4 向

```java
final int[] dirs = {0, 1, 0, -1, 0};

for (int k = 0; k < 4; k++) {
	int nx = x + dirs[k], ny = y + dirs[k + 1];
	if (checkXY(nx, rows, ny, cols) && /*...*/) {
		// ...
	}
}
```

- 8 向

```java
final int[][] dirs = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};

for (int dir : dirs) {
	int nx = x + dir[0], ny = y + dir[1];
	if (checkXY(nx, rows, ny, cols) && /*...*/) {
		// ...
	}
}
```

#### `inBound()`

DFS 时，检查新的 `(x, y)` 是否在界内。

```java
private boolean inBound(int x, int y) {
    return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### 记忆化搜索

```java
private int solve(int n) {
	if (n == 1) return 0;
	if (!cache.containsKey(n)) {
		if (n % 2 == 0) cache.put(n, solve(n / 2));
		else cache.put(n, solve(n - 1));
	}
	return cache.get(n);
}
```

#### Union-Find

```java
private int find(int[] root, int i) {
	return root[i] == i ? i : (root[i] = find(root, root[i]));
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

#### Floyd

```java
private void floyd(int[][] g) {
	for (int k = 0; k < g.length; k++) {
		for (int i = 0; i < g.length; i++) {
			for (int j = 0; j < g.length; j++) {
				if (g[i][k] != Integer.MAX_VALUE && g[k][j] != Integer.MAX_VALUE) {
					g[i][j] = Math.min(g[i][k] + g[k][j], g[i][j]);
				}
			}
		}
	}
}
```

### 字典树

模板题：[140. 单词拆分 II](https://leetcode.cn/problems/word-break-ii/)

```java
class TrieNode {
	TrieNode[] children;
	boolean isWord;
	
	TrieNode() {
		this.children = new TrieNode[26];
		this.isWord = false;
	}
}
```

#### `buildTrie()`

```java
private void buildTrie(String[] words) {
	root = new TrieNode();
	for (String word : words) {
		TrieNode node = root;
		for (char c : word.toCharArray()) {
			if (node.children[c - A] == null) node.children[c - A] = new TrieNode();
			node = node.children[c - A];
		}
		node.isWord = true;
	}
}
```

### 原地堆化

#### 大顶堆

```java
private void heapify(int[] heap) {
	// 倒序遍历, 从而保证 i 的左右子树一定是堆, sink(heap, i) 就可以合并左右子树
	// >= heap.length/2 的结点为叶, 无需下沉
	for (int i = heap.length / 2 - 1; i >= 0; i--) sink(heap, i);
}

// 每次下沉, node 与左右子结点中较大者交换
private void sink(int[] heap, int node) {
	while (node * 2 + 1 < heap.length) {
		int left = node * 2 + 1;
		if (left + 1 < heap.length && heap[left] < heap[left + 1]) left++; // 左子结点 < 右子结点
		if (heap[left] <= heap[node]) break; // node 的左右子结点都 <= node 本身, 停止下沉
		swap(heap, node, node = left); // 下沉
	}
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

```java
private void heapify(int[] heap) {
	for (int i = heap.length / 2 - 1; i >= 0; i--) sink(heap, i);
}

private void sink(int[] heap, int node) {
	while (node * 2 + 1 < heap.length) {
		int left = node * 2 + 1;
		if (left + 1 < heap.length && heap[left] < heap[left + 1]) left++;
		if (heap[left] <= heap[node]) break;
		swap(heap, node, node = left);
	}
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

# Optimization

## 时间

### 结构

#### `Stack` / `Deque`

`Deque<> stk = new ArrayDeque<>()` 快于 `Stack<> stk = new Stack<>()`。

#### 重新赋值 / `clear()`

```java
Set<> set = new HashSet<>();
loop {
	set.clear();
	// ...
}
```

快于

```java
loop {
	Set<> set = new HashSet<>();
	// ...
}
```

#### 字符串连接 / `StringBuilder`

```java
StringBuilder ans = new StringBuilder();
ans.append(x)
   .append("A")
   .append(y)
   .append("B");
return ans.toString();
```

快于

```java
return x + "A" + y + "B";
```

### 方法

#### `Comparator` / Lambda

`Arrays.sort(arr, (a, b) -> a[1] - b[1])` 快于 `Arrays.sort(arr, Comparator.comparingInt(a -> a[1]))`。

# Snippet

### 计数

#### `Map` 统计 `int[]`

```java
for (int i = 0; i < nums.length; i++) {
	pos.computeIfAbsent(nums[i], e -> new ArrayList<>()).add(i);
}
```

#### `int[]` 统计 `String`

```java
int[] count = new int[26];
for (char c : s.toCharArray()) count[c - 'a']++;
```

### 字符串

#### 去除字符串的前导"0"

```java
/*子串*/
while (s.length() > 1 && s.startsWith("0")) s = s.substring(1);
```

```java
/*正则表达式*/
s = s.replaceFirst("^0+", "");
```

取子串的方法为线性，效率更高；使用正则表达式的方法可读性更好。

#### 以空格为分隔符拆分字符串

```java
/*正则表达式*/
String[] words = s.split("\\s+");
```

只有用正则表达式才能处理连续空格的情况，如果使用`" "`作为分隔符：

```java
String[] words = s.split(" ");
```

则会在连续空格之间拆分出多余的空串`""`。

```java
/*示例程序*/
class Main {
    public static void main(String[] args) {
        String s = " a  b   c    d     e      ";
		System.out.println(Arrays.toString(s.split("\\s+")));
        System.out.println(Arrays.toString(s.split(" ")));
    }
}
```

输出：

```text
[, a, b, c, d, e]
[, a, , b, , , c, , , , d, , , , , e]
```

注意到使用正则表达式的语句也拆分出了空串，可以先用**方法trim**去除`s`首尾的空格再拆分。

```java
/*示例程序*/
class Main {
    public static void main(String[] args) {
        String s = " a  b   c    d     e      ";
        s = s.trim(); //去除s首尾的空格
        System.out.println(Arrays.toString(s.split("\\s+")));
    }
}
```

输出：

```text
[a, b, c, d, e]
```

#### 以指定的 `char` 分割字符串

```java
char c = (char) ('a' + i);
...
String[] parts = s.split("\\Q" + c + "\\E");
// 或者
String[] parts = s.split(String.valueOf(c));
```

### 数组

#### 数组覆盖

```java
int[] nums = ...;
int[] ans = new int[len];
...
System.arraycopy(nums, 0, ans, 0, len); //用nums覆盖ans
```

#### 列表转数组

```java
// List<String>
List<String> list1 = new ArrayList<>();
...
String[] strs = list1.toArray(new String[0]);

// List<int[]>
List<int[]> list2 = new ArrayList<>();
...
int[][] nums = list2.toArray(new int[list2.size()][2]);
```

### 自定义排序

### 位运算

#### 位运算大小写转换

- 大小写互换：`c ^= 32`。
- 均变为小写：`c |= 32`。
- 均变为大写：`c &= -33`。

### 数据结构

#### 哈希表判空操作简化

以哈希表的value类型Integer为例，与其先判断`containsKey(key)`再将`get(key)`赋给int变量，可以直接赋给Integer变量，再判断其是否为null。

```java
Map<KeyType, Integer> hashmap = new HashMap<>();
...
Integer x = hashmap.get(key);
if (i != null && i > 0) ...
```

#### 初始化哈希表

```java
Map<KeyType, ValueType> map = new HashMap<>() {{
    put(key1, value1);
    put(key2, value2);
    ...
}};
```

#### 向List\<List\<Integer>>添加新列表

```java
List<List<Integer>> ans = new ArrayList<>();
...
loop {
	...
	ans.add(Arrays.asList(a, b, c));
}
```
