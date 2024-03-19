```text
题库: 94 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 114 144 145 156 173 199 208 222 226 230 235 236 250 255 257 285 298 314 331 333 366 404 429 449 450 513 515 530 543 549 589 590 637 654 655 662 663 669 687 701 814 863 865 889 938 958 987 993 1038 1123 1161 1372 1373 1457 1530 1993 2049 2236 2415 2476 2509 2583 2641 2673
LCR: 143
```

#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

@2023.09.19

🟢

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
private List<Integer> ans;

public List<Integer> inorderTraversal(TreeNode root) {
	ans = new ArrayList<>();
	inorder(root);
	return ans;
}

private void inorder(TreeNode root) {
	if (root == null) return;
	inorder(root.left);
	ans.add(root.val);
	inorder(root.right);
}
```

2. 迭代

```java
/**
 * 迭代
 * 力扣官方题解
 */
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

#### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

@2023.10.04

🟡

1. DFS

利用 BST 左小右大的性质，传参上下界，DFS 检查左右子树。

```java
/**
 * DFS
 * 力扣官方题解
 */
public boolean isValidBST(TreeNode root) {
	return check(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

private boolean check(TreeNode node, long lower, long upper) {
	if (node == null) return true;
	if (node.val <= lower || node.val >= upper) return false;
	return check(node.left, lower, node.val) && check(node.right, node.val, upper);
}
```

2. 中序遍历

利用 BST 中序遍历递增的性质，全局变量 `pre` 表示遍历的上一个结点，先递归检查左子树，检查 `pre.val < root.val`，更新 `pre` 后检查右子树。

```java
/**
 * 中序遍历
 * adminNB
 */
private TreeNode pre;

public boolean isValidBST(TreeNode root) {
	if (root == null) return true;
	boolean left = isValidBST(root.left);
	if (!left || pre != null && pre.val >= root.val) return false;
	pre = root;
	return isValidBST(root.right);
}
```

#### [99. 恢复二叉搜索树](https://leetcode.cn/problems/recover-binary-search-tree/)

@2023.11.05

🟡

```java
/**
 * 中序遍历
 * tiantian
 */
private TreeNode pre, t1, t2;

public void recoverTree(TreeNode root) {
	inorder(root);
	swap();
}

private void inorder(TreeNode root) {
	if (root == null) return;
	inorder(root.left);
	if (pre != null && pre.val > root.val) {
		if (t1 == null) t1 = pre;
		t2 = root;
	}
	pre = root;
	inorder(root.right);
}

private void swap() {
	int t = t1.val;
	t1.val = t2.val;
	t2.val = t;
}
```

#### [100. 相同的树](https://leetcode.cn/problems/same-tree/)

