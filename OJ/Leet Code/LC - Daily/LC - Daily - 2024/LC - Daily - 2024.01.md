#### .01

🟡1548 / [1599. 经营摩天轮的最大利润](https://leetcode.cn/problems/maximum-profit-of-operating-a-centennial-wheel/) / #模拟 

```java
public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {
	int wait = 0, i = 0, cur = 0, max = 0, ans = -1;
	while (wait > 0 || i < customers.length) {
		if (i < customers.length) wait += customers[i];
		i++;
		cur += Math.min(wait, 4) * boardingCost - runningCost;
		wait = Math.max(wait - 4, 0);
		if (cur > max) {
			max = cur;
			ans = i;
		}
	}
	return ans;
}
```

#### .02

🔴N/A / [466. 统计重复个数](https://leetcode.cn/problems/count-the-repetitions/) / #动态规划 

```java
public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
	int l1 = s1.length(), l2 = s2.length();
	int[][] d = new int[l2][0];
	for (int i = 0; i < l2; i++) {
		int j = i, cnt = 0;
		for (int k = 0; k < l1; k++) {
			if (s1.charAt(k) == s2.charAt(j) && ++j == l2) {
				j = 0;
				cnt++;
			}
		}
		d[i] = new int[]{cnt, j};
	}
	int ans = 0;
	for (int j = 0; n1 > 0; n1--) {
		ans += d[j][0];
		j = d[j][1];
	}
	return ans / n2;
}
```

#### .03 ✨

🟡1454 / [2487. 从链表中移除节点](https://leetcode.cn/problems/remove-nodes-from-linked-list/) / #单调栈 #递归 

1. 单调栈

```java
public ListNode removeNodes(ListNode head) {
	Deque<ListNode> dq = new ArrayDeque<>();
	while (head != null) {
		while (!dq.isEmpty() && dq.peek().val < head.val) dq.poll();
		dq.push(head);
		head = head.next;
	}
	ListNode next = null;
	while (dq.size() > 1) {
		ListNode p = dq.poll();
		p.next = next;
		next = p;
	}
	head = dq.poll();
	head.next = next;
	return head;
}
```

2. 递归

```java
public ListNode removeNodes(ListNode head) {
	if (head == null) return null;
	ListNode node = removeNodes(head.next);
	if (node == null || head.val >= node.val) {
		head.next = node;
		return head;
	}
	return node;
}
```

```java
public ListNode removeNodes(ListNode head) {
	if (head == null || head.next == null) return head;
	head.next = removeNodes(head.next);
	if (head.next != null && head.val < head.next.val) return head.next;
	return head;
}
```

#### .04 ✨

🟡1718 / [2397. 被列覆盖的最多行数](https://leetcode.cn/problems/maximum-rows-covered-by-columns/) / #暴力 #位运算 

```java
public int maximumRows(int[][] mat, int numSelect) {
	int rows = mat.length, cols = mat[0].length;
	int[] mask = new int[rows];
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			mask[i] |= mat[i][j] << j;
		}
	}
	int ans = 0;
	for (int sub = 0; sub < (1 << cols); sub++) {
		if (Integer.bitCount(sub) == numSelect) {
			int cur = 0;
			for (int row : mask) {
				if ((row & sub) == row) cur++;
			}
			ans = Math.max(cur, ans);
		}
	}
	return ans;
}
```

#### .05 ✨✨

🔴2104 / [1944. 队列中可以看到的人数](https://leetcode.cn/problems/number-of-visible-people-in-a-queue/) / #单调栈 

```java
public int[] canSeePersonsCount(int[] heights) {
	Deque<Integer> stk = new ArrayDeque<>();
	int[] ans = new int[heights.length];
	for (int i = heights.length - 1; i >= 0; i--) {
		while (!stk.isEmpty() && stk.peek() < heights[i]) {
			stk.pop();
			ans[i]++;
		}
		if (!stk.isEmpty()) ans[i]++;
		stk.push(heights[i]);
	}
	return ans;
}
```

#### .06

🟡1279 / [2807. 在链表中插入最大公约数](https://leetcode.cn/problems/insert-greatest-common-divisors-in-linked-list/) / #链表 #数学 

```java
public ListNode insertGreatestCommonDivisors(ListNode head) {
	ListNode dummy = new ListNode(0, head);
	while (head.next != null) {
		int a = head.val, b = head.next.val;
		ListNode next = head.next;
		head.next = new ListNode(gcd(a, b));
		head.next.next = next;
		head = next;
	}
	return dummy.next;
}

private int gcd(int x, int y) {
	return y == 0 ? x : gcd(y, x % y);
}
```

```java
public ListNode insertGreatestCommonDivisors(ListNode head) {
	for (ListNode cur = head; cur.next != null; cur = cur.next.next) {
		cur.next = new ListNode(gcd(cur.val, cur.next.val), cur.next);
	}
	return head;
}

private int gcd(int x, int y) {
	return y == 0 ? x : gcd(y, x % y);
}
```

#### .07

🟢N/A / [383. 赎金信](https://leetcode.cn/problems/ransom-note/) / #字符串 

```java
public boolean canConstruct(String ransomNote, String magazine) {
	if (ransomNote.length() > magazine.length()) return false;
	int[] count = new int[26];
	for (char c : magazine.toCharArray()) count[c - 'a']++;
	for (char c : ransomNote.toCharArray()) {
		count[c - 'a']--;
		if (count[c - 'a'] < 0) return false;
	}
	return true;
}
```

#### .08

🟡N/A / [447. 回旋镖的数量](https://leetcode.cn/problems/number-of-boomerangs/) / #数学 #哈希表 

```java
public int numberOfBoomerangs(int[][] points) {
	Map<Integer, Integer> count = new HashMap<>();
	int ans = 0;
	for (int[] a : points) {
		count.clear();
		for (int[] b : points) {
			ans += 2 * (count.merge((a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]), 1, Integer::sum) - 1);
		}
	}
	return ans;
}
```

#### .09 ✨

🟡1735 / [2707. 字符串中的额外字符](https://leetcode.cn/problems/extra-characters-in-a-string/) / #动态规划 

```java
public int minExtraChar(String s, String[] dictionary) {
	Set<String> set = new HashSet<>();
	Collections.addAll(set, dictionary);
	int n = s.length();
	int[] dp = new int[n + 1];
	for (int i = 1; i <= n; i++) {
		dp[i] = dp[i - 1] + 1;
		for (int j = 0; j < i; j++) {
			if (dp[j] < dp[i] && set.contains(s.substring(j, i))) dp[i] = dp[j];
		}
	}
	return dp[n];
}
```

#### .10

🟢1282 / [2696. 删除子串后的字符串最小长度](https://leetcode.cn/problems/minimum-string-length-after-removing-substrings/) / #字符串 

1. 暴力

```java
/**
 * 暴力
 * Somnia1337
 */
public int minLength(String s) {
	boolean v;
	do {
		v = false;
		for (int i = 0; i < s.length() - 1; i++) {
			String sub = s.substring(i, i + 2);
			if (sub.equals("AB") || sub.equals("CD")) {
				s = s.substring(0, i) + s.substring(i + 2);
				v = true;
				break;
			}
		}
	} while (v);
	return s.length();
}
```

```java
/**
 * 暴力
 * 灵茶山艾府
 */
public int minLength(String s) {
	while (s.contains("AB") || s.contains("CD")) s = s.replace("AB", "").replace("CD", "");
	return s.length();
}
```

2. 栈

```java
/**
 * 栈
 * 灵茶山艾府
 */
public int minLength(String s) {
	Deque<Character> stk = new ArrayDeque<>();
	for (char c : s.toCharArray()) {
		if (!stk.isEmpty() && (c == 'B' && stk.peek() == 'A' || c == 'D' && stk.peek() == 'C')) stk.pop();
		else stk.push(c);
	}
	return stk.size();
}
```

#### .11 ✨

🟡1477 / [2645. 构造有效字符串的最少插入数](https://leetcode.cn/problems/minimum-additions-to-make-valid-string/) / #贪心 

```java
public int addMinimum(String word) {
	int l = word.length(), cnt = 1;
	for (int i = 0; i < l - 1; i++) {
		if (word.charAt(i + 1) <= word.charAt(i)) cnt++;
	}
	return cnt * 3 - word.length();
}
```

#### .12

🟢1307 / [2085. 统计出现过一次的公共字符串](https://leetcode.cn/problems/count-common-words-with-one-occurrence/) / #哈希表 

```java
public int countWords(String[] words1, String[] words2) {
	Map<String, Integer> count1 = new HashMap<>(), count2 = new HashMap<>();
	for (String w1 : words1) count1.merge(w1, 1, Integer::sum);
	for (String w2 : words2) count2.merge(w2, 1, Integer::sum);
	int ans = 0;
	for (String w1 : count1.keySet()) {
		if (count1.get(w1) == 1 && count2.getOrDefault(w1, 0) == 1) ans++;
	}
	return ans;
}
```

#### .13

🟡1680 / [2182. 构造限制重复的字符串](https://leetcode.cn/problems/construct-string-with-repeat-limit/) / #贪心 

```java
public String repeatLimitedString(String s, int repeatLimit) {
	int[] count = new int[26];
	for (char c : s.toCharArray()) count[c - 'a']++;
	StringBuilder ans = new StringBuilder();
	for (int i = 25, j = 24; i >= 0; i--) {
		j = Math.min(i - 1, j);
		while (true) {
			int r = Math.min(count[i], repeatLimit);
			ans.append(String.valueOf((char) ('a' + i)).repeat(r));
			if ((count[i] -= r) == 0) break;
			while (j >= 0 && count[j] == 0) j--;
			if (j < 0) break;
			ans.append((char) ('a' + j));
			count[j]--;
		}
	}
	return ans.toString();
}
```

#### .14

🟢N/A / [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/) / #链表 

```java
public ListNode deleteDuplicates(ListNode head) {
	if (head == null || head.next == null) return head;
	head.next = deleteDuplicates(head.next);
	return head.val == head.next.val ? head.next : head;
}
```

#### .15

🟡N/A / [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/) / #链表 

```java
public ListNode deleteDuplicates(ListNode head) {
	if (head == null) return null;
	else if (head.next != null && head.val == head.next.val) {
		while (head.next != null && head.val == head.next.val) head = head.next;
		return deleteDuplicates(head.next);
	}
	else head.next = deleteDuplicates(head.next);
	return head;
}
```

#### .16

🔴2354 / [2719. 统计整数数目](https://leetcode.cn/problems/count-of-integers/) / #动态规划 

```java
private final int MOD = 1000000007;
private int[][] mem;

public int count(String num1, String num2, int minSum, int maxSum) {
	int n = num2.length();
	num1 = "0".repeat(n - num1.length()) + num1;
	mem = new int[n][Math.min(9 * n, maxSum) + 1];
	for (int[] row : mem) Arrays.fill(row, -1);
	return dfs(0, 0, true, true, num1.toCharArray(), num2.toCharArray(), minSum, maxSum);
}

private int dfs(int i, int sum, boolean limL, boolean limR, char[] n1, char[] n2, int L, int R) {
	if (sum > R) return 0;
	if (i == n2.length) return sum >= L ? 1 : 0;
	if (!limL && !limR && mem[i][sum] != -1) return mem[i][sum];
	
	int l = limL ? n1[i] - '0' : 0, r = limR ? n2[i] - '0' : 9, ret = 0;
	for (int d = l; d <= r; d++) ret = (ret + dfs(i + 1, sum + d, limL && d == l, limR && d == r, n1, n2, L, R)) % MOD;
	if (!limL && !limR) mem[i][sum] = ret;
	return ret;
}
```

#### .17

🟢1405 / [2744. 最大字符串配对数目](https://leetcode.cn/problems/find-maximum-number-of-string-pairs/) / #字符串 

```java
public int maximumNumberOfStringPairs(String[] words) {
	boolean[][] vis = new boolean[26][26];
	int ans = 0;
	for (String s : words) {
		int x = s.charAt(0) - 'a', y = s.charAt(1) - 'a';
		if (vis[y][x]) ans++;
		else vis[x][y] = true;
	}
	return ans;
}
```

#### .18

🟡1748 / [2171. 拿出最少数目的魔法豆](https://leetcode.cn/problems/removing-minimum-number-of-magic-beans/) / #前缀和 #数学 

```java
public long minimumRemoval(int[] beans) {
	Arrays.sort(beans);
	long sum = 0;
	for (int bean : beans) sum += bean;
	long ans = sum;
	for (int i = 0; i < beans.length; i++) {
		ans = Math.min(sum - (long) beans[i] * (beans.length - i), ans);
	}
	return ans;
}
```

#### .19

🔴2978 / [2809. 使数组和小于等于 x 的最少时间](https://leetcode.cn/problems/minimum-time-to-make-array-sum-at-most-x/) / #动态规划 

```java
public int minimumTime(List<Integer> nums1, List<Integer> nums2, int x) {
	int n = nums1.size(), s1 = 0, s2 = 0;
	int[][] join = new int[n][2];
	for (int i = 0; i < n; i++) {
		int a = nums1.get(i), b = nums2.get(i);
		s1 += join[i][0] = a;
		s2 += join[i][1] = b;
	}
	Arrays.sort(join, Comparator.comparingInt(a -> a[1]));
	int[] dp = new int[n + 1];
	for (int i = 0; i < n; i++) {
		int a = join[i][0], b = join[i][1];
		for (int j = i + 1; j > 0; j--) dp[j] = Math.max(dp[j], dp[j - 1] + a + b * j);
	}
	for (int t = 0; t <= n; t++) {
		if (s1 + s2 * t - dp[t] <= x) return t;
	}
	return -1;
}
```

#### .20

🟢1239 / [2788. 按分隔符拆分字符串](https://leetcode.cn/problems/split-strings-by-separator/) / #库函数 

```java
public List<String> splitWordsBySeparator(List<String> words, char separator) {
	List<String> ans = new ArrayList<>();
	for (String word : words) {
		for (String split : word.split(Pattern.quote(String.valueOf(separator)))) {
			if (!split.isEmpty()) ans.add(split);
		}
	}
	return ans;
}
```

#### .21

🔴N/A / [410. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/) / #二分查找 #动态规划 

```java
public int splitArray(int[] nums, int k) {
	int n = nums.length, max = nums[0];
	int[] pre = new int[n + 1];
	for (int i = 1; i < n + 1; i++) {
		pre[i] = pre[i - 1] + nums[i - 1];
		max = Math.max(nums[i - 1], max);
	}
	int l = max, r = pre[n];
	while (l <= r) {
		int m = l + r >> 1;
		if (check(pre, m) <= k) r = m - 1;
		else l = m + 1;
	}
	return l;
}

private int check(int[] pre, int max) {
	int pr = 0, ret = 1;
	for (int i = 1; i < pre.length; i++) {
		if (pre[i] - pre[pr] > max) {
			ret++;
			pr = i - 1;
		}
	}
	return ret;
}
```

```java
public int splitArray(int[] nums, int m) {
	int n = nums.length;
	int[] pre = new int[n + 1];
	for (int i = 1; i < n + 1; i++) pre[i] = pre[i - 1] + nums[i - 1];
	int[][] dp = new int[n + 1][m + 1];
	for (int i = 0; i < n + 1; i++) Arrays.fill(dp[i], Integer.MAX_VALUE);
	dp[0][0] = 0;
	for (int i = 1; i < n + 1; i++) {
		for (int j = 1; j < Math.min(i + 1, m + 1); j++) {
			for (int k = 0; k < i; k++) {
				dp[i][j] = Math.min(Math.max(dp[k][j - 1], pre[i] - pre[k]), dp[i][j]);
			}
		}
	}
	return dp[n][m];
}
```

#### .22

🟡N/A / [670. 最大交换](https://leetcode.cn/problems/maximum-swap/) / #贪心 

```java
public int maximumSwap(int num) {
	char[] chs = Integer.toString(num).toCharArray();
	int maxIdx = chs.length - 1, p = -1, q = 0;
	for (int i = chs.length - 2; i >= 0; i--) {
		if (chs[i] > chs[maxIdx]) maxIdx = i;
		else if (chs[i] < chs[maxIdx]) {
			p = i;
			q = maxIdx;
		}
	}
	if (p == -1) return num;
	swap(chs, p, q);
	return Integer.parseInt(new String(chs));
}

private void swap(char[] arr, int i, int j) {
	char t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### .23

🟢1581 / [2765. 最长交替子数组](https://leetcode.cn/problems/longest-alternating-subarray/) / #暴力 

```java
public int alternatingSubarray(int[] nums) {
	int ans = -1;
	for (int i = 1; i < nums.length; i++) {
		if (nums[i] - nums[i - 1] == 1) {
			int it = i + 1, tar = nums[i - 1] + nums[i], cur = 2;
			while (it < nums.length && nums[it] + nums[it - 1] == tar) {
				it++;
				cur++;
			}
			ans = Math.max(cur, ans);
			i = it - 1;
		}
	}
	return ans;
}
```

#### .24

🟡1519 / [2865. 美丽塔 I](https://leetcode.cn/problems/beautiful-towers-i/) / #单调栈 

```java
public long maximumSumOfHeights(List<Integer> maxHeights) {
	long ans = 0;
	for (int i = 0; i < maxHeights.size(); i++) {
		ans = Math.max(solve(maxHeights, i), ans);
	}
	return ans;
}

private long solve(List<Integer> h, int idx) {
	int peak = h.get(idx);
	long ret = peak;
	int l = peak, r = peak;
	for (int i = idx - 1; i >= 0; i--) {
		l = Math.min(h.get(i), l);
		ret += l;
	}
	for (int i = idx + 1, size = h.size(); i < size; i++) {
		r = Math.min(h.get(i), r);
		ret += r;
	}
	return ret;
}
```

#### .25

🟢1218 / [2859. 计算 K 置位下标对应元素的和](https://leetcode.cn/problems/sum-of-values-at-indices-with-k-set-bits/) / #库函数 

```java
public int sumIndicesWithKSetBits(List<Integer> nums, int k) {
	int ans = 0;
	for (int i = 0; i < nums.size(); i++) {
		if (Integer.bitCount(i) == k) ans += nums.get(i);
	}
	return ans;
}
```

#### .26

🔴2508 / [2846. 边权重均等查询](https://leetcode.cn/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/) / #搜索 

```java
private int[] parent, leftMin, rightMin, pos;
private int[][] blockMin;

private int id = 0, size, maxWeight = 0;
private int index;
private int[] edge, next, head, weight;

public int[] minOperationsQueries(int n, int[][] edges, int[][] queries) {
	int m = (n << 1) - 1;
	edge = new int[m];
	weight = new int[m];
	next = new int[m];
	head = new int[n];
	for (int i = 0; i < n; ++i)
		head[i] = -1;
	index = 0;
	for (int[] edge : edges) {
		int u = edge[0], v = edge[1], w = edge[2] - 1;
		add(u, v, w);
		add(v, u, w);
		maxWeight = Math.max(maxWeight, w);
	}
	pos = new int[n];
	parent = new int[m];
	int[] depth = new int[n];
	int[][] weightCount = new int[n][maxWeight + 1];
	dfs(0, 0, 0, depth, weightCount);
	size = (int) Math.sqrt(m);
	int blocks = (m - 1) / size + 1;
	leftMin = new int[m];
	rightMin = new int[m];
	blockMin = new int[blocks][blocks];
	for (int i = 0; i < blocks; ++i) {
		int l = i * size;
		int r = Math.min(m, l + size) - 1;
		int min = leftMin[l] = parent[l];
		for (int j = l; j <= r; ++j) leftMin[j] = min = min(min, parent[j]);
		min = rightMin[r] = parent[r];
		for (int j = r; j >= l; --j) rightMin[j] = min = min(min, parent[j]);
		min = blockMin[i][i] = rightMin[l];
		for (int j = 0; j < i; ++j) blockMin[j][i] = min(min, blockMin[j][i - 1]);
	}

	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; ++i) {
		int u = queries[i][0], v = queries[i][1], l = lca(u, v);
		int maxWeightCount = 0;
		for (int k = 0; k <= maxWeight; ++k)
			maxWeightCount = Math.max(maxWeightCount, weightCount[u][k] + weightCount[v][k] - (weightCount[l][k] << 1));
		ans[i] = depth[u] + depth[v] - (depth[l] << 1) - maxWeightCount;
	}
	return ans;
}

private void add(int u, int v, int w) {
	edge[index] = v;
	weight[index] = w;
	next[index] = head[u];
	head[u] = index++;
}

private void dfs(int u, int pre, int dep, int[] depth, int[][] weightCount) {
	pos[u] = id;
	parent[id++] = u;
	depth[u] = dep;
	for (int idx = head[u]; idx != -1; idx = next[idx]) {
		int v = edge[idx], w = weight[idx];
		if (v == pre) continue;
		if (maxWeight + 1 >= 0) System.arraycopy(weightCount[u], 0, weightCount[v], 0, maxWeight + 1);
		weightCount[v][w]++;
		dfs(v, u, dep + 1, depth, weightCount);
		parent[id++] = u;
	}
}

private int lca(int a, int b) {
	if (pos[a] > pos[b]) return lca(b, a);
	int blockA = pos[a] / size, blockB = pos[b] / size;
	if (blockA == blockB) {
		int min = a;
		for (int i = pos[a]; i <= pos[b]; ++i)
			min = min(min, parent[i]);
		return min;
	}
	int min = min(rightMin[pos[a]], leftMin[pos[b]]);
	return blockA + 1 == blockB ? min : min(min, blockMin[blockA + 1][blockB - 1]);
}

private int min(int a, int b) {
	return pos[a] > pos[b] ? b : a;
}
```

#### .27

🟡1981 / [2861. 最大合金数](https://leetcode.cn/problems/maximum-number-of-alloys) / #二分查找 

```java
public int maxNumberOfAlloys(int n, int k, int budget, List<List<Integer>> composition, List<Integer> stock, List<Integer> cost) {
	int ans = 0;
	for (int i = 0; i < k; i++) ans = Math.max(solve(composition.get(i), stock, cost, budget), ans);
	return ans;
}

private int solve(List<Integer> need, List<Integer> stock, List<Integer> cost, int bud) {
	int l = 0, r = 1000000000;
	while (l <= r) {
		int m = l + (r - l >> 1);
		long cur = 0;
		for (int i = 0; i < need.size(); i++) {
			long left = Math.max((long) need.get(i) * m - stock.get(i), 0);
			cur += left * cost.get(i);
		}
		if (cur <= bud) l = m + 1;
		else r = m - 1;
	}
	return r;
}
```

#### .28

🟡N/A / [365. 水壶问题](https://leetcode.cn/problems/water-and-jug-problem/) / #数学 

```java
public boolean canMeasureWater(int x, int y, int z) {
	if (x + y < z) return false;
	if (x == 0 || y == 0) return z == 0 || x + y == z;
	return z % gcd(x, y) == 0;
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### .29

🔴N/A / [514. 自由之路](https://leetcode.cn/problems/freedom-trail/) / #技巧 

```java
private char[] s, t;
private int[][] l, r, memo;

public int findRotateSteps(String ring, String key) {
	s = ring.toCharArray();
	t = key.toCharArray();
	int n = s.length, m = t.length;
	int[] pos = new int[26];
	for (int i = 0; i < n; i++) {
		s[i] -= 'a';
		pos[s[i]] = i;
	}
	l = new int[n][26];
	for (int i = 0; i < n; i++) {
		System.arraycopy(pos, 0, l[i], 0, 26);
		pos[s[i]] = i;
	}
	
	for (int i = n - 1; i >= 0; i--) pos[s[i]] = i;
	r = new int[n][26];
	for (int i = n - 1; i >= 0; i--) {
		System.arraycopy(pos, 0, r[i], 0, 26);
		pos[s[i]] = i;
	}
	
	memo = new int[m][n];
	for (int[] row : memo) Arrays.fill(row, -1);
	return dfs(0, 0) + m;
}

private int dfs(int j, int i) {
	if (j == t.length) return 0;
	if (memo[j][i] != -1) return memo[j][i];
	int c = t[j] - 'a';
	if (s[i] == c) return memo[j][i] = dfs(j + 1, i);
	int L = l[i][c], R = r[i][c];
	return memo[j][i] = Math.min(dfs(j + 1, L) + (L > i ? s.length - L + i : i - L), dfs(j + 1, R) + (R < i ? s.length - i + R : R - i));
}
```

#### .30

🟡1875 / [2808. 使循环数组所有元素相等的最少秒数](https://leetcode.cn/problems/minimum-seconds-to-equalize-a-circular-array/) / #暴力 

```java
public int minimumSeconds(List<Integer> nums) {
	int n = nums.size();
	Map<Integer, List<Integer>> pos = new HashMap<>();
	for (int i = 0; i < n; i++) {
		pos.computeIfAbsent(nums.get(i), k -> new ArrayList<>()).add(i);
	}
	
	int ans = n;
	for (List<Integer> p : pos.values()) {
		int maxDist = n - p.get(p.size() - 1) + p.get(0);
		for (int i = 1; i < p.size(); i++) {
			maxDist = Math.max(p.get(i) - p.get(i - 1), maxDist);
		}
		ans = Math.min(maxDist >> 1, ans);
	}
	return ans;
}
```

#### .31

🟢1267 / [2670. 找出不同元素数目差数组](https://leetcode.cn/problems/find-the-distinct-difference-array/) / #前后缀分解 

```java
public int[] distinctDifferenceArray(int[] nums) {
	int n = nums.length, cnt = 0;
	boolean[] vis = new boolean[51];
	int[] pre = new int[n], suf = new int[n], ans = new int[n];
	for (int i = n - 2; i >= 0; i--) {
		suf[i] = cnt += !vis[nums[i + 1]] ? 1 : 0;
		vis[nums[i + 1]] = true;
	}
	vis = new boolean[51];
	cnt = 0;
	for (int i = 0; i < n; i++) {
		pre[i] = cnt += !vis[nums[i]] ? 1 : 0;
		vis[nums[i]] = true;
		ans[i] = pre[i] - suf[i];
	}
	return ans;
}
```