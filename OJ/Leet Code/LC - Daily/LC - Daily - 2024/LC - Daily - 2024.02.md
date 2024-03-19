#### .01

🔴N/A / [LCP 24. 数字游戏](https://leetcode.cn/problems/5TxKeK/) / 贪心

```java
public int[] numsGame(int[] nums) {
	final int MOD = 1000000007;
	int[] ans = new int[nums.length];
	Queue<Integer> l = new PriorityQueue<>(Collections.reverseOrder());
	Queue<Integer> r = new PriorityQueue<>();
	long ls = 0, rs = 0;
	for (int i = 0; i < nums.length; i++) {
		int b = nums[i] - i;
		if (i % 2 == 0) {
			if (!l.isEmpty() && b < l.peek()) {
				ls -= l.peek() - b;
				l.offer(b);
				b = l.poll();
			}
			rs += b;
			r.offer(b);
			ans[i] = (int) ((rs - r.peek() - ls) % MOD);
		}
		else {
			if (b > r.peek()) {
				rs += b - r.peek();
				r.offer(b);
				b = r.poll();
			}
			ls += b;
			l.offer(b);
			ans[i] = (int) ((rs - ls) % MOD);
		}
	}
	return ans;
}
```

#### .02

🟡2001 / [1686. 石子游戏 VI](https://leetcode.cn/problems/stone-game-vi/) / #贪心 

```java
public int stoneGameVI(int[] a, int[] b) {
	Integer[] ids = new Integer[a.length];
	Arrays.setAll(ids, x -> x);
	Arrays.sort(ids, (i, j) -> a[j] + b[j] - a[i] - b[i]);
	int dif = 0;
	for (int i = 0; i < a.length; i++) dif += i % 2 == 0 ? a[ids[i]] : -b[ids[i]];
	return Integer.compare(dif, 0);
}
```

#### .03

🟡1951 / [1690. 石子游戏 VII](https://leetcode.cn/problems/stone-game-vii/) / #动态规划 

```java
public int stoneGameVII(int[] stones) {
	int n = stones.length;
	int[] pre = new int[n + 1];
	for (int i = 1; i <= n; i++) pre[i] += pre[i - 1] + stones[i - 1];
	int[] dp = new int[n];
	for (int i = n - 2; i >= 0; i--) {
		for (int j = i + 1; j < n; j++) {
			dp[j] = Math.max(pre[j + 1] - pre[i + 1] - dp[j], pre[j] - pre[i] - dp[j - 1]);
		}
	}
	return dp[n - 1];
}
```

#### .04

🟢N/A / [292. Nim 游戏](https://leetcode.cn/problems/nim-game/) / #技巧 

```java
public boolean canWinNim(int n) {
	return n % 4 != 0;
}
```

#### .05

🟡1954 / [1696. 跳跃游戏 VI](https://leetcode.cn/problems/jump-game-vi/) / #动态规划 

```java
public int maxResult(int[] nums, int k) {
	int n = nums.length;
	int[] dp = new int[n];
	Arrays.fill(dp, Integer.MIN_VALUE);
	dp[0] = nums[0];
	for (int i = 0; i < n; i++) {
		for (int j = i + 1; j < n && j <= i + k; j++) {
			int next = dp[i] + nums[j];
			if (dp[j] < next) dp[j] = next;
			if (dp[j] >= dp[i]) break;
		}
	}
	return dp[n - 1];
}
```

#### .06

🟡N/A / [LCP 30. 魔塔游戏](https://leetcode.cn/problems/p0NxJO/) / #贪心 

```java
public int magicTower(int[] nums) {
	long sum = 0, hp = 1;
	for (int x : nums) sum += x;
	if (sum < 0) return -1;
	int ans = 0;
	Queue<Integer> pq = new PriorityQueue<>();
	for (int x : nums) {
		if (x < 0) pq.offer(x);
		hp += x;
		if (hp < 1) {
			hp -= pq.poll();
			ans++;
		}
	}
	return ans;
}
```

#### .07

🟡1677 / [2641. 二叉树的堂兄弟节点 II](https://leetcode.cn/problems/cousins-in-binary-tree-ii/) / #树 

```java
private List<Integer> sum;

public TreeNode replaceValueInTree(TreeNode root) {
	sum = new ArrayList<>();
	dfs1(root, 0);
	dfs2(root, 0, root.val);
	return root;
}

private void dfs1(TreeNode node, int d) {
	if (node == null) return;
	if (sum.size() <= d) sum.add(node.val);
	else sum.set(d, sum.get(d) + node.val);
	dfs1(node.left, d + 1);
	dfs1(node.right, d + 1);
}

private void dfs2(TreeNode node, int d, int sib) {
	if (node == null) return;
	node.val = sum.get(d) - sib;
	int nsib = (node.left != null ? node.left.val : 0) + (node.right != null ? node.right.val : 0);
	dfs2(node.left, d + 1, nsib);
	dfs2(node.right, d + 1, nsib);
}
```

#### .08

🟢1288 / [993. 二叉树的堂兄弟节点](https://leetcode.cn/problems/cousins-in-binary-tree/) / #树 

```java
private boolean ans;
private int depth;
private TreeNode father;

public boolean isCousins(TreeNode root, int x, int y) {
	dfs(root, null, 1, x, y);
	return ans;
}

private boolean dfs(TreeNode node, TreeNode fa, int d, int x, int y) {
	if (node == null || depth > 0 && d > depth) return false;
	if (node.val == x || node.val == y) {
		if (depth > 0) {
			ans = depth == d && father != fa;
			return true;
		}
		depth = d;
		father = fa;
	}
	return dfs(node.left, node, d + 1, x, y) || dfs(node.right, node, d + 1, x, y);
}
```

#### .09

🟡N/A / [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) / #树 

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	if (root == null) return null;
	if (root == p || root == q) return root;
	
	TreeNode L = lowestCommonAncestor(root.left, p, q), R = lowestCommonAncestor(root.right, p, q);
	if (L != null && R != null) return root;
	return L != null ? L : R;
}
```

#### .10

🟢N/A / [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/) / #树 

```java
public List<Integer> inorderTraversal(TreeNode root) {
	List<Integer> ans = new ArrayList<>();
	Deque<TreeNode> stk = new LinkedList<>();
	while (root != null || !stk.isEmpty()) {
		while (root != null) {
			stk.push(root);
			root = root.left;
		}
		root = stk.pop();
		ans.add(root.val);
		root = root.right;
	}
	return ans;
}
```

#### .11

🟢N/A / [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/) / #树 

```java
public List<Integer> preorderTraversal(TreeNode root) {
	List<Integer> ans = new ArrayList<>();
	preorder(root, ans);
	return ans;
}

private void preorder(TreeNode root, List<Integer> ans) {
	if (root == null) return;
	ans.add(root.val);
	preorder(root.left, ans);
	preorder(root.right, ans);
}
```

#### .12

🟢N/A / [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/) / #树 

```java
public List<Integer> postorderTraversal(TreeNode root) {
	List<Integer> ans = new ArrayList<>();
	postorder(root, ans);
	return ans;
}

private void postorder(TreeNode root, List<Integer> ans) {
	if (root == null) return;
	postorder(root.left, ans);
	postorder(root.right, ans);
	ans.add(root.val);
}
```

#### .13

🔴1676 / [987. 二叉树的垂序遍历](https://leetcode.cn/problems/vertical-order-traversal-of-a-binary-tree/) / #树 

```java
private List<int[]> coll;

public List<List<Integer>> verticalTraversal(TreeNode root) {
	coll = new ArrayList<>();
	dfs(root, 0, 0);
	List<List<Integer>> ans = new ArrayList<>();
	coll.sort((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] != b[1] ? a[1] - b[1] : a[2] - b[2]);
	int last = Integer.MIN_VALUE;
	for (int[] d : coll) {
		if (d[0] != last) {
			last = d[0];
			ans.add(new ArrayList<>());
		}
		ans.get(ans.size() - 1).add(d[2]);
	}
	return ans;
}

private void dfs(TreeNode node, int r, int c) {
	if (node == null) return;
	coll.add(new int[]{c, r, node.val});
	dfs(node.left, r + 1, c - 1);
	dfs(node.right, r + 1, c + 1);
}
```

#### .14

🟡N/A / [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/) / #树 

```java
public List<List<Integer>> levelOrder(TreeNode root) {
	List<List<Integer>> ans = new ArrayList<>();
	if (root == null) return ans;
	Queue<TreeNode> q = new LinkedList<>();
	q.offer(root);
	while (!q.isEmpty()) {
		List<Integer> row = new ArrayList<>();
		int size = q.size();
		while (size-- > 0) {
			TreeNode p = q.poll();
			row.add(p.val);
			if (p.left != null) q.offer(p.left);
			if (p.right != null) q.offer(p.right);
		}
		ans.add(row);
	}
	return ans;
}
```

#### .15

🟡N/A / [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/) / #树 

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
	List<List<Integer>> ans = new ArrayList<>();
	if (root == null) return ans;
	Queue<TreeNode> q = new LinkedList<>();
	q.offer(root);
	while (!q.isEmpty()) {
		List<Integer> row = new ArrayList<>();
		int size = q.size();
		while (size-- > 0) {
			TreeNode p = q.poll();
			row.add(p.val);
			if (p.left != null) q.offer(p.left);
			if (p.right != null) q.offer(p.right);
		}
		ans.add(row);
	}
	Collections.reverse(ans);
	return ans;
}
```

#### .16

🟡N/A / [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/) / #树 

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
	Queue<TreeNode> q = new LinkedList<>();
	if (root != null) q.add(root);
	List<List<Integer>> ans = new ArrayList<>();
	while (!q.isEmpty()) {
		LinkedList<Integer> row = new LinkedList<>();
		for (int i = q.size(); i > 0; i--) {
			TreeNode p = q.poll();
			if (ans.size() % 2 == 0) row.addLast(p.val);
			else row.addFirst(p.val);
			if (p.left != null) q.add(p.left);
			if (p.right != null) q.add(p.right);
		}
		ans.add(row);
	}
	return ans;
}
```

#### .17

🟡N/A / [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/) / #树 

```java
public List<List<Integer>> levelOrder(Node root) {
	List<List<Integer>> ans = new ArrayList<>();
	if (root == null) return ans;
	Queue<Node> q = new LinkedList<>();
	q.offer(root);
	while (!q.isEmpty()) {
		List<Integer> row = new ArrayList<>();
		int size = q.size();
		while (size-- > 0) {
			Node p = q.poll();
			List<Node> ch = p.children;
			row.add(p.val);
			if (ch != null) {
				for (Node child : ch) q.offer(child);
			}
		}
		ans.add(row);
	}
	return ans;
}
```

#### .18

🟡N/A / [589. N 叉树的前序遍历](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/) / #树 

```java
private List<Integer> ans;

public List<Integer> preorder(Node root) {
	ans = new ArrayList<>();
	dfs(root);
	return ans;
}

private void dfs(Node node) {
	if (node == null) return;
	ans.add(node.val);
	List<Node> ch = node.children;
	for (Node child : ch) dfs(child);
}
```

#### .19

🟢N/A / [590. N 叉树的后序遍历](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/) / #树 

```java
private List<Integer> ans;

public List<Integer> postorder(Node root) {
	ans = new ArrayList<>();
	dfs(root);
	return ans;
}

private void dfs(Node node) {
	if (node == null) return;
	if (node.children != null) {
		for (Node child : node.children) {
			dfs(child);
		}
	}
	ans.add(node.val);
}
```

#### .20

🟡N/A / [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) / #树 

```java
private Map<Integer, Integer> indices;

public TreeNode buildTree(int[] preorder, int[] inorder) {
	int n = preorder.length;
	indices = new HashMap<>();
	for (int i = 0; i < n; i++) indices.put(inorder[i], i);
	return solve(preorder, 0, n - 1, 0);
}

private TreeNode solve(int[] pre, int preL, int preR, int inL) {
	if (preL > preR) return null;
	int inRoot = indices.get(pre[preL]);
	TreeNode root = new TreeNode(pre[preL]);
	int sizeL = inRoot - inL;
	root.left = solve(pre, preL + 1, preL + sizeL, inL);
	root.right = solve(pre, preL + sizeL + 1, preR, inRoot + 1);
	return root;
}
```

```java
private int in, pre;

public TreeNode buildTree(int[] preorder, int[] inorder) {
	in = pre = 0;
	return solve(preorder, inorder, Integer.MIN_VALUE);
}

private TreeNode solve(int[] p, int[] i, int stop) {
	if (pre >= p.length) return null;
	if (i[in] == stop) {
		in++;
		return null;
	}
	TreeNode node = new TreeNode(p[pre++]);
	node.left = solve(p, i, node.val);
	node.right = solve(p, i, stop);
	return node;
}
```

#### .21

🟡N/A / [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) / #树 

```java
private Map<Integer, Integer> idx = new HashMap<>();

public TreeNode buildTree(int[] inorder, int[] postorder) {
	int n = inorder.length;
	if (n == 0) return null;
	for (int i = 0; i < n; i++) idx.put(inorder[i], i);
	return solve(postorder, n - 1, 0, 0, n - 1);
}

private TreeNode solve(int[] p, int inEd, int inSt, int poSt, int poEd) {
	if (inSt > inEd || poSt > poEd) return null;
	int val = p[poEd];
	TreeNode root = new TreeNode(val);
	int k = idx.get(val);
	root.left = solve(p, k - 1, inSt, poSt, poSt + k - inSt - 1);
	root.right = solve(p, inEd, k + 1, poSt + k - inSt, poEd - 1);
	return root;
}
```

#### .22

🟡1732 / [889. 根据前序和后序遍历构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) / #树 

```java
public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
	int n = preorder.length;
	int[] idx = new int[n + 1];
	for (int i = 0; i < n; i++) idx[postorder[i]] = i;
	return dfs(preorder, 0, n, 0, n, idx);
}

private TreeNode dfs(int[] p, int prL, int prR, int psL, int psR, int[] idx) {
	if (prL == prR) return null;
	if (prL + 1 == prR) return new TreeNode(p[prL]);
	int leftSize = idx[p[prL + 1]] - psL + 1;
	TreeNode L = dfs(p, prL + 1, prL + 1 + leftSize, psL, psL + leftSize, idx);
	TreeNode R = dfs(p, prL + 1 + leftSize, prR, psL + leftSize, psR - 1, idx);
	return new TreeNode(p[prL], L, R);
}
```

#### .23

🟡1374 / [2583. 二叉树中的第 K 大层和](https://leetcode.cn/problems/kth-largest-sum-in-a-binary-tree/) / #树 

```java
public long kthLargestLevelSum(TreeNode root, int k) {
	List<Long> arr = new ArrayList<>();
	Queue<TreeNode> q = new LinkedList<>();
	q.offer(root);
	while (!q.isEmpty()) {
		int size = q.size();
		long sum = 0;
		while (size-- > 0) {
			TreeNode p = q.poll();
			sum += p.val;
			if (p.left != null) q.offer(p.left);
			if (p.right != null) q.offer(p.right);
		}
		arr.add(sum);
	}
	
	int n = arr.size();
	if (n < k) return -1;
	Collections.sort(arr);
	return arr.get(n - k);
}
```

#### .24

🟡1597 / [2476. 二叉搜索树最近节点查询](https://leetcode.cn/problems/closest-nodes-queries-in-a-binary-search-tree/) / #树 

```java
private List<Integer> arr;

public List<List<Integer>> closestNodes(TreeNode root, List<Integer> queries) {
	arr = new ArrayList<>();
	inorder(root);
	int n = arr.size();
	int[] a = new int[n];
	for (int i = 0; i < n; i++) a[i] = arr.get(i);
	
	List<List<Integer>> ans = new ArrayList<>(queries.size());
	for (int q : queries) {
		int j = lowerBound(a, q);
		int max = j < n ? a[j] : -1;
		if (j == n || a[j] != q) j--;
		int min = j >= 0 ? a[j] : -1;
		ans.add(List.of(min, max));
	}
	return ans;
}

private void inorder(TreeNode node) {
	if (node == null) return;
	inorder(node.left);
	arr.add(node.val);
	inorder(node.right);
}

private int lowerBound(int[] a, int target) {
	int l = -1, r = a.length;
	while (l + 1 < r) {
		int m = l + r >> 1;
		if (a[m] >= target) r = m;
		else l = m;
	}
	return r;
}
```

#### .25

🟡N/A / [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/) / #树 

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	if (root == null || root == p || root == q) return root;
	if (root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
	else if (root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
	else return root;
}
```

#### .26

🟢1335 / [938. 二叉搜索树的范围和](https://leetcode.cn/problems/range-sum-of-bst/) / #树 

```java
private int low, high, ans;

public int rangeSumBST(TreeNode root, int low, int high) {
	this.low = low;
	this.high = high;
	ans = 0;
	dfs(root);
	return ans;
}

private void dfs(TreeNode node) {
	if (node == null) return;
	if (inRange(node.val)) {
		ans += node.val;
		dfs(node.left);
		dfs(node.right);
	}
	else if (node.val < low) dfs(node.right);
	else if (node.val > low) dfs(node.left);
}

private boolean inRange(int val) {
	return val >= low && val <= high;
}
```

#### .27

🔴2428 / [2867. 统计树中的合法路径数目](https://leetcode.cn/problems/count-valid-paths-in-a-tree/) / #树 #搜索 

```java
private final static int MX = (int) 1e5;
private final static boolean[] np = new boolean[MX + 1]; // 质数=false 非质数=true

static {
	np[1] = true;
	for (int i = 2; i * i <= MX; i++) {
		if (!np[i]) {
			for (int j = i * i; j <= MX; j += i) {
				np[j] = true;
			}
		}
	}
}

public long countPaths(int n, int[][] edges) {
	List<Integer>[] g = new ArrayList[n + 1];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (var e : edges) {
		int x = e[0], y = e[1];
		g[x].add(y);
		g[y].add(x);
	}
	long ans = 0;
	int[] size = new int[n + 1];
	var nodes = new ArrayList<Integer>();
	for (int x = 1; x <= n; x++) {
		if (np[x]) continue; // 跳过非质数
		int sum = 0;
		for (int y : g[x]) { // 质数 x 把这棵树分成了若干个连通块
			if (!np[y]) continue;
			if (size[y] == 0) { // 尚未计算过
				nodes.clear();
				dfs(y, -1, g, nodes); // 遍历 y 所在连通块，在不经过质数的前提下，统计有多少个非质数
				for (int z : nodes) size[z] = nodes.size();
			}
			// 这 size[y] 个非质数与之前遍历到的 sum 个非质数，两两之间的路径只包含质数 x
			ans += (long) size[y] * sum;
			sum += size[y];
		}
		ans += sum; // 从 x 出发的路径
	}
	return ans;
}

private void dfs(int x, int fa, List<Integer>[] g, List<Integer> nodes) {
	nodes.add(x);
	for (int y : g[x]) {
		if (y != fa && np[y]) dfs(y, x, g, nodes);
	}
}
```

#### .28

🟡1917 / [2673. 使二叉树所有路径值相等的最小代价](https://leetcode.cn/problems/make-costs-of-paths-equal-in-a-binary-tree/) / #树 #贪心 #树形动态规划 

```java
private int ans;

public int minIncrements(int n, int[] cost) {
	ans = 0;
	dfs(cost, 0);
	return ans;
}

private int dfs(int[] cost, int node) {
	if (node >= cost.length) return 0;
	int L = dfs(cost, node * 2 + 1), R = dfs(cost, node * 2 + 2);
	ans += Math.abs(L - R);
	return cost[node] + Math.max(L, R);
}
```

```java
public int minIncrements(int n, int[] cost) {
	int ans = 0;
	for (int i = n / 2; i > 0; i--) {
		ans += Math.abs(cost[i * 2 - 1] - cost[i * 2]);
		cost[i - 1] += Math.max(cost[i * 2 - 1], cost[i * 2]);
	}
	return ans;
}
```

#### .29

🔴2228 / [2581. 统计可能的树根数目](https://leetcode.cn/problems/count-number-of-possible-root-nodes/) / #树 #搜索 

```java
private List<Integer>[] g;
private Set<Long> s = new HashSet<>();
private int k, ans, cnt0;

public int rootCount(int[][] edges, int[][] guesses, int k) {
	this.k = k;
	g = new ArrayList[edges.length + 1];
	Arrays.setAll(g, i -> new ArrayList<>());
	for (int[] e : edges) {
		int x = e[0];
		int y = e[1];
		g[x].add(y);
		g[y].add(x); // 建图
	}
	
	for (int[] e : guesses) { // guesses 转成哈希表
		s.add((long) e[0] << 32 | e[1]); // 两个 4 字节 int 压缩成一个 8 字节 long
	}
	
	dfs(0, -1);
	reroot(0, -1, cnt0);
	return ans;
}

private void dfs(int x, int fa) {
	for (int y : g[x]) {
		if (y != fa) {
			// 以 0 为根时, 猜对了
			if (s.contains((long) x << 32 | y)) cnt0++;
			dfs(y, x);
		}
	}
}

private void reroot(int x, int par, int cnt) {
	if (cnt >= k) ans++; // 此时 cnt 就是以 x 为根时的猜对次数
	for (int y : g[x]) {
		if (y != par) {
			int c = cnt;
			if (s.contains((long) x << 32 | y)) c--; // 原来是对的, 现在错了
			if (s.contains((long) y << 32 | x)) c++; // 原来是错的, 现在对了
			reroot(y, x, c);
		}
	}
}
```