@2023.10.13

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public boolean isSameTree(TreeNode p, TreeNode q) {
	if (p == null && q == null) return true;
	if (p == null || q == null) return false;
	return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

#### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

@2023.09.23

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public boolean isSymmetric(TreeNode root)
{
	if (root == null) return true;
	return compare(root.left, root.right);
}

public boolean compare(TreeNode left, TreeNode right)
{
	if (left == null && right == null) return true;
	else if (left == null || right == null || left.val != right.val) return false;
	return compare(left.left, right.right) && compare(left.right, right.left);
}
```

#### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

@2023.09.20

🟡

```java
/**
 * 迭代
 * Somnia1337
 */
public List<List<Integer>> levelOrder(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	List<List<Integer>> ans = new ArrayList<>();
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		List<Integer> temp = new ArrayList<>();
		int size = queue.size();
		while (size > 0)
		{
			TreeNode current = queue.poll();
			temp.add(current.val);
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
		ans.add(temp);
	}
	return ans;
}
```

#### [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)

@2023.10.04

🟡

```java
/**
 * BFS
 * Krahets
 */
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

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

@2023.09.20

🟢

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public int maxDepth(TreeNode root)
{
	if (root == null) return 0;
	int ans = 0;
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		List<Integer> temp = new ArrayList<>();
		int size = queue.size();
		while (size > 0)
		{
			TreeNode current = queue.poll();
			temp.add(current.val);
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
		ans++;
	}
	return ans;
}
```

2. DFS

实在太优雅了😭

```java
/**
 * DFS
 * 不上guardian不改名
 */
public int maxDepth(TreeNode root)
{
	if (root == null) return 0;
	return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

@2023.10.24

🟡

```java
/**
 * DFS
 * 力扣官方题解
 */
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
/**
 * DFS
 * ?
 */
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

#### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

@2023.09.30

🟡

```java
/**
 * DFS
 * 稚于当初、
 */
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

#### [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

@2023.09.20

🟡

只需将[102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)的结果翻转即可。

```java
/**
 * 迭代
 * Somnia1337
 */
public List<List<Integer>> levelOrderBottom(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	List<List<Integer>> ans = new ArrayList<>();
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		List<Integer> temp = new ArrayList<>();
		int size = queue.size();
		while (size > 0)
		{
			TreeNode current = queue.poll();
			temp.add(current.val);
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
		ans.add(temp);
	}
	Collections.reverse(ans);
	return ans;
}
```

#### [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

@2023.10.16

🟢

```java
/**
 * 中序遍历
 * Somnia1337
 */
public TreeNode sortedArrayToBST(int[] nums)
{
	return solve(nums, 0, nums.length - 1);
}

public TreeNode solve(int[] nums, int left, int right)
{
	if (left > right) return null;
	int mid = left + right >> 1;
	TreeNode root = new TreeNode(nums[mid]);
	root.left = solve(nums, left, mid - 1);
	root.right = solve(nums, mid + 1, right);
	return root;
}
```

#### [109. 有序链表转换二叉搜索树](https://leetcode.cn/problems/convert-sorted-list-to-binary-search-tree/)

@2023.11.04

🟡

```java
/**
 * 辅助结构
 * Somnia1337
 */
public TreeNode sortedListToBST(ListNode head)
{
	if (head == null) return null;
	List<Integer> nums = new ArrayList<>();
	while (head != null)
	{
		nums.add(head.val);
		head = head.next;
	}
	return solve(nums, 0, nums.size() - 1);
}

private TreeNode solve(List<Integer> nums, int l, int r)
{
	if (l > r) return null;
	int m = l + r >> 1;
	TreeNode root = new TreeNode(nums.get(m));
	root.left = solve(nums, l, m - 1);
	root.right = solve(nums, m + 1, r);
	return root;
}
```

```java
/**
 * 双指针
 * salmanvin
 */
public TreeNode sortedListToBST(ListNode head)
{
	return solve(head, null);
}

private TreeNode solve(ListNode l, ListNode r)
{
	if (l == null || l == r) return null;
	ListNode f = l, s = l;
	while (f != null && f.next != null && f != r)
	{
		f = f.next;
		if (f == r) break;
		f = f.next;
		s = s.next;
	}
	TreeNode ret = new TreeNode(s.val);
	ret.left = solve(l, s);
	ret.right = solve(s.next, r);
	return ret;
}
```

#### [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

@2023.09.23

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public boolean isBalanced(TreeNode root)
{
	if (root == null) return true;
	return Math.abs(maxDepth(root.left) - maxDepth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
}

public int maxDepth(TreeNode root)
{
	if (root == null) return 0;
	return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

#### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

@2023.09.20

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public int minDepth(TreeNode root)
{
	if (root == null) return 0;
	int left = minDepth(root.left), right = minDepth(root.right);
	if (left * right == 0) return Math.max(left, right) + 1;
	return Math.min(left, right) + 1;
}
```

#### [112. 路径总和](https://leetcode.cn/problems/path-sum/)

@2023.09.24

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public boolean ans = false;

public boolean hasPathSum(TreeNode root, int targetSum)
{
	solve(root, 0, targetSum);
	return ans;
}

public void solve(TreeNode root, int sum, int target)
{
	if (root == null) return;
	else if (root.left == null && root.right == null && sum + root.val == target)
	{
		ans = true;
		return;
	}
	solve(root.left, sum + root.val, target);
	if (ans) return;
	solve(root.right, sum + root.val, target);
}
```

#### [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

@2023.10.22

🟡

```java
/**
 * DFS
 * zviolet
 */
private TreeNode pre = null;

public void flatten(TreeNode root)
{
	if (root == null) return;
	flatten(root.right);
	flatten(root.left);
	
	root.right = pre;
	root.left = null;
	pre = root;
}
```

分为三步：

1. 将根节点的左子树变成链表
2. 将根节点的右子树变成链表
3. 将变成链表的右子树放在变成链表的左子树的最右边

```java
/**
 * DFS
 * 明知山有虎？
 */
public void flatten(TreeNode root)
{
	if (root == null) return;
	flatten(root.left);
	flatten(root.right);
	
	TreeNode temp = root.right;
	root.right = root.left;
	root.left = null;
	while (root.right != null) root = root.right;
	root.right = temp;
}
```

#### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

@2023.09.19

🟢

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
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

2. 迭代

```java
/**
 * 迭代
 * 力扣官方题解
 */
public List<Integer> preorderTraversal(TreeNode root) {
	List<Integer> ans = new ArrayList<>();
	if (root == null) return ans;
	Deque<TreeNode> stk = new LinkedList<>();
	TreeNode node = root;
	while (!stk.isEmpty() || node != null) {
		while (node != null) {
			ans.add(node.val);
			stk.push(node);
			node = node.left;
		}
		node = stk.pop();
		node = node.right;
	}
	return ans;
}
```

#### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

@2023.09.19

🟢

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
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

2. 迭代

```java
/**
 * 迭代
 * 力扣官方题解
 */
public List<Integer> postorderTraversal(TreeNode root) {
	List<Integer> ans = new ArrayList<>();
	if (root == null) return ans;
	Deque<TreeNode> stk = new LinkedList<>();
	TreeNode pre = null;
	while (root != null || !stk.isEmpty()) {
		while (root != null) {
			stk.push(root);
			root = root.left;
		}
		root = stk.pop();
		if (root.right == null || root.right == pre) {
			ans.add(root.val);
			pre = root;
			root = null;
		}
		else {
			stk.push(root);
			root = root.right;
		}
	}
	return ans;
}
```

#### [156. 上下翻转二叉树](https://leetcode.cn/problems/binary-tree-upside-down/)

@2024.01.02

🟡

```java
/**
 * 
 * Somnia1337
 */
public TreeNode upsideDownBinaryTree(TreeNode root) {
	TreeNode right = null, pre = null;
	while (root != null) {
		TreeNode left = root.left;
		root.left = right;
		right = root.right;
		root.right = pre;
		pre = root;
		root = left;
	}
	return pre;
}
```

#### [173. 二叉搜索树迭代器](https://leetcode.cn/problems/binary-search-tree-iterator/)

@2023.10.29

🟡

1. 列表

```java
/**
 * 列表
 * Somnia1337
 */
class BSTIterator
{
    private List<Integer> inorder;
    private int p;
	
    public BSTIterator(TreeNode root)
    {
        inorder = new ArrayList<>();
        inorder(root);
        p = 0;
    }
	
    private void inorder(TreeNode root)
    {
        if (root == null) return;
        inorder(root.left);
        inorder.add(root.val);
        inorder(root.right);
    }
	
    public int next()
    {
        return inorder.get(p++);
    }
	
    public boolean hasNext()
    {
        return p < inorder.size();
    }
}
```

2. 栈

```java
/**
 * 栈
 * 力扣官方题解
 */
class BSTIterator
{
    private TreeNode cur;
    private Deque<TreeNode> stack;
	
    public BSTIterator(TreeNode root)
    {
        cur = root;
        stack = new LinkedList<>();
    }
	
    public int next()
    {
        while (cur != null)
        {
            stack.push(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        int ret = cur.val;
        cur = cur.right;
        return ret;
    }
	
    public boolean hasNext()
    {
        return cur != null || !stack.isEmpty();
    }
}
```

#### [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

@2023.09.20

🟡

```java
/**
 * 迭代
 * Somnia1337
 */
public List<Integer> rightSideView(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	List<Integer> ans = new ArrayList<>();
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		int size = queue.size();
		while (size > 0)
		{
			TreeNode current = queue.poll();
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			if (size == 1) ans.add(current.val); // 当前行的最后一个节点值加入解集
			size--;
		}
	}
	return ans;
}
```

#### [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)

@2023.10.22

🟡

```java
/**
 * 多叉树
 * 路漫漫我不畏
 */
class Trie
{
    private boolean isEnd;
    private Trie[] next;
	
    public Trie()
    {
        isEnd = false;
        next = new Trie[26];
    }
	
    public void insert(String word)
    {
        Trie node = this;
        for (char c : word.toCharArray())
        {
            if (node.next[c - 'a'] == null)
            {
                node.next[c - 'a'] = new Trie();
            }
            node = node.next[c - 'a'];
        }
        node.isEnd = true;
    }
	
    public boolean search(String word)
    {
        Trie node = this;
        for (char c : word.toCharArray())
        {
            node = node.next[c - 'a'];
            if (node == null) return false;
        }
        return node.isEnd;
    }
	
    public boolean startsWith(String prefix)
    {
        Trie node = this;
        for (char c : prefix.toCharArray())
        {
            node = node.next[c - 'a'];
            if (node == null) return false;
        }
        return true;
    }
}
```

#### [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

@2023.09.23

🟡

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public int countNodes(TreeNode root)
{
	if (root == null) return 0;
	return countNodes(root.left) + countNodes(root.right) + 1;
}
```

2. 二分查找

```java
/**
 * 二分查找
 * Somnia1337
 */
public int countNodes(TreeNode root)
{
	if (root == null) return 0;
	int level = 0;
	TreeNode node = root;
	while (node.left != null)
	{
		level++;
		node = node.left;
	}
	int low = 1 << level, high = (1 << (level + 1)) - 1;
	while (low < high)
	{
		int mid = (high - low + 1) / 2 + low;
		if (exists(root, level, mid)) low = mid;
		else high = mid - 1;
	}
	return low;
}

public boolean exists(TreeNode root, int level, int k)
{
	int bits = 1 << (level - 1);
	TreeNode node = root;
	while (node != null && bits > 0)
	{
		if ((bits & k) == 0) node = node.left;
		else node = node.right;
		bits >>= 1;
	}
	return node != null;
}
```

#### [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

@2023.09.23

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode invertTree(TreeNode root)
{
	if (root == null) return null;
	invertTree(root.left);
	invertTree(root.right);
	swapChildren(root);
	return root;
}

public void swapChildren(TreeNode root)
{
	TreeNode temp = root.left;
	root.left = root.right;
	root.right = temp;
}
```

#### [230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

@2023.10.23

🟡

```java
private int k;

public int kthSmallest(TreeNode root, int k)
{
	this.k = k;
	return dfs(root);
}

private int dfs(TreeNode root)
{
	if (root == null) return -1;
	int l = dfs(root.left);
	if (l >= 0) return l;
	if (--k == 0) return root.val;
	return dfs(root.right);
}
```

#### [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

@2023.10.27

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	if (root == null || root == p || root == q) return root;
	if (root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
	else if (root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
	else return root;
}
```

这是针对于 BST 的代码，通用代码见下题。

#### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

@2023.10.05

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	if (root == null) return null;
	if (root == p || root == q) return root;
	
	TreeNode L = lowestCommonAncestor(root.left, p, q), R = lowestCommonAncestor(root.right, p, q);
	if (L != null && R != null) return root;
	return L != null ? L : R;
}
```

#### [250. 统计同值子树](https://leetcode.cn/problems/count-univalue-subtrees/)

@2023.10.03

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public int ans = 0;

public int countUnivalSubtrees(TreeNode root)
{
	dfs(root);
	return ans;
}

public boolean dfs(TreeNode root)
{
	if (root == null) return true;
	boolean left = dfs(root.left) && (root.left == null || root.left.val == root.val);
	boolean right = dfs(root.right) && (root.right == null || root.right.val == root.val);
	boolean ret = left && right;
	if (ret) ans++;
	return ret;
}
```

#### [255. 验证前序遍历序列二叉搜索树](https://leetcode.cn/problems/verify-preorder-sequence-in-binary-search-tree/)

@2023.11.03

🟡

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public boolean verifyPreorder(int[] preorder)
{
	return solve(preorder, 0, preorder.length);
}

private boolean solve(int[] arr, int l, int r)
{
	if (r - l <= 2) return true;
	int m = l + 1;
	while (m < r && arr[m] < arr[l]) m++;
	for (int j = m + 1; j < r; j++)
	{
		if (arr[j] < arr[l]) return false;
	}
	return solve(arr, l + 1, m) && solve(arr, m, r);
}
```

2. 单调栈

```java
/**
 * 单调栈
 * 力扣官方题解
 */
public boolean verifyPreorder(int[] preorder)
{
	Stack<Integer> stack = new Stack<>();
	int min = Integer.MIN_VALUE;
	for (int n : preorder)
	{
		if (n <= min) return false;
		while (!stack.isEmpty() && n > stack.peek()) min = stack.pop();
		stack.push(n);
	}
	return true;
}
```

基于数组：

```java
/**
 * 单调栈
 * ?
 */
public boolean verifyPreorder(int[] preorder)
{
	int min = Integer.MIN_VALUE;
	int i = -1;
	for (int n : preorder)
	{
		if (n < min) return false;
		while (i >= 0 && n > preorder[i]) min = preorder[i--];
		preorder[++i] = n;
	}
	return true;
}
```

#### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

@2023.09.23

🟢

```java
/**
 * DFS
 * Somnia1337
 */
List<String> ans = new ArrayList<>();

public List<String> binaryTreePaths(TreeNode root)
{
	if (root != null) solve(root, "");
	return ans;
}

public void solve(TreeNode root, String path)
{
	if (root.left == null && root.right == null)
	{
		ans.add(path + root.val);
		return;
	}
	StringBuilder temp = new StringBuilder(path);
	int hold = temp.length();
	temp.append(root.val)
		.append("->");
	if (root.left != null) solve(root.left, temp.toString());
	if (root.right != null) solve(root.right, temp.toString());
	temp.delete(hold, temp.length());
}
```

#### [285. 二叉搜索树中的中序后继](https://leetcode.cn/problems/inorder-successor-in-bst/)

@2023.11.05

🟡

> 中序后继是所有节点值大于 `p` 的节点中最小者。

```java
/**
 * 迭代
 * 人类二分法精华
 */
public TreeNode inorderSuccessor(TreeNode root, TreeNode p)
{
	TreeNode ans = null;
	while (root != null)
	{
		if (root.val > p.val)
		{
			ans = root;
			root = root.left;
		}
		else
		{
			root = root.right;
		}
	}
	return ans;
}
```

#### [298. 二叉树最长连续序列](https://leetcode.cn/problems/binary-tree-longest-consecutive-sequence/)

@2023.11.04

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private int ans;

public int longestConsecutive(TreeNode root)
{
	ans = 1;
	dfs(root);
	return ans;
}

private int dfs(TreeNode root)
{
	if (root == null) return 0;
	int ret = 1;
	int l = dfs(root.left), r = dfs(root.right);
	if (root.left != null && root.left.val == root.val + 1) ret = Math.max(l + 1, ret);
	if (root.right != null && root.right.val == root.val + 1) ret = Math.max(r + 1, ret);
	ans = Math.max(Math.max(l, r), Math.max(ret, ans));
	return ret;
}
```

#### [314. 二叉树的垂直遍历](https://leetcode.cn/problems/binary-tree-vertical-order-traversal/)

@2023.11.03

🟡

```java
/**
 * BFS
 * Somnia1337
 */
public List<List<Integer>> verticalOrder(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	Map<Integer, List<Integer>> collect = new HashMap<>();
	Queue<TreeNode> nq = new LinkedList<>(); // 节点队
	Queue<Integer> cq = new LinkedList<>(); // 列号队
	nq.add(root);
	cq.add(0);
	
	// BFS
	int size = nq.size(), max = 0, min = 0;
	while (size > 0)
	{
		for (int i = 0; i < size; i++)
		{
			// 出队, 更新左右边界
			TreeNode n = nq.poll();
			int c = cq.poll();
			max = Math.max(c, max);
			min = Math.min(c, min);
			
			// 收集节点值
			List<Integer> col = collect.getOrDefault(c, new ArrayList<>());
			col.add(n.val);
			collect.put(c, col);
			
			// 左右非空子节点入队
			if (n.left != null)
			{
				nq.add(n.left);
				cq.add(c - 1);
			}
			if (n.right != null)
			{
				nq.add(n.right);
				cq.add(c + 1);
			}
		}
		size = nq.size(); // 下轮 BFS 的范围
	}
	
	// 从两边界间按顺序加入解集
	List<List<Integer>> ans = new ArrayList<>();
	for (int i = min; i <= max; i++)
	{
		if (collect.containsKey(i)) ans.add(collect.get(i));
	}
	return ans;
}
```

#### [331. 验证二叉树的前序序列化](https://leetcode.cn/problems/verify-preorder-serialization-of-a-binary-tree/)

@2023.11.05

🟡

```java
/**
 * 栈
 * 克格勃佳佳
 */
public boolean isValidSerialization(String preorder)
{
	String[] spl = preorder.split(",");
	int l = spl.length, cnt = 0;
	for (int i = l - 1; i >= 0; i--)
	{
		if (spl[i].equals("#")) cnt++;
		else if (cnt >= 2) cnt--;
		else return false;
	}
	return cnt == 1;
}
```

#### [333. 最大 BST 子树](https://leetcode.cn/problems/largest-bst-subtree/)

@2023.10.19

🟡

调用已有解的题……

```java
/**
 * DFS
 * Somnia1337
 */
public int largestBSTSubtree(TreeNode root)
{
	if (isValidBST(root)) return countNodes(root);
	return Math.max(largestBSTSubtree(root.left), largestBSTSubtree(root.right));
}

// 98. 验证二叉搜索树
private boolean isValidBST(TreeNode root)
{
	return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

private boolean isValidBST(TreeNode node, long lower, long upper)
{
	if (node == null) return true;
	if (node.val <= lower || node.val >= upper) return false;
	return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
}

// 222. 完全二叉树的节点个数
private int countNodes(TreeNode root)
{
	if (root == null) return 0;
	return countNodes(root.left) + countNodes(root.right) + 1;
}
```

#### [366. 寻找二叉树的叶子节点](https://leetcode.cn/problems/find-leaves-of-binary-tree/)

@2023.11.05

🟡

```java
/**
 * DFS
 * 晴晴晴雪
 */
public List<List<Integer>> findLeaves(TreeNode root)
{
	List<List<Integer>> ans = new ArrayList<>();
	while (root != null)
	{
		List<Integer> leaves = new ArrayList<>();
		ans.add(leaves);
		root = dfs(root, leaves);
	}
	return ans;
}

private TreeNode dfs(TreeNode root, List<Integer> leaves)
{
	if (root == null) return null;
	if (root.left == null && root.right == null)
	{
		leaves.add(root.val);
		return null;
	}
	root.left = dfs(root.left, leaves);
	root.right = dfs(root.right, leaves);
	return root;
}
```

#### [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

@2023.09.24

🟢

```java
/**
 * DFS
 * Somnia1337
 */
public int ans = 0;

public int sumOfLeftLeaves(TreeNode root)
{
	if (root == null) return 0;
	solve(root);
	return ans;
}

public void solve(TreeNode root)
{
	if (root == null) {}
	else if (root.left == null) solve(root.right);
	else if (root.left.left == null && root.left.right == null)
	{
		ans += root.left.val;
		solve(root.right);
	}
	else
	{
		solve(root.left);
		solve(root.right);
	}
}
```

```java
/**
 * DFS
 * 力扣官方题解
 */
public int sumOfLeftLeaves(TreeNode root)
{
	return root != null ? dfs(root) : 0;
}

public int dfs(TreeNode node)
{
	int ans = 0;
	if (node.left != null)
	{
		ans += isLeafNode(node.left) ? node.left.val : dfs(node.left);
	}
	if (node.right != null && !isLeafNode(node.right))
	{
		ans += dfs(node.right);
	}
	return ans;
}

public boolean isLeafNode(TreeNode node)
{
	return node.left == null && node.right == null;
}
```

#### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

@2023.09.20

🟡

```java
/**
 * 迭代
 * Somnia1337
 */
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

#### [449. 序列化和反序列化二叉搜索树](https://leetcode.cn/problems/serialize-and-deserialize-bst/)

@2023.09.04

🟡

```java
/**
 * 后序遍历
 * 力扣官方题解
 */
public String serialize(TreeNode root)
{
	List<Integer> list = new ArrayList<Integer>();
	postOrder(root, list);
	String str = list.toString();
	return str.substring(1, str.length() - 1);
}

public TreeNode deserialize(String data)
{
	if (data.isEmpty())
	{
		return null;
	}
	String[] arr = data.split(", ");
	Deque<Integer> stack = new ArrayDeque<Integer>();
	int length = arr.length;
	for (int i = 0; i < length; i++)
	{
		stack.push(Integer.parseInt(arr[i]));
	}
	return construct(Integer.MIN_VALUE, Integer.MAX_VALUE, stack);
}

private void postOrder(TreeNode root, List<Integer> list)
{
	if (root == null)
	{
		return;
	}
	postOrder(root.left, list);
	postOrder(root.right, list);
	list.add(root.val);
}

private TreeNode construct(int lower, int upper, Deque<Integer> stack)
{
	if (stack.isEmpty() || stack.peek() < lower || stack.peek() > upper)
	{
		return null;
	}
	int val = stack.pop();
	TreeNode root = new TreeNode(val);
	root.right = construct(val, upper, stack);
	root.left = construct(lower, val, stack);
	return root;
}
```

#### [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

@2023.11.10

🟡

```java
/**
 * DFS
 * Eloquent CraykUK
 */
public TreeNode deleteNode(TreeNode root, int key)
{
	if (root == null) return null;
	if (root.val == key)
	{
		if (root.right == null) return root.left;
		else
		{
			TreeNode t = root.right;
			while (t.left != null) t = t.left;
			t.left = root.left;
			return root.right;
		}
	}
	
	if (root.val < key) root.right = deleteNode(root.right, key);
	else root.left = deleteNode(root.left, key);
	return root;
}
```

#### [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

@2023.09.24

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public int maxDepth = 0;
public int ans = 0;

public int findBottomLeftValue(TreeNode root)
{
	ans = root.val;
	solve(root, 0);
	return ans;
}

public void solve(TreeNode root, int count)
{
	if (root == null) {}
	else if (root.left == null && root.right == null && count > maxDepth)
	{
		maxDepth = count;
		ans = root.val;
	}
	else
	{
		if (root.left != null) solve(root.left, count + 1);
		if (root.right != null) solve(root.right, count + 1);
	}
}
```

#### [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

@2023.09.20

🟡

```java
/**
 * 迭代
 * Somnia1337
 */
public List<Integer> largestValues(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	List<Integer> ans = new ArrayList<>();
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		int size = queue.size();
		int max = Integer.MIN_VALUE;
		while (size > 0)
		{
			TreeNode current = queue.poll();
			max = Math.max(current.val, max);
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
		ans.add(max);
	}
	return ans;
}
```

#### [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

@2023.10.24

🟢

```java
/**
 * 中序遍历
 * Somnia1337
 */
private int pre, ans;

public int getMinimumDifference(TreeNode root)
{
	pre = 100001;
	ans = 100001;
	inorder(root);
	return ans;
}

private void inorder(TreeNode root)
{
	if (root == null) return;
	inorder(root.left);
	ans = Math.min(Math.abs(root.val - pre), ans);
	pre = root.val;
	inorder(root.right);
}
```

#### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

@2023.10.16

🟢

```java
/**
 * DFS
 * lioncattt
 */
private int ans = 0;

public int diameterOfBinaryTree(TreeNode root)
{
	if (root == null) return 0;
	ans = Math.max(depthOfBinaryTree(root.left) + depthOfBinaryTree(root.right), ans);
	diameterOfBinaryTree(root.left);
	diameterOfBinaryTree(root.right);
	return ans;
}

private int depthOfBinaryTree(TreeNode root)
{
	if (root == null) return 0;
	return 1 + Math.max(depthOfBinaryTree(root.left), depthOfBinaryTree(root.right));
}
```

```java
/**
 * DFS
 * ?
 */
int ans = 1;

public int diameterOfBinaryTree(TreeNode root)
{
	if (root == null) return 0;
	depth(root);
	return ans - 1;
}

public int depth(TreeNode node)
{
	if (node == null) return 0;
	int left = depth(node.left);
	int right = depth(node.right);
	ans = Math.max(ans, left + right + 1);
	return Math.max(left, right) + 1;
}
```

#### [549. 二叉树中最长的连续序列](https://leetcode.cn/problems/binary-tree-longest-consecutive-sequence-ii/)

@2023.10.22

🟡

```java
/**
 * DFS
 * 照兮
 */
public int longestConsecutive(TreeNode root)
{
	if (root == null) return 0;
	int a = through(root), b = longestConsecutive(root.left), c = longestConsecutive(root.right);
	return Math.max(a, Math.max(b, c));
}

private int through(TreeNode root)
{
	return increaseFrom(root) + decreaseFrom(root) - 1;
}

private int increaseFrom(TreeNode root)
{
	int ret = 0;
	if (root.left != null && root.left.val == root.val + 1) ret = Math.max(ret, increaseFrom(root.left));
	if (root.right != null && root.right.val == root.val + 1) ret = Math.max(ret, increaseFrom(root.right));
	return ++ret;
}

private int decreaseFrom(TreeNode root)
{
	int ret = 0;
	if (root.left != null && root.left.val == root.val - 1) ret = Math.max(ret, decreaseFrom(root.left));
	if (root.right != null && root.right.val == root.val - 1) ret = Math.max(ret, decreaseFrom(root.right));
	return ++ret;
}
```

```java
/**
 * DFS
 * 盛夏与微风
 */
private int ans = 0;

public int longestConsecutive(TreeNode root)
{
	helper(root);
	return ans;
}

private int[] helper(TreeNode root)
{
	int[] arr = new int[]{1, 1};
	if (root == null) return arr;
	int[] left = helper(root.left);
	int[] right = helper(root.right);
	if (root.left != null)
	{
		if (root.left.val - 1 == root.val) arr[0] = left[0] + 1;
		if (root.left.val + 1 == root.val) arr[1] = left[1] + 1;
	}
	if (root.right != null)
	{
		if (root.right.val - 1 == root.val) arr[0] = Math.max(arr[0], right[0] + 1);
		if (root.right.val + 1 == root.val) arr[1] = Math.max(arr[1], right[1] + 1);
	}
	ans = Math.max(arr[0] + arr[1] - 1, ans);
	return arr;
}
```

```java
/**
 * DFS
 * ?
 */
private int ans = 0;

public int longestConsecutive(TreeNode root)
{
	dfs(root);
	return ans;
}

private int[] dfs(TreeNode root)
{
	if (root == null) return new int[]{0, 0};
	int dec = 1;
	int inc = 1;
	if (root.left != null)
	{
		int[] left = dfs(root.left);
		if (root.left.val - 1 == root.val)
		{
			dec = Math.max(left[0] + 1, dec);
		}
		if (root.left.val + 1 == root.val)
		{
			inc = Math.max(left[1] + 1, inc);
		}
	}
	if (root.right != null)
	{
		int[] right = dfs(root.right);
		if (root.right.val - 1 == root.val)
		{
			dec = Math.max(right[0] + 1, dec);
		}
		if (root.right.val + 1 == root.val)
		{
			inc = Math.max(right[1] + 1, inc);
		}
	}
	ans = Math.max(dec + inc - 1, ans);
	return new int[]{dec, inc};
}
```

#### [589. N 叉树的前序遍历](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/)

@2024.02.18

🟢

```java
/**
 * DFS
 * Somnia1337
 */
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

#### [590. N 叉树的后序遍历](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/)

@2024.02.19



```java
/**
 * DFS
 * Somnia1337
 */
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

#### [637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

@2023.09.20

🟢

```java
/**
 * 迭代
 * Somnia1337
 */
public List<Double> averageOfLevels(TreeNode root)
{
	if (root == null) return new ArrayList<>();
	List<Double> ans = new ArrayList<>();
	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		int size = queue.size();
		long sum = 0; // 有1个测试点需要开long
		for (int i = 0; i < size; i++)
		{
			TreeNode current = queue.poll();
			sum += current.val;
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
		}
		ans.add(sum / (double) size);
	}
	return ans;
}
```

#### [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

@2023.09.30

🟡

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode constructMaximumBinaryTree(int[] nums)
{
	return solve(nums, 0, nums.length - 1);
}

public TreeNode solve(int[] nums, int start, int end)
{
	if (start > end) return null;
	int max = nums[start], maxIdx = start;
	for (int i = start; i <= end; i++)
	{
		if (nums[i] > max)
		{
			max = nums[i];
			maxIdx = i;
		}
	}
	TreeNode ret = new TreeNode(max);
	ret.left = solve(nums, start, maxIdx - 1);
	ret.right = solve(nums, maxIdx + 1, end);
	return ret;
}
```

2. 单调栈

```java
/**
 * 单调栈
 * 力扣官方题解
 */
public TreeNode constructMaximumBinaryTree(int[] nums)
{
	int len = nums.length;
	List<Integer> stack = new ArrayList<>();
	TreeNode[] tree = new TreeNode[len];
	for (int i = 0; i < len; i++)
	{
		tree[i] = new TreeNode(nums[i]);
		while (!stack.isEmpty() && nums[i] > nums[stack.get(stack.size() - 1)])
		{
			tree[i].left = tree[stack.get(stack.size() - 1)];
			stack.remove(stack.size() - 1);
		}
		if (!stack.isEmpty())
		{
			tree[stack.get(stack.size() - 1)].right = tree[i];
		}
		stack.add(i);
	}
	return tree[stack.get(0)];
}
```

#### [655. 输出二叉树](https://leetcode.cn/problems/print-binary-tree/)

@2023.11.20

🟡

先 DFS 求出树高 `h`，再次 DFS，遍历时维护行号及列号，在对应位置填入值或空串。

```java
/**
 * DFS
 * Benhao
 */
private List<List<String>> ans;
private int h;

public List<List<String>> printTree(TreeNode root)
{
	h = getH(root);
	ans = new ArrayList<>(h);
	int n = (1 << h) - 1;
	for (int i = 0; i < h; i++)
	{
		List<String> list = new ArrayList<>(n);
		for (int j = 0; j < n; j++) list.add("");
		ans.add(list);
	}
	dfs(root, 0, (n - 1) >> 1);
	return ans;
}

private int getH(TreeNode node)
{
	return node != null ? Math.max(getH(node.left), getH(node.right)) + 1 : 0;
}

private void dfs(TreeNode node, int row, int col)
{
	ans.get(row).set(col, node.val + "");
	int diff = 1 << h - 2 - row;
	if (node.left != null)
	{
		dfs(node.left, row + 1, col - diff);
	}
	if (node.right != null)
	{
		dfs(node.right, row + 1, col + diff);
	}
}
```

#### [662. 二叉树最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/)

@2023.11.02

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private List<List<Integer>> layer;

public int widthOfBinaryTree(TreeNode root)
{
	layer = new ArrayList<>();
	dfs(root, 0, 0);
	int ans = 0;
	for (List<Integer> l : layer)
	{
		if (l.size() >= 2) ans = Math.max(l.get(l.size() - 1) - l.get(0) + 1, ans);
	}
	return ans;
}

private void dfs(TreeNode root, int level, int x)
{
	if (root == null) return;
	List<Integer> l;
	if (layer.size() > level) l = layer.get(level);
	else
	{
		l = new ArrayList<>();
		layer.add(l);
	}
	l.add(x);
	dfs(root.left, level + 1, 2 * x);
	dfs(root.right, level + 1, 2 * x + 1);
}
```

#### [663. 均匀树划分](https://leetcode.cn/problems/equal-tree-partition/)

@2023.11.15

```java
/**
 * 后序遍历
 * Somnia1337
 */
public boolean checkEqualTree(TreeNode root)
{
	modify(root);
	if (Math.floorMod(root.val, 2) == 1) return false;
	return check(root.left, root.val / 2) || check(root.right, root.val / 2);
}

private void modify(TreeNode root)
{
	if (root == null) return;
	modify(root.left);
	modify(root.right);
	if (root.left != null) root.val += root.left.val;
	if (root.right != null) root.val += root.right.val;
}

private boolean check(TreeNode root, int tar)
{
	if (root == null) return false;
	if (root.val == tar) return true;
	return check(root.left, tar) || check(root.right, tar);
}
```

#### [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

@2023.12.12

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode trimBST(TreeNode root, int low, int high) {
	if (root == null) return null;
	if (root.val < low) return trimBST(root.right, low, high);
	if (root.val > high) return trimBST(root.left, low, high);
	root.left = trimBST(root.left, low, high);
	root.right = trimBST(root.right, low, high);
	return root;
}
```

#### [687. 最长同值路径](https://leetcode.cn/problems/longest-univalue-path/)

@2023.11.01

🟡

```java
/**
 * DFS
 * 力扣官方题解
 */
int ans;

public int longestUnivaluePath(TreeNode root)
{
	ans = 0;
	dfs(root);
	return ans;
}

private int dfs(TreeNode root)
{
	if (root == null) return 0;
	int left = dfs(root.left), right = dfs(root.right);
	int l = 0, r = 0;
	if (root.left != null && root.left.val == root.val) l = left + 1;
	if (root.right != null && root.right.val == root.val) r = right + 1;
	ans = Math.max(l + r, ans);
	return Math.max(l, r);
}
```

#### [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

@2023.10.27

🟡

```java
/**
 * DFS
 * Somnia1337
 */
public TreeNode insertIntoBST(TreeNode root, int val)
{
	if (root == null) return new TreeNode(val);
	if (root.left == null && root.val > val)
	{
		root.left = new TreeNode(val);
		return root;
	}
	if (root.right == null && root.val < val)
	{
		root.right = new TreeNode(val);
		return root;
	}
	if (root.val > val) insertIntoBST(root.left, val);
	else insertIntoBST(root.right, val);
	return root;
}
```

#### [814. 二叉树剪枝](https://leetcode.cn/problems/binary-tree-pruning/)

@2023.11.21

🟡

```java
/**
 * DFS
 * 月亮
 */
public TreeNode pruneTree(TreeNode root)
{
	if (root == null) return null;
	root.left = pruneTree(root.left);
	root.right = pruneTree(root.right);
	return root.val == 1 || root.left != null || root.right != null ? root : null;
}
```

#### [863. 二叉树中所有距离为 K 的结点](https://leetcode.cn/problems/all-nodes-distance-k-in-binary-tree/)

@2023.12.22

🟡

```java
/**
 * DFS
 * ......
 */
private Map<TreeNode, TreeNode> par = new HashMap<>();
private Set<TreeNode> vis = new HashSet<>();
private List<Integer> ans;
private TreeNode tar;

public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
	tar = target;
	find(root, null);
	ans = new LinkedList<>();
	dfs(tar, K);
	return ans;
}

private void find(TreeNode node, TreeNode par) {
	if (null == node) return;
	if (node.val == tar.val) tar = node;
	this.par.put(node, par);
	find(node.left, node);
	find(node.right, node);
}

private void dfs(TreeNode node, int d) {
	if (node != null && !vis.contains(node)) {
		vis.add(node);
		if (d == 0) {
			ans.add(node.val);
			return;
		}
		dfs(node.left, d - 1);
		dfs(node.right, d - 1);
		dfs(par.get(node), d - 1);
	}
}
```

#### [865. 具有所有最深节点的最小子树](https://leetcode.cn/problems/smallest-subtree-with-all-the-deepest-nodes/)

@2023.12.30

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private TreeNode ans;
private int maxD = -1;

public TreeNode subtreeWithAllDeepest(TreeNode root) {
	dfs(root, 0);
	return ans;
}

private int dfs(TreeNode node, int d) {
	if (node == null) {
		maxD = Math.max(d, maxD);
		return d;
	}
	int L = dfs(node.left, d + 1), R = dfs(node.right, d + 1);
	if (L == R && L == maxD) ans = node;
	return Math.max(L, R);
}
```

#### [889. 根据前序和后序遍历构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

@2024.02.22

🟡



```java
/**
 * DFS
 * 灵茶山艾府
 */
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

#### [938. 二叉搜索树的范围和](https://leetcode.cn/problems/range-sum-of-bst/)

@2024.02.26

🟢



```java
/**
 * DFS
 * Somnia1337
 */
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
	dfs(node.left);
	if (inRange(node.val)) ans += node.val;
	dfs(node.right);
}

private boolean inRange(int val) {
	return val >= low && val <= high;
}
```

剪枝：

```java
/**
 * DFS
 * ?
 */
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

#### [958. 二叉树的完全性检验](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/)

@2023.12.02

🟡

```java
/**
 * BFS
 * Somnia1337
 */
public boolean isCompleteTree(TreeNode root)
{
	boolean visNull = false;
	Queue<TreeNode> q = new LinkedList<>();
	q.add(root);
	while (!q.isEmpty())
	{
		TreeNode node = q.poll();
		if (node == null) visNull = true;
		else
		{
			if (visNull) return false;
			q.add(node.left);
			q.add(node.right);
		}
	}
	return true;
}
```

```java
/**
 * DFS
 * ?
 */
private int n, p;

public boolean isCompleteTree(TreeNode root)
{
	dfs(root, 1);
	return n == p;
}

private void dfs(TreeNode root, int u)
{
	if (root == null) return;
	n++;
	p = Math.max(p, u);
	dfs(root.left, u * 2);
	dfs(root.right, u * 2 + 1);
}
```

#### [987. 二叉树的垂序遍历](https://leetcode.cn/problems/vertical-order-traversal-of-a-binary-tree/)

@2024.02.13

🔴



```java
/**
 * DFS
 * 灵茶山艾府
 */
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

#### [993. 二叉树的堂兄弟节点](https://leetcode.cn/problems/cousins-in-binary-tree/)

@2024.02.08

🟢



```java
/**
 * DFS
 * 灵茶山艾府
 */
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

#### [1038. 从二叉搜索树到更大和树](https://leetcode.cn/problems/binary-search-tree-to-greater-sum-tree/)

@2023.12.04

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private int sum;

public TreeNode bstToGst(TreeNode root)
{
	sum = 0;
	dfs(root);
	return root;
}

private void dfs(TreeNode root)
{
	if (root == null) return;
	dfs(root.right);
	sum += root.val;
	root.val = sum;
	dfs(root.left);
}
```

#### [1123. 最深叶节点的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-deepest-leaves/)

@2023.09.06

🟡

```java
/**
 * DFS
 * 灵茶山艾府
 */
private TreeNode ans;
private int maxD = -1;

public TreeNode lcaDeepestLeaves(TreeNode root) {
	dfs(root, 0);
	return ans;
}

private int dfs(TreeNode node, int d) {
	if (node == null) {
		maxD = Math.max(d, maxD);
		return d;
	}
	int L = dfs(node.left, d + 1), R = dfs(node.right, d + 1);
	if (L == R && L == maxD) ans = node;
	return Math.max(L, R);
}
```

```java
/**
 * DFS
 * 灵茶山艾府
 */
public TreeNode lcaDeepestLeaves(TreeNode root) {
	return dfs(root).getKey();
}

private Pair<TreeNode, Integer> dfs(TreeNode root) {
	if (root == null) return new Pair<>(null, 0);
	Pair<TreeNode, Integer> L = dfs(root.left), R = dfs(root.right);
	if (L.getValue() > R.getValue()) return new Pair<>(L.getKey(), L.getValue() + 1);
	if (L.getValue() < R.getValue()) return new Pair<>(R.getKey(), R.getValue() + 1);
	return new Pair<>(root, L.getValue() + 1);
}
```

#### [1161. 最大层内元素和](https://leetcode.cn/problems/maximum-level-sum-of-a-binary-tree/)

@2023.12.30

```java
/**
 * 层序遍历
 * Somnia1337
 */
public int maxLevelSum(TreeNode root) {
	int max = Integer.MIN_VALUE, d = 1, ans = 0;
	Queue<TreeNode> q = new LinkedList<>();
	q.add(root);
	while (!q.isEmpty()) {
		int size = q.size(), cur = 0;
		while (size-- > 0) {
			TreeNode p = q.poll();
			cur += p.val;
			if (p.left != null) q.add(p.left);
			if (p.right != null) q.add(p.right);
		}
		if (cur > max) {
			max = cur;
			ans = d;
		}
		d++;
	}
	return ans;
}
```

#### [1372. 二叉树中的最长交错路径](https://leetcode.cn/problems/longest-zigzag-path-in-a-binary-tree/)

@2023.12.26

🟡

```java
/**
 * DFS
 * ksy-try
 */
private int ans;

public int longestZigZag(TreeNode root) {
	ans = 0;
	dfs(root, -1, -1);
	return ans;
}

private void dfs(TreeNode node, int l, int r) {
	if (node == null) return;
	ans = Math.max(Math.max(l, r) + 1, ans);
	dfs(node.left, -1, l + 1);
	dfs(node.right, r + 1, -1);
}
```

#### [1373. 二叉搜索子树的最大键值和](https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/)

@2024.01.04

🔴

```java
/**
 * 树形动态规划
 * liao47
 */
private int ans;

public int maxSumBST(TreeNode root) {
	ans = 0;
	dfs(root);
	return ans;
}

private int[] dfs(TreeNode node) {
	if (node == null) return new int[]{0, Integer.MAX_VALUE, Integer.MIN_VALUE};
	int[] L = dfs(node.left), R = dfs(node.right);
	if (L == null || R == null || node.val <= L[2] || node.val >= R[1]) return null;
	int sum = L[0] + R[0] + node.val;
	ans = Math.max(sum, ans);
	return new int[]{sum, Math.min(node.val, L[1]), Math.max(node.val, R[2])};
}
```

#### [1457. 二叉树中的伪回文路径](https://leetcode.cn/problems/pseudo-palindromic-paths-in-a-binary-tree/)

@2023.11.25

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private int[] count;
private int ans;

public int pseudoPalindromicPaths(TreeNode root)
{
	count = new int[10];
	ans = 0;
	dfs(root);
	return ans;
}

private void dfs(TreeNode root)
{
	if (root == null) return;
	count[root.val]++;
	if (root.left == null && root.right == null)
	{
		if (check()) ans++;
	}
	else
	{
		dfs(root.left);
		dfs(root.right);
	}
	count[root.val]--;
}

private boolean check()
{
	int odd = 0;
	for (int cnt : count)
	{
		if ((cnt & 1) == 1) odd++;
	}
	return odd <= 1;
}
```

优化：位运算

```java
/**
 * DFS
 * ?
 */
private int ans = 0;

public int pseudoPalindromicPaths(TreeNode root)
{
	dfs(root, 0);
	return ans;
}

private void dfs(TreeNode root, int mask)
{
	if (root == null) return;
	mask ^= 1 << root.val;
	if (root.left == null && root.right == null)
	{
		if (mask == (mask & -mask)) ans++;
		return;
	}
	dfs(root.left, mask);
	dfs(root.right, mask);
}
```

```java
/**
 * DFS
 * 灵茶山艾府
 */
public int pseudoPalindromicPaths(TreeNode root)
{
	return dfs(root, 0);
}

private int dfs(TreeNode root, int mask)
{
	if (root == null) return 0;
	mask ^= 1 << root.val;
	if (root.left == root.right) return (mask & (mask - 1)) == 0 ? 1 : 0;
	return dfs(root.left, mask) + dfs(root.right, mask);
}
```

#### [1530. 好叶子节点对的数量](https://leetcode.cn/problems/number-of-good-leaf-nodes-pairs/)

@2023.12.20

🟡

`dfs(node)` 返回一个长为 `d + 1`（`d = distance`）的数组 `count`，记 `node` 的父结点为 `par`，`count[i]` 表示以 `par` 为根结点的子树中到 `par` 距离为 `i` 的叶结点数量。

每次调用：

1. 对左右子树调用 `dfs()` 获取其 `count`，记为 `L`、`R`。
2. 枚举 `L` 和 `R` 中距离之和 `<= d` 的叶结点对，累加到答案。
3. 将 `L` 和 `R` 中叶结点的深度 `+1`，推出自身的 `count`，传递给父结点。

```java
/**
 * DFS
 * ?
 */
private int d, ans = 0;

public int countPairs(TreeNode root, int distance) {
	d = distance;
	dfs(root);
	return ans;
}

private int[] dfs(TreeNode node) {
	// count 开到 d+1, 因为考虑的最大距离为 d, 且没有距离为 0 的叶结点
	int[] count = new int[d + 1];
	if (node == null) return count;
	if (node.left == null && node.right == null) {
		// 叶结点, 到 par 的距离为 1
		count[1] = 1;
		return count;
	}
	
	int[] L = dfs(node.left), R = dfs(node.right);
	// 累加左右子树中到 node 的距离之和 <=d 的叶结点对
	for (int d1 = 1; d1 <= d; d1++) {
		for (int d2 = 1; d1 + d2 <= d; d2++) ans += L[d1] * R[d2];
	}
	
	// 根据左右子树的 count 计算自身 count, 各叶结点深度 +1
	for (int i = 2; i <= d; i++) count[i] = L[i - 1] + R[i - 1];
	return count;
}
```

#### [1993. 树上的操作](https://leetcode.cn/problems/operations-on-tree/)

@2023.09.23

🟡

```java
/**
 * 模拟
 * ?
 */
int[] parent;
int[] lockedBy;
int[] descendantLock;
List<Integer>[] children;

public LockingTree(int[] parent)
{
	int n = parent.length;
	this.parent = parent;
	lockedBy = new int[n];
	descendantLock = new int[n];
	
	children = new List[n];
	for (int i = 0; i < n; i++)
	{
		children[i] = new ArrayList<>();
	}
	for (int i = 1; i < n; i++)
	{
		var p = parent[i];
		children[p].add(i);
	}
}

public boolean lock(int num, int user)
{
	if (lockedBy[num] != 0) return false;
	
	lockedBy[num] = user;
	int p = parent[num];
	while (p != -1)
	{
		descendantLock[p]++;
		p = parent[p];
	}
	return true;
}

public boolean unlock(int num, int user)
{
	if (lockedBy[num] != user) return false;
	
	lockedBy[num] = 0;
	var p = parent[num];
	while (p != -1)
	{
		descendantLock[p]--;
		p = parent[p];
	}
	return true;
}

void unlockDescendants(int num)
{
	descendantLock[num] = 0;
	lockedBy[num] = 0;
	
	for (int child : children[num])
	{
		if (descendantLock[child] != 0 || lockedBy[child] > 0)
		{
			unlockDescendants(child);
		}
	}
}

public boolean upgrade(int num, int user)
{
	if (lockedBy[num] != 0 || descendantLock[num] == 0)
	{
		return false;
	}
	var p = parent[num];
	while (p != -1)
	{
		if (lockedBy[p] != 0)
		{
			return false;
		}
		p = parent[p];
	}
	
	unlockDescendants(num);
	return lock(num, user);
}
```

#### [2049. 统计最高分的节点数目](https://leetcode.cn/problems/count-nodes-with-the-highest-score/)

@2024.01.04

🟡

删去一个结点将树划分为左子树、右子树、根树 3 个部分，`dfs()` 自底向上返回每个结点左右子树的结点数 `a`、`b`，则其得分为 `a * b * (n-a-b-1)`，如果 `a`、`b` 为 `0` 则取 `1`。

```java
/**
 * 树形动态规划
 * verygoodlee
 */
private int n, ans = 0;
private long max = 0;
private int[] L, R;

public int countHighestScoreNodes(int[] parents) {
	n = parents.length;
	L = new int[n];
	R = new int[n];
	for (int i = 0; i < n; i++) L[i] = R[i] = -1;
	for (int i = 1; i < n; i++) {
		int p = parents[i];
		if (L[p] == -1) L[p] = i;
		else R[p] = i;
	}
	dfs(0);
	return ans;
}

// 自底向上, 返回当前子树的节点总数 dp[node] = dp[left] + dp[right] + 1
private int dfs(int i) {
	if (i == -1) return 0;
	int a = dfs(L[i]), b = dfs(R[i]);
	// 左子树 * 右子树 * 父树, 空树 *1
	long score = (long) Math.max(a, 1) * Math.max(b, 1) * Math.max(n - a - b - 1, 1);
	if (score > max) {
		max = score;
		ans = 1;
	}
	else if (score == max) ans++;
	return a + b + 1;
}
```

#### [2236. 判断根结点是否等于子结点之和](https://leetcode.cn/problems/root-equals-sum-of-children/)

@2023.12.09

🟢

```java
/**
 * 直接判断
 * Somnia1337
 */
public boolean checkTree(TreeNode root)
{
	return root.val == root.left.val + root.right.val;
}
```

#### [2415. 反转二叉树的奇数层](https://leetcode.cn/problems/reverse-odd-levels-of-binary-tree/)

@2023.12.15

```java
/**
 * 两次 DFS
 * Somnia1337
 */
private List<int[]> rows;

public TreeNode reverseOddLevels(TreeNode root) {
	rows = new ArrayList<>();
	dfs1(root, 0, 0);
	dfs2(root, 0, 0);
	return root;
}

private void dfs1(TreeNode node, int d, int i) {
	if (node == null) return;
	if ((d & 1) == 1) {
		if (rows.size() < d) rows.add(new int[(int) Math.pow(2, d)]);
		rows.get(d / 2)[i] = node.val;
	}
	dfs1(node.left, d + 1, i * 2);
	dfs1(node.right, d + 1, i * 2 + 1);
}

private void dfs2(TreeNode node, int d, int i) {
	if (node == null) return;
	if ((d & 1) == 1) {
		node.val = rows.get(d / 2)[(int) Math.pow(2, d) - i - 1];
	}
	dfs2(node.left, d + 1, i * 2);
	dfs2(node.right, d + 1, i * 2 + 1);
}
```

```java
/**
 * DFS
 * ?
 */
public TreeNode reverseOddLevels(TreeNode root) {
	dfs(root.left, root.right, true);
	return root;
}

private void dfs(TreeNode node1, TreeNode node2, boolean odd) {
	if (node1 == null) return;
	if (odd) swap(node1, node2);
	dfs(node1.left, node2.right, !odd);
	dfs(node1.right, node2.left, !odd);
}

private void swap(TreeNode n1, TreeNode n2) {
	int t = n1.val;
	n1.val = n2.val;
	n2.val = t;
}
```

#### [2476. 二叉搜索树最近节点查询](https://leetcode.cn/problems/closest-nodes-queries-in-a-binary-search-tree/)

@2024.02.24

🟡



```java
/**
 * 中序遍历 + 二分查找
 * 灵茶山艾府
 */
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

#### [2509. 查询树中环的长度](https://leetcode.cn/problems/cycle-length-queries-in-a-tree/)

@2024.01.08

🔴

记 `LCA` 为 `a` 和 `b` 的最近公共祖先，环长即为 `LCA` 分别到 `a`、`b` 的距离之和 `+1`。

找 `LCA` 时，比较 `a`、`b` 大小（深度），将较深结点 `/=2` 上移至其父结点，直到 `a == b`。

`while` 内每次将一个结点上移 1 层，直到在 `LCA` 相遇，因此循环次数 `+1` 即为答案。

```java
/**
 * LCA
 * 灵茶山艾府
 */
public int[] cycleLengthQueries(int n, int[][] queries) {
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) {
		int cur = 1, a = queries[i][0], b = queries[i][1];
		while (a != b) {
			if (a > b) a /= 2;
			else b /= 2;
			cur++;
		}
		ans[i] = cur;
	}
	return ans;
}
```

位运算优化：完全二叉树的性质：`root == 1` 时，结点编号的二进制长度即为结点深度。

以 `a = 6 = (110)`，`b = 29 = (11101)` 为例：结点深度之差 `d = 5-3 = 2`，将 `b` 右移 `2` 位为 `(111)`，这样 `b` 与 `a` 同层，如果——

- `a == b`，已经找到 `LCA = a`，`d + 1` 即为答案。
- `a != b`，`a^b = 1 = (1)`，二进制长度 `L = 1`，`a` 和 `b` 各需上跳 `L` 步到达 `LCA`，`d + 1 + 2*L` 即为答案。

```java
/**
 * LCA
 * 灵茶山艾府
 */
public int[] cycleLengthQueries(int n, int[][] queries) {
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) {
		int a = Math.min(queries[i][0], queries[i][1]);
		int b = Math.max(queries[i][0], queries[i][1]);
		int d = Integer.numberOfLeadingZeros(a) - Integer.numberOfLeadingZeros(b);
		ans[i] = d + (32 - Integer.numberOfLeadingZeros(a ^ (b >> d))) * 2 + 1;
	}
	return ans;
}
```

#### [2583. 二叉树中的第 K 大层和](https://leetcode.cn/problems/kth-largest-sum-in-a-binary-tree/)

@2024.02.23

🟡



```java
/**
 * BFS
 * Somnia1337
 */
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

#### [2641. 二叉树的堂兄弟节点 II](https://leetcode.cn/problems/cousins-in-binary-tree-ii/)

@2023.12.14

🟡

```java
/**
 * 两次 DFS
 * Somnia1337
 */
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

#### [2673. 使二叉树所有路径值相等的最小代价](https://leetcode.cn/problems/make-costs-of-paths-equal-in-a-binary-tree/)

@2024.01.05

🟡

1. 树形动态规划

```java
/**
 * 树形动态规划
 * Somnia1337
 */
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

2. 贪心

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int minIncrements(int n, int[] cost) {
	int ans = 0;
	for (int i = n / 2; i > 0; i--) {
		ans += Math.abs(cost[i * 2 - 1] - cost[i * 2]);
		cost[i - 1] += Math.max(cost[i * 2 - 1], cost[i * 2]);
	}
	return ans;
}
```

#### [LCR 143. 子结构判断](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/)

@2023.12.08

🟡

```java
/**
 * DFS
 * Somnia1337
 */
private TreeNode B;

public boolean isSubStructure(TreeNode A, TreeNode B)
{
	if (B == null) return false;
	this.B = B;
	return dfs(A);
}

private boolean dfs(TreeNode a)
{
	if (a == null) return false;
	if (a.val == B.val && isSameTree(a, B)) return true;
	return dfs(a.left) || dfs(a.right);
}

private boolean isSameTree(TreeNode a, TreeNode b)
{
	if (b == null) return true;
	if (a == null) return false;
	return a.val == b.val && isSameTree(a.left, b.left) && isSameTree(a.right, b.right);
}
```

简化到 2 行：

```java
/**
 * DFS
 * Somnia1337
 */
public boolean isSubStructure(TreeNode A, TreeNode B)
{
	return A != null && B != null && (A.val == B.val && isSameTree(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
}

private boolean isSameTree(TreeNode a, TreeNode b)
{
	return b == null || a != null && a.val == b.val && isSameTree(a.left, b.left) && isSameTree(a.right, b.right);
}
```

#### [LCP 34. 二叉树染色](https://leetcode.cn/problems/er-cha-shu-ran-se-UGC/)

@2023.12.18

#树形动态规划 

题目规定的“蓝色相连部分”，为简便起见，简称为“染色块”。

`dfs(node, k)` 返回一个数组 `int[k+1] dp`，`dp[i]` 表示 `node` 处于结点个数不超过 `i` 的“染色块”中的子问题答案，最终原问题的答案就是 `dfs(root, k)[k]`。

具体思路见注释：

```java
/**
 * 树形动态规划
 * 空
 */
public int maxValue(TreeNode root, int k) {
	return dfs(root, k)[k];
}

// 返回 int[k+1] dp,
// dp[i] 表示 node 处于结点个数不超过 i 的"染色块"中的子问题答案
// 特别地, dp[0] 表示 node 不染色
private int[] dfs(TreeNode node, int k) {
	int[] dp = new int[k + 1];
	if (node == null) return dp;
	
	// 两子结点的 dp
	int[] dpL = dfs(node.left, k), dpR = dfs(node.right, k);
	
	// node 不染色, 那么 node.left 和 node.right 都可以处于最大的"染色块"中
	dp[0] = dpL[k] + dpR[k];
	
	// node 染色, 枚举其处于的"染色块"的结点个数 i
	for (int i = 1; i <= k; i++) {
		// 由 dp 定义, dp[i]>=dp[i-1]
		dp[i] = dp[i - 1];
		// 枚举 node.left 处于的"染色块"的结点个数 j
		for (int j = 0; j < i; j++) {
			// node.left 那块有 j 个, 外加 node 本身的 1,
			// 因此 node.right 可处于最多有 i-j-1 个结点的"染色块"中
			dp[i] = Math.max(dpL[j] + dpR[i - j - 1] + node.val, dp[i]);
		}
	}
	return dp;
}
```