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