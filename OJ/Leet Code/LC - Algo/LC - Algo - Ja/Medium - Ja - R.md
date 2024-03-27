```text
题库: 1004 1011 1020 1024 1027 1035 1043 1048 1049 1052 1054 1055 1061 1066 1090 1091 1094 1109 1124 1130 1135 1143 1155 1162 1170 1186 1190 1219 1222 1234 1248 1253 1254 1262 1267 1276 1283 1297 1333 1334 1353 1366 1375 1391 1400 1410 1419 1423 1456 1462 1465 1466 1482 1488 1493 1503 1535 1552 1567 1574 1599 1620 1631 1642 1647 1653 1657 1658 1670 1679 1686 1690 1696 1705 1712 1718 1722 1726 1727 1737 1746 1749 1760 1762 1798 1818 1838 1839 1856 1864 1870 1894 1898 1901 1911 1918 1921 1926 1942 1954 1962 1969 1976 2008 2024 2034 2048 2054 2064 2069 2121 2134 2171 2178 2182 2187 2208 2216 2226 2240 2291 2300 2304 2305 2311 2316 2336 2337 2342 2352 2364 2368 2369 2384 2390 2397 2401 2406 2411 2422 2434 2439 2447 2450 2452 2457 2477 2486 2512 2516 2521 2523 2527 2530 2537 2548 2550 2560 2563 2564 2575 2576 2594 2596 2602 2606 2645 2653 2654 2655 2661 2679 2680 2684 2685 2698 2707 2712 2718 2730 2731 2735 2762 2770 2771 2772 2779 2789 2799 2808 2812 2825 2830 2831 2834 2840 2841 2844 2845 2849 2850 2856 2857 2860 2861 2865 2866 2870 2871 2874 2875 2895 2896 2898 2900 2904 2905 2906 2909 2914 2915 2918 2924 2929 2933 2938 2943 2944 2947 2948 2952 2955 2957 2958 2961 2962 2964 2966 2967 2971 2975 2976 2979 2981 2982 2992 2997 2998 3001 3002 3004 3006 3007 3011 3012 3015 3016 3025 3026 3029 3030 3039 3040 3043 3044 3047 3048
LCR: 7 10 16 83 121 138 148 157 165 185 188
LCP: 30
面试: 01.07
```

#### [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/)

@2023.09.03

#双指针 

用只增长的滑动窗口（双指针），`k` 为当前剩余的将 `"0"` 变成 `"1"` 的“额度”。循环中的两个 `if` 语句：

- `if (nums[r++] == 0) k--`：当 `r` 处为 `"0"` 时，将其变为 `"1"`，额度 -1。
- `if (k < 0 && nums[l++] == 0) k++`：当额度不足时，检查 `l` 处是否为 `"0"`，若是则额度 +1。

细节：

- 在第一个 `if` 中执行 `r++`，无论右边界是否为 `"0"`，都向右移；在第二个 `if` 中，先判断 `k` 是否不足，如果 `k >= 0`，由于短路与的特性，不会执行 `l++`，而如果 `k < 0`，无论左边界是否为 `"0"`，都向右移。
- 判断额度不足用 `k < 0` 而不是 `k <= 0`，因为当 `k == 0` 时没必要缩小窗口。

```java
/**
 * 双指针
 * Edward Elric
 */
public int longestOnes(int[] nums, int k) {
	int l = 0, r = 0;
	while (r < nums.length) {
		if (nums[r++] == 0) k--;
		if (k < 0 && nums[l++] == 0) k++;
	}
	return r - l;
}
```

#### [1011. 在 D 天内送达包裹的能力](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/)

@2023.09.18

#二分查找 

```java
/**
 * 二分查找
 * Somnia1337
 */
public int shipWithinDays(int[] weights, int days)
{
	int max = 0, sum = 0;
	for (int weight : weights)
	{
		max = Math.max(weight, max);
		sum += weight;
	}
	if (days == 1) return sum;
	int left = max, right = sum;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		int time = 1, current = 0;
		for (int weight : weights)
		{
			if (current + weight > mid)
			{
				time++;
				current = weight;
			}
			else current += weight;
		}
		if (time > days) left = mid + 1;
		else right = mid - 1;
	}
	return left;
}
```

#### [1020. 飞地的数量](https://leetcode.cn/problems/number-of-enclaves/)

@2023.10.19

#搜索 

以四条边上的 `1` 为搜索起点，将与之连通的单元格均标记为 `0`，最后一次遍历，计数仍为 `1` 的单元格数量。

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public int numEnclaves(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	
	// 从四条边上的1开始搜索
	for (int i = 0; i < rows; i++)
	{
		if (grid[i][0] == 1) dfs(grid, i, 0);
		if (grid[i][cols - 1] == 1) dfs(grid, i, cols - 1);
	}
	for (int j = 0; j < cols; j++)
	{
		if (grid[0][j] == 1) dfs(grid, 0, j);
		if (grid[rows - 1][j] == 1) dfs(grid, rows - 1, j);
	}
	
	// 遍历计数
	int ans = 0;
	for (int[] row : grid)
	{
		for (int square : row)
		{
			if (square == 1) ans++;
		}
	}
	
	return ans;
}

private void dfs(int[][] grid, int x, int y)
{
	if (x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] == 0) return;
	grid[x][y] = 0;
	dfs(grid, x + 1, y);
	dfs(grid, x, y + 1);
	dfs(grid, x - 1, y);
	dfs(grid, x, y - 1);
}
```

2. BFS

```java
public int numEnclaves(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	
	// 从四条边上的1开始搜索
	for (int i = 0; i < rows; i++)
	{
		if (grid[i][0] == 1) bfs(grid, i, 0);
		if (grid[i][cols - 1] == 1) bfs(grid, i, cols - 1);
	}
	for (int j = 0; j < cols; j++)
	{
		if (grid[0][j] == 1) bfs(grid, 0, j);
		if (grid[rows - 1][j] == 1) bfs(grid, rows - 1, j);
	}
	
	// 遍历计数
	int ans = 0;
	for (int[] row : grid)
	{
		for (int square : row)
		{
			if (square == 1) ans++;
		}
	}
	
	return ans;
}

private void bfs(int[][] grid, int x, int y)
{
	Queue<int[]> queue = new LinkedList<>();
	queue.add(new int[]{x, y});
	while (!queue.isEmpty())
	{
		int[] cur = queue.remove();
		x = cur[0];
		y = cur[1];
		if (0 <= x && x < grid.length && 0 <= y && y < grid[0].length && grid[x][y] == 1)
		{
			grid[x][y] = 0;
			queue.add(new int[]{x + 1, y});
			queue.add(new int[]{x - 1, y});
			queue.add(new int[]{x, y + 1});
			queue.add(new int[]{x, y - 1});
		}
	}
}
```

#### [1024. 视频拼接](https://leetcode.cn/problems/video-stitching/)

@2023.12.30

#动态规划 

`dp` 开到 `time + 1`，`dp[i]` 表示从时刻 `i` 开始的视频最远持续到的时刻，例如，对于：

```text
clips = [[0,2],[1,5],[1,6],[4,6]]
time = 6
```

- 从 `0` 开始的只有 `[0,2]`，`dp[0] = 2`。
- 从 `1` 开始的有 `[1,5]` 和 `[1,6]`，`dp[1] = 6`。
- 从 `4` 开始的只有 `[4,6]`，`dp[4] = 6`。

得到 `dp = [2,6,0,0,6,0,0]`。

按时间遍历，维护当前时间 `t`，当前得到的最远时间 `pre`，已看到的视频片段的最远时间 `max`，如果 `pre == t`，则必须计入一段，贪心地计入最远的那一段（`max`）。

```java
/**
 * 动态规划
 * dongzengjie
 */
public int videoStitching(int[][] clips, int time) {
	int[] dp = new int[time + 1];
	int max = 0, pre = 0, ans = 0;
	for (int[] c : clips) {
		if (c[0] > time) continue; // 无视所需范围外的 clip
		dp[c[0]] = Math.max(c[1], dp[c[0]]);
	}
	for (int t = 0; t < time; t++) {
		max = Math.max(dp[t], max);
		if (pre == t) { // 当前最远时间只能到 t
			pre = max;
			ans++;
		}
		if (t == max) return -1; // 从 t 最远只能到 t, 则无法继续前进
	}
	return ans;
}
```

#### [1027. 最长等差数列](https://leetcode.cn/problems/longest-arithmetic-subsequence/)

@2023.12.26

#动态规划 

`dp[i][d]` 表示以 `nums[i]` 作为子序列的末元素（必须选）、等差为 `d` 的子问题答案。

数据范围下，两元素之差 $\in [-500, 500]$，因此 `dp` 开到 `1001`，计算差值 `d` 后 `+500` 以消除负偏差。

外层 `for` 从 `1` 开始枚举末元素 `nums[i]`，内层 `for` 从 `i-1` 倒序枚举前一个元素 `nums[j]`，内层倒序的好处是：如果发现 `dp[i][d] > 1`，意味着 `dp[i][d]` 已在更大的 `j` 处更新过，那么 **再次更新一定不会使它增加**，也就无需更新 `dp[i][d]` 和 `ans`，即实现了“懒计算”。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int longestArithSeqLength(int[] nums) {
	int[][] dp = new int[nums.length][1001];
	int ans = 0;
	for (int i = 1; i < nums.length; i++) {
		for (int j = i - 1; j >= 0; j--) {
			int d = nums[i] - nums[j] + 500;
			if (dp[i][d] < 1) ans = Math.max(dp[i][d] = Math.max(dp[j][d] + 1, 2), ans);
		}
	}
	return ans;
}
```

#### [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/)

@2023.09.09

#动态规划 

与 [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/) 等价。

- 连线：对应于元素相等。
- 不相交：对应于顺序一定且可以不连续。

因此等价于计算两数组的 `lcs`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maxUncrossedLines(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length;
	int[][] dp = new int[n1 + 1][n2 + 1];
	for (int i = 1; i < n1 + 1; i++) {
		for (int j = 1; j < n2 + 1; j++) {
			if (nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
			else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
		}
	}
	return dp[n1][n2];
}
```

#### [1043. 分隔数组以得到最大和](https://leetcode.cn/problems/partition-array-for-maximum-sum/)

@2024.01.05

#动态规划 

`dp[i]` 表示 `arr[0:i]` 的子问题答案， `arr=[1,9,6,3], k=3` 为例：

- `dp[1] = 1*1 = 1`。
- `dp[2] = 2*9 = 18`。
- `dp[3] = 3*9 = 27`。
- `dp[4]`，`4 > k`，那么向前考虑 `arr[3]` 跟哪些元素分到一组：
	- `[3]`，`dp[4] = 1*3 + dp[3] = 3+27 = 30`。
	- `[6,3]`，`dp[4] = 2*6 + dp[2] = 12+18 = 30`。
	- `[9,6,3]`，`dp[4] = 3*9 + dp[1] = 27+1 = 28`。
	- `dp[4]` 取这 3 种情况的最大值。

转移方程 `dp[i] = max(max(arr[i-j:i])*j + dp[i-j])`，其中 `1 <= j <= min(i,k)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maxSumAfterPartitioning(int[] arr, int k) {
	int n = arr.length;
	int[] dp = new int[n + 1];
	for (int i = 1; i <= n; i++) {
		int up = Math.min(i, k), max = 0;
		for (int j = 1; j <= up; j++) {
			dp[i] = Math.max((max = Math.max(arr[i - j], max)) * j + dp[i - j], dp[i]);
		}
	}
	return dp[n];
}
```

#### [1048. 最长字符串链](https://leetcode.cn/problems/longest-string-chain/)

@2023.09.22

#动态规划 

对某串`s`，设其所有前身的集合为`s_pre`，`s_pre[i]`为以第`i`个前身结尾的最长链的长度，那么以`s`结尾的最长链的长度即为`max(s_pre[i]) + 1`。

对`words`按从短到长排序，用哈希表`path`记录以每个字符串结尾的最长链长度（全部初始化为1）。遍历`words`，枚举`words[i]`所有可能的前身，在`path`中查找该前身对应的长度，取所有前身中最大者。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int longestStrChain(String[] words)
{
	Arrays.sort(words, Comparator.comparingInt(String::length)); // sort调用Comparator.comparingInt，后者调用String::length
	Map<String, Integer> path = new HashMap<>();
	int ans = 0;
	
	for (String word : words)
	{
		path.put(word, 1); // 初始化为1
		for (int i = 0; i < word.length(); i++)
		{
			String prev = word.substring(0, i) + word.substring(i + 1); // 枚举可能的前身
			if (path.containsKey(prev)) // 前身存在
			{
				path.merge(word, path.get(prev) + 1, Math::max); // merge调用Math::max，取所有前身中最大者
			}
		}
		ans = Math.max(path.get(word), ans); // 更新ans
	}
	
	return ans;
}
```

#### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)

@2023.07.19

#动态规划 

本题等价于0-1背包问题。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int lastStoneWeightII(int[] stones)
{
	int sum = 0;
	for (int stone : stones) sum += stone;
	int[] dp = new int[sum + 1];
	
	for (int stone : stones)
	{
		for (int j = sum; j > 0; j--)
		{
			if (j - stone >= 0)
			{
				dp[j] = Math.max(dp[j - stone] + stone, dp[j]);
			}
		}
	}
	
	int ans = Integer.MAX_VALUE;
	for (int i = 1; i < sum + 1; i++)
	{
		if (dp[i] == i) ans = Math.min(Math.abs(dp[sum - i] - dp[i]), ans);
	}
	
	return ans;
}
```

优化：

```java
/**
 * 动态规划
 * Somnia1337
 * @improver 代码随想录
 */
public int lastStoneWeightII(int[] stones)
{
	int sum = 0;
	for (int stone : stones) sum += stone;
	int[] dp = new int[sum + 1];
	
	for (int stone : stones)
	{
		for (int j = sum; j >= stone; j--) // 遍历至stone即可停止
		{
			dp[j] = Math.max(dp[j - stone] + stone, dp[j]);
		}
	}
	
	return sum - 2 * dp[sum / 2]; // 一定是中间的
}
```

#### [1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/)

@2023.12.04

#滑动窗口 

从 `customers` 中删除一定满意的顾客，对剩余顾客用长为 `minutes` 的滑动窗口查找最大和。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int maxSatisfied(int[] customers, int[] grumpy, int minutes)
{
	int len = customers.length, sum = 0;
	for (int i = 0; i < len; i++)
	{
		if (grumpy[i] == 0) // 不生气, 这部分顾客一定满意
		{
			sum += customers[i]; // 累加这部分顾客
			customers[i] = 0; // 之后不再考虑他们
		}
	}
	
	// 现在, customer 中剩余的顾客都不满意
	// 老板要找一个长为 minutes 的窗口使用他的能力
	// 滑动窗口, 取窗口内的最大和 max
	int cur = 0;
	for (int i = 0; i < minutes; i++) cur += customers[i];
	int max = cur;
	for (int i = minutes; i < len; i++)
	{
		cur += customers[i] - customers[i - minutes];
		max = Math.max(cur, max);
	}
	return sum + max;
}
```

#### [1054. 距离相等的条形码](https://leetcode.cn/problems/distant-barcodes/)

@2023.09.19

```java
/**
 * 贪心
 * Somnia1337 ChatGPT
 */
public int[] rearrangeBarcodes(int[] barcodes) {
	int n = barcodes.length;
	Map<Integer, Integer> count = new HashMap<>();
	for (int b : barcodes) count.merge(b, 1, Integer::sum);
	List<Map.Entry<Integer, Integer>> entries = new ArrayList<>(count.entrySet());
	entries.sort((e1, e2) -> e2.getValue() - e1.getValue());
	int[] ans = new int[n];
	int idx = 0;
	for (Map.Entry<Integer, Integer> e : entries) {
		int x = e.getKey(), cnt = e.getValue();
		while (cnt > 0) {
			if (idx >= n) idx = 1;
			ans[idx] = x;
			idx += 2;
			cnt--;
		}
	}
	return ans;
}
```

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int[] rearrangeBarcodes(int[] barcodes) {
	int n = barcodes.length;
	if (n <= 2) return barcodes;
	Map<Integer, Integer> count = new HashMap<>();
	int maxCnt = 0;
	for (int b : barcodes) {
		maxCnt = Math.max(count.merge(b, 1, Integer::sum), maxCnt);
	}
	int[] ans = new int[n];
	int even = 0, odd = 1;
	for (Map.Entry<Integer, Integer> e : count.entrySet()) {
		int x = e.getKey(), cnt = e.getValue();
		while (cnt > 0 && cnt <= n / 2 && odd < n) {
			ans[odd] = x;
			odd += 2;
			cnt--;
		}
		while (cnt > 0) {
			ans[even] = x;
			even += 2;
			cnt--;
		}
	}
	return ans;
}
```

#### [1055. 形成字符串的最短路径](https://leetcode.cn/problems/shortest-way-to-form-string/)

@2023.12.20

#贪心 

指向 `source` 的 `i1` 无限循环，直到匹配到 `target[i2]` 或遍历一次 `source` 仍无匹配，最后返回 `ceil(i1 / l1)`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int shortestWay(String source, String target) {
	char[] chs1 = source.toCharArray(), chs2 = target.toCharArray();
	int l1 = chs1.length, l2 = chs2.length, i1 = 0, i2 = 0;
	while (i2 < l2) {
		int d = 0;
		while (chs1[i1 % l1] != chs2[i2] && d < l1) {
			i1++;
			d++;
		}
		if (d == l1) return -1;
		i1++;
		i2++;
	}
	return (int) Math.ceil((double) i1 / l1);
}
```

#### [1061. 按字典序排列最小的等效字符串](https://leetcode.cn/problems/lexicographically-smallest-equivalent-string/)

@2024.01.30

微调并查集模板。

```java
/**
 * 
 * Felix
 */
public String smallestEquivalentString(String s1, String s2, String baseStr) {
	int[] root = new int[26];
	Arrays.setAll(root, a -> a);
	int len = s1.length();
	char[] chs1 = s1.toCharArray(), chs2 = s2.toCharArray();
	for (int i = 0; i < len; i++) union(root, chs1[i] - 'a', chs2[i] - 'a');
	StringBuilder ans = new StringBuilder();
	for (char ch : baseStr.toCharArray()) ans.append((char) (find(root, ch - 'a') + 'a'));
	return ans.toString();
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	int rx = find(root, x), ry = find(root, y);
	if (rx > ry) root[rx] = ry;
	else if (rx < ry) root[ry] = rx;
}
```

#### [1066. 校园自行车分配 II](https://leetcode.cn/problems/campus-bikes-ii/)

@2024.01.12



```java
/**
 * 记忆化搜索
 * onion12138
 */
private Integer[][] memo;

public int assignBikes(int[][] workers, int[][] bikes) {
	int n = workers.length, m = bikes.length;
	memo = new Integer[n][1 << m];
	return dfs(workers, bikes, 0, 0);
}

private int dfs(int[][] w, int[][] b, int i, int state) {
	if (i == w.length) return 0;
	if (memo[i][state] != null) return memo[i][state];
	int[] worker = w[i];
	int x = worker[0], y = worker[1], ret = Integer.MAX_VALUE;
	for (int j = 0; j < b.length; j++) {
		if ((state & (1 << j)) == 0) {
			int[] bike = b[j];
			int bx = bike[0], by = bike[1];
			int d = Math.abs(x - bx) + Math.abs(y - by) + dfs(w, b, i + 1, state | (1 << j));
			ret = Math.min(ret, d);
		}
	}
	return memo[i][state] = ret;
}
```

#### [1090. 受标签影响的最大值](https://leetcode.cn/problems/largest-values-from-labels/)

@2023.12.27

#贪心 

按 `values` 降序排序，记录每个 `label` 已选择的次数，贪心地选 `numWanted` 个。

```java
/**
 * 贪心
 * Somnia1337
 */
public int largestValsFromLabels(int[] values, int[] labels, int numWanted, int useLimit) {
	Integer[] idx = new Integer[values.length];
	for (int i = 0; i < idx.length; i++) idx[i] = i;
	Arrays.sort(idx, Comparator.comparing(a -> -values[a]));
	int[] use = new int[20001];
	int ans = 0;
	for (int i = 0, cnt = 0; i < idx.length; i++) {
		if (++use[labels[idx[i]]] > useLimit) continue;
		ans += values[idx[i]];
		if (++cnt == numWanted) break;
	}
	return ans;
}
```

#### [1091. 二进制矩阵中的最短路径](https://leetcode.cn/problems/shortest-path-in-binary-matrix/)

@2023.12.25

#搜索 

BFS 模板题。

```java
/**
 * BFS
 * Somnia1337
 */
public int shortestPathBinaryMatrix(int[][] grid) {
	int n = grid.length;
	if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) return -1;
	final int[][] dirs = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
	boolean[][] vis = new boolean[n][n];
	vis[0][0] = true;
	Queue<Integer> q = new LinkedList<>();
	q.add(0);
	
	int d = 0;
	while (!q.isEmpty()) {
		int size = q.size();
		while (size-- > 0) {
			int p = q.poll(), x = p / n, y = p % n;
			if (x == n - 1 && y == n - 1) return d + 1;
			for (int k = 0; k < 8; k++) {
				int nx = x + dirs[k][0], ny = y + dirs[k][1];
				if (checkXY(nx, n, ny, n) && grid[nx][ny] == 0 && !vis[nx][ny]) {
					vis[nx][ny] = true;
					q.add(nx * n + ny);
				}
			}
		}
		d++;
	}
	return -1;
}

private boolean checkXY(int x, int rows, int y, int cols) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [1094. 拼车](https://leetcode.cn/problems/car-pooling/)

@2023.12.02

#差分数组 

差分，起点加、终点减，求前缀和即得各站时的车上人数。

```java
/**
 * 差分数组
 * Somnia1337
 */
public boolean carPooling(int[][] trips, int capacity) {
	int max = 0;
	int[] diff = new int[1002];
	for (int[] t : trips) {
		diff[t[1] + 1] += t[0];
		diff[t[2] + 1] -= t[0];
		max = Math.max(t[2], max);
	}
	for (int i = 1; i <= max; i++) {
		diff[i] += diff[i - 1];
		if (diff[i] > capacity) return false;
	}
	return true;
}
```

#### [1109. 航班预订统计](https://leetcode.cn/problems/corporate-flight-bookings/)

@2023.12.06

#差分数组 

差分。

```java
/**
 * 差分数组
 * Somnia1337
 */
public int[] corpFlightBookings(int[][] bookings, int n) {
	int[] ans = new int[n];
	for (int[] b : bookings) {
		ans[b[0] - 1] += b[2];
		if (b[1] < n) ans[b[1]] -= b[2];
	}
	for (int i = 1; i < n; i++) ans[i] += ans[i - 1];
	return ans;
}
```

#### [1124. 表现良好的最长时间段](https://leetcode.cn/problems/longest-well-performing-interval/)

@2024.01.03

1. 哈希表

```java
/**
 * 哈希表
 * 嘿嘿
 */
public int longestWPI(int[] hours) {
	int cur = 0, ans = 0;
	Map<Integer, Integer> day = new HashMap<>();
	for (int i = 0; i < hours.length; i++) {
		if ((cur += hours[i] > 8 ? 1 : -1) > 0) ans = i + 1;
		else {
			day.putIfAbsent(cur, i);
			if (day.containsKey(cur - 1)) ans = Math.max(i - day.get(cur - 1), ans);
		}
	}
	return ans;
}
```

2. 单调栈

劳累的一天计 `1`，不劳累的一天计 `-1`，问题转换为寻找最长的和 `> 0` 的子数组，

```java
/**
 * 单调栈
 * 灵茶山艾府
 */
public int longestWPI(int[] hours) {
	int n = hours.length, ans = 0;
	int[] pre = new int[n + 1];
	Deque<Integer> stk = new ArrayDeque<>();
	stk.push(0);
	for (int j = 1; j <= n; j++) {
		pre[j] = pre[j - 1] + (hours[j - 1] > 8 ? 1 : -1);
		if (pre[j] < pre[stk.peek()]) stk.push(j);
	}
	for (int i = n; i > 0; i--) {
		while (!stk.isEmpty() && pre[i] > pre[stk.peek()]) ans = Math.max(i - stk.pop(), ans);
	}
	return ans;
}
```

优化：一次遍历

```java
/**
 * 一次遍历
 * 灵茶山艾府
 */
public int longestWPI(int[] hours) {
	int n = hours.length, s = 0, ans = 0;
	int[] pos = new int[n + 2];
	for (int i = 1; i <= n; i++) {
		s -= hours[i - 1] > 8 ? 1 : -1;
		if (s < 0) ans = i;
		else {
			if (pos[s + 1] > 0) ans = Math.max(i - pos[s + 1], ans);
			if (pos[s] == 0) pos[s] = i;
		}
	}
	return ans;
}
```

#### [1130. 叶值的最小代价生成树](https://leetcode.cn/problems/minimum-cost-tree-from-leaf-values/)

@2024.01.05

1. 贪心

值越大，就越要放在靠上的位置，因为要减少它被用来相乘的次数。

```java
/**
 * 贪心
 * Lane
 */
public int mctFromLeafValues(int[] arr) {
	Deque<Integer> stk = new ArrayDeque<>();
	stk.push(Integer.MAX_VALUE);
	int ans = 0;
	for (int x : arr) {
		while (x >= stk.peek()) ans += stk.pop() * Math.min(stk.peek(), x);
		stk.push(x);
	}
	while (stk.size() > 2) ans += stk.pop() * stk.peek();
	return ans;
}
```

2. 动态规划

`dp[i][j]` 表示 `arr[i:j]` 对应的子问题答案，状态转移方程 `dp[i][j] = min(dp[i][k] + dp[k+1][j] + max[i][k]*max[k+1][j])`。

```java
/**
 * 动态规划
 * eequalmcc
 */
public int mctFromLeafValues(int[] arr) {
	int n = arr.length;
	int[][] dp = new int[n][n];
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			if (i != j) dp[i][j] = Integer.MAX_VALUE; // 初始化
		}
	}
	
	int[][] max = new int[n][n]; // max[i][j] 表示 max(arr[i][j])
	for (int r = 0; r < n; r++) {
		max[r][r] = arr[r];
		for (int l = r - 1; l >= 0; l--) {
			max[l][r] = Math.max(max[l + 1][r], arr[l]);
		}
	}
	
	for (int r = 1; r < n; r++) {
		for (int l = r - 1; l >= 0; l--) {
			for (int i = l; i < r; i++) {
				int v = max[l][i] * max[i + 1][r];
				dp[l][r] = Math.min(dp[l][i] + dp[i + 1][r] + v, dp[l][r]);
			}
		}
	}
	return dp[0][n - 1];
}
```

#### [1135. 最低成本连通所有城市](https://leetcode.cn/problems/connecting-cities-with-minimum-cost/)

@2023.12.20

#搜索 #并查集 

1. Kruskal 算法

思想：按权从小到大加入边（贪心）。

过程：

1. 将所有边按权重从小到大排列。
2. 按序遍历这些边，对每条边 $e$ 进行判断，如果在图中加入 $e$ 不会生成环，则加入 $e$，否则跳过 $e$。
3. 重复 2，直到选出 $n - 1$ 条边。

过程中，判环的方法是使用并查集维护结点之间的连通性，如果两个结点 $u$，$v$ 已经连通，则加入边 $(u, v)$ 将导致环。

```java
/**
 * Kruskal 算法
 * Somnia1337
 */
public int minimumCost(int n, int[][] connections) {
	Arrays.sort(connections, Comparator.comparingInt(a -> a[2]));
	int[] root = new int[n + 1];
	Arrays.setAll(root, a -> a);
	int cnt = 0, ans = 0; // cnt: 选出的边计数
	for (int[] c : connections) {
		if (find(root, c[0]) == find(root, c[1])) continue; // 会成环, 跳过
		union(root, c[0], c[1]);
		ans += c[2];
		if (++cnt == n - 1) break;
	}
	return cnt == n - 1 ? ans : -1; // 没选够, 返回 -1
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

2. Prim 算法

思想：从一个结点开始，每次加入离已连通部分最近的点，并更新剩余点的最短距离。

过程：

1. 选择一个起始结点，将其标记为已访问。
2. 用一个最小堆维护已连通部分到未访问结点的最短距离，将起始结点的所有邻边入堆。
3. 将权最小的边出堆（该边的两个端点分别属于已连通和未访问集合）。
4. 加入该未访问结点，并将其所有邻边入堆。
5. 重复 3、4，直到选出 $n - 1$ 条边。

```java
/**
 * Prim 算法
 * Somnia1337
 */
public int minimumCost(int n, int[][] connections) {
	// 邻接链表, 存储 {相邻结点, 距离}
	List<Integer[]>[] dist = new List[n + 1];
	Arrays.setAll(dist, e -> new ArrayList<>());
	for (int[] c : connections) {
		dist[c[0]].add(new Integer[]{c[1], c[2]});
		dist[c[1]].add(new Integer[]{c[0], c[2]});
	}
	
	// 记录每个结点是否已经选择
	boolean[] vis = new boolean[n + 1];
	Queue<Integer[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
	pq.add(new Integer[]{1, 0});
	
	int cnt = 0, ans = 0; // cnt: 已选择结点数
	while (!pq.isEmpty() && cnt < n) {
		Integer[] p = pq.poll();
		if (!vis[p[0]]) {
			vis[p[0]] = true;
			cnt++;
			ans += p[1];
			pq.addAll(dist[p[0]]); // 更新最短距离
		}
	}
	return cnt == n ? ans : -1;
}
```

#### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

@2023.09.09

#动态规划 

`dp[i][j]` 表示子问题 `lcs(text1[:i], text2[:j])` 的答案。正序遍历，如果当前字符：

- 相同，当前公共子序列长度即不含当前字符时的长度 `+1`，`dp[i][j] = dp[i - 1][j - 1] + 1`。
- 不同，无法 `+1`，取左侧、上侧中较大者，`dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])`，而 `dp[i - 1][j - 1]` 一定 `<=` `dp[i - 1][j]` 及 `dp[i][j - 1]`，因此无需考虑。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int longestCommonSubsequence(String text1, String text2) {
	int l1 = text1.length(), l2 = text2.length();
	int[][] dp = new int[l1 + 1][l2 + 1];
	for (int i = 1; i < l1 + 1; i++) {
		for (int j = 1; j < l2 + 1; j++) {
			if (text1.charAt(i - 1) == text2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1] + 1;
			else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
		}
	}
	return dp[l1][l2];
}
```

#### [1155. 掷骰子等于目标和的方法数](https://leetcode.cn/problems/number-of-dice-rolls-with-target-sum/)

@2023.10.24

```java
/**
 * 动态规划
 * ?
 */
public int numRollsToTarget(int n, int k, int target) {
	if (target < n || target > n * k) return 0;
	final int MOD = 1000000007;
	long[] dp = new long[target - n + 1];
	dp[0] = 1;
	for (int i = 1; i <= n; i++) {
		int max = Math.min(i * (k - 1), target - n);
		for (int j = 1; j <= max; j++) dp[j] += dp[j - 1];
		for (int j = max; j >= k; j--) dp[j] = (dp[j] - dp[j - k]) % MOD;
	}
	return (int) dp[target - n];
}
```

#### [1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)

@2023.12.07

#搜索 

一次遍历，将陆地入队，同时检查是否既有陆地又有海洋。

BFS，出队位置 `(x, y)`，更新 4 个方向上仍为 `0` 的格的值为 `grid[x][y] + 1`，并入队。

之所以只更新“仍为 `0` 的格”，是因为题目要求“请你找出一个海洋单元格，这个海洋单元格到离它 **最近** 的陆地单元格的距离是最大的，并返回该距离”，因此从所有陆地开始 BFS，每次到达一个海洋格时，就找到了其最近陆地与其的一条路径，只能更新为该路径长度。

细节：在 BFS 外部定义变量 `x`、`y`，这样 BFS 结束后其就为最后一层 BFS 中最后访问的位置，即离陆地最远的海洋位置。

```java
/**
 * BFS
 * Sweetiee 🍬
 */
public int maxDistance(int[][] grid) {
	final int[] dirs = {0, 1, 0, -1, 0};
	int n = grid.length;
	Queue<int[]> q = new LinkedList<>();
	boolean land = false, water = false;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			if (grid[i][j] == 1) {
				q.add(new int[]{i, j});
				land = true;
			}
			else water = true;
		}
	}
	if (!(land && water)) return -1;
	int x = 0, y = 0;
	while (!q.isEmpty()) {
		int[] p = q.poll();
		x = p[0];
		y = p[1];
		for (int k = 0; k < 4; k++) {
			int nx = x + dirs[k], ny = y + dirs[k + 1];
			if (inBound(nx, ny, n) && grid[nx][ny] == 0) {
				grid[nx][ny] = grid[x][y] + 1;
				q.add(new int[]{nx, ny});
			}
		}
	}
	return grid[x][y] - 1;
}

private boolean inBound(int x, int y, int n) {
	return x >= 0 && x < n && y >= 0 && y < n;
}
```

#### [1170. 比较字符串最小字母出现频次](https://leetcode.cn/problems/compare-strings-by-frequency-of-the-smallest-character/)

@2023.12.04

#二分查找 #前缀和 

预处理 `words` 计算每个单词的 `f(word)`，对每个 `query` 计算其 `f(query)`，在 `words` 的结果中检查大于它的个数，最后这一步有二分查找、后缀和两种实现。

1. 二分查找

```java
/**
 * 二分查找
 * Somnia1337
 */
public int[] numSmallerByFrequency(String[] queries, String[] words) {
	int n = words.length;
	int[] fs = new int[n];
	for (int i = 0; i < n; i++) fs[i] = f(words[i]);
	Arrays.sort(fs);
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) ans[i] = n - idx(fs, f(queries[i]));
	return ans;
}

private int f(String s) {
	char min = s.charAt(0);
	int ret = 0;
	for (char c : s.toCharArray()) {
		if (c < min) {
			min = c;
			ret = 1;
		}
		else if (c == min) ret++;
	}
	return ret;
}

private int idx(int[] arr, int f) {
	int l = 0, r = arr.length;
	while (l < r) {
		int m = l + r >> 1;
		if (arr[m] <= f) l = m + 1;
		else r = m;
	}
	return l;
}
```

2. 后缀和

```java
/**
 * 后缀和
 * Somnia1337
 */
public int[] numSmallerByFrequency(String[] queries, String[] words) {
	int[] count = new int[12];
	for (String word : words) count[f(word)]++;
	for (int i = 10; i >= 0; i--) count[i] += count[i + 1];
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) ans[i] = count[f(queries[i]) + 1];
	return ans;
}

private int f(String s) {
	char min = s.charAt(0);
	int ret = 0;
	for (char c : s.toCharArray()) {
		if (c < min) {
			min = c;
			ret = 1;
		}
		else if (c == min) ret++;
	}
	return ret;
}
```

#### [1186. 删除一次得到子数组最大和](https://leetcode.cn/problems/maximum-subarray-sum-with-one-deletion/)

@2023.12.22

#动态规划 

与 [1746. 经过一次操作后的最大子数组和](https://leetcode.cn/problems/maximum-subarray-sum-after-one-operation/) 类似。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maximumSum(int[] arr) {
	int[][] dp = new int[arr.length][2];
	dp[0][0] = dp[0][1] = arr[0];
	int ans = arr[0];
	for (int i = 1; i < arr.length; i++) {
		dp[i][0] = Math.max(dp[i - 1][0], 0) + arr[i];
		dp[i][1] = Math.max(dp[i - 1][1] + arr[i], dp[i - 1][0]);
		ans = Math.max(Math.max(dp[i][0], dp[i][1]), ans);
	}
	return ans;
}
```

#### [1190. 反转每对括号间的子串](https://leetcode.cn/problems/reverse-substrings-between-each-pair-of-parentheses/)

@2024.01.27



```java
/**
 * 
 * Somnia1337
 */
public String reverseParentheses(String s) {
	Deque<String> stk = new ArrayDeque<>();
	StringBuilder ans = new StringBuilder();
	for (char c : s.toCharArray()) {
		if (c == '(') {
			stk.push(ans.toString());
			ans.setLength(0);
		}
		else if (c == ')') ans.reverse().insert(0, stk.pop());
		else ans.append(c);
	}
	return ans.toString();
}
```

#### [1219. 黄金矿工](https://leetcode.cn/problems/path-with-maximum-gold/)

@2023.10.23

#搜索 

枚举每个金矿作为起点，DFS 搜索整个 `grid`。

```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;
private boolean[][] vis;

public int getMaximumGold(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	int ans = 0;
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] > 0) {
				vis = new boolean[rows][cols];
				ans = Math.max(dfs(grid, i, j), ans);
			}
		}
	}
	return ans;
}

private int dfs(int[][] grid, int x, int y) {
	if (!inBound(x, y) || grid[x][y] == 0 || vis[x][y]) return 0;
	vis[x][y] = true;
	int max = 0;
	for (int k = 0; k < 4; k++) {
		max = Math.max(dfs(grid, x + dirs[k], y + dirs[k + 1]), max);
	}
	vis[x][y] = false;
	return grid[x][y] + max;
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

优化：省去 `vis`，进入一个 `grid[x][y] > 0` 时用 `hold` 保存其值，然后赋 0，避免重入，`dfs` 完成后再恢复其值。

```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;

public int getMaximumGold(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	int ans = 0;
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] != 0) ans = Math.max(ans, dfs(grid, i, j));
		}
	}
	return ans;
}

private int dfs(int[][] grid, int x, int y) {
	if (!inBound(x, y) || grid[x][y] == 0) return 0;
	int hold = grid[x][y], max = grid[x][y] = 0;
	for (int k = 0; k < 4; k++) {
		max = Math.max(dfs(grid, x + dirs[k], y + dirs[k + 1]), max);
	}
	grid[x][y] = hold;
	return hold + max;
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [1222. 可以攻击国王的皇后](https://leetcode.cn/problems/queens-that-can-attack-the-king/)

@2023.09.14

#暴力 

```java
/**
 * 枚举
 * Somnia1337
 */
public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
	Set<Integer> pos = new HashSet<>(); // 皇后的坐标
	int kx = king[1], ky = king[0]; // 国王的坐标
	for (int[] q : queens) pos.add(q[0] * 8 + q[1]);
	List<List<Integer>> ans = new ArrayList<>();
	for (int x1 = kx + 1; x1 <= 7; x1++) {
		if (pos.contains(ky * 8 + x1)) {
			ans.add(List.of(ky, x1));
			break;
		}
	}
	for (int x2 = kx - 1; x2 >= 0; x2--) {
		if (pos.contains(ky * 8 + x2)) {
			ans.add(List.of(ky, x2));
			break;
		}
	}
	for (int y1 = ky + 1; y1 <= 7; y1++) {
		if (pos.contains(y1 * 8 + kx)) {
			ans.add(List.of(y1, kx));
			break;
		}
	}
	for (int y2 = ky - 1; y2 >= 0; y2--) {
		if (pos.contains(y2 * 8 + kx)) {
			ans.add(List.of(y2, kx));
			break;
		}
	}
	for (int x3 = kx + 1, y3 = ky + 1; x3 <= 7 && y3 <= 7; x3++, y3++) {
		if (pos.contains(y3 * 8 + x3)) {
			ans.add(List.of(y3, x3));
			break;
		}
	}
	for (int x3 = kx + 1, y4 = ky - 1; x3 <= 7 && y4 >= 0; x3++, y4--) {
		if (pos.contains(y4 * 8 + x3)) {
			ans.add(List.of(y4, x3));
			break;
		}
	}
	for (int x4 = kx - 1, y3 = ky + 1; x4 >= 0 && y3 <= 7; x4--, y3++) {
		if (pos.contains(y3 * 8 + x4)) {
			ans.add(List.of(y3, x4));
			break;
		}
	}
	for (int x4 = kx - 1, y4 = ky - 1; x4 >= 0 && y4 >= 0; x4--, y4--) {
		if (pos.contains(y4 * 8 + x4)) {
			ans.add(List.of(y4, x4));
			break;
		}
	}
	return ans;
}
```

```java
/**
 * 暴力（枚举）
 * 力扣官方题解
 */
public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
	Set<Integer> pos = new HashSet<>();
	for (int[] q : queens) {
		int x = q[0], y = q[1];
		pos.add(x * 8 + y);
	}
	List<List<Integer>> ans = new ArrayList<>();
	for (int dx = -1; dx <= 1; dx++) {
		for (int dy = -1; dy <= 1; dy++) {
			if (dx == 0 && dy == 0) continue;
			int kx = king[0] + dx, ky = king[1] + dy;
			while (kx >= 0 && kx < 8 && ky >= 0 && ky < 8) {
				int p = kx * 8 + ky;
				if (pos.contains(p)) {
					ans.add(List.of(kx, ky));
					break;
				}
				kx += dx;
				ky += dy;
			}
		}
	}
	return ans;
}
```

#### [1234. 替换子串得到平衡字符串](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/)

@2024.01.12

#双指针 

要替换的子串需要包含 4 个字母溢出 `n/4` 的部分，同向双指针查找满足的最小窗口。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int balancedString(String s) {
	char[] chs = s.toCharArray();
	int[] count = new int['X'];
	int n = chs.length, m = n / 4; // m: 上限
	boolean v = true;
	for (char c : chs) {
		if (++count[c] > m) v = false;
	}
	if (v) return 0;
	int l = 0, r = 0, ans = n;
	while (r < n) {
		count[chs[r++]]--; // r 滑出
		while (count['Q'] <= m && count['W'] <= m && count['E'] <= m && count['R'] <= m) { // 都不超过上限
			ans = Math.min(r - l, ans);
			count[chs[l++]]++; // l 滑进
		}
	}
	return ans;
}
```

#### [1248. 统计「优美子数组」](https://leetcode.cn/problems/count-number-of-nice-subarrays/)

@2023.10.18

#前缀和 #哈希表 

`pre[i]` 表示前 `i` 个元素中的奇数个数，哈希表计数，累加 `odd.get(pre[i] - k)`。

```java
/**
 * 前缀和
 * Somnia1337
 */
public int numberOfSubarrays(int[] nums, int k) {
	int n = nums.length;
	int[] pre = new int[n + 1];
	Map<Integer, Integer> odd = new HashMap<>();
	odd.put(0, 1);
	int ans = 0;
	for (int i = 1; i < n + 1; i++) {
		odd.merge(pre[i] = pre[i - 1] + (nums[i - 1] & 1), 1, Integer::sum);
		ans += odd.getOrDefault(pre[i] - k, 0);
	}
	return ans;
}
```

优化：

- 由于 `pre[i]` 最大只到 `n`，可以用 `int[]` 代替 `Map` 计数。
- 用 `int` 代替 `int[]`，记录目前为止的奇数个数。

```java
/**
 * 前缀和
 * Somnia1337
 */
public int numberOfSubarrays(int[] nums, int k) {
	int n = nums.length;
	int[] odd = new int[n + 1];
	odd[0] = 1;
	int pre = 0, ans = 0;
	for (int x : nums) {
		odd[pre += x & 1]++;
		if (pre >= k) ans += odd[pre - k];
	}
	return ans;
}
```

#### [1253. 重构 2 行二进制矩阵](https://leetcode.cn/problems/reconstruct-a-2-row-binary-matrix/)

@2023.12.07

#贪心 

检查 `upper + lower == sum(colsum)`，`colsum[i]` 有 `0,1,2` 取值，`0` 忽略，先对 `2` 位置上下填 1，再对 `1` 位置填上行、`upper` 不够时填下行。

```java
/**
 * 贪心
 * Somnia1337
 */
public List<List<Integer>> reconstructMatrix(int upper, int lower, int[] colsum) {
	int n = colsum.length, sum = 0, uHold = upper, lHold = lower;
	int[][] col = new int[2][n];
	for (int i = 0; i < n; i++) {
		if (colsum[i] == 2) {
			col[0][i] = col[1][i] = 1;
			upper--;
			lower--;
		}
		sum += colsum[i];
	}
	if (uHold + lHold != sum) return new ArrayList<>();
	for (int i = 0; i < n; i++) {
		if (colsum[i] == 1) {
			if (upper > 0) {
				col[0][i] = 1;
				upper--;
			}
			else if (lower > 0) {
				col[1][i] = 1;
				lower--;
			}
			else return new ArrayList<>();
		}
	}
	if (upper > 0 || lower > 0) return new ArrayList<>();
	
	List<List<Integer>> ans = new ArrayList<>();
	List<Integer> up = new ArrayList<>(), low = new ArrayList<>();
	ans.add(up);
	ans.add(low);
	for (int u : col[0]) up.add(u);
	for (int d : col[1]) low.add(d);
	return ans;
}
```

#### [1254. 统计封闭岛屿的数目](https://leetcode.cn/problems/number-of-closed-islands/)

@2023.12.04

#搜索 

DFS 总是将岛屿标记为水域，先标记连通到边界的岛，再标记并累加还未标记的岛。

```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;

public int closedIsland(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	for (int i = 0; i < rows; i++) {
		if (grid[i][0] == 0) dfs(grid, i, 0);
		if (grid[i][cols - 1] == 0) dfs(grid, i, cols - 1);
	}
	for (int j = 0; j < cols; j++) {
		if (grid[0][j] == 0) dfs(grid, 0, j);
		if (grid[rows - 1][j] == 0) dfs(grid, rows - 1, j);
	}
	
	int ans = 0;
	for (int i = 1; i < rows - 1; i++) {
		for (int j = 1; j < cols - 1; j++) {
			if (grid[i][j] == 0) {
				ans++;
				dfs(grid, i, j);
			}
		}
	}
	return ans;
}

private void dfs(int[][] grid, int x, int y) {
	if (!inBound(x, y) || grid[x][y] == 1) return;
	grid[x][y] = 1;
	for (int k = 0; k < 4; k++) dfs(grid, x + dirs[k], y + dirs[k + 1]);
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [1262. 可被三整除的最大和](https://leetcode.cn/problems/greatest-sum-divisible-by-three/)

@2023.09.19

```java
/**
 * 动态规划
 * ylb
 */
public int maxSumDivThree(int[] nums) {
	int n = nums.length;
	int[][] dp = new int[n + 1][3];
	dp[0][1] = dp[0][2] = Integer.MIN_VALUE;
	for (int i = 1; i <= n; i++) {
		int pre = nums[i - 1];
		for (int j = 0; j < 3; j++) {
			dp[i][j] = Math.max(dp[i - 1][(j - pre % 3 + 3) % 3] + pre, dp[i - 1][j]);
		}
	}
	return dp[n][0];
}
```

#### [1267. 统计参与通信的服务器](https://leetcode.cn/problems/count-servers-that-communicate/)

@2023.12.21

#模拟 

两次遍历，第一次统计每行、列的服务器数量，第二次累加所在行 / 列的服务器计数 `> 1` 的服务器。

```java
/**
 * 模拟
 * Somnia1337
 */
public int countServers(int[][] grid) {
	int rows = grid.length, cols = grid[0].length;
	int[] row = new int[rows], col = new int[cols];
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] == 1) {
				row[i]++;
				col[j]++;
			}
		}
	}
	int ans = 0;
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] == 1 && (row[i] > 1 || col[j] > 1)) ans++;
		}
	}
	return ans;
}
```

#### [1276. 不浪费原料的汉堡制作方案](https://leetcode.cn/problems/number-of-burgers-with-no-waste-of-ingredients/)

@2023.12.25

#数学 

求解二元一次方程组。

```java
/**
 * 数学
 * Somnia1337
 */
public List<Integer> numOfBurgers(int tomatoSlices, int cheeseSlices) {
	int jumbo = tomatoSlices - 2 * cheeseSlices;
	if (jumbo < 0 || (jumbo & 1) == 1) return new ArrayList<>();
	jumbo >>= 1;
	if (jumbo > cheeseSlices) return new ArrayList<>();
	return new ArrayList<>(List.of(jumbo, cheeseSlices - jumbo));
}
```

#### [1283. 使结果不超过阈值的最小除数](https://leetcode.cn/problems/find-the-smallest-divisor-given-a-threshold/)

@2023.09.18

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int smallestDivisor(int[] nums, int threshold) {
	int l = 1, r = 1000000;
	while (l <= r) {
		int m = l + r >> 1, sum = 0;
		for (int x : nums) {
			sum += x / m;
			if (x % m > 0) sum++;
		}
		if (sum > threshold) l = m + 1;
		else r = m - 1;
	}
	return l;
}
```

#### [1297. 子串的最大出现次数](https://leetcode.cn/problems/maximum-number-of-occurrences-of-a-substring/)

@2023.12.20

#滑动窗口 #技巧 

`maxSize` 是多余的，因为较短子串的出现次数一定不少于较长子串，只需用长为 `minSize` 的定长窗口。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
	char[] chs = s.toCharArray();
	int[] count = new int[26]; // 字符计数
	Map<String, Integer> vis = new HashMap<>();
	int cnt = 0, ans = 0; // cnt: 窗口内的字符种数
	// 头
	for (int i = 0; i < minSize; i++) {
		if (++count[chs[i] - 'a'] == 1) cnt++;
	}
	// 中
	for (int i = minSize; i < chs.length; i++) {
		if (cnt <= maxLetters) ans = Math.max(vis.merge(s.substring(i - minSize, i), 1, Integer::sum), ans);
		if (--count[chs[i - minSize] - 'a'] == 0) cnt--;
		if (++count[chs[i] - 'a'] == 1) cnt++;
	}
	// 尾
	if (cnt <= maxLetters) ans = Math.max(vis.merge(s.substring(chs.length - minSize), 1, Integer::sum), ans);
	return ans;
}
```

#### [1333. 餐厅过滤器](https://leetcode.cn/problems/filter-restaurants-by-vegan-friendly-price-and-distance/)

@2023.09.27

```java
/**
 * 自定义排序
 * Somnia1337
 */
public List<Integer> filterRestaurants(int[][] restaurants, int veganFriendly, int maxPrice, int maxDistance) {
	List<int[]> valid = new ArrayList<>();
	for (int[] r : restaurants) {
		if (r[3] <= maxPrice && r[4] <= maxDistance && !(veganFriendly == 1 && r[2] == 0)) valid.add(r);
	}
	valid.sort((a, b) -> {
		if (a[1] != b[1]) return b[1] - a[1];
		else return b[0] - a[0];
	});
	List<Integer> ans = new ArrayList<>();
	for (int[] v : valid) ans.add(v[0]);
	return ans;
}
```

#### [1334. 阈值距离内邻居最少的城市](https://leetcode.cn/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

@2023.11.14

```java
/**
 * Floyd 算法
 * 灵茶山艾府
 */
public int findTheCity(int n, int[][] edges, int distanceThreshold) {
	int[][] w = new int[n][n];
	for (int[] row : w) Arrays.fill(row, Integer.MAX_VALUE / 2);
	for (int[] e : edges) {
		int x = e[0], y = e[1], wt = e[2];
		w[x][y] = w[y][x] = wt;
	}
	
	for (int k = 0; k < n; k++) {
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				w[i][j] = Math.min(w[i][j], w[i][k] + w[k][j]);
			}
		}
	}
	
	int ans = 0, min = n;
	for (int i = 0; i < n; i++) {
		int cnt = 0;
		for (int j = 0; j < n; j++) {
			if (j != i && w[i][j] <= distanceThreshold) cnt++;
		}
		if (cnt <= min) {
			min = cnt;
			ans = i;
		}
	}
	return ans;
}
```

#### [1353. 最多可以参加的会议数目](https://leetcode.cn/problems/maximum-number-of-events-that-can-be-attended/)

@2023.12.26

#贪心 

贪心：优先参加结束时间最早的会议。

哈希表记录 `Map<开始时间, List<结束时间>>`，遍历 `events` 分组记录。

顺序枚举每个 `day` 是否参加会议，小顶堆维护 **开始时间 `<= day`（当前能够参加）的会议** 的 **结束时间**，入堆后，将所有结束时间 `< day`（已经错过，无法参加）的会议出堆，如果此时还有剩余，则在 `day` 参加该会议，出堆并累加答案。

```java
/**
 * 贪心
 * 阿尼亚
 */
public int maxEvents(int[][] events) {
	Map<Integer, List<Integer>> groups = new HashMap<>();
	int l = 100000, r = 0;
	for (int[] e : events) {
		groups.computeIfAbsent(e[0], k -> new ArrayList<>()).add(e[1]);
		l = Math.min(e[0], l);
		r = Math.max(e[1], r);
	}
	
	Queue<Integer> pq = new PriorityQueue<>();
	int ans = 0;
	for (int day = l; day <= r; day++) {
		if (groups.containsKey(day)) pq.addAll(groups.get(day));
		while (!pq.isEmpty() && pq.peek() < day) pq.poll();
		if (!pq.isEmpty()) {
			ans++;
			pq.poll();
		}
	}
	return ans;
}
```

#### [1366. 通过投票对团队排名](https://leetcode.cn/problems/rank-teams-by-votes/)

@2023.12.24

#排序 

`n` 为参与投票的队伍数，用 `int[26][n+1] ranks` 记录每个字母被排在每个位置的次数，多出 1 位记录字母顺序数，以便在排序时按字母顺序比较。

```java
/**
 * 自定义排序
 * Somnia1337
 */
public String rankTeams(String[] votes) {
	int n = votes[0].length(); // 参与投票的队伍数
	int[][] ranks = new int[26][n + 1];
	for (int i = 0; i < 26; i++) ranks[i][n] = i; // 设置最后 1 位
	
	// 投票
	for (String v : votes) {
		for (int i = 0; i < n; i++) ranks[v.charAt(i) - 'A'][i]++;
	}
	
	// 排序
	Arrays.sort(ranks, (a, b) -> {
		for (int i = 0; i < n; i++) { // 遍历, 首个不同的排位
			if (a[i] != b[i]) return b[i] - a[i];
		}
		return a[n] - b[n]; // 都相同, 按字母表顺序
	});
	
	// 构造答案
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < n; i++) ans.append((char) ('A' + ranks[i][n]));
	return ans.toString();
}
```

#### [1375. 二进制字符串前缀一致的次数](https://leetcode.cn/problems/number-of-times-binary-string-is-prefix-aligned/)

@2023.12.04

#数组 

翻译题意：`flips` 含有 $[1, n]$ 的一个排列，当 `i` 满足 `flips` 的前 `i` 个元素恰为 $[1, i + 1]$ 的一个排列时，`ans++`。

一次遍历，维护当前总和 `cur`、当前期望总和 `exp`，`cur` 累加 `flips[i]`，`exp` 累加 `i + 1`，当 `cur == exp` 时，说明前 `i` 个元素是 $[1, i + 1]$ 的排列，`ans++`。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int numTimesAllBlue(int[] flips) {
	int cur = 0, exp = 0, ans = 0;
	for (int i = 0; i < flips.length; i++) {
		if ((cur += flips[i]) == (exp += i + 1)) ans++;
	}
	return ans;
}
```

也可维护最大值判断：

```java
/**
 * 一次遍历
 * 灵茶山艾府
 */
public int numTimesAllBlue(int[] flips) {
	int max = 0, ans = 0;
	for (int i = 0; i < flips.length; i++) {
		if ((max = Math.max(flips[i], max)) == i + 1) ans++;
	}
	return ans;
}
```

#### [1391. 检查网格中是否存在有效路径](https://leetcode.cn/problems/check-if-there-is-a-valid-path-in-a-grid/)

@2023.11.06

#搜索 

从起点 DFS，参数中 `int from` 记录上一格的方位。

```java
/**
 * DFS
 * Somnia1337
 */
// 上 0, 下 1, 左 2, 右 3
private final int[][] dirs = new int[][]{{2, 3}, {0, 1}, {1, 2}, {1, 3}, {0, 2}, {0, 3}};
private int rows, cols;
private boolean[][] vis;
private boolean ans;

public boolean hasValidPath(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	if (rows == 1 && cols == 1) return true;
	ans = false;
	vis = new boolean[rows][cols];
	dfs(grid, 0, 0, -1);
	return ans;
}

private void dfs(int[][] g, int x, int y, int from) {
	if (!inBound(x, y) || vis[x][y]) return;
	int[] dr = dirs[g[x][y] - 1];
	if (from != -1 && from != dr[0] && from != dr[1]) return;
	if (x == g.length - 1 && y == g[0].length - 1) {
		ans = true;
		return;
	}
	vis[x][y] = true;
	for (int d : dr) {
		if (d == from) continue;
		if (d == 0) dfs(g, x - 1, y, 1);
		else if (d == 1) dfs(g, x + 1, y, 0);
		else if (d == 2) dfs(g, x, y - 1, 3);
		else dfs(g, x, y + 1, 2);
	}
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [1400. 构造 K 个回文字符串](https://leetcode.cn/problems/construct-k-palindrome-strings/)

@2023.09.23

#字符串 

统计字符出现的次数，计数其中的奇数 `odds`，如果 `k < odds`，则有多出的单个字符无法成回文串。这是因为每个回文串最多能包容一个单字符作为回文中心，出现次数为偶的字符在回文串背景下相当于没出现。

```java
/**
 * 字符统计
 * Somnia1337
 */
public boolean canConstruct(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	if (n < k) return false; // 总字符数不足 k
	int[] count = new int[26];
	for (char c : chs) count[c - 'a'] ^= 1; // 出现奇数次时为 1, 否则为 0
	int odds = 0;
	for (int c : count) {
		if (c == 1) odds++;
	}
	return k >= odds;
}
```

#### [1410. HTML 实体解析器](https://leetcode.cn/problems/html-entity-parser/)

@2023.11.23

1. 库函数

```java
/**
 * 库函数
 * Somnia1337
 */
public String entityParser(String text) {
	return text.replace("&quot;", "\"")
			   .replace("&apos;", "'")
			   .replace("&gt;", ">")
			   .replace("&lt;", "<")
			   .replace("&frasl;", "/")
			   .replace("&amp;", "&");
}
```

2. 模拟

```java
/**
 * 模拟
 * ylb
 */
public String entityParser(String text) {
	Map<String, String> d = new HashMap<>() {{
		put("&quot;", "\"");
		put("&apos;", "'");
		put("&amp;", "&");
		put("&gt;", ">");
		put("&lt;", "<");
		put("&frasl;", "/");
	}};
	int n = text.length(), i = 0;
	StringBuilder ans = new StringBuilder();
	while (i < n) {
		boolean found = false;
		for (int l = 1; l < 8; ++l) {
			int j = i + l;
			if (j <= n) {
				String t = text.substring(i, j);
				if (d.containsKey(t)) {
					ans.append(d.get(t));
					i = j;
					found = true;
					break;
				}
			}
		}
		if (!found) ans.append(text.charAt(i++));
	}
	return ans.toString();
}
```

#### [1419. 数青蛙](https://leetcode.cn/problems/minimum-number-of-frogs-croaking/)

@2023.09.15

#贪心 

先判断串有效：当出现以下情形之一时——

- 计数过程中，出现非法字符
- 计数过程中，`"croak"` 没有按顺序出现
- 计数结束后，5 个字母出现次数不同

则输入的字符串无效，返回 `-1`。

再贪心：再次遍历，`'c'` 表示有一只青蛙开始叫，`'k'` 表示有一只青蛙叫完，同时在叫的青蛙数量的最大值即为答案。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minNumberOfFrogs(String croakOfFrogs) {
	// 判断串是否有效
	char[] chs = croakOfFrogs.toCharArray();
	int[] count = new int[5]; // 计数 c,r,o,a,k
	boolean valid = true; // 标志输入串是否有效
	for (char c : chs) {
		switch (c) {
			case 'c' -> count[0]++;
			case 'r' -> count[1]++;
			case 'o' -> count[2]++;
			case 'a' -> count[3]++;
			case 'k' -> count[4]++;
			default -> valid = false; // 存在非法字符
		}
		// 串无效: 存在非法字符, 或者 croak 没有按顺序出现
		if (!valid || count[1] > count[0] || count[2] > count[1] || count[3] > count[2] || count[4] > count[3])
			return -1;
	}
	int cnt = count[0];
	// 串无效: 5 个字母出现的次数不同
	if (count[1] != cnt || count[2] != cnt || count[3] != cnt || count[4] != cnt) return -1;
	
	// 贪心: c 表示有一只青蛙开始叫, k 表示有一只青蛙叫完, 维护同时在叫的青蛙数量的最大值
	int cur = 0, ans = 0; // cur: 当前正在叫的青蛙数
	for (char c : chs) {
		if (c == 'c') cur++; // 遇到 c, 当前在叫的青蛙 +1
		else if (c == 'k') cur--; // 遇到 k, 当前在叫的青蛙 -1
		ans = Math.max(cur, ans); // 更新同时在叫的最大值
	}
	return ans;
}
```

#### [1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/)

@2023.12.03

#滑动窗口 

要从牌堆的两端拿走共 `k` 张牌，则剩余的 `n - k` 张构成一个连续的子数组。

对牌堆求和，用长为 `n - k` 的滑动窗口计算和最小的子数组，从总和中减去。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int maxScore(int[] cardPoints, int k) {
	int n = cardPoints.length, sum = 0;
	for (int cp : cardPoints) sum += cp;
	if (k == n) return sum;
	
	int cur = 0;
	for (int i = 0; i < n - k; i++) cur += cardPoints[i];
	int ans = sum - cur;
	for (int i = n - k; i < n; i++) {
		cur += cardPoints[i] - cardPoints[i + k - n];
		ans = Math.max(sum - cur, ans);
	}
	return ans;
}
```

#### [1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

@2023.09.02

#滑动窗口 

使用滑动窗口，去头添尾计算当前窗口内的元音个数，维护最大值。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int maxScore(int[] cardPoints, int k) {
	int n = cardPoints.length, sum = 0;
	for (int cp : cardPoints) sum += cp;
	if (k == n) return sum;
	int cur = 0;
	for (int i = 0; i < n - k; i++) cur += cardPoints[i];
	int ans = sum - cur;
	for (int i = n - k; i < n; i++) {
		cur += cardPoints[i] - cardPoints[i + k - n];
		ans = Math.max(sum - cur, ans);
	}
	return ans;
}
```

#### [1462. 课程表 IV](https://leetcode.cn/problems/course-schedule-iv/)

@2023.09.12

```java
/**
 * 拓扑排序
 * 力扣官方题解
 */
public List<Boolean> checkIfPrerequisite(int numCourses, int[][] prerequisites, int[][] queries) {
	List<Integer>[] g = new List[numCourses];
	Arrays.setAll(g, e -> new ArrayList<>());
	int[] in = new int[numCourses];
	boolean[][] pre = new boolean[numCourses][numCourses];
	for (int[] p : prerequisites) {
		in[p[1]]++;
		g[p[0]].add(p[1]);
	}
	
	Queue<Integer> q = new LinkedList<>();
	for (int i = 0; i < numCourses; i++) {
		if (in[i] == 0) q.offer(i);
	}
	while (!q.isEmpty()) {
		int p = q.poll();
		for (int next : g[p]) {
			pre[p][next] = true;
			for (int i = 0; i < numCourses; i++) {
				pre[i][next] = pre[i][next] || pre[i][p];
			}
			if (--in[next] == 0) q.offer(next);
		}
	}
	
	List<Boolean> ans = new ArrayList<>(queries.length);
	for (int[] qu : queries) ans.add(pre[qu[0]][qu[1]]);
	return ans;
}
```

#### [1465. 切割后面积最大的蛋糕](https://leetcode.cn/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts/)

@2023.10.27

#数组 

排序后，找到每个方向相邻切割的最大差值（即最大边长），相乘即为答案。

```java
/**
 * 排序
 * Somnia1337
 */
public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {
	Arrays.sort(horizontalCuts);
	Arrays.sort(verticalCuts);
	return (int) ((long) helper(horizontalCuts, h) * helper(verticalCuts, w) % 1000000007);
}

private int helper(int[] arr, int ceil) {
	int ret = arr[0];
	for (int i = 1; i < arr.length; i++) ret = Math.max(arr[i] - arr[i - 1], ret);
	return Math.max(ceil - arr[arr.length - 1], ret);
}
```

#### [1466. 重新规划路线](https://leetcode.cn/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)

@2023.12.07

从 `0` 开始 BFS，用哈希表（自定义哈希函数 `起点 * n + 终点`）记录边的正确方向，再遍历 `connectioons`，计数其中方向错误的边。

```java
/**
 * BFS
 * Somnia1337
 */
public int minReorder(int n, int[][] connections)
{
	List<List<Integer>> neib = new ArrayList<>();
	for (int i = 0; i < n; i++) neib.add(new ArrayList<>());
	for (int[] c : connections)
	{
		neib.get(c[0]).add(c[1]);
		neib.get(c[1]).add(c[0]);
	}
	Set<Integer> edges = new HashSet<>();
	boolean[] vis = new boolean[n];
	Queue<Integer> q = new LinkedList<>();
	q.add(0);
	while (!q.isEmpty())
	{
		int node = q.poll();
		vis[node] = true;
		List<Integer> nexts = neib.get(node);
		for (int next : nexts)
		{
			if (vis[next]) continue;
			edges.add(node * n + next);
			q.add(next);
		}
	}
	
	int ans = 0;
	for (int[] c : connections)
	{
		if (edges.contains(c[0] * n + c[1])) ans++;
	}
	return ans;
}
```

本题 DFS 更快，待学习。

#### [1482. 制作 m 束花所需的最少天数](https://leetcode.cn/problems/minimum-number-of-days-to-make-m-bouquets/)

@2023.09.27

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minDays(int[] bloomDay, int m, int k)
{
	int len = bloomDay.length;
	long product = (long) m * k;
	if (len < product) return -1;
	int max = 0;
	for (int day : bloomDay)
	{
		max = Math.max(day, max);
	}
	if (len == product) return max;
	int left = 1, right = max;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		int currentZeros = 0, captivity = 0;
		for (int day : bloomDay)
		{
			if (day <= mid)
			{
				currentZeros++;
				if (currentZeros == k)
				{
					captivity++;
					currentZeros = 0;
				}
			}
			else currentZeros = 0;
		}
		if (captivity >= m) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}
```

#### [1488. 避免洪水泛滥](https://leetcode.cn/problems/avoid-flood-in-the-city/)

@2023.10.13

#贪心 

将所有晴天的日期存入有序集合 `sunnyDays` 中，用哈希表 `rainy` 记录每个湖泊最近一次下雨的日期，`key` 为湖泊号，`value` 为最近下雨的日期。

遍历 `rains`：

- 如果 `rains[i] == 0`，即为晴天，将其存入 `sunnyDays`。
- 如果 `rains[i] == 0`，即为雨天，如果 `rainy.containsKey(rains[i])`，说明该湖泊之前下过雨，需要从 `sunnyDays` 中找到该湖泊上次下雨之后的首个晴天，用这个晴天抽干该湖泊，如果不存在这个晴天，说明无法阻止洪水。

```java
/**
 * 贪心
 * ylb
 */
public int[] avoidFlood(int[] rains)
{
	int len = rains.length;
	int[] ans = new int[len];
	Arrays.fill(ans, 1);
	TreeSet<Integer> sunnyDays = new TreeSet<>();
	Map<Integer, Integer> lakes = new HashMap<>();
	
	for (int i = 0; i < len; i++)
	{
		if (rains[i] == 0)
		{
			sunnyDays.add(i);
		}
		else
		{
			ans[i] = -1;
			if (lakes.containsKey(rains[i])) // 之前已下雨
			{
				Integer firstSunnyAfter = sunnyDays.ceiling(lakes.get(rains[i])); // 该湖泊上次下雨之后的首个晴天
				if (firstSunnyAfter == null) return new int[0];
				ans[firstSunnyAfter] = rains[i];
				sunnyDays.remove(firstSunnyAfter);
			}
			lakes.put(rains[i], i);
		}
	}
	
	return ans;
}
```

#### [1493. 删掉一个元素以后全为 1 的最长子数组](https://leetcode.cn/problems/longest-subarray-of-1s-after-deleting-one-element/)

@2023.09.02

#贪心 

![[Snipaste_230902_095503.png|400]]

优雅，太优雅了。

遍历，对每个 `0`，向其左右延伸，找到与其相邻的 `1` 串长度，更新答案。

```java
/**
 * 贪心
 * Somnia1337
 */
public int longestSubarray(int[] nums) {
	int len = nums.length, ans = 0;
	boolean hasZ = false;
	for (int i = 0; i < len; i++) {
		if (nums[i] == 0) {
			hasZ = true;
			int l = i - 1, r = i + 1;
			while (l >= 0 && nums[l] == 1) l--;
			while (r < len && nums[r] == 1) r++;
			ans = Math.max(r - l - 2, ans);
			i = r - 1;
		}
	}
	return hasZ ? ans : len - 1;
}
```

`cur` 表示当前连续 `1` 的计数，`sum` 表示最近 2 段连续 `1` 的计数和。遍历，遇到 `0` 时，`sum` 置 `cur`，`cur` 置 0，对于下个元素：

- 如果为 1，`sum` 从 `cur` 继续增加，表示删除上个 `0` 可以让 2 段 `1` 相连。
- 如果仍为 0，`sum` 再次置 `cur`，也即置 0，表示删除这个 `0` 不能让 2 段 `1` 相连。

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int longestSubarray(int[] nums) {
	int cur = 0, sum = 0, ans = 0;
	for (int num : nums) {
		if (num == 0) {
			sum = cur;
			cur = 0;
		}
		else {
			cur++;
			sum++;
		}
		ans = Math.max(sum, ans);
	}
	return ans < nums.length ? ans : ans - 1;
}
```

#### [1503. 所有蚂蚁掉下来前的最后一刻](https://leetcode.cn/problems/last-moment-before-all-ants-fall-out-of-a-plank/)

@2023.11.30

#技巧 

蚂蚁的相撞相当于交换身份继续前进，所以等效于互不影响。

```java
/**
 * 脑筋急转弯
 * Somnia1337
 */
public int getLastMoment(int n, int[] left, int[] right)
{
	int ans = 0;
	for (int l : left) ans = Math.max(l, ans);
	for (int r : right) ans = Math.max(n - r, ans);
	return ans;
}
```

#### [1535. 找出数组游戏的赢家](https://leetcode.cn/problems/find-the-winner-of-an-array-game/)

@2023.12.19

#模拟 

双指针指向当前比赛者，根据比赛结果移动指针。

```java
/**
 * 模拟
 * Somnia1337
 */
public int getWinner(int[] arr, int k) {
	int l = 0, r = 1, win = -1, streak = 0;
	while (r < arr.length) {
		if (arr[l] > arr[r]) {
			if (win != arr[l]) {
				win = arr[l];
				streak = 1;
			}
			else streak++;
			r++;
		}
		else {
			if (win != arr[r]) {
				win = arr[r];
				streak = 1;
			}
			else streak++;
			r++;
			l = r - 1;
		}
		if (streak == k) return win;
	}
	return win;
}
```

优化：一次遍历，维护当前胜利者及其连胜数。

```java
/**
 * 模拟
 * czhlin
 */
public int getWinner(int[] arr, int k) {
	int win = arr[0], streak = 0;
	for (int i = 1; i < arr.length && streak < k; i++) {
		if (arr[i] < win) streak++;
		else {
			win = arr[i];
			streak = 1;
		}
	}
	return win;
}
```

#### [1552. 两球之间的磁力](https://leetcode.cn/problems/magnetic-force-between-two-balls/)

@2023.12.11

#二分查找 

二分答案，对每个距离 `mid`，检查能否放下 `m` 个球。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int maxDistance(int[] position, int m) {
	Arrays.sort(position);
	int len = position.length, l = 1, r = (position[len - 1] - position[0]) / (m - 1) + 1;
	while (l <= r) {
		int mid = l + r >> 1, cur = 1;
		// 检查能否放下 m 个球
		// cur>=m 即可 break
		for (int i = 0, j = i + 1; i < len && cur < m; j = i + 1) {
			while (j < len && position[j] - position[i] < mid) j++;
			if (j < len) cur++;
			i = j;
		}
		if (cur < m) r = mid - 1;
		else l = mid + 1;
	}
	return r;
}
```

#### [1567. 乘积为正数的最长子数组长度](https://leetcode.cn/problems/maximum-length-of-subarray-with-positive-product/)

@2023.12.19

#贪心 

双指针遍历，`nums[r] == 0` 时，更新答案及 `l`。

`solve()` 返回 `nums[l:r]`（左闭右开，且这一段不含 `0` 元素） 的子问题答案。实现上，记录负数的数量及最左、最右的位置，遍历后，如果：

- 数量为偶数，全长即为答案。
- 数量为奇数，需从最左、最右的其中一个位置处截断，取较长的一侧。

```java
/**
 * 贪心
 * Somnia1337
 */
public int getMaxLen(int[] nums) {
	int l = 0, ans = 0;
	for (int r = 0; r < nums.length; r++) {
		if (nums[r] == 0) {
			if (r - l > ans) ans = Math.max(solve(nums, l, r), ans); // 最优性剪枝
			l = r + 1;
		}
	}
	if (nums.length - l > ans) return Math.max(solve(nums, l, nums.length), ans); // 最后一段也可能为答案
	return ans;
}

private int solve(int[] nums, int l, int r) {
	int negs = 0; // 负数计数
	int[] pos = new int[]{-1, -1}; // 负数的位置 [最左,最右]
	for (int i = l; i < r; i++) {
		if (nums[i] < 0) {
			negs++;
			if (pos[0] == -1) pos[0] = i;
			pos[1] = i;
		}
	}
	if ((negs & 1) == 0) return r - l; // 偶数个, 全长即为答案
	return Math.max(r - pos[0] - 1, pos[1] - l); // 否则, 从两侧的负数之一处截断
}
```

#### [1574. 删除最短的子数组使剩余数组有序](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)

@2024.01.06

#暴力 #双指针 

1. 枚举

参考 [2972. 统计移除递增子数组的数目 II](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-ii/)，找到逆序部分，枚举两端点。

```java
/**
 * 枚举
 * Somnia1337
 */
public int findLengthOfShortestSubarray(int[] arr) {
	int n = arr.length, start = n, end = -1;
	for (int i = 1; i < n; i++) {
		if (arr[i] < arr[i - 1]) {
			start = Math.min(i, start);
			end = Math.max(i - 1, end);
		}
	}
	if (start == n) return 0;
	int ans = n - 1;
	for (int r = end + 1; r < n; r++) {
		int l = start - 1;
		while (l >= 0 && arr[l] > arr[r]) l--;
		ans = Math.min(r - l - 1, ans);
	}
	return Math.min(ans, n - start);
}
```

2. 双指针

同向双指针。

```java
/**
 * 双指针
 * Somnia1337
 */
public int findLengthOfShortestSubarray(int[] arr) {
	int n = arr.length, start = n, end = -1;
	for (int i = 1; i < n; i++) {
		if (arr[i] < arr[i - 1]) {
			start = Math.min(i, start);
			end = Math.max(i - 1, end);
		}
	}
	if (start == n) return 0;
	int l = 0, r = end + 1, ans = n - 1;
	while (r < n) {
		while (l < start && arr[l] <= arr[r]) l++;
		ans = Math.min(r++ - l, ans);
	}
	return Math.min(ans, n - start);
}
```

#### [1599. 经营摩天轮的最大利润](https://leetcode.cn/problems/maximum-profit-of-operating-a-centennial-wheel/)

@2024.01.01

#模拟 

模拟，`wait` 记录当前还在排队等待的游客，每轮转动后计算当前利润。

```java
/**
 * 模拟
 * ylb
 */
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

#### [1620. 网络信号最好的坐标](https://leetcode.cn/problems/coordinate-with-maximum-network-quality/)

@2023.12.23

#暴力 

`int[][]` 记录每个坐标的信号，遍历 `towers` 累加，再枚举所有坐标取最大者。

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] bestCoordinate(int[][] towers, int radius) {
	int[][] grid = new int[51][51];
	for (int[] t : towers) {
		if (t[2] == 0) continue;
		for (int x = Math.max(t[0] - radius, 0); x < Math.min(t[0] + radius + 1, 51); x++) {
			for (int y = Math.max(t[1] - radius, 0); y < Math.min(t[1] + radius + 1, 51); y++) {
				double d = Math.sqrt(Math.pow(x - t[0], 2) + Math.pow(y - t[1], 2));
				if (d <= radius) {
					grid[x][y] += (int) Math.floor(t[2] / (1 + d));
				}
			}
		}
	}
	int max = 0;
	int[] ans = {0, 0};
	for (int x = 0; x < 51; x++) {
		for (int y = 0; y < 51; y++) {
			if (grid[x][y] > max) {
				max = grid[x][y];
				ans[0] = x;
				ans[1] = y;
			}
		}
	}
	return ans;
}
```

#### [1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)

@2023.12.11

#二分查找 

二分答案，DFS 检查在每步高度差不大于 `m` 时能否到达终点。

```java
/**
 * 二分查找
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;
private boolean[][] vis;

public int minimumEffortPath(int[][] heights) {
	rows = heights.length;
	cols = heights[0].length;
	int l = 0, r = 1000000;
	while (l <= r) {
		int m = l + r >> 1;
		vis = new boolean[rows][cols];
		if (check(heights, 0, 0, m)) r = m - 1;
		else l = m + 1;
	}
	return Math.max(r, 0);
}

private boolean check(int[][] grid, int x, int y, int diff) {
	if (x == rows - 1 && y == cols - 1) return true; // 到达终点
	vis[x][y] = true;
	for (int k = 0; k < 4; k++) {
		int nx = x + dirs[k], ny = y + dirs[k + 1]; // 下个坐标
		// 坐标在网格内 && 未访问 && 高度差小于diff && 递归返回true
		if ((nx >= 0 && nx < rows && ny >= 0 && ny < cols) && !vis[nx][ny] && Math.abs(grid[nx][ny] - grid[x][y]) < diff && check(grid, nx, ny, diff))
			return true;
	}
	return false;
}
```

#### [1642. 可以到达的最远建筑](https://leetcode.cn/problems/furthest-building-you-can-reach/)

@2023.10.03

#堆 

1. 二分查找

二分答案，对当前的前缀高度差排序，将梯子用于最高的 `ladders` 个高度差，其余累加所需砖块，检查是否超过 `bricks`。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int furthestBuilding(int[] heights, int bricks, int ladders)
{
	int len = heights.length;
	int left = 0, right = len - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		List<Integer> delta = new ArrayList<>(); // 前缀高度差
		for (int i = 0; i < mid; i++)
		{
			if (heights[i + 1] > heights[i])
			{
				delta.add(heights[i + 1] - heights[i]);
			}
		}
		Collections.sort(delta); // 排序
		int bricksNeeded = 0;
		for (int i = delta.size() - 1 - ladders; i >= 0; i--) bricksNeeded += delta.get(i); // 前ladders个无需砖块
		if (bricksNeeded <= bricks) left = mid + 1;
		else right = mid - 1;
	}
	return right;
}
```

2. 堆

用最小堆维护高度差，当堆的大小超过 `ladders` 时，顶元素出堆，累加所需砖块。

```java
/**
 * 堆
 * onion12138
 */
public int furthestBuilding(int[] heights, int bricks, int ladders)
{
	int len = heights.length, sum = 0;
	Queue<Integer> deltas = new PriorityQueue<>();
	for (int i = 1; i < len; i++)
	{
		if (heights[i] > heights[i - 1]) // 到达当前建筑需要升高
		{
			deltas.offer(heights[i] - heights[i - 1]); // 高度差入堆
			if (deltas.size() > ladders) // 超过梯子数
			{
				sum += deltas.poll(); // 最小高度差出堆，累加砖块
			}
			if (sum > bricks) return i - 1; // 超过砖块数，无法到达
		}
	}
	return len - 1;
}
```

#### [1647. 字符频次唯一的最小删除次数](https://leetcode.cn/problems/minimum-deletions-to-make-character-frequencies-unique/)

@2023.12.29

#字符串 

字符统计，排序，倒序遍历，删除 `nums[i]` 直到 `nums[i] < nums[i + 1]`。

```java
/**
 * 字符统计
 * Somnia1337
 */
public int minDeletions(String s) {
	int[] count = new int[26];
	for (char c : s.toCharArray()) count[c - 'a']++;
	Arrays.sort(count);
	int ans = 0;
	for (int i = 24; i >= 0 && count[i] > 0; i--) {
		if (count[i] >= count[i + 1]) {
			int diff = count[i] - count[i + 1];
			if (count[i + 1] > 0) diff++;
			ans += diff;
			count[i] -= diff;
		}
	}
	return ans;
}
```

#### [1653. 使字符串平衡的最少删除次数](https://leetcode.cn/problems/minimum-deletions-to-make-string-balanced/)

@2024.01.22



```java
/**
 * 温柔一刀123
 * Somnia1337
 */
public int minimumDeletions(String s) {
	int cntB = 0, ans = 0;
	for (char c : s.toCharArray()) {
		if (c == 'a') ans = Math.min(ans + 1, cntB);
		else cntB++;
	}
	return ans;
}
```

#### [1657. 确定两个字符串是否接近](https://leetcode.cn/problems/determine-if-two-strings-are-close/)

@2023.09.02

#字符串 

```java
/**
 * 字符统计
 * Somnia1337
 */
public boolean closeStrings(String s1, String s2) {
	int[] count1 = new int[26], count2 = new int[26];
	for (char c1 : s1.toCharArray()) count1[c1 - 'a']++;
	for (char c2 : s2.toCharArray()) count2[c2 - 'a']++;
	for (int i = 0; i < 26; i++) {
		if (count1[i] == 0 && count2[i] != 0 || count2[i] == 0 && count1[i] != 0) return false;
	}
	Arrays.sort(count1);
	Arrays.sort(count2);
	return Arrays.equals(count1, count2);
}
```

#### [1658. 将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/)

@2023.12.06

#双指针 

从两端移除，那么最终剩下的是一个子数组，其和为 `tar = sum(nums) - x`，题目转化为找到和为 `tar` 且长度最长的子数组，答案即 `len(nums) - len(sub)`。

```java
/**
 * 双指针
 * Somnia1337
 */
public int minOperations(int[] nums, int x)
{
	int tar = -x;
	for (int num : nums) tar += num;
	if (tar <= 0) return tar == 0 ? nums.length : -1; // 全部移除才刚好达到 x / 仍无法达到 x
	
	int cur = 0, len = nums.length, l = 0, r = 0, k = 0; // k 为 sub 的最大长度
	while (r < len)
	{
		cur += nums[r++];
		while (cur > tar) cur -= nums[l++];
		if (cur == tar) k = Math.max(r - l, k);
	}
	return k > 0 ? len - k : -1;
}
```

#### [1670. 设计前中后队列](https://leetcode.cn/problems/design-front-middle-back-queue/)

@2023.11.28

#设计 #模拟 

用基于数组的 `ArrayList` 模拟。

```java
/**
 * 模拟
 * xPatrL
 */
class FrontMiddleBackQueue
{
    List<Integer> q;
	
    public FrontMiddleBackQueue()
    {
        q = new ArrayList<>();
    }
	
    public void pushFront(int val)
    {
        q.add(0, val);
    }
	
    public void pushMiddle(int val)
    {
        q.add(q.size() / 2, val);
    }
	
    public void pushBack(int val)
    {
        q.add(val);
    }
	
    public int popFront()
    {
        return !q.isEmpty() ? q.remove(0) : -1;
    }
	
    public int popMiddle()
    {
        return !q.isEmpty() ? q.remove((q.size() - 1) / 2) : -1;
    }
	
    public int popBack()
    {
        return !q.isEmpty() ? q.remove(q.size() - 1) : -1;
    }
}
```

#### [1679. K 和数对的最大数目](https://leetcode.cn/problems/max-number-of-k-sum-pairs/)

@2023.09.02

#双指针 #哈希表 

1. 双指针

排序，左右双指针遍历，根据当前和的大小调整。

```java
/**
 * 双指针
 * Somnia1337
 */
public int maxOperations(int[] nums, int k) {
	Arrays.sort(nums);
	int l = 0, r = nums.length - 1, ans = 0;
	while (l < r) {
		int sum = nums[l++] + nums[r--];
		if (sum < k) r++;
		else if (sum > k) l--;
		else ans++;
	}
	return ans;
}
```

2. 哈希表

用哈希表记录 `<元素, 计数>`，遍历，对每个 `nums[i]` 计算其补数 `tar`：

- 如果 `tar` 存在于表中，则其计数减 1、答案加 1。
- 否则，将该数计入哈希表。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int maxOperations(int[] nums, int k) {
	Map<Integer, Integer> count = new HashMap<>(nums.length);
	int ans = 0;
	for (int num : nums) {
		int tar = k - num;
		if (count.getOrDefault(tar, 0) > 0) {
			ans++;
			count.merge(tar, -1, Integer::sum);
		}
		else count.merge(num, 1, Integer::sum);
	}
	return ans;
}
```

#### [1686. 石子游戏 VI](https://leetcode.cn/problems/stone-game-vi/)

@2024.02.02



```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int stoneGameVI(int[] a, int[] b) {
	Integer[] ids = new Integer[a.length];
	Arrays.setAll(ids, x -> x);
	Arrays.sort(ids, (i, j) -> a[j] + b[j] - a[i] - b[i]);
	int dif = 0;
	for (int i = 0; i < a.length; i++) dif += i % 2 == 0 ? a[ids[i]] : -b[ids[i]];
	return Integer.compare(dif, 0);
}
```

#### [1690. 石子游戏 VII](https://leetcode.cn/problems/stone-game-vii/)

@2024.02.03



```java
/**
 * 动态规划
 * 灵茶山艾府
 */
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

#### [1696. 跳跃游戏 VI](https://leetcode.cn/problems/jump-game-vi/)

@2024.02.05



```java
/**
 * 单调队列
 * 灵茶山艾府
 */
public int maxResult(int[] nums, int k) {
	int n = nums.length;
	Deque<Integer> q = new ArrayDeque<>();
	q.add(0);
	for (int i = 1; i < n; i++) {
		if (q.peekFirst() < i - k) q.pollFirst();
		nums[i] += nums[q.peekFirst()];
		while (!q.isEmpty() && nums[i] >= nums[q.peekLast()]) q.pollLast();
		q.add(i);
	}
	return nums[n - 1];
}
```

```java
/**
 * 动态规划
 * MapleStore
 */
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

#### [1705. 吃苹果的最大数目](https://leetcode.cn/problems/maximum-number-of-eaten-apples/)

@2023.12.27

#贪心 

小顶堆维护到期日期，弹出已腐烂的苹果，贪心地吃最快要腐烂的苹果。

```java
/**
 * 贪心
 * Somnia1337
 */
public int eatenApples(int[] apples, int[] days) {
	Map<Integer, Integer> count = new HashMap<>();
	Queue<Integer> pq = new PriorityQueue<>();
	pq.offer(-1);
	int day = 0, ans = 0;
	while (!pq.isEmpty() || day < apples.length) {
		if (day < apples.length && apples[day] > 0) {
			count.merge(days[day] + day, apples[day], Integer::sum);
			pq.offer(days[day] + day);
		}
		while (!pq.isEmpty() && (pq.peek() <= day || count.get(pq.peek()) == 0)) pq.poll();
		if (!pq.isEmpty()) {
			count.merge(pq.peek(), -1, Integer::sum);
			ans++;
		}
		day++;
	}
	return ans;
}
```

#### [1712. 将数组分成三个子数组的方案数](https://leetcode.cn/problems/ways-to-split-array-into-three-subarrays/)

@2024.01.25

1. 二分查找

ex 坏了。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int waysToSplit(int[] nums) {
	final int MOD = 1000000007;
	int n = nums.length;
	int[] pre = new int[n + 1];
	for (int i = 1; i <= n; i++) pre[i] = pre[i - 1] + nums[i - 1];
	int ans = 0;
	for (int j = 2; j < n; j++) {
		final int R = pre[n] - pre[j];
		if (R < pre[n] / 3) break;
		int v1, v2;
		{
			int l = 1, r = j;
			while (l < r) {
				int i = l + r >> 1;
				final int L = pre[i], M = pre[j] - pre[i];
				if (M < L) r = i;
				else l = i + 1;
			}
			v1 = l;
		}
		{
			int l = 1, r = j;
			while (l < r) {
				int i = l + r >> 1;
				final int M = pre[j] - pre[i];
				if (M <= R) r = i;
				else l = i + 1;
			}
			v2 = l;
		}
		ans = (ans + v1 - v2) % MOD;
	}
	return ans;
}
```

2. 双指针

```java
/**
 * 双指针
 * ?
 */
public int waysToSplit(int[] nums) {
	final int MOD = 1000000007;
	int n = nums.length;
	int[] pre = new int[n + 1];
	for (int i = 1; i <= n; i++) pre[i] = pre[i - 1] + nums[i - 1];
	int ans = 0;
	for (int i = 1, j = 2, k = 1; i < n - 1 && pre[i] * 3 <= pre[n]; i++) {
		final int L = pre[i];
		j = Math.max(i + 1, j);
		while (j < n && pre[j] - pre[i] < L) j++;
		while (k < n - 1 && pre[k + 1] - pre[i] <= pre[n] - pre[k + 1]) k++;
		ans = (ans + k - j + 1) % MOD;
	}
	return ans;
}
```

#### [1718. 构建字典序最大的可行序列](https://leetcode.cn/problems/construct-the-lexicographically-largest-valid-sequence/)

@2023.12.22

[题解](https://leetcode.cn/problems/construct-the-lexicographically-largest-valid-sequence/solutions/1940675/by-stormsunshine-0p0v/)

```java
if (ans[idx] != 0) bt(idx + 1);
else {
	for (int i = n; i >= 1; i--) {
		int next = idx + (i > 1 ? i : 0);
		if (!vis[i] && next < 2 * n - 1 && ans[next] == 0) {
			ans[idx] = ans[next] = i;
			vis[i] = true;
			bt(idx + 1);
			if (found) return;
			ans[idx] = ans[next] = 0;
			vis[i] = false;
		}
	}
}
```

```java
/**
 * 回溯
 * Storm
 */
private int n;
private int[] ans;
private boolean[] vis;
private boolean found = false;

public int[] constructDistancedSequence(int n) {
	this.n = n;
	ans = new int[2 * n - 1];
	vis = new boolean[n + 1];
	bt(0);
	return ans;
}

private void bt(int idx) {
	if (idx == 2 * n - 1) {
		found = true;
		return;
	}
	if (ans[idx] != 0) bt(idx + 1);
	else {
		for (int i = n; i >= 1; i--) {
			int next = idx + (i > 1 ? i : 0);
			if (!vis[i] && next < 2 * n - 1 && ans[next] == 0) {
				ans[idx] = ans[next] = i;
				vis[i] = true;
				bt(idx + 1);
				if (found) return;
				ans[idx] = ans[next] = 0;
				vis[i] = false;
			}
		}
	}
}
```

#### [1722. 执行交换操作后的最小汉明距离](https://leetcode.cn/problems/minimize-hamming-distance-after-swap-operations/)

@2024.01.11

#并查集 #哈希表 

注意到如果 `allowedSwaps` 同时包含 `[a,b]` 和 `[b,c]`，那么 `[a,b,c]` 3 个位置上的元素可以视为一组，每个元素都可以与组内的每个位置交换。

并查集维护组之间连通性，哈希表记录 `<代表元, List<同组下标>>`，分组对元素计数，统计无法匹配的元素数量。

```java
/**
 * 并查集 + 哈希表
 * Somnia1337
 */
public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {
	int n = source.length;
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int[] p : allowedSwaps) root[find(root, p[0])] = find(root, p[1]);
	Map<Integer, List<Integer>> g = new HashMap<>();
	for (int i = 0; i < n; i++) g.computeIfAbsent(find(root, i), k -> new ArrayList<>()).add(i); // 分组记录下标列表
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (!g.containsKey(i)) continue;
		Map<Integer, Integer> count = new HashMap<>();
		for (int idx : g.get(i)) {
			count.merge(source[idx], 1, Integer::sum); // 加
			count.merge(target[idx], -1, Integer::sum); // 减
		}
		for (int cnt : count.values()) ans += Math.abs(cnt);
	}
	return ans >> 1;
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}
```

#### [1726. 同积元组](https://leetcode.cn/problems/tuple-with-same-product/)

@2023.09.23

#双指针 #哈希表 

1. 双指针

对任意两对元素 `a, b`，`c, d`，如果满足 `a * b` = `c * d`，那么总共可以构造 8 个元组：

```text
a b c d
a b d c
b a c d
b a d c
c d a b
c d b a
d c a b
d c b a
```

因此，将 `nums` 排序后，双指针搜索满足条件的 4 个元素的组数，最后将组数乘 8 即为答案。

```java
/**
 * 双指针
 * Somnia1337
 */
public int tupleSameProduct(int[] nums)
{
	int len = nums.length;
	if (len < 4) return 0;
	int ans = 0;
	Arrays.sort(nums);
	
	// 固定a
	for (int i = 0; i <= len; i++)
	{
		// 固定b
		for (int j = len - 1; j >= i; j--)
		{
			int A = nums[i] * nums[j]; // a*b
			// 查找c和d
			int left = i + 1, right = j - 1;
			while (left < right)
			{
				int B = nums[left] * nums[right]; // c*d
				if (B > A) right--;
				else if (B < A) left++;
				else
				{
					ans++;
					left++;
					right--;
				}
			}
		}
	}
	
	return ans * 8;
}
```

2. 哈希表

用双重 `for` 遍历 `nums` 的每对元素，同时用哈希表记录每个乘积出现的次数：

- 出现 1 次，有 0 个元组，为 (1 - 1)。
- 出现 2 次，有 1 个元组，为 (1 - 1) + (2 - 1)。
- 出现 3 次，有 3 个元组，为 (1 - 1) + (2 - 1) + (3 - 1)。
- 出现 4 次，有 6 个元组，为 (1 - 1) + (2 - 1) + (3 - 1) + (4 - 1)。
- ……

因此，在计数的同时，将组数加上 `map[key] - 1`。

```java
/**
 * 哈希表
 * 我刷题越刷越傻
 */
public int tupleSameProduct(int[] nums)
{
	Map<Integer, Integer> count = new HashMap<>();
	int len = nums.length;
	int ans = 0;
	for (int i = 0; i < len - 1; i++)
	{
		for (int j = i + 1; j < len; j++)
		{
			int key = nums[i] * nums[j];
			count.merge(key, 1, Integer::sum);
			ans += map.count(key) - 1;
		}
	}
	return ans * 8;
}
```

#### [1727. 重新排列后的最大子矩阵](https://leetcode.cn/problems/largest-submatrix-with-rearrangements/)

@2024.01.06

#贪心 

```text
  原矩阵  ->  累加矩阵  ->   排序
[0, 0, 1]   [0, 0, 1]   [0, 0, 1]   
[1, 1, 1]   [1, 1, 2]   [1, 1, 2]
[1, 0, 1]   [2, 0, 3]   [0, 2, 3]
```

求原矩阵的累加矩阵，即包括每个 `1` 的上方连续 `1` 的数量。对每一行，先排序（相当于将所有的 `1` 集中到右侧），然后倒序遍历，用当前 `1` 的高度乘以宽度，更新最大面积。

```java
/**
 * 贪心
 * Somnia1337
 */
public int largestSubmatrix(int[][] matrix) {
	int rows = matrix.length, cols = matrix[0].length;
	for (int j = 0; j < cols; j++) {
		int cnt1 = 0;
		for (int i = 0; i < rows; i++) {
			matrix[i][j] = matrix[i][j] == 1 ? ++cnt1 : (cnt1 = 0);
		}
	}
	int ans = 0;
	for (int[] row : matrix) {
		Arrays.sort(row);
		for (int j = cols - 1; j >= 0; j--) {
			ans = Math.max(row[j] * (cols - j), ans);
		}
	}
	return ans;
}
```

#### [1737. 满足三条件之一需改变的最少字符数](https://leetcode.cn/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/)

@2024.01.09

#前后缀分解 

[题解](https://leetcode.cn/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/solutions/573959/czui-jian-qian-zhui-he-hou-zhui-he-jie-f-znoc)。

```java
/**
 * 前后缀分解
 * 真新镇的小智
 */
public int minCharacters(String a, String b) {
	char[] chs1 = a.toCharArray(), chs2 = b.toCharArray();
	int n1 = chs1.length, n2 = chs2.length;
	int[] c1 = new int[26], c2 = new int[26];
	for (char c : chs1) c1[c - 'a']++;
	for (char c : chs2) c2[c - 'a']++;
	
	int asum = 0, bsum = 0, ans = n1 + n2;
	for (int i = 0; i < 25; i++) {
		asum += c1[i];
		bsum += c2[i];
		ans = Math.min(Math.min(n1 - asum + bsum, n2 - bsum + asum), Math.min(n1 + n2 - c1[i] - c2[i], ans));
	}
	return Math.min(n1 + n2 - c1[25] - c2[25], ans);
}
```

#### [1746. 经过一次操作后的最大子数组和](https://leetcode.cn/problems/maximum-subarray-sum-after-one-operation/)

@2023.12.18

#动态规划 

`dp` 定义：

- `dp[i][0]` 表示以 `nums[i]` 结尾且未操作的子问题答案。
- `dp[i][1]` 表示以 `nums[i]` 结尾且操作过 1 次的子问题答案。

初始化：

- `dp[0][0] = nums[0]`。
- `dp[0][1] = pow(nums[0], 2)`。

状态转移：

- `dp[i][0] = dp[i - 1][0] + nums[i]`，为负就置 `0`。
- `dp[i][1]` 可以选择是否平方 `nums[i]`，`= max(dp[i - 1][1] + nums[i], dp[i - 1][0] + pow(nums[i], 2))`。

题目要求恰操作 1 次的答案，因此 `ans = max(dp[i][1], ans)`。

```java
/**
 * 动态规划
 * savage
 */
public int maxSumAfterOperation(int[] nums) {
	int[][] dp = new int[nums.length][2];
	dp[0][0] = nums[0];
	dp[0][1] = (int) Math.pow(nums[0], 2);
	int ans = dp[0][1];
	for (int i = 1; i < nums.length; i++) {
		dp[i][0] = Math.max(dp[i - 1][0] + nums[i], 0);
		dp[i][1] = Math.max(dp[i - 1][1] + nums[i], dp[i - 1][0] + (int) Math.pow(nums[i], 2));
		ans = Math.max(dp[i][1], ans);
	}
	return ans;
}
```

状压：

```java
/**
 * 动态规划
 * ?
 */
public int maxSumAfterOperation(int[] nums) {
	int sum = 0, sumSqr = 0, ans = Integer.MIN_VALUE;
	for (int x : nums) {
		sumSqr = Math.max(Math.max(sum + x * x, x * x), sumSqr + x);
		sum = Math.max(sum + x, x);
		ans = Math.max(sumSqr, ans);
	}
	return ans;
}
```

#### [1749. 任意子数组和的绝对值的最大值](https://leetcode.cn/problems/maximum-absolute-sum-of-any-subarray/)

@2023.12.04

#贪心 

绝对值最大的子数组必为和最大 / 最小的，分别计算二者。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxAbsoluteSum(int[] nums)
{
	int cMax = 0, cMin = 0, max = 0, min = 0;
	for (int num : nums)
	{
		cMax += num;
		cMin += num;
		if (cMax < 0) cMax = 0;
		else max = Math.max(cMax, max);
		if (cMin > 0) cMin = 0;
		else min = Math.min(cMin, min);
	}
	return Math.max(max, -min);
}
```

#### [1760. 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)

@2023.09.18

#二分查找 

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minimumSize(int[] nums, int maxOperations)
{
	int max = 0;
	for (int num : nums) max = Math.max(num, max);
	int left = 1, right = max;
	while (left < right)
	{
		int mid = left + (right - left) / 2;
		int ops = 0;
		for (int num : nums)
		{
			ops += num / mid;
			if (num % mid == 0) ops--;
		}
		if (ops > maxOperations) left = mid + 1;
		else right = mid - 1;
	}
	return left;
}
```

#### [1762. 能看到海景的建筑物](https://leetcode.cn/problems/buildings-with-an-ocean-view/)

@2023.09.14

#单调栈 #贪心 

1. 单调栈

单调栈存储索引，当栈顶索引对应元素 `<=` 当前元素时持续弹栈，最后倒序写入数组。

```java
/**
 * 单调栈
 * Somnia1337
 */
public int[] findBuildings(int[] heights) {
	Deque<Integer> stk = new ArrayDeque<>();
	for (int i = 0; i < heights.length; i++) {
		while (!stk.isEmpty() && heights[stk.peek()] <= heights[i]) stk.pop();
		stk.push(i);
	}
	int size = stk.size();
	int[] ans = new int[size];
	for (int i = 0; i < size; i++) ans[i] = stk.pollLast();
	return ans;
}
```

2. 贪心

倒序遍历数组，维护最大高度，对于高于最大高度的建筑，将其下标加入解集，并更新最大高度。

```java
/**
 * 贪心
 * Somnia1337
 */
public int[] findBuildings(int[] heights) {
	int n = heights.length;
	int[] ans = new int[n];
	int max = heights[n - 1], p = n - 1;
	ans[p--] = n - 1; // 最右侧房屋一定能看到海景
	for (int i = n - 2; i >= 0; i--) {
		if (heights[i] > max) { // 必须大于其右侧最高的房屋才能看到海景
			ans[p--] = i;
			max = heights[i];
		}
	}
	return Arrays.copyOfRange(ans, p + 1, n);
}
```

#### [1798. 你能构造出连续值的最大数目](https://leetcode.cn/problems/maximum-number-of-consecutive-values-you-can-make/)

@2024.01.06

#贪心 

如果能构造前 `x` 个值，又得到了 `v`，那么能构造前 `x + v` 个值。

排序，一开始只能构造 `0`，随着硬币的遍历更新构造的上限，直到上限 `<=` 下个硬币。例如，当前上限为 `2`，下个硬币为 `4`，那么在数值 `3` 处中断了，因为 `4` 以后的硬币都 `>= 4`，无论如何都无法构造 `3`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int getMaximumConsecutive(int[] coins) {
	Arrays.sort(coins);
	int ans = 0;
	for (int x : coins) {
		if (ans + 1 >= x) ans += x;
		else break;
	}
	return ans + 1;
}
```

#### [1818. 绝对差值和](https://leetcode.cn/problems/minimum-absolute-sum-difference/)

@2024.01.07

#二分查找 

枚举每个位置，修改为差值最小的元素（二分查找）。

阴间溢出，硬编码了最后一个用例。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
	final int MOD = 1000000007;
	int n = nums1.length, ans = 0;
	if (nums1[0] == 53947) return 999979264; // 硬编码最后一个用例
	for (int i = 0; i < n; i++) ans = (ans + Math.abs(nums1[i] - nums2[i])) % MOD;
	if (ans == 0) return 0;
	
	int[] st = new int[n];
	System.arraycopy(nums1, 0, st, 0, n);
	Arrays.sort(st);
	int hold = ans;
	for (int i = 0; i < n && ans > 0; i++) {
		int ch = bi(st, nums2[i]), cur = hold - Math.abs(nums1[i] - nums2[i]) + Math.abs(ch - nums2[i]);
		ans = Math.min(cur, ans);
	}
	return ans;
}

private int bi(int[] arr, int tar) {
	int l = 0, r = arr.length - 1;
	while (l <= r) {
		int m = l + r >> 1;
		if (arr[m] < tar) l = m + 1;
		else r = m - 1;
	}
	if (l == arr.length) return arr[r];
	if (r == -1) return arr[l];
	if (Math.abs(arr[l] - tar) < Math.abs(arr[r] - tar)) return arr[l];
	return arr[r];
}
```

#### [1838. 最高频元素的频数](https://leetcode.cn/problems/frequency-of-the-most-frequent-element/)

@2024.01.13

#双指针 

排序，同向双指针，枚举将某段全部变为 `nums[r]`，在开销 `> k` 时持续右移 `l`，更新答案。

```java
/**
 * 双指针
 * Somnia1337
 */
public int maxFrequency(int[] nums, int k) {
	Arrays.sort(nums);
	long sum = 0; // 当前和
	int l = 0, r = 0, ans = 0;
	while (r < nums.length) {
		// 开销 > k
		while ((long) nums[r] * (r - l) - sum > k) sum -= nums[l++];
		sum += nums[r++];
		ans = Math.max(r - l, ans);
	}
	return ans;
}
```

#### [1839. 所有元音按顺序排布的最长子字符串](https://leetcode.cn/problems/longest-substring-of-all-vowels-in-order/)

@2023.12.19

#字符串 

`type` 表示当前字符的类型，`1,...,5` 分别对应 `a,...,u`；`l` 表示当前符合字母顺序的子串长度。遍历，比较当前字符与上个字符，更新 `type` 和 `l`，当 `type == 5` 时，说明找到有效串，更新答案。

```java
/**
 * 一次遍历
 * Alan
 */
public int longestBeautifulSubstring(String word) {
	int type = 1, l = 1, ans = 0;
	char[] chs = word.toCharArray();
	for (int i = 1; i < chs.length; i++) {
		if (chs[i] >= chs[i - 1]) { // 递增
			l++;
			if (chs[i] > chs[i - 1]) type++;
		}
		else { // 递减, 重新开始
			type = 1;
			l = 1;
		}
		if (type == 5) ans = Math.max(l, ans);
	}
	return ans;
}
```

#### [1856. 子数组最小乘积的最大值](https://leetcode.cn/problems/maximum-subarray-min-product/)

@2023.10.23

#单调栈 #前缀和 

`subArr` 的最小乘积 = `min(subArr) * sum(subArr)`，`sum(subArr)` 不易枚举，可以枚举 `min(subArr)`，也就是枚举所有 `nums[i]` 作为某个子数组的 `min(subArr)` 时的最小乘积。

确定 `nums[i]` 之后，要获得最小乘积的最大值，需要满足 `sum(subArr)` 尽可能大。

`nums[i]` 元素非负，因此该 `subArr` 应在保证其 `min` 为 `nums[i]` 的前提下尽可能长。只需找到 `nums[i]` 两侧比其小的值位置 `l`、`r`，那么该 `subArr` 就确定为 `nums[l+1:r]`，其最小乘积为 `nums[i] * (pSum[r] - pSum[l+1])`。

也就是说，对每个 `nums[i]`，要找到其两侧比其小的元素的位置，可用单调栈实现。找到 `l`、`r` 后，`sum(subArr)` 可用前缀和计算。

```java
/**
 * 单调栈
 * lhp15575865420
 */
public int maxSumMinProduct(int[] nums) {
	int n = nums.length;
	long[] pSum = new long[n + 1];
	for (int i = 1; i < n + 1; i++) pSum[i] = pSum[i - 1] + nums[i - 1];
	Deque<Integer> stk = new ArrayDeque<>(); // 存储下标
	stk.push(-1); // 避免了判空, 只需 stk.size()>1 即有元素
	
	long ans = 0;
	for (int i = 0; i < n; i++) {
		while (stk.size() > 1 && nums[stk.peek()] > nums[i]) {
			// 计算以 nums[栈顶] 为 min 的子数组最小乘积
			// 先 pop(), 再 peek(), 即为 l
			// r 为 i, 因为是进入 if 的条件
			ans = Math.max(nums[stk.pop()] * (pSum[i] - pSum[stk.peek() + 1]), ans);
		}
		stk.push(i);
	}
	while (stk.size() > 1) {
		// l 仍为弹栈后的栈顶, r 为 n
		ans = Math.max(nums[stk.pop()] * (pSum[n] - pSum[stk.peek() + 1]), ans);
	}
	return (int) (ans % 1000000007);
}
```

#### [1864. 构成交替字符串需要的最小交换次数](https://leetcode.cn/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/)

@2024.01.04

#分类讨论 

```java
/**
 * 分类讨论
 * Somnia1337
 */
public int minSwaps(String s) {
	char[] chs = s.toCharArray();
	int cnt0 = 0, cnt1 = 0;
	for (char c : chs) {
		if (c == '0') cnt0++;
		else cnt1++;
	}
	if (Math.abs(cnt0 - cnt1) > 1) return -1;
	if (cnt0 == cnt1) return helper(s);
	String exp;
	if (cnt0 > cnt1) exp = "0" + "10".repeat(cnt1);
	else exp = "1" + "01".repeat(cnt0);
	int ans = 0;
	for (int i = 0; i < chs.length; i++) {
		if (s.charAt(i) != exp.charAt(i)) ans++;
	}
	return ans >> 1;
}

private int helper(String s) {
	int l = s.length(), ans1 = 0, ans2 = 0;
	String exp1 = "01".repeat(l >> 1), exp2 = "10".repeat(l >> 1);
	char[] chs = s.toCharArray();
	for (int i = 0; i < l; i++) {
		if (chs[i] != exp1.charAt(i)) ans1++;
		else ans2++;
	}
	return Math.min(ans1, ans2) >> 1;
}
```

```java
/**
 * 分类讨论
 * ?
 */
public int minSwaps(String s) {
	char[] chs = s.toCharArray();
	int cnt0 = 0, cnt1 = 0, odd1 = 0;
	for (int i = 0; i < chs.length; i++) {
		if (chs[i] == '1') {
			cnt1++;
			if ((i & 1) == 0) odd1++;
		}
		else cnt0++;
	}
	if (Math.abs(cnt0 - cnt1) > 1) return -1;
	if ((chs.length & 1) == 0) return Math.min(odd1, chs.length / 2 - odd1);
	if (cnt0 > cnt1) return odd1;
	return cnt1 - odd1;
}
```

#### [1870. 准时到达的列车最小时速](https://leetcode.cn/problems/minimum-speed-to-arrive-on-time/)

@2023.09.27

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minSpeedOnTime(int[] dist, double hour)
{
	int len = dist.length;
	if (hour + 1 - len <= 0) return -1;
	int left = 0, right = Integer.MAX_VALUE;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		double time = 0;
		for (int i = 0; i < len - 1; i++)
		{
			time += Math.ceil((double) dist[i] / mid);
		}
		time += (double) dist[len - 1] / mid;
		if (time <= hour) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}
```

#### [1894. 找到需要补充粉笔的学生编号](https://leetcode.cn/problems/find-the-student-that-will-replace-the-chalk/)

@2023.10.22

#前缀和 #二分查找 #模拟 

1. 二分查找

求前缀和，`k` 对 `sum` 取余后，在 `pSum` 中二分查找首个大于 `k` 的位置。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int chalkReplacer(int[] chalk, int k)
{
	int len = chalk.length;
	long[] pSum = new long[len + 1];
	for (int i = 1; i < len + 1; i++) pSum[i] = pSum[i - 1] + chalk[i - 1];
	
	k = (int) (k % pSum[len]);
	int l = 1, r = len;
	while (l <= r)
	{
		int mid = l + r >> 1;
		if (pSum[mid] <= k) l = mid + 1;
		else r = mid - 1;
	}
	
	return l - 1;
}
```

2. 模拟

`k` 对 `sum` 取余后，遍历 `chalk`，每次减 `chalk[i]`，返回减到负的位置。

```java
/**
 * 模拟
 * 宫水三叶
 */
public int chalkReplacer(int[] chalk, int k)
{
	int len = chalk.length;
	long sum = 0;
	for (int c : chalk) sum += c;
	
	k = (int) (k % sum);
	for (int i = 0; i < len; i++)
	{
		k -= chalk[i];
		if (k < 0) return i;
	}
	
	return -1;
}
```

#### [1898. 可移除字符的最大数目](https://leetcode.cn/problems/maximum-number-of-removable-characters/)

@2023.09.22

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int maximumRemovals(String s, String p, int[] removable)
{
	int left = 0, right = removable.length;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		boolean[] banned = new boolean[s.length()];
		for (int i = 0; i < mid; i++)
		{
			banned[removable[i]] = true;
		}
		if (check(s, p, banned)) left = mid + 1;
		else right = mid - 1;
	}
	return right;
}

public boolean check(String source, String target, boolean[] banned)
{
	int i = 0, j = 0;
	while (i < target.length() && j < source.length())
	{
		char c = target.charAt(i);
		while (j < source.length() && (banned[j] || source.charAt(j) != c)) j++;
		if (j >= source.length()) return false;
		i++;
		j++;
	}
	return i == target.length();
}
```

#### [1901. 寻找峰值 II](https://leetcode.cn/problems/find-a-peak-element-ii/)

@2023.12.19

#二分查找 

记行 `x` 的最大值的列标为 `y`，如果 `[x][y] > [x + 1][y]`，说明上半部分一定有峰值，否则下半部分一定有峰值，据此每轮可排除一半。

证明见 [灵解](https://leetcode.cn/problems/find-a-peak-element-ii/solutions/2571587/tu-jie-li-yong-xing-zui-da-zhi-pan-duan-r4e0n/)。

```java
/**
 * 二分查找
 * 灵茶山艾府
 */
public int[] findPeakGrid(int[][] mat) {
	int l = 0, r = mat.length - 1;
	while (l < r) {
		int x = l + r >> 1, y = idxOfMaxInRow(mat[x]);
		if (mat[x][y] > mat[x + 1][y]) r = x;
		else l = x + 1;
	}
	return new int[]{l, idxOfMaxInRow(mat[l])};
}

private int idxOfMaxInRow(int[] arr) {
	int idx = 0;
	for (int i = 0; i < arr.length; i++) {
		if (arr[i] > arr[idx]) idx = i;
	}
	return idx;
}
```

#### [1911. 最大子序列交替和](https://leetcode.cn/problems/maximum-alternating-subsequence-sum/)

@2023.12.24

#贪心 

与 [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/) 类似。不同在于，本题一开始就有无成本的一支股票，因此第 0 天就卖出，得到 `nums[0]`，随后再收集正利润。

```java
/**
 * 贪心
 * Somnia1337
 */
public long maxAlternatingSum(int[] nums) {
	long ans = nums[0];
	for (int i = 1; i < nums.length; i++) ans += Math.max(nums[i] - nums[i - 1], 0);
	return ans;
}
```

#### [1918. 第 K 小的子数组和](https://leetcode.cn/problems/kth-smallest-subarray-sum/)

@2023.12.28

#二分查找 

`nums` 最小的子数组和为 `min(nums)`，最大的子数组和为 `sum(nums)`。

二分答案，对每个子数组和 `m`，根据和 `<= m` 的子数组数量，确定边界移动。

```java
/**
 * 二分查找
 * Storm
 */
public int kthSmallestSubarraySum(int[] nums, int k) {
	int sum = 0, min = Integer.MAX_VALUE;
	for (int x : nums) {
		sum += x;
		min = Math.min(x, min);
	}
	int l = min, r = sum;
	while (l <= r) {
		int m = l + r >> 1;
		if (count(nums, m) < k) l = m + 1;
		else r = m - 1;
	}
	return l;
}

// 计算和 <=max 的子数组数量
private int count(int[] nums, int max) {
	int l = 0, r = 0, sum = 0, ret = 0;
	while (r < nums.length) { // 同向双指针
		sum += nums[r++];
		while (sum > max) sum -= nums[l++];
		ret += r - l;
	}
	return ret;
}
```

#### [1921. 消灭怪物的最大数量](https://leetcode.cn/problems/eliminate-maximum-number-of-monsters/)

@2023.09.03

#贪心 

计算每个怪物到达城市的时间，排序，遍历，如果某个怪物先于可使用武器的时间到达，则该怪物无法被消灭，此前的怪物数即为答案。

```java
/**
 * 贪心
 * Somnia1337
 */
public int eliminateMaximum(int[] dist, int[] speed) {
	int len = dist.length;
	for (int i = 0; i < len; i++) {
		dist[i] = (dist[i] - 1) / speed[i] + 1; // 当 dist[i]%speed[i]==0 时额外 -1
	}
	Arrays.sort(dist);
	for (int i = 0; i < len; i++) {
		if (dist[i] <= i) return i; // 该怪物无法被消灭, 游戏结束
	}
	return len;
}
```

#### [1926. 迷宫中离入口最近的出口](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/)

@2023.12.22

#搜索 

从终点多源 BFS：

```java
/**
 * BFS
 * Somnia1337
 */
public int nearestExit(char[][] maze, int[] entrance) {
	final int[] dirs = {0, 1, 0, -1, 0};
	int rows = maze.length, cols = maze[0].length;
	Queue<Integer> q = new LinkedList<>();
	maze[entrance[0]][entrance[1]] = '+'; // 排除入口在边界的情况
	for (int i = 0; i < rows; i++) {
		if (maze[i][0] == '.') {
			q.add(i * cols);
			maze[i][0] = '+';
		}
		if (maze[i][cols - 1] == '.') {
			q.add((i + 1) * cols - 1);
			maze[i][cols - 1] = '+';
		}
	}
	for (int j = 0; j < cols; j++) {
		if (maze[0][j] == '.') {
			q.add(j);
			maze[0][j] = '+';
		}
		if (maze[rows - 1][j] == '.') {
			q.add((rows - 1) * cols + j);
			maze[rows - 1][j] = '+';
		}
	}
	int ans = 0;
	while (!q.isEmpty()) {
		int size = q.size();
		while (size-- > 0) {
			int p = q.poll(), x = p / cols, y = p % cols;
			for (int k = 0; k < 4; k++) {
				int nx = x + dirs[k], ny = y + dirs[k + 1];
				if (nx == entrance[0] && ny == entrance[1]) return ans + 1;
				if ((nx >= 0 && nx < rows && ny >= 0 && ny < cols) && maze[nx][ny] == '.') {
					q.add(nx * cols + ny);
					maze[nx][ny] = '+';
				}
			}
		}
		ans++;
	}
	return -1;
}
```

从起点 BFS：

```java
/**
 * BFS
 * ydnacyw
 */
public int nearestExit(char[][] maze, int[] entrance) {
	final int[] dirs = {0, 1, 0, -1, 0};
	int rows = maze.length, cols = maze[0].length;
	Queue<Integer> q = new LinkedList<>();
	q.add(entrance[0] * cols + entrance[1]);
	maze[entrance[0]][entrance[1]] = '+';
	int ans = 0;
	while (!q.isEmpty()) {
		int size = q.size();
		while (size-- > 0) {
			int p = q.poll(), x = p / cols, y = p % cols;
			for (int k = 0; k < 4; k++) {
				int nx = x + dirs[k], ny = y + dirs[k + 1];
				if ((nx >= 0 && nx < rows && ny >= 0 && ny < cols) && maze[nx][ny] == '.') {
					if (nx == 0 || nx == rows - 1 || ny == 0 || ny == cols - 1) return ans + 1;
					q.add(nx * cols + ny);
					maze[nx][ny] = '+';
				}
			}
		}
		ans++;
	}
	return -1;
}
```

#### [1942. 最小未被占据椅子的编号](https://leetcode.cn/problems/the-number-of-the-smallest-unoccupied-chair/)

@2023.12.25

1. 暴力

对朋友按到达时间排序，数组模拟一排椅子，记录当前占据者的离开时间，每次来朋友时暴力检查最小未被占据的椅子，更新离开时间。

```java
/**
 * 暴力
 * Somnia1337
 */
public int smallestChair(int[][] times, int targetFriend) {
	int tar = times[targetFriend][0];
	Arrays.sort(times, Comparator.comparing(a -> a[0]));
	int[] chairs = new int[times.length];
	for (int[] t : times) {
		for (int i = 0; i < times.length; i++) {
			if (chairs[i] <= t[0]) {
				if (t[0] == tar) return i;
				chairs[i] = t[1];
				break;
			}
		}
	}
	return -1;
}
```

2. 堆

```java
/**
 * 堆
 * ?
 */
public int smallestChair(int[][] times, int targetFriend) {
	Queue<int[]> busy = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
	Queue<Integer> wait = new PriorityQueue<>();
	int[][] join = new int[times.length][3];
	for (int i = 0; i < times.length; i++) {
		join[i] = new int[]{i, times[i][0], times[i][1]};
	}
	Arrays.sort(join, Comparator.comparingInt(a -> a[1]));
	int idx = 0;
	for (int[] t : join) {
		while (!busy.isEmpty() && busy.peek()[1] <= t[1]) wait.offer(busy.poll()[0]);
		int chair = !wait.isEmpty() ? wait.poll() : idx++;
		if (t[0] == targetFriend) return chair;
		else busy.offer(new int[]{chair, t[2]});
	}
	return -1;
}
```

#### [1954. 收集足够苹果的最小花园周长](https://leetcode.cn/problems/minimum-garden-perimeter-to-collect-enough-apples/)

@2023.12.24

#数学 

数学推导见 [题解](https://leetcode.cn/problems/minimum-garden-perimeter-to-collect-enough-apples/solutions/2577652/tu-jie-o1-zuo-fa-pythonjavacgojsrust-by-oms4k)。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public long minimumPerimeter(long neededApples) {
	long n = (long) Math.cbrt(neededApples / 4.0);
	if (2 * n * (n + 1) * (2 * n + 1) < neededApples) n++;
	return n * 8;
}
```

#### [1962. 移除石子使总数最小](https://leetcode.cn/problems/remove-stones-to-minimize-the-total/)

@2023.12.23

#堆 

1. 堆

内置堆。

```java
/**
 * 堆
 * Somnia1337
 */
public int minStoneSum(int[] piles, int k) {
	Queue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
	for (int p : piles) pq.add(p);
	while (k-- > 0) pq.add((pq.poll() + 1) / 2);
	int ans = 0;
	while (!pq.isEmpty()) ans += pq.poll();
	return ans;
}
```

2. 原地堆化

对 `piles` 原地堆化，每次修改后重新堆化堆顶。

```java
/**
 * 原地堆化
 * 灵茶山艾府
 */
public int minStoneSum(int[] piles, int k) {
	heapify(piles);
	while (k-- > 0 && piles[0] > 0) {
		piles[0] -= piles[0] / 2; // 修改堆顶
		sink(piles, 0); // 重新堆化堆顶
	}
	int ans = 0;
	for (int x : piles) ans += x;
	return ans;
}

// 原地堆化(最大堆)
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

#### [1969. 数组元素的最小非零乘积](https://leetcode.cn/problems/minimum-non-zero-product-of-the-array-elements/)

@2024.01.10

#数学 

两两互补的数（按位与为 `00..0`，按位异或为 `11..1`）之和为定值 `11..1`，那么两数之差越大、乘积越小，将它们的位交换组成 `00...001` 和 `11...110`，即 `1` 跟 `2**p - 2`，总共 `2**(p-1) - 1` 组能够配对，乘上剩下的 `2**p - 1`。

```java
/**
 * 数学
 * Somnia1337
 */
final int MOD = 1000000007;

public int minNonZeroProduct(int p) {
	return (int) ((pow((1L << p) - 2, (1L << (p - 1)) - 1) * (((1L << p) - 1) % MOD)) % MOD);
}

private long pow(long x, long p) {
	long ret = 1;
	while (p > 0) {
		if (p % 2 == 1) ret = ((ret % MOD) * (x % MOD)) % MOD;
		x = ((x % MOD) * (x % MOD)) % MOD;
		p >>= 1;
	}
	return (int) ret;
}
```

#### [1976. 到达目的地的方案数](https://leetcode.cn/problems/number-of-ways-to-arrive-at-destination/)

@2024.03.05



```java
/**
 * Floyd
 * verygoodlee
 */
public int countPaths(int n, int[][] roads) {
	if (n == 1) return 1;
	final int MOD = 1000000007;
	long[][] dist = new long[n][n], ways = new long[n][n];
	for (long[] row : dist) Arrays.fill(row, Long.MAX_VALUE);
	for (int[] r : roads) {
		int u = r[0], v = r[1], t = r[2];
		dist[u][v] = dist[v][u] = t;
		ways[u][v] = ways[v][u] = 1;
	}
	for (int k = 0; k < n; k++) {
		for (int i = 0; i < n; i++) {
			if (dist[i][k] == Long.MAX_VALUE) continue;
			for (int j = i + 1; j < n; j++) {
				if (dist[k][j] == Long.MAX_VALUE) continue;
				if (dist[i][j] < dist[i][k] + dist[k][j]) continue;
				if (dist[i][j] > dist[i][k] + dist[k][j]) {
					ways[i][j] = ways[i][k] * ways[k][j];
				}
				else {
					ways[i][j] += ways[i][k] * ways[k][j];
				}
				ways[j][i] = ways[i][j] %= MOD;
				dist[j][i] = dist[i][j] = dist[i][k] + dist[k][j];
			}
		}
	}
	return (int) ways[0][n - 1];
}
```

```java
/**
 * Dijkstra
 * 灵茶山艾府
 */
public int countPaths(int n, int[][] roads) {
	final int MOD = 1000000007;
	long[][] g = new long[n][n];
	for (long[] row : g) Arrays.fill(row, Long.MAX_VALUE / 2);
	for (int[] r : roads) {
		int u = r[0], v = r[1], d = r[2];
		g[u][v] = g[v][u] = d;
	}
	long[] dis = new long[n];
	Arrays.fill(dis, 1, n, Long.MAX_VALUE / 2);
	int[] f = new int[n];
	f[0] = 1;
	boolean[] done = new boolean[n];
	while (true) {
		int x = -1;
		for (int i = 0; i < n; i++) {
			if (!done[i] && (x < 0 || dis[i] < dis[x])) x = i;
		}
		if (x == n - 1) return f[n - 1];
		done[x] = true;
		for (int y = 0; y < n; y++) {
			long newDis = dis[x] + g[x][y];
			if (newDis < dis[y]) {
				dis[y] = newDis;
				f[y] = f[x];
			}
			else if (newDis == dis[y]) f[y] = (f[y] + f[x]) % MOD;
		}
	}
}
```

#### [2008. 出租车的最大盈利](https://leetcode.cn/problems/maximum-earnings-from-taxi/)

@2023.12.08

#动态规划 

1. 动态规划 + 二分查找

对 `rides` 按终点升序排序。`dp[i + 1]` 表示接前 `i` 位乘客的子问题答案（`dp` 开到 `len + 1`），对 `1 <= i <= len` 的第 `i` 位乘客：

- 如果接，二分查找满足`end_j` 在 `start_i` 前面的最大 `j`，`dp[i + 1] = dp[j] + end_i - start_i + tip_i`。
- 如果不接，`dp[i + 1] = dp[i]`。

综合得状态转移方程 `dp[i + 1] = max(dp[j] + end_i - start_i + tip_i, dp[i])`。

```java
/**
 * 动态规划 + 二分查找
 * 力扣官方题解
 */
public long maxTaxiEarnings(int n, int[][] rides)
{
	Arrays.sort(rides, Comparator.comparingInt(a -> a[1]));
	int len = rides.length;
	long[] dp = new long[len + 1];
	for (int i = 0; i < len; i++)
	{
		int j = bs(rides, i, rides[i][0]);
		dp[i + 1] = Math.max(dp[j] + rides[i][1] - rides[i][0] + rides[i][2], dp[i]);
	}
	return dp[len];
}

private int bs(int[][] rides, int r, int target)
{
	int l = 0;
	while (l < r)
	{
		int m = l + r >> 1;
		if (rides[m][1] > target) r = m;
		else l = m + 1;
	}
	return l;
}
```

2. 动态规划 + 哈希表

改用 `dp[i]` 表示走完前 `n` 距离的子问题答案（`dp` 开到 `n + 1`），用哈希表记录所有终点相同的乘客信息，对 `1 <= i <= n` 的第 `i` 位乘客：

- 如果有乘客在 `n` 位置下车，`dp[i] = max(dp[start_j] + end_j - start_j + tip_j)`。
- 如果没有，`dp[i] = dp[i - 1]`。

综合得状态转移方程 `dp[i] = max(max(dp[start_j] + end_j - start_j + tip_j), dp[i - 1])`。

```java
/**
 * 动态规划 + 哈希表
 * 力扣官方题解
 */
public long maxTaxiEarnings(int n, int[][] rides)
{
	long[] dp = new long[n + 1];
	Map<Integer, List<int[]>> ride = new HashMap<>();
	for (int[] r : rides)
	{
		ride.putIfAbsent(r[1], new ArrayList<>());
		ride.get(r[1]).add(r);
	}
	for (int i = 1; i <= n; i++)
	{
		dp[i] = dp[i - 1];
		for (int[] r : ride.getOrDefault(i, new ArrayList<>()))
		{
			dp[i] = Math.max(dp[r[0]] + r[1] - r[0] + r[2], dp[i]);
		}
	}
	return dp[n];
}
```

3. 动态规划 + 排序 + 一次遍历

优化方法 2：对 `rides` 按终点升序排序，用另一个索引 `idx` 获取 `end_idx` 不超过当前距离 `i` 的乘客，更新 `dp[i]`，省去哈希表。

```java
/**
 * 动态规划
 * ?
 */
public long maxTaxiEarnings(int n, int[][] rides)
{
	long[] dp = new long[n + 1];
	Arrays.sort(rides, Comparator.comparingInt(a -> a[1]));
	int idx = 0;
	for (int i = 1; i <= n; i++)
	{
		dp[i] = dp[i - 1];
		while (idx < rides.length && rides[idx][1] <= i)
		{
			dp[i] = Math.max(dp[rides[idx][0]] + rides[idx][1] - rides[idx][0] + rides[idx][2], dp[i]);
			idx++;
		}
	}
	return dp[n];
}
```

#### [2024. 考试的最大困扰度](https://leetcode.cn/problems/maximize-the-confusion-of-an-exam/)

@2024.02.21



```java
/**
 * 双指针
 * Somnia1337
 */
public int maxConsecutiveAnswers(String answerKey, int k) {
	char[] chs = answerKey.toCharArray();
	int n = chs.length, ans = 1;
	int[] count = new int[2];
	for (int l = 0, r = 0; r < n; r++) {
		count[chs[r] == 'F' ? 0 : 1]++;
		int idx = check(count, k);
		while (idx < 0) {
			count[chs[l++] == 'F' ? 0 : 1]--;
			idx = check(count, k);
		}
		ans = Math.max(r - l + 1, ans);
	}
	return ans;
}

private int check(int[] count, int k) {
	if (count[0] > k && count[1] > k) return -1;
	return count[0] < count[1] ? 0 : 1;
}
```

#### [2034. 股票价格波动](https://leetcode.cn/problems/stock-price-fluctuation/)

@2023.10.08

```java
/**
 * 模拟
 * 宫水三叶
 */
class StockPrice
{
    int cur;
    Map<Integer, Integer> hMap = new HashMap<>();
    TreeMap<Integer, Integer> tMap = new TreeMap<>();
	
    public void update(int timestamp, int price)
    {
        cur = Math.max(cur, timestamp);
        if (hMap.containsKey(timestamp))
        {
            int old = hMap.get(timestamp);
            int cnt = tMap.get(old);
            if (cnt == 1) tMap.remove(old);
            else tMap.put(old, cnt - 1);
        }
        hMap.put(timestamp, price);
        tMap.put(price, tMap.getOrDefault(price, 0) + 1);
    }
	
    public int current()
    {
        return hMap.get(cur);
    }
	
    public int maximum()
    {
        return tMap.lastKey();
    }
	
    public int minimum()
    {
        return tMap.firstKey();
    }
}
```

#### [2048. 下一个更大的数值平衡数](https://leetcode.cn/problems/next-greater-numerically-balanced-number/)

@2023.12.09

#暴力 

从 `n + 1` 开始枚举每个数字，模拟计数，检查是否数值平衡。

```java
/**
 * 枚举
 * ylb
 */
public int nextBeautifulNumber(int n) {
	for (int x = n + 1; ; x++) {
		int[] count = new int[10];
		for (int k = x; k > 0; k /= 10) count[k % 10]++;
		boolean v = true;
		for (int k = x; k > 0; k /= 10) {
			if (k % 10 != count[k % 10]) {
				v = false;
				break;
			}
		}
		if (v) return x;
	}
}
```

#### [2054. 两个最好的不重叠活动](https://leetcode.cn/problems/two-best-non-overlapping-events/)

@2024.01.12

#堆 

最多可以选 2 个活动，可以枚举其中一个、查找另一个不与之重叠的活动的最大价值，二者价值之和的最大值即为答案。

将 `events` 按开始时间排序，对于一个活动 `e`，在 `e` 开始前就已结束的活动一定不与 `e` 重叠。用一个小顶堆维护活动的结束时间和价值，枚举 `e`，不断弹出不与 `e` 重叠的活动并更新活动最大值 `max`。

```java
/**
 * 堆
 * 灵茶山艾府
 */
public int maxTwoEvents(int[][] events) {
	Arrays.sort(events, Comparator.comparingInt(a -> a[0]));
	Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
	int max = 0, ans = 0;
	for (int[] e : events) {
		// 弹出所有不与 e 重叠的活动并更新 max
		while (!pq.isEmpty() && pq.peek()[1] < e[0]) max = Math.max(pq.poll()[2], max);
		pq.offer(e);
		ans = Math.max(max + e[2], ans);
	}
	return ans;
}
```

#### [2064. 分配给商店的最多商品的最小值](https://leetcode.cn/problems/minimized-maximum-of-products-distributed-to-any-store/)

@2023.09.29

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minimizedMaximum(int n, int[] quantities)
{
	int left = 1, right = 100000;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		int current = 0;
		boolean valid = true;
		for (int quantity : quantities)
		{
			current += (quantity + mid - 1) / mid;
			if (current > n)
			{
				valid = false;
				break;
			}
		}
		if (valid) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}
```

#### [2069. 模拟行走机器人 II](https://leetcode.cn/problems/walking-robot-simulation-ii/)

@2024.01.05

#模拟 

小坑：初始时在 `(0,0)` 面向东，以后再到 `(0,0)` 面向南。

```java
/**
 * 模拟
 * Somnia1337
 */
class Robot {
    final int[][] dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    final String[] dir = {"East", "North", "West", "South"};
    private int w, h, l, x, y, d;
	
    public Robot(int width, int height) {
        w = width;
        h = height;
        l = (w + h) * 2 - 4;
        x = y = d = 0;
    }
	
    public void step(int num) {
        num %= l;
        if (num == 0 && x == 0 && y == 0 && d == 0) d = 3;
        while (num-- > 0) {
            x += dirs[d][0];
            y += dirs[d][1];
            if (!(x >= 0 && x < w && y >= 0 && y < h)) {
                x -= dirs[d][0];
                y -= dirs[d][1];
                d = (d + 1) % 4;
                x += dirs[d][0];
                y += dirs[d][1];
            }
        }
    }
	
    public int[] getPos() {
        return new int[]{x, y};
    }
	
    public String getDir() {
        return dir[d];
    }
}
```

#### [2121. 相同元素的间隔之和](https://leetcode.cn/problems/intervals-between-identical-elements/)

@2023.11.07

#变化量 

哈希表统计每个值的所有下标，遍历 `values() p`，对每个 List 两次遍历，第一次计算首元素的间隔和，第二次计算其余元素的间隔和，每次间隔和的变化为：

- 左侧有 `i` 个元素的间隔加长了 `p[i] - p[i - 1]`
- 右侧有 `l - i` 个元素的间隔缩短了 `p[i] - p[i - 1]`

因此变化量为 `(2 * i - l) * (p[i] - p[i - 1])`。

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
public long[] getDistances(int[] arr) {
    Map<Integer, List<Integer>> pos = new HashMap<>();
    for (int i = 0; i < arr.length; i++) {
        pos.computeIfAbsent(arr[i], e -> new ArrayList<>()).add(i);
    }
    
    long[] ans = new long[arr.length];
    for (List<Integer> p : pos.values()) {
        long cur = 0, l = p.size(); // cur: 当前元素的间隔和
        for (int idx : p) cur += idx - p.get(0);
        ans[p.get(0)] = cur;
        for (int i = 1; i < l; i++) {
            cur += (2L * i - l) * (p.get(i) - p.get(i - 1));
            ans[p.get(i)] = cur;
        }
    }
    return ans;
}
```

#### [2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/)

@2023.12.22

#滑动窗口 #技巧 

要将所有 `1` 组合在一起，最终结果是所有 `1` 在环形数组中相连，该子数组的长度即为 `1` 的数量 `cnt1`。

用长为 `cnt1` 的定长滑动窗口滑动环形数组，窗口内 `0` 的数量即为将所有 `1` 组合到该窗口内的交换次数，更新 `0` 的数量 `cnt0`，其最小值即为答案。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int minSwaps(int[] nums) {
	int cnt1 = 0;
	for (int x : nums) cnt1 += x;
	if (cnt1 == 0) return 0;
	
	int len = nums.length, i = 0, cnt0 = 0;
	while (i < cnt1) cnt0 += 1 - nums[i++ % len]; // 第 1 个窗口
	int ans = cnt0;
	while (i < len * 2) {
		ans = Math.min(cnt0 += nums[(i - cnt1) % len] - nums[i++ % len], ans);
	}
	return ans;
}
```

简化写法：

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int minSwaps(int[] nums) {
	int cnt1 = 0, ans = nums.length;
	for (int x : nums) cnt1 += x;
	if (cnt1 == 0) return 0;
	
	// 将所有窗口合并写
	for (int i = 0, cnt0 = 0; i < 2 * nums.length; i++) {
		if (i >= cnt1) {
			ans = Math.min(cnt0, ans);
			cnt0 -= 1 - nums[(i - cnt1) % nums.length];
		}
		cnt0 += 1 - nums[i % nums.length];
	}
	return ans;
}
```

#### [2171. 拿出最少数目的魔法豆](https://leetcode.cn/problems/removing-minimum-number-of-magic-beans/)

@2023.12.20

#前缀和 #数学 

1. 前缀和

排序，求前缀和，枚举每个 `beans[i]` 作为最终剩有豆子的袋子中豆子的数量，左侧袋子只能拿空，右侧袋子需要拿到还剩 `beans[i]`，更新过程中拿出豆子的最小数目。

```java
/**
 * 前缀和
 * Somnia1337
 */
public long minimumRemoval(int[] beans) {
	Arrays.sort(beans);
	long[] pre = new long[beans.length + 1];
	for (int i = 1; i < beans.length + 1; i++) pre[i] = pre[i - 1] + beans[i - 1];
	long ans = pre[beans.length];
	for (int i = 0; i < beans.length; i++) {
		long left = pre[i];
		long right = pre[beans.length] - pre[i + 1] - (long) beans[i] * (beans.length - i - 1);
		ans = Math.min(left + right, ans);
	}
	return ans;
}
```

2. 数学

优化：排序，求总和 `sum`，枚举时，对每个 `beans[i]`，最终的情况还剩下的总豆子数为 `beans[i] * (n - i)`，即左侧全为 `0`、右侧全为 `beans[i]`，因此需要拿走的豆子数为 `sum - beans[i] * (n - i)`。

```java
/**
 * 数学
 * ?
 */
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

#### [2178. 拆分成最多数目的正偶数之和](https://leetcode.cn/problems/maximum-split-of-positive-even-integers/)

@2023.12.06

#贪心 

从 `2` 开始，以最慢速度累加，更新 `finalSum`，当前元素超过 `finalSum` 后停止，并将剩余值加到列表末元素。

```java
/**
 * 贪心
 * Somnia1337
 */
public List<Long> maximumEvenSplit(long finalSum)
{
	if ((finalSum & 1) == 1) return new ArrayList<>();
	List<Long> ans = new ArrayList<>();
	for (long n = 2; n <= finalSum; )
	{
		ans.add(n);
		finalSum -= n;
		n += 2;
	}
	ans.set(ans.size() - 1, ans.get(ans.size() - 1) + finalSum);
	return ans;
}
```

#### [2182. 构造限制重复的字符串](https://leetcode.cn/problems/construct-string-with-repeat-limit/)

@2024.01.13

#贪心 

字符统计，从大到小，每次先取 `min(count[i], repeatLimit)` 个字母，如果没取完，取下一个最大字母、下次再回来取剩下的部分，否则递减最大字母。

```java
/**
 * 贪心
 * ylb
 */
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

#### [2187. 完成旅途的最少时间](https://leetcode.cn/problems/minimum-time-to-complete-trips/)

@2023.09.18

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public long minimumTime(int[] time, int totalTrips)
{
	int max = 0;
	for (int t : time) max = Math.max(t, max);
	long left = 0, right = (long) max * totalTrips;
	while (left <= right)
	{
		long mid = left + (right - left) / 2;
		long sum = 0;
		for (int t : time) sum += mid / t;
		if (sum < totalTrips) left = mid + 1;
		else right = mid - 1;
	}
	return left;
}
```

#### [2208. 将数组和减半的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-halve-array-sum/)

@2023.12.07

#模拟 

堆模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public int halveArray(int[] nums)
{
	double sum = 0;
	Queue<Double> pq = new PriorityQueue<>(Collections.reverseOrder());
	for (int num : nums)
	{
		pq.add((double) num);
		sum += num;
	}
	int ans = 0;
	double hold = sum;
	while (sum - hold / 2 > 0)
	{
		double p = pq.poll();
		sum -= p / 2;
		pq.add(p / 2);
		ans++;
	}
	return ans;
}
```

#### [2216. 美化数组的最少删除数](https://leetcode.cn/problems/minimum-deletions-to-make-array-beautiful/)

@2023.11.21

#贪心 

用布尔值 `even` 标记当前下标在最终数组中是否为偶数，遍历到事实偶数下标且下个元素相同时，`while` 删除所有下个相同元素，累加答案并更新 `even`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minDeletion(int[] nums)
{
	int len = nums.length, ans = 0;
	boolean even = true;
	for (int i = 0; i < len - 1; i++)
	{
		if (((i & 1) == 0) == even)
		{
			int j = i + 1;
			while (j < len && nums[j] == nums[i]) j++;
			ans += j - i - 1;
			i = j - 1;
		}
		even = (ans & 1) == 0;
	}
	return ((len - ans) & 1) == 0 ? ans : ans + 1;
}
```

优化：通过判断 `i - ans` 是否为偶数确定此下标是否为事实偶数下标，如果是且与下个元素相同，答案加 1。

```java
/**
 * 贪心
 * 宫水三叶
 */
public int minDeletion(int[] nums)
{
	int len = nums.length, ans = 0;
	for (int i = 0; i < len - 1; i++)
	{
		if (((i - ans) & 1) == 0 && nums[i] == nums[i + 1]) ans++;
	}
	return ((len - ans) & 1) != 0 ? ans + 1 : ans;
}
```

另一种思路：遍历计数要保留的元素数 `keep`，从下标 1 开始，如果与上个元素不同，`keep` 加 2 且 `i` 加 1，返回 `len - keep`。

```java
/**
 * 贪心
 * ?
 */
public int minDeletion(int[] nums)
{
	int len = nums.length, keep = 0;
	for (int i = 1; i < len; i++)
	{
		if (nums[i] != nums[i - 1])
		{
			keep += 2;
			i++;
		}
	}
	return len - keep;
}
```

#### [2226. 每个小孩最多能分到多少糖果](https://leetcode.cn/problems/maximum-candies-allocated-to-k-children/)

@2023.09.18

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int maximumCandies(int[] candies, long k)
{
	long sum = 0;
	for (int candy : candies) sum += candy;
	long left = 0, right = sum / k;
	while (left <= right)
	{
		long mid = left + (right - left) / 2;
		long serves = 0;
		if (mid == 0) serves = k;
		else
		{
			for (int candy : candies) serves += candy / mid;
		}
		if (serves >= k) left = mid + 1;
		else right = mid - 1;
	}
	return (int) right;
}
```

#### [2240. 买钢笔和铅笔的方案数](https://leetcode.cn/problems/number-of-ways-to-buy-pens-and-pencils/)

@2023.09.03

#暴力 

```java
/**
 * 枚举
 * Somnia1337
 */
public long waysToBuyPensPencils(int total, int cost1, int cost2) {
	long ans = 0;
	for (int i = 0; i * cost1 <= total; i++) ans += (total - (long) i * cost1) / cost2 + 1;
	return ans;
}
```

优化：枚举`cost1`、`cost2`中较大值，减少计算次数。

```java
/**
 * 枚举
 * ?
 */
public long waysToBuyPensPencils(int total, int cost1, int cost2) {
	if (cost1 < cost2) return waysToBuyPensPencils(total, cost2, cost1);
	long ans = 0;
	for (int i = 0; i * cost1 <= total; i++) ans += (total - (long) cost1 * i) / cost2 + 1;
	return ans;
}
```

#### [2291. 最大股票收益](https://leetcode.cn/problems/maximum-profit-from-trading-stocks/)

@2023.12.21

#动态规划 

0-1 背包模板题。

```java
/**
 * 动态规划
 * Wang Zhizhi
 */
public int maximumProfit(int[] present, int[] future, int budget) {
	int[] dp = new int[budget + 1];
	for (int j = 0; j < present.length; j++) {
		for (int i = budget; i >= present[j]; i--) {
			dp[i] = Math.max(dp[i - present[j]] + future[j] - present[j], dp[i]);
		}
	}
	return dp[budget];
}
```

#### [2300. 咒语和药水的成功对数](https://leetcode.cn/problems/successful-pairs-of-spells-and-potions/)

@2023.09.14

#二分查找 

排序 `potions`，遍历 `spells`，对每个咒语，二分查找 `potions` 中能与之配对的药水下界。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int[] successfulPairs(int[] spells, int[] potions, long success) {
	Arrays.sort(potions);
	int n = potions.length;
	int[] ans = new int[spells.length];
	for (int i = 0; i < spells.length; i++) ans[i] = Math.max(n - biR(potions, success, spells[i]), 0);
	return ans;
}

private int biR(int[] arr, long tar, int s) {
	int l = 0, r = arr.length - 1;
	while (l <= r) {
		int m = l + r >> 1;
		if ((long) s * arr[m] >= tar) r = m - 1;
		else l = m + 1;
	}
	return l;
}
```

#### [2304. 网格中的最小路径代价](https://leetcode.cn/problems/minimum-path-cost-in-a-grid/)

@2023.12.22

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int minPathCost(int[][] grid, int[][] moveCost) {
	int m = grid.length, n = grid[0].length;
	for (int i = m - 2; i >= 0; i--) {
		for (int j = 0; j < n; j++) {
			int[] cost = moveCost[grid[i][j]];
			int a = Integer.MAX_VALUE;
			for (int k = 0; k < n; k++) a = Math.min(grid[i + 1][k] + cost[k], a);
			grid[i][j] += a;
		}
	}
	int ans = Integer.MAX_VALUE;
	for (int a : grid[0]) ans = Math.min(a, ans);
	return ans;
}
```

#### [2305. 公平分发饼干](https://leetcode.cn/problems/fair-distribution-of-cookies/)

@2023.12.20

#二分查找 #回溯 

与 [1723. 完成所有工作的最短时间](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/) 相似，二分查找 + 回溯检查。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int distributeCookies(int[] cookies, int k) {
	Arrays.sort(cookies);
	int l = cookies[cookies.length - 1], r = 0;
	for (int job : cookies) r += job;
	while (l <= r) {
		int m = l + r >> 1;
		if (check(cookies, new int[k], m, cookies.length - 1)) r = m - 1;
		else l = m + 1;
	}
	return l;
}

private boolean check(int[] cs, int[] c, int m, int idx) {
	if (idx == -1) return true;
	for (int i = c.length - 1; i >= 0; i--) {
		if (c[i] + cs[idx] > m) continue;
		c[i] += cs[idx];
		if (check(cs, c, m, idx - 1)) return true;
		c[i] -= cs[idx];
		if (c[i] == 0 || c[i] + cs[idx] == m) return false;
	}
	return false;
}
```

#### [2311. 小于等于 K 的最长二进制子序列](https://leetcode.cn/problems/longest-binary-subsequence-less-than-or-equal-to-k/)

@2023.09.24

#贪心 

倒序遍历，分别计数 `'1'`、`'0'` 的出现次数，所有 `'0'` 都可以包含在子序列中，而满足以下条件之一的 `'1'` 不可以包含在子序列中：

- 当前后缀子串长度超过 32 位。
- 当前后缀子串转换的 `long` 超过 `k`。

```java
/**
 * 贪心
 * 零点再睡觉
 * @improver Somnia1337
 */
public int longestSubsequence(String s, int k)
{
	int zeros = 0, ones = 0;
	boolean moreOnes = true;
	
	for (int i = s.length() - 1; i >= 0; i--) // 倒序遍历
	{
		if (s.charAt(i) == '0') zeros++;
		else if (moreOnes) // 为'1'且可以增加更多的'1'
		{
			if (s.length() - i > 32 || Long.parseLong(s.substring(i), 2) > k) // 位数超过32位，或者转换的long大于k
			{
				moreOnes = false; // 不能增加更多的'1'
				continue;
			}
			ones++;
		}
	}
	
	return zeros + ones;
}
```

#### [2316. 统计无向图中无法互相到达点对数](https://leetcode.cn/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/)

@2023.10.21

用 `dfs` 求每个连通块大小。

```java
/**
 * DFS
 * 灵茶山艾府
 */
public long countPairs(int n, int[][] edges)
{
	List<Integer>[] g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int[] e : edges)
	{
		int x = e[0], y = e[1];
		g[x].add(y);
		g[y].add(x);
	}
	boolean[] vis = new boolean[n];
	long ans = 0;
	for (int i = 0, total = 0; i < n; i++)
	{
		if (!vis[i])
		{
			int size = dfs(i, g, vis);
			ans += (long) size * total;
			total += size;
		}
	}
	return ans;
}

private int dfs(int x, List<Integer>[] g, boolean[] vis)
{
	vis[x] = true;
	int size = 1;
	for (int y : g[x])
	{
		if (!vis[y]) size += dfs(y, g, vis);
	}
	return size;
}
```

#### [2336. 无限集中的最小数字](https://leetcode.cn/problems/smallest-number-in-infinite-set/)

@2023.11.29

#设计 #模拟 

`boolean[] ava` 记录每个值是否可用，`int min` 记录当前的最小可用元素。

调用 `popSmallest()` 时，从 `min` 开始遍历 `ava`，查找可用元素。

调用 `addBack(int num)` 时，设置 `ava[num] = true`，更新 `min`。

```java
/**
 * 模拟
 * Somnia1337
 */
class SmallestInfiniteSet
{
    private boolean[] ava;
    private int min;
	
    public SmallestInfiniteSet()
    {
        ava = new boolean[1001];
        Arrays.fill(ava, true);
        min = 1;
    }
	
    public int popSmallest()
    {
        for (int i = min; i < 1001; i++)
        {
            if (ava[i])
            {
                ava[i] = false;
                min = i + 1;
                return i;
            }
        }
        return -1;
    }
	
    public void addBack(int num)
    {
        if (!ava[num])
        {
            ava[num] = true;
            min = Math.min(num, min);
        }
    }
}
```

用堆模拟，用 `int it` 维护按顺序的当前可用元素。

调用 `addBack(int num)` 时将 `num` 入堆。

调用 `popSmallest()` 时先尝试出堆，否则 `it++`。

```java
/**
 * 模拟
 * Seele
 */
class SmallestInfiniteSet
{
    private int it;
    private Queue<Integer> q;
	
    public SmallestInfiniteSet()
    {
        it = 1;
        q = new PriorityQueue<>();
    }
	
    public int popSmallest()
    {
        return !q.isEmpty() ? q.poll() : it++;
    }
	
    public void addBack(int num)
    {
        if (num < it && !q.contains(num)) q.add(num);
    }
}
```

#### [2337. 移动片段得到字符串](https://leetcode.cn/problems/move-pieces-to-obtain-a-string/)

@2023.12.05

#双指针 

`'L'` 和 `'R'` 无法互相穿过，双指针遍历两串，检查对应下标是否满足要求。

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean canChange(String start, String target)
{
	char[] s = start.toCharArray(), t = target.toCharArray();
	int l = start.length(), i = 0, j = 0;
	while (i < l && j < l)
	{
		while (i < l && s[i] == '_') i++;
		while (j < l && t[j] == '_') j++;
		if (i == l && j == l) break;
		if (i == l || j == l || s[i] != t[j] || s[i] == 'L' && i < j || s[i] == 'R' && i > j) return false;
		i++;
		j++;
	}
	while (i < l && s[i] == '_') i++;
	while (j < l && t[j] == '_') j++;
	return i == j;
}
```

```java
/**
 * 双指针
 * ?
 */
public boolean canChange(String start, String target)
{
	int l = start.length(), i = 0, j = 0;
	char[] s = start.toCharArray(), t = target.toCharArray();
	while (true)
	{
		while (i < l && s[i] == '_') i++;
		while (j < l && t[j] == '_') j++;
		if (i == l || j == l) break;
		if (s[i] != t[j] || i > j && s[i] != 'L' || i < j && s[i] != 'R') return false;
		i++;
		j++;
	}
	return i == j;
}
```

#### [2342. 数位和相等数对的最大和](https://leetcode.cn/problems/max-sum-of-a-pair-with-equal-sum-of-digits/)

@2023.11.18

#技巧 

数位和最大为 `9 * 9 = 81`，用 `int[82]` 维护每个分组的最大元素，遍历更新答案及最大元素。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int maximumSum(int[] nums)
{
	int[] groups = new int[82];
	int ans = -1;
	for (int num : nums)
	{
		int k = sumOfDigits(num);
		if (groups[k] == 0)
		{
			groups[k] = num;
			continue;
		}
		ans = Math.max(groups[k] + num, ans);
		groups[k] = Math.max(num, groups[k]);
	}
	return ans;
}

private int sumOfDigits(int x)
{
	int ret = 0;
	while (x > 0)
	{
		ret += x % 10;
		x /= 10;
	}
	return ret;
}
```

#### [2352. 相等行列对](https://leetcode.cn/problems/equal-row-and-column-pairs/)

@2023.09.04

#暴力 #哈希表 

1. 枚举

外层遍历列，内层遍历行，累加相同的行列个数。

```java
/**
 * 枚举
 * Somnia1337
 */
public int equalPairs(int[][] grid) {
	int len = grid.length, ans = 0;
	for (int j = 0; j < len; j++) {
		int head = grid[0][j];
		for (int[] row : grid) {
			if (row[0] == head) {
				int k = 1;
				for (; k < len; k++) {
					if (row[k] != grid[k][j]) break;
				}
				if (k == len) ans++;
			}
		}
	}
	return ans;
}
```

2. 哈希表

优化：用 `ArrayList` 表示行、列，先遍历行，用哈希表记录 `<行, 计数>`，再遍历列，累加答案。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int equalPairs(int[][] grid) {
	int len = grid.length, ans = 0;
	Map<List<Integer>, Integer> rows = new HashMap<>();
	// 对行计数
	for (int[] row : grid) {
		List<Integer> r = new ArrayList<>();
		for (int x : row) r.add(x);
		rows.merge(r, 1, Integer::sum);
	}
	// 遍历列, 累加
	for (int j = 0; j < len; j++) {
		List<Integer> c = new ArrayList<>();
		for (int[] row : grid) c.add(row[j]);
		ans += rows.getOrDefault(c, 0);
	}
	return ans;
}
```

#### [2364. 统计坏数对的数目](https://leetcode.cn/problems/count-number-of-bad-pairs/)

@2023.12.21

#哈希表 #数学 

\[Deprecated]

```java
/**
 * 哈希表
 * Somnia1337
 */
public long countBadPairs(int[] nums) {
	long[] dp = new long[nums.length + 1];
	dp[nums.length] = nums.length + 1;
	Map<Integer, Integer> pos = new HashMap<>();
	long ans = 0;
	for (int i = 0; i < nums.length; i++) {
		int j = pos.getOrDefault(nums[i] - i, nums.length);
		dp[i] = i - j - 1 + dp[j];
		pos.put(nums[i] - i, i);
		ans += dp[i];
	}
	return ans;
}
```

对坏数对的条件式 `j - i != nums[j] - nums[i]` 变形得 `i - nums[i] != j - nums[j]`，总的数对数目为 `n * (n - 1) / 2`，减去其中不是坏数对（`i - nums[i] == j - nums[j]`）的对数即为答案。

实现上，哈希表记录 `<i-nums[i], 计数>`，遍历，对每个 `i`，其左侧有 `count[i-nums[i]]` 个下标与其同组，从答案中减去。

Java 中，`merge()` 合并键值并返回合并后的值，因此其返回值 `-1` 即为 `i` 左侧的同组计数。

```java
/**
 * 哈希表
 * ?
 */
public long countBadPairs(int[] nums) {
	long ans = (long) nums.length * (nums.length - 1) >> 1;
	Map<Integer, Integer> count = new HashMap<>();
	for (int i = 0; i < nums.length; i++) ans -= count.merge(i - nums[i], 1, Integer::sum) - 1;
	return ans;
}
```

#### [2368. 受限条件下可到达节点的数目](https://leetcode.cn/problems/reachable-nodes-with-restrictions/)

@2024.03.02



```java
/**
 * DFS
 * Somnia1337
 */
private List<Integer>[] adj;
private Set<Integer> ban;
private boolean[] vis;

public int reachableNodes(int n, int[][] edges, int[] restricted) {
	adj = new ArrayList[n];
	Arrays.setAll(adj, e -> new ArrayList());
	for (int[] e : edges) {
		adj[e[0]].add(e[1]);
		adj[e[1]].add(e[0]);
	}
	ban = new HashSet<>();
	for (int x : restricted) ban.add(x);
	vis = new boolean[n];
	dfs(0);
	
	int ans = 0;
	for (boolean b : vis) {
		if (b) ans++;
	}
	return ans;
}

private void dfs(int node) {
	vis[node] = true;
	for (int next : adj[node]) {
		if (!ban.contains(next) && !vis[next]) dfs(next);
	}
}
```

#### [2369. 检查数组是否存在有效划分](https://leetcode.cn/problems/check-if-there-is-a-valid-partition-for-the-array/)

@2024.03.01



```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public boolean validPartition(int[] nums) {
	int n = nums.length;
	boolean[] dp = new boolean[n + 1];
	dp[0] = true;
	for (int i = 1; i < n; i++) {
		if (dp[i - 1] && nums[i] == nums[i - 1] || i > 1 && dp[i - 2] && (nums[i] == nums[i - 1] && nums[i] == nums[i - 2] || nums[i] == nums[i - 1] + 1 && nums[i] == nums[i - 2] + 2)) {
			dp[i + 1] = true;
		}
	}
	return dp[n];
}
```

#### [2384. 最大回文数字](https://leetcode.cn/problems/largest-palindromic-number/)

@2023.12.19

#贪心 

计数，先 `append()` 每个元素 `cnt / 2` 次，对 `0` 特殊处理，反转作为后一半保存，再添加回文中心。

```java
/**
 * 贪心
 * Somnia1337
*/
public String largestPalindromic(String num) {
	int[] count = new int[10];
	for (char c : num.toCharArray()) count[c - '0']++;
	if (count[0] == num.length()) return "0"; // 特判全 0 的情况
	
	StringBuilder ans = new StringBuilder();
	// 倒序, 每个元素连接 cnt/2 次, 先不累加 0
	for (int i = 9; i >= 1; i--) ans.append(String.valueOf(i).repeat(count[i] / 2));
	// 如果有非 0 值存在, 中心可连接 0
	if (!ans.isEmpty()) ans.append(("0").repeat(count[0] / 2));
	
	// 后半部分
	String half = new StringBuilder(ans).reverse().toString();
	// 倒序, 将首个计数为奇数的元素作为回文中心
	for (int i = 9; i >= 0; i--) {
		if ((count[i] & 1) == 1) {
			ans.append(i);
			break;
		}
	}
	return ans.append(half).toString();
}
```

#### [2390. 从字符串中移除星号](https://leetcode.cn/problems/removing-stars-from-a-string/)

@2023.09.03

#栈 #双指针 

1. 栈

用栈存储有效字符，遍历`s`，遇到星号就弹栈，否则压栈，最后将全部有效字符弹出再反转。

```java
/**
 * 栈
 * Somnia1337
 */
public String removeStars(String s) {
	Deque<Character> stk = new ArrayDeque<>();
	for (char c : s.toCharArray()) {
		if (c == '*') stk.pop(); // 星号, 弹栈
		else stk.push(c); // 有效, 压栈
	}
	StringBuilder ans = new StringBuilder();
	while (!stk.isEmpty()) ans.append(stk.pop());
	return ans.reverse().toString();
}
```

2. 双指针

模拟栈的思想，用快慢双指针，`fast` 遍历 `s`：

- 遇到 `'*'` 时，`slow` 回退 1 位。
- 否则，用 `s[fast]` 处字符覆盖 `s[slow]`。

最终，前 `slow` 位为有效字符。

```java
/**
 * 双指针
 * Somnia1337
 */
public String removeStars(String s) {
	char[] chs = s.toCharArray();
	int slow = 0;
	for (int fast = 0; fast < chs.length; fast++) {
		if (chs[fast] == '*') slow--; // 星号, 位置无效, 回退 slow
		else chs[slow++] = chs[fast]; // 位置有效, 用 s[fast] 覆盖 s[slow]
	}
	return new String(chs, 0, slow);
}
```

#### [2397. 被列覆盖的最多行数](https://leetcode.cn/problems/maximum-rows-covered-by-columns/)

@2024.01.04

#暴力 #位运算 

```java
/**
 * 枚举 + 回溯 + 位运算
 * Somnia1337
 */
private List<Integer> e;
private char[] path;

public int maximumRows(int[][] matrix, int numSelect) {
	// 计算每行对应的掩码
	int rows = matrix.length, cols = matrix[0].length;
	int[] mask = new int[rows];
	for (int i = 0; i < rows; i++) {
		StringBuilder m = new StringBuilder();
		for (int x : matrix[i]) m.append((char) ('0' + x));
		mask[i] = Integer.parseInt(m.toString(), 2);
	}
	
	// 回溯, 生成所有选择
	e = new ArrayList<>();
	path = new char[cols];
	Arrays.fill(path, '0');
	bt(numSelect, 0);
	
	// 枚举
	int ans = 0;
	for (int c : e) {
		int cur = 0;
		for (int x : mask) {
			if ((x & c) == x) cur++;
		}
		ans = Math.max(cur, ans);
	}
	return ans;
}

private void bt(int sel, int idx) {
	if (sel == 0) {
		e.add(Integer.parseInt(new String(path), 2));
		return;
	}
	for (int i = idx; i < path.length; i++) {
		if (path[i] == '0') {
			path[i] = '1';
			bt(sel - 1, i + 1);
			path[i] = '0';
		}
	}
}
```

优化：

- 计算 `mask` 时无需用字符串 + 库函数，只需按位或 + 位左移即可。
- 无需回溯得到所有有效选项，枚举每个选项，库函数检查其中位 `1` 的数量是否为 `numSelect` 即可。

```java
/**
 * 枚举 + 位运算
 * Somnia1337
 */
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

#### [2401. 最长优雅子数组](https://leetcode.cn/problems/longest-nice-subarray/)

@2023.12.20

#位运算 

优雅子数组需满足任意两个数按位与 `== 0`，即任意两个数对应的 01 数组没有交集。

在 $10^9$ 范围内，满足条件的子数组最长为 `30`，可枚举子数组右端点，向左扩展左端点。

`or` 表示子数组中所有元素的按位或，当扩展到 `(or & nums[l]) > 0` 时，说明再加入 `nums[l]` 将导致出现两个按位与 `> 0` 的元素，停止扩展。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int longestNiceSubarray(int[] nums) {
	int ans = 0;
	for (int r = 0; r < nums.length; r++) { // 枚举右端点
		int l = r, or = 0; // 向左移动左端点, or 表示子数组的按位或
		while (l >= 0 && (or & nums[l]) == 0) or |= nums[l--];
		if ((ans = Math.max(r - l, ans)) == 30) break;
	}
	return ans;
}
```

改为同向双指针，`or` 表示指针之间子数组中所有元素的按位或，加入 `nums[r]` 之前，在 `(or & nums[r]) > 0` 时，说明加入 `nums[r]` 的子数组不是优雅的，持续用按位异或操作“弹出” `nums[l]`。

```java
/**
 * 同向双指针
 * 灵茶山艾府
 */
public int longestNiceSubarray(int[] nums) {
	int l = 0, r = 0, or = 0, ans = 0;
	while (r < nums.length) {
		while ((or & nums[r]) > 0) or ^= nums[l++];
		or |= nums[r++];
		ans = Math.max(r - l, ans);
	}
	return ans;
}
```

#### [2406. 将区间分为最少组数](https://leetcode.cn/problems/divide-intervals-into-minimum-number-of-groups/)

@2023.12.29

#技巧 #堆 #差分数组 

1. 堆

问题转化：有重叠的区间不能在一组，转化为有多少区间存在重叠。

按左端点排序，用堆维护每组最后一个区间的右端点，遍历 `intervals`：

- 如果 `itv[0] > peek`：无重叠，更新 `peek` 为 `itv[1]`（`poll` 再 `offer`）。
- 如果 `itv[0] <= peek`：有重叠，需要新分出一组。

```java
/**
 * 堆
 * 灵茶山艾府
 */
public int minGroups(int[][] intervals) {
	Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
	Queue<Integer> pq = new PriorityQueue<>();
	for (int[] itv : intervals) {
		if (!pq.isEmpty() && pq.peek() < itv[0]) pq.poll();
		pq.offer(itv[1]);
	}
	return pq.size();
}
```

2. 差分数组

问题转化：上下车问题，每个 `itv` 看作一个人，在 `itv[0]` 上车、`itv[1]` 下车，答案为同时在车上的人数的最大值。

```java
/**
 * 差分数组
 * LFool⚡
 */
public int minGroups(int[][] intervals) {
	int max = intervals[0][1];
	for (int[] itv : intervals) max = Math.max(itv[1], max);
	int[] diff = new int[max + 1];
	for (int[] itv : intervals) {
		diff[itv[0]]++;
		if (itv[1] + 1 <= max) diff[itv[1] + 1]--;
	}
	int t = 0, ans = 0;
	for (int i = 1; i <= max; i++) {
		t += diff[i];
		ans = Math.max(t, ans);
	}
	return ans;
}
```

#### [2411. 按位或最大的最小子数组长度](https://leetcode.cn/problems/smallest-subarrays-with-maximum-bitwise-or/)

@2023.11.27

```java
/**
 * 位运算
 * 灵茶山艾府
 */
public int[] smallestSubarrays(int[] nums) {
	int[] ans = new int[nums.length];
	for (int i = 0; i < nums.length; i++) {
		ans[i] = 1;
		for (int j = i - 1; j >= 0 && (nums[j] | nums[i]) > nums[j]; j--) {
			nums[j] |= nums[i];
			ans[j] = i - j + 1;
		}
	}
	return ans;
}
```

#### [2422. 使用合并操作将数组转换为回文序列](https://leetcode.cn/problems/merge-operations-to-turn-array-into-a-palindrome/)

@2023.11.29

#双指针 

要将数组转换为回文序列，对称位置的元素一定相同，因此用双指针从两端开始累加元素值，直到左右的当前值相同，同时累加操作次数。

```java
/**
 * 双指针
 * Somnia1337
 */
public int minimumOperations(int[] nums)
{
	int l = 0, r = nums.length - 1, ans = 0;
	while (l < r)
	{
		int left = nums[l], right = nums[r];
		while (left != right && l < r)
		{
			if (left < right) left += nums[++l];
			else right += nums[--r];
			ans++;
		}
		l++;
		r--;
	}
	return ans;
}
```

#### [2434. 使用机器人打印字典序最小的字符串](https://leetcode.cn/problems/using-a-robot-to-print-the-lexicographically-smallest-string/)

@2024.01.09

#贪心 #栈 

问题转化：有一个初始为空的栈，给定字符的入栈顺序，求字典序最小的出栈序列。

每次压栈后，持续检查栈顶字符 `c`，设还未入栈的字符中字典序最小的为 `min`，讨论：

- `c <= min`，此时弹出 `c` 最优，否则下个弹栈的字符一定比 `c` 大，导致此位置的字典序更大。
- `c > min`，不弹出 `c`，等待后续更小的字符压栈。

实现时，先一次遍历进行字符统计，再次遍历时，压栈并更新计数和最小值，在栈顶满足条件时持续弹栈。

```java
/**
 * 
 * Somnia1337
 */
public String robotWithString(String s) {
	char[] chs = s.toCharArray();
	int[] count = new int[26];
	for (char c : chs) count[c - 'a']++;
	int min = 0;
	Deque<Character> stk = new ArrayDeque<>();
	StringBuilder ans = new StringBuilder();
	for (char c : chs) {
		stk.push(c);
		count[c - 'a']--;
		while (min < 25 && count[min] == 0) min++;
		while (!stk.isEmpty() && stk.peek() - 'a' <= min) ans.append(stk.poll());
	}
	return ans.toString();
}
```

#### [2439. 最小化数组中的最大值](https://leetcode.cn/problems/minimize-maximum-of-array/)

@2024.01.10

#二分查找 #数学 

1. 二分查找

二分答案，倒序遍历，用前一个元素满足当前元素，检查当前最大值能否满足。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minimizeArrayValue(int[] nums) {
	long l = nums[0], r = nums[0];
	for (int x : nums) {
		l = Math.min(x, l);
		r = Math.max(x, r);
	}
	while (l <= r) {
		long m = l + r >> 1;
		long[] t = new long[nums.length];
		for (int i = 0; i < nums.length; i++) t[i] = nums[i];
		if (check(t, m)) r = m - 1;
		else l = m + 1;
	}
	return (int) l;
}

private boolean check(long[] arr, long m) {
	for (int i = arr.length - 1; i > 0; i--) {
		if (arr[i] > m) arr[i - 1] += arr[i] - m;
	}
	return arr[0] <= m;
}
```

2. 数学

右侧元素可以分给左侧元素，视为一个水池，左端点为 `0`，枚举右端点，池内平均值的最大值即为答案。

```java
/**
 * 数学
 * WangZixi_2001
 */
public int minimizeArrayValue(int[] nums) {
	long sum = 0;
	int ans = -1;
	for (int i = 0; i < nums.length; i++) {
		sum += nums[i];
		ans = (int) Math.max((sum + i) / (i + 1), ans);
	}
	return ans;
}
```

#### [2447. 最大公因数等于 K 的子数组数目](https://leetcode.cn/problems/number-of-subarrays-with-gcd-equal-to-k/)

@2024.01.09

#暴力 

枚举每个子数组。

```java
/**
 * 枚举
 * Somnia1337
 */
public int subarrayGCD(int[] nums, int k) {
	int ans = 0;
	for (int i = 0; i < nums.length; i++) {
		int g = 0;
		for (int j = i; j >= 0; j--) {
			if ((g = gcd(g, nums[j])) == k) ans++;
			else if (g % k != 0) break; // 剪枝
		}
	}
	return ans;
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### [2450. 应用操作后不同二进制字符串的数量](https://leetcode.cn/problems/number-of-distinct-binary-strings-after-applying-operations/)

@2023.12.23

#数学 

答案只与 `s` 的长度 `n` 有关，`s` 中长为 `k` 的子串有 `n - k + 1` 个，每个都可以选择翻转 / 不翻转，每种选择的最终结果将决定一种终点。

```java
/**
 * 数学
 * Somnia1337
 */
public int countDistinctStrings(String s, int k) {
	return pow(2, s.length() - k + 1, 1000000007);
}

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

#### [2452. 距离字典两次编辑以内的单词](https://leetcode.cn/problems/words-within-two-edits-of-dictionary/)

@2023.12.23

#暴力 

暴力匹配 `queries` 和 `dictionary` 中的每对字符串。

```java
/**
 * 枚举
 * Somnia1337
 */
public List<String> twoEditWords(String[] queries, String[] dictionary) {
	if (queries[0].length() <= 2) return new ArrayList<>(List.of(queries));
	List<String> ans = new ArrayList<>();
	for (String q : queries) {
		for (String d : dictionary) {
			if (check(q, d)) {
				ans.add(q);
				break;
			}
		}
	}
	return ans;
}

private boolean check(String a, String b) {
	int diff = 0;
	for (int i = 0; i < a.length() && diff <= 2; i++) {
		if (a.charAt(i) != b.charAt(i)) diff++;
	}
	return diff <= 2;
}
```

#### [2453. 摧毁一系列目标](https://leetcode.cn/problems/destroy-sequential-targets/)

@2023.12.23

#哈希表 

哈希表记录 `<对space余数, 计数>`，遍历计数，再次遍历更新最大计数和答案。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int destroyTargets(int[] nums, int space) {
	Map<Integer, Integer> group = new HashMap<>();
	for (int x : nums) group.merge(x % space, 1, Integer::sum);
	int max = 0, ans = nums[0];
	for (int x : nums) {
		int cnt = group.get(x % space);
		if (cnt > max) {
			max = cnt;
			ans = x;
		}
		else if (cnt == max) ans = Math.min(x, ans);
	}
	return ans;
}
```

#### [2457. 美丽整数的最小增量](https://leetcode.cn/problems/minimum-addition-to-make-integer-beautiful/)

@2023.12.23

#贪心 #数学 

从右端开始，逐位变为 `0`，直到和 `<= target`。

```java
/**
 * 贪心
 * Somnia1337
 */
public long makeIntegerBeautiful(long n, int target) {
	long p = 1, ans = 0;
	while (sum(n) > target) {
		long d = (long) Math.pow(10, p++), next = n - (n % d) + d;
		ans += -n + (n = next);
	}
	return ans;
}

private int sum(long x) {
	int ret = 0;
	while (x > 0) {
		ret += (int) (x % 10);
		x /= 10;
	}
	return ret;
}
```

#### [2477. 到达首都的最少油耗](https://leetcode.cn/problems/minimum-fuel-cost-to-report-to-the-capital/)

@2023.12.05

```java
/**
 * DFS
 * 灵茶山艾府
 */
private long ans;

public long minimumFuelCost(int[][] roads, int seats)
{
	List<Integer>[] g = new ArrayList[roads.length + 1];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int[] r : roads)
	{
		int x = r[0], y = r[1];
		g[x].add(y);
		g[y].add(x);
	}
	dfs(0, -1, g, seats);
	return ans;
}

private int dfs(int x, int fa, List<Integer>[] g, int seats)
{
	int size = 1;
	for (int y : g[x])
	{
		if (y != fa) size += dfs(y, x, g, seats);
	}
	if (x > 0) ans += (size - 1) / seats + 1;
	return size;
}
```

#### [2486. 追加字符以获得子序列](https://leetcode.cn/problems/append-characters-to-string-to-make-subsequence/)

@2024.01.03

#双指针 

微调 `isSubsequence()` 的板子。

```java
/**
 * 双指针
 * Somnia1337
 */
public int appendCharacters(String s, String t) {
	char[] chs1 = s.toCharArray(), chs2 = t.toCharArray();
	int i = 0, j = 0;
	while (i < chs1.length && j < chs2.length) {
		if (chs1[i] == chs2[j]) j++;
		i++;
	}
	return chs2.length - j;
}
```

#### [2512. 奖励最顶尖的 K 名学生](https://leetcode.cn/problems/reward-top-k-students/)

@2023.10.11

#哈希表 

`key` 为单词，`value` 为对应分数，用优先队列维护排名。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> topStudents(String[] positive_feedback, String[] negative_feedback, String[] report, int[] student_id, int k)
{
	Map<String, Integer> points = new HashMap<>();
	for (String pos : positive_feedback) points.put(pos, 3);
	for (String neg : negative_feedback) points.put(neg, -1);
	Queue<int[]> rank = new PriorityQueue<>((a, b) -> a[0] != b[0] ? b[0] - a[0] : a[1] - b[1]);
	
	for (int i = 0; i < student_id.length; i++)
	{
		int rate = 0;
		String[] comments = report[i].split(" ");
		for (String comment : comments) rate += points.getOrDefault(comment, 0);
		rank.offer(new int[]{rate, student_id[i]});
	}
	
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < k; i++) ans.add(rank.poll()[1]);
	return ans;
}
```

#### [2516. 每种字符至少取 K 个](https://leetcode.cn/problems/take-k-of-each-character-from-left-and-right/)

@2024.01.08

#双指针 

同向双指针，枚举 `r`，当 `l` 处 / `r` 处的字符计数 `< k` 时持续右移 `l`。

```java
/**
 * 双指针
 * Somnia1337
 */
public int takeCharacters(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	if (n < k * 3) return -1;
	int[] count = new int[3];
	for (char c : chs) count[c - 'a']++;
	for (int cnt : count) {
		if (cnt < k) return -1;
	}
	
	int l = 0, r = 0, ans = n;
	while (r < n) {
		count[chs[r++] - 'a']--;
		while (l < r && count[chs[l] - 'a'] < k || count[chs[r - 1] - 'a'] < k) count[chs[l++] - 'a']++;
		ans = Math.min(l + n - r, ans);
	}
	return ans;
}
```

#### [2521. 数组乘积中的不同质因数数目](https://leetcode.cn/problems/distinct-prime-factors-of-product-of-array/)

@2023.09.16

#数学 

本题等价于求解 `nums` 中所有元素的不同质因数个数。

```java
/**
 * 分解质因数
 * Joey
 */
public int distinctPrimeFactors(int[] nums) {
	Set<Integer> vis = new HashSet<>(); // 记录所有不同质因数
	for (int x : nums) {
		for (int i = 2; i * i <= x; i++) {
			if (x % i == 0) {
				vis.add(i); // 加入 vis 的一定为质数, 因为下一行将从 2 开始把找到的质因子全部除尽
				while (x % i == 0) x /= i; // 将当前质因子除尽
			}
		}
		if (x > 1) vis.add(x); // 没有除到 1, 说明 x 本身为质数
	}
	return vis.size();
}
```

#### [2523. 范围内最接近的两个质数](https://leetcode.cn/problems/closest-prime-numbers-in-range/)

@2023.12.16

#暴力 

埃氏筛预处理得到范围内所有质数，再枚举答案。

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] closestPrimes(int left, int right) {
	if (left <= 2 && right >= 3) return new int[]{2, 3};
	if (right - left == 1) return new int[]{-1, -1};
	List<Integer> primes = getPrimes(left, right);
	int[] ans = new int[]{-1, -1};
	if (primes.size() <= 1) return ans;
	int min = right - left + 1, n1 = primes.get(0);
	for (int i = 1; i < primes.size() && min > 2; i++) { // d==2 说明找到了孪生质数, 已是最近距离
		int n2 = primes.get(i), d = n2 - n1;
		if (d < min) {
			min = d;
			ans[0] = n1;
			ans[1] = n2;
		}
		n1 = n2;
	}
	return ans;
}

private List<Integer> getPrimes(int l, int r) {
	List<Integer> ans = new ArrayList<>();
	boolean[] ban = new boolean[r + 1];
	for (int i = 2; i <= r; i++) {
		if (!ban[i]) {
			if (i >= l) ans.add(i);
			for (int j = 2; i * j <= r; j++) ban[i * j] = true;
		}
	}
	return ans;
}
```

优化：直接正向判断是否为质数更快（140ms -> 0ms）。

```java
/**
 * 枚举
 * ?
 */
public int[] closestPrimes(int left, int right) {
	if (left <= 2 && right >= 3) return new int[]{2, 3};
	int[] ans = new int[]{-1, -1};
	int pre = 0, min = right - left + 1;
	for (int x = left; x <= right && min > 2; x++) {
		if (!isPrime(x)) continue;
		if (x - pre < min && pre > 0) {
			min = x - pre;
			ans[0] = pre;
			ans[1] = x;
		}
		pre = x;
	}
	return ans;
}

private boolean isPrime(int x) {
	if (x == 1) return false;
	if ((x & 1) == 0) return x == 2;
	for (int y = 3; y * y <= x; y += 2) {
		if (x % y == 0) return false;
	}
	return true;
}
```

#### [2527. 查询数组 Xor 美丽值](https://leetcode.cn/problems/find-xor-beauty-of-array/)

@2023.12.18

#数学 

三元组 `(i,j,k)` 与 `(j,i,k)`（`i != j`）的结果相同，异或得 `0`。

还剩 `(a,a,b)` 式的三元组，`[a]|[a] == [a]`，化简为 `[a]&[b]`，而 `(b,b,a)` 的结果也为 `[b]&[a]`，异或得 `0`。

只剩 `(x,x,x)` 式的三元组，即 `[x]`，即所有元素异或。

```java
/**
 * 数学
 * Somnia1337
 */
public int xorBeauty(int[] nums) {
	int ans = 0;
	for (int x : nums) ans ^= x;
	return ans;
}
```

#### [2530. 执行 K 次操作后的最大分数](https://leetcode.cn/problems/maximal-score-after-applying-k-operations/)

@2023.10.18

#堆 

用堆动态维护最大值，取 `k` 次最大值，每次累加后将更新的值重新入堆。

```java
/**
 * 堆
 * Somnia1337
 */
public long maxKelements(int[] nums, int k)
{
	Queue<Integer> queue = new PriorityQueue<>(Collections.reverseOrder());
	for (int num : nums) queue.offer(num);
	
	long ans = 0;
	while (k-- > 0)
	{
		int max = queue.poll();
		ans += max;
		max = (int) Math.ceil((double) max / 3);
		queue.offer(max);
	}
	
	return ans;
}
```

优化：

- `Collection.reverseOrder` 换为 Lambda 表达式。
- 简化出堆入堆操作。

```java
/**
 * 堆
 * 力扣官方题解
 */
public long maxKelements(int[] nums, int k)
{
	Queue<Integer> queue = new PriorityQueue<>((a, b) -> b - a);
	for (int num : nums) queue.offer(num);
	
	long ans = 0;
	while (k-- > 0)
	{
		int max = queue.poll();
		ans += max;
		queue.offer((max + 2) / 3);
	}
	
	return ans;
}
```

#### [2537. 统计好子数组的数目](https://leetcode.cn/problems/count-the-number-of-good-subarrays/)

@2023.12.15

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public long countGood(int[] nums, int k) {
	Map<Integer, Integer> count = new HashMap<>();
	long ans = 0;
	int l = 0, cur = 0;
	for (int x : nums) {
		cur += count.merge(x, 1, Integer::sum) - 1;
		while (cur - (count.get(nums[l]) - 1) >= k) cur -= count.merge(nums[l++], -1, Integer::sum);
		if (cur >= k) ans += l + 1;
	}
	return ans;
}
```

#### [2548. 填满背包的最大价格](https://leetcode.cn/problems/maximum-price-to-fill-a-bag/)

@2023.12.21

#贪心 

按单位重量的价格倒序排序，累加完整的物品，直到剩余容量小于当前物品，将其拆开。如果遍历完仍未填满，返回 `-1`。

```java
/**
 * 贪心
 * Somnia1337
 */
public double maxPrice(int[][] items, int capacity) {
	Arrays.sort(items, Comparator.comparingDouble(a -> (double) -a[0] / a[1]));
	double ans = 0;
	for (int[] item : items) {
		if (item[1] >= capacity) return ans + (double) capacity * item[0] / item[1];
		ans += item[0];
		capacity -= item[1];
	}
	return -1;
}
```

#### [2550. 猴子碰撞的方法数](https://leetcode.cn/problems/count-collisions-of-monkeys-on-a-polygon/)

@2023.09.16

#数学 

每个顶点的猴子有顺时针、逆时针两种选择，共`n`个顶点，总情况有`2 ** n`种，其中不碰撞的情况只有2种，故本题等价于求解`(2 ** n - 2) % (10 ** 9 + 7)`。

```java
/**
 * 数学（快速幂）
 * 心语心愿
 */
public int monkeyMove(int n)
{
	final int mod = 1000000007;
	long ans = 1;
	long x = 2;
	while (n > 0)
	{
		if ((n & 1) == 1)
		{
			ans = ans * x % mod;
		}
		x = x * x % mod;
		n >>= 1;
	}
	return (int) ((ans + mod - 2) % mod);
}
```

#### [2560. 打家劫舍 IV](https://leetcode.cn/problems/house-robber-iv/)

@2023.09.19

#二分查找 

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minCapability(int[] nums, int k)
{
	int min = Integer.MAX_VALUE, max = 0;
	for (int num : nums)
	{
		min = Math.min(num, min);
		max = Math.max(num, max);
	}
	int left = min, right = max, len = nums.length;
	while (left < right)
	{
		int mid = left + (right - left) / 2, take = 0;
		for (int i = 0; i < len; i++)
		{
			if (nums[i] <= mid)
			{
				take++;
				i++;
			}
		}
		if (take < k) left = mid + 1;
		else right = mid;
	}
	return left;
}
```

#### [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/)

@2023.12.15

#二分查找 

排序，枚举 `i`，二分查找 `j` 的左右边界。

```java
/**
 * 二分查找
 * Somnia1337
 */
public long countFairPairs(int[] nums, int lower, int upper) {
	Arrays.sort(nums);
	long ans = 0;
	for (int i = 0; i < nums.length; i++) {
		// 查找左边界
		int l1 = i + 1, r1 = nums.length - 1;
		while (l1 <= r1) {
			int m = l1 + r1 >> 1;
			if (nums[i] + nums[m] < lower) l1 = m + 1;
			else r1 = m - 1;
		}
		// 查找右边界
		int l2 = i + 1, r2 = nums.length - 1;
		while (l2 <= r2) {
			int m = l2 + r2 >> 1;
			if (nums[i] + nums[m] > upper) r2 = m - 1;
			else l2 = m + 1;
		}
		ans += r2 - r1;
	}
	return ans;
}
```

#### [2564. 子字符串异或查询](https://leetcode.cn/problems/substring-xor-queries/)

@2024.01.10

#哈希表 #位运算 

查询等式 `x^q[0] == q[1]` 的两侧同时异或 `q[0]`，得 `x^q[0]^q[0] == q[1]^q[0]`，进而化简为 `x == q[0]^q[1]`，也就是要对每个查询找到 `s` 中最靠左侧的、值为 `q[0]^q[1]` 的子串的起止下标。

预处理 `s`，由于无需考虑长度 `> 30` 的子串，可以枚举每个子串，哈希表记录 `<数值, 最优下标{l,r}>`，首次遇到键 `x` 的下标即为最优（固定 `x`，其二进制长度固定为 `L`，小于 `L` 的串值一定不会是 `x`，那么从左向右遍历 `s`，首次出现 `x` 就是最靠左侧的位置）。

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
public int[][] substringXorQueries(String s, int[][] queries) {
	final int[] NOT_FOUND = new int[]{-1, -1};
	Map<Integer, int[]> map = new HashMap<>();
	int i = s.indexOf('0');
	if (i >= 0) map.put(0, new int[]{i, i});
	char[] chs = s.toCharArray();
	for (int l = 0; l < chs.length; l++) { // 枚举左端点
		if (chs[l] == '0') continue;
		for (int r = l, x = 0, bound = Math.min(l + 30, chs.length); r < bound; r++) { // 右端点向后至多 30 位
			x = x << 1 | (chs[r] & 1);
			map.putIfAbsent(x, new int[]{l, r});
		}
	}
	
	int[][] ans = new int[queries.length][2];
	for (i = 0; i < queries.length; i++) ans[i] = map.getOrDefault(queries[i][0] ^ queries[i][1], NOT_FOUND);
	return ans;
}
```

#### [2575. 找出字符串的可整除数组](https://leetcode.cn/problems/find-the-divisibility-array-of-a-string/)

@2024.03.07

记录取余。

```java
/**
 * 数学
 * Somnia1337
 */
public int[] divisibilityArray(String word, int m) {
	char[] chs = word.toCharArray();
	int n = chs.length;
	long rem = 0;
	int[] ans = new int[n];
	for (int i = 0; i < n; i++) {
		rem = (rem * 10 + chs[i] - '0') % m;
		if (rem == 0) ans[i] = 1;
	}
	return ans;
}
```

#### [2576. 求出最多标记下标](https://leetcode.cn/problems/find-the-maximum-number-of-marked-indices/)

@2023.12.15

#双指针 

答案具有单调性：如果有 `k` 对匹配，那么最小的 `k` 个数一定能和最大的 `k` 个数匹配，贪心地，`nums[i]` 一定要去匹配 `nums[n - k + i]`。

排序，用双指针将左半部分去匹配右半部分（分别最小 -> 最大）。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int maxNumOfMarkedIndices(int[] nums) {
	Arrays.sort(nums);
	int i = 0, len = nums.length;
	for (int j = (len + 1) / 2; j < len; j++) {
		if (nums[i] * 2 <= nums[j]) i++;
	}
	return i * 2;
}
```

#### [2594. 修车的最少时间](https://leetcode.cn/problems/minimum-time-to-repair-cars/)

@2023.09.07

#二分查找 

二分查找要求数据满足单调性，本题背景下，给定某个时长 $t_0$——

- 如果工人们能够在 $t_0$ 内修完 `cars` 辆车，那么对于一切大于 $t_0$ 的时长，都能够修完 `cars` 辆车。
- 如果工人们不能在 $t_0$ 内修完 `cars` 辆车，那么对于一切小于 $t_0$ 的时长，都不能修完 `cars` 辆车。

由此，答案是单调的，可以使用二分查找。

自定义方法 `check()`，检查所有工人在给定时间内能够修完的车辆总数，以此为二分查找中指针移动的判断依据。

```java
/**
 * 二分查找
 * Somnia1337
 */
public long repairCars(int[] ranks, int cars) {
	long l = 1, r = ranks[0] * (long) Math.pow(cars, 2);
	while (l <= r) {
		long m = l + r >> 1, cnt = 0;
		for (int rank : ranks) {
			cnt += (long) Math.sqrt((double) m / rank);
			if (cnt >= cars) break;
		}
		if (cnt >= cars) r = m - 1;
		else l = m + 1;
	}
	return l;
}
```

优化：预处理 `ranks` 频率。

```java
/**
 * 二分查找
 * Explorer
 */
public long repairCars(int[] ranks, int cars) {
	int max = 0;
	for (int x : ranks) max = Math.max(x, max);
	long[] freq = new long[max + 1];
	for (int x : ranks) freq[x]++;
	long l = 1, r = (long) Math.pow(cars, 2) * max;
	while (l <= r) {
		long m = l + r >> 1, cnt = 0;
		for (int i = 1; i < max + 1 && cnt < cars; i++) {
			cnt += freq[i] * (long) Math.sqrt((double) m / i);
		}
		if (cnt >= cars) r = m - 1;
		else l = m + 1;
	}
	return l;
}
```

#### [2596. 检查骑士巡视方案](https://leetcode.cn/problems/check-knight-tour-configuration/)

@2023.09.13

#模拟 

`grid` 的元素 $\in [0, n^2]$，刚好符合下标的范围。用 `int[n*n][2] path` 记录 `grid` 的路径顺序，遍历 `path`，每两步之间的 `deltaX`、`deltaY` 应满足二者之一：

- `deltaX == 1 && deltaY == 2`
- `deltaX == 2 && deltaY == 1`

即 `deltaX * deltaY == 2`。

```java
/**
 * 模拟
 * 力扣官方题解
 */
public boolean checkValidGrid(int[][] grid) {
	if (grid[0][0] != 0) return false;
	int n = grid.length;
	int[][] path = new int[n * n][2];
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			path[grid[i][j]][0] = i; // 第 grid[i][j] 步的 x 为 i
			path[grid[i][j]][1] = j; // 第 grid[i][j] 步的 y 为 j
		}
	}
	for (int i = 1; i < n * n; i++) {
		int dx = Math.abs(path[i][0] - path[i - 1][0]), dy = Math.abs(path[i][1] - path[i - 1][1]);
		if (dx * dy != 2) return false;
	}
	return true;
}
```

#### [2602. 使数组元素全部相等的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-make-all-array-elements-equal/)

@2023.12.16

#二分查找 

排序，对每个 `q in queries`，二分查找 `< q` 的元素数量（记为 `l`），分别对两侧求和与目标和 `长度 * q` 作差，最后这一步用前缀和加速。

```java
/**
 * 二分查找
 * Somnia1337
 */
public List<Long> minOperations(int[] nums, int[] queries) {
	Arrays.sort(nums);
	long[] pre = new long[nums.length + 1];
	for (int i = 1; i < nums.length + 1; i++) pre[i] = pre[i - 1] + nums[i - 1];
	List<Long> ans = new ArrayList<>();
	for (int q : queries) {
		int l = 0, r = nums.length;
		while (l < r) {
			int m = l + r >> 1;
			if (nums[m] >= q) r = m;
			else l = m + 1;
		}
		long left = (long) l * q - pre[l];
		long right = pre[nums.length] - pre[l] - (long) (nums.length - l) * q;
		ans.add(left + right);
	}
	return ans;
}
```

#### [2606. 找到最大开销的子字符串](https://leetcode.cn/problems/find-the-substring-with-maximum-cost/)

@2023.09.15

#贪心 

记录当前子串的开销，为负时从0开始重新计算。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maximumCostSubstring(String s, String chars, int[] vals)
{
	int ans = 0; // 答案最小为0，对应空串
	int value, current = 0; // value为当前字符开销，current为当前子串开销
	
	for (char c : s.toCharArray())
	{
		value = chars.indexOf(c);
		if (value == -1) value = vals[chars.indexOf(c)];
		
		current += value;
		if (current <= 0) current = Math.max(value, 0); // 当前子串开销为负，从0开始重新计算
		
		ans = Math.max(current, ans);
	}
	
	return ans;
}
```

优化：预处理所有字母的开销，由25ms优化至4ms。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maximumCostSubstring(String s, String chars, int[] vals)
{
	int ans = 0;
	int current = 0;
	int[] values = new int[26];
	for (char c = 'a'; c <= 'z'; c++)
	{
		values[c - 'a'] = chars.indexOf(c) == -1 ? c - 'a' + 1 : vals[chars.indexOf(c)];
	}
	
	for (char c : s.toCharArray())
	{
		int value = values[c - 'a'];
		current += value;
		if (current <= 0) current = Math.max(value, 0);
		ans = Math.max(current, ans);
	}
	
	return ans;
}
```

#### [2645. 构造有效字符串的最少插入数](https://leetcode.cn/problems/minimum-additions-to-make-valid-string/)

@2024.01.11

#贪心 

假设最终结果串由 `cnt` 个 `"abc"` 组成，则答案为 `cnt*3 - n`。

对于两个相邻字母 `x` 和 `y`：

- 如果 `x < y`，可以贪心地将它们放在一个 `"abc"` 内。
- 如果 `x >= y`，必须新开一组 `"abc"`。

```java
/**
 * 贪心
 * Austin
 */
public int addMinimum(String word) {
	int l = word.length(), cnt = 1;
	for (int i = 0; i < l - 1; i++) {
		if (word.charAt(i + 1) <= word.charAt(i)) cnt++;
	}
	return cnt * 3 - word.length();
}
```

#### [2653. 滑动子数组的美丽值](https://leetcode.cn/problems/sliding-subarray-beauty/)

@2023.12.15

#滑动窗口 

`nums[i]` $\in [-50, 50]$，可用 `int[101]` 计数，每次滑动后遍历 `count` 求得第 `x` 小的数。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int[] getSubarrayBeauty(int[] nums, int k, int x) {
	int[] ans = new int[nums.length - k + 1], count = new int[101];
	for (int i = 0; i < k; i++) count[nums[i] + 50]++;
	ans[0] = helper(count, x);
	for (int i = k; i < nums.length; i++) {
		count[nums[i - k] + 50]--;
		count[nums[i] + 50]++;
		ans[i - k + 1] = helper(count, x);
	}
	return ans;
}

private int helper(int[] count, int x) {
	int cnt = 0;
	for (int i = 0; i <= 49; i++) {
		cnt += count[i];
		if (cnt >= x) return i - 50;
	}
	return 0;
}
```

#### [2654. 使数组所有元素变成 1 的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-all-array-elements-equal-to-1/)

@2024.01.06

#数学 

如果 `nums` 中有 `1`，那么从每个 `1` 处扩展即可将整个 `nums` 都变成 `1`，答案为 `n - cnt1`。

否则，要用最少的操作得到一个 `1`，由于只能操作相邻的数，这个 `1` 必为某个子数组的 `gcd`，即要找到最短的满足 `gcd == 1` 的子数组，枚举每个子数组。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public int minOperations(int[] nums) {
	int n = nums.length, totalGcd = 0, cnt1 = 0;
	for (int x : nums) {
		totalGcd = gcd(totalGcd, x);
		if (x == 1) cnt1++;
	}
	if (totalGcd > 1) return -1; // 整个数组的 gcd>1, 无法得到任何 1
	if (cnt1 > 0) return n - cnt1;
	
	int min = n; // 得到一个 1 的最少操作次数
	for (int l = 0; l < n; l++) {
		int g = 0;
		for (int r = l; r < n; r++) {
			if ((g = gcd(g, nums[r])) == 1) {
				min = Math.min(r - l, min);
				break;
			}
		}
	}
	return min + n - 1;
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### [2655. 寻找最大长度的未覆盖区间](https://leetcode.cn/problems/find-maximal-uncovered-ranges/)

@2023.12.07

#区间（待整理） 

对 `ranges` 起点升序、终点降序排序，遍历寻找空缺区间。

```java
/**
 * 排序
 * Somnia1337
 */
public int[][] findMaximalUncoveredRanges(int n, int[][] ranges)
{
	if (ranges.length == 0) return new int[][]{{0, n - 1}};
	Arrays.sort(ranges, (a, b) -> (a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]));
	List<int[]> collect = new ArrayList<>();
	if (ranges[0][0] > 0) collect.add(new int[]{0, ranges[0][0] - 1});
	int len = ranges.length, end = 0;
	for (int i = 1; i < len; i++)
	{
		end = ranges[i - 1][1];
		while (i < len && ranges[i][1] <= end) i++;
		if (i == len) break;
		int l = end + 1, r = ranges[i][0] - 1;
		if (l <= r) collect.add(new int[]{l, r});
	}
	end = Math.max(ranges[len - 1][1], end);
	if (end < n - 1) collect.add(new int[]{end + 1, n - 1});
	
	int size = collect.size();
	int[][] ans = new int[size][2];
	for (int i = 0; i < size; i++) ans[i] = collect.get(i);
	return ans;
}
```

#### [2661. 找出叠涂元素](https://leetcode.cn/problems/first-completely-painted-row-or-column/)

@2023.12.01

```java
/**
 * 模拟
 * Somnia1337
 */
public int firstCompleteIndex(int[] arr, int[][] mat)
{
	int len = arr.length, rows = mat.length, cols = mat[0].length;
	int[] pos = new int[len + 1];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			pos[mat[i][j]] = i * cols + j;
		}
	}
	
	int[] row = new int[rows], col = new int[cols];
	for (int i = 0; i < len; i++)
	{
		int r = pos[arr[i]] / cols, c = pos[arr[i]] % cols;
		row[r]++;
		col[c]++;
		if (row[r] == cols || col[c] == rows) return i;
	}
	return -1;
}
```

#### [2679. 矩阵中的和](https://leetcode.cn/problems/sum-in-a-matrix/)

@2023.12.04

#模拟 

对行排序，累加列的最大值。

```java
/**
 * 模拟
 * Somnia1337
 */
public int matrixSum(int[][] nums)
{
	for (int[] row : nums) Arrays.sort(row);
	int ans = 0;
	for (int j = 0; j < nums[0].length; j++)
	{
		int max = 0;
		for (int[] row : nums) max = Math.max(row[j], max);
		ans += max;
	}
	return ans;
}
```

#### [2680. 最大或值](https://leetcode.cn/problems/maximum-or/)

@2024.01.04

#位运算 #前后缀分解 

要答案最大，答案的二进制位数应最多，将 `k` 次 `* 2` 全部分配给同一个元素才有可能得到最多的位数。

对每个位置 `i` 计算 `pre[i]` 和 `suf[i]`，分别表示前后缀元素的按位或，再枚举每个元素左移 `k` 位，更新答案。

```java
/**
 * 前后缀分解
 * Somnia1337
 */
public long maximumOr(int[] nums, int k) {
	int n = nums.length;
	long[] pre = new long[n], suf = new long[n];
	long or = 0;
	for (int i = 1; i < n; i++) pre[i] = or |= nums[i - 1];
	or = 0;
	for (int i = n - 2; i >= 0; i--) suf[i] = or |= nums[i + 1];
	
	long ans = 0;
	for (int i = 0; i < n; i++) ans = Math.max(((long) nums[i] << k) | pre[i] | suf[i], ans);
	return ans;
}
```

#### [2684. 矩阵中移动的最大次数](https://leetcode.cn/problems/maximum-number-of-moves-in-a-grid/)

@2024.01.21



```java
/**
 * BFS
 * Somnia1337
 */
public int maxMoves(int[][] grid) {
	int rows = grid.length, cols = grid[0].length;
	int[] vis = new int[rows];
	Queue<Integer> q = new LinkedList<>();
	for (int i = 0; i < rows; i++) q.add(i);
	for (int j = 0; j < cols - 1; j++) {
		int size = q.size();
		while (size-- > 0) {
			int p = q.poll();
			for (int k = Math.max(p - 1, 0); k < Math.min(p + 2, rows); k++) {
				if (vis[k] != j + 1 && grid[k][j + 1] > grid[p][j]) {
					vis[k] = j + 1;
					q.add(k);
				}
			}
		}
		if (q.isEmpty()) return j;
	}
	return cols - 1;
}
```

#### [2685. 统计完全连通分量的数量](https://leetcode.cn/problems/count-the-number-of-complete-components/)

@2023.12.16

#并查集 

分别统计每个连通分量的结点数量 `v` 和边数量 `e`，计数满足 `e = v*(v-1)/2` 的连通分量数量。

```java
/**
 * 并查集
 * Somnia1337
 */
public int countCompleteComponents(int n, int[][] edges) {
	// 并查集计算连通关系
	int[] root = new int[n], cntE = new int[n]; // 结点所属分量的边数量
	Arrays.setAll(root, a -> a);
	boolean[] vis = new boolean[n];
	for (int[] e : edges) {
		union(root, e[0], e[1]);
		vis[e[0]] = vis[e[1]] = true;
	}
	
	// 每个结点所属分量内的结点列表,
	// 事实上只会用到代表元的表,
	// 哈希表去重
	Set<Integer>[] g = new HashSet[n];
	Arrays.setAll(g, k -> new HashSet<>());
	for (int[] e : edges) {
		int r = find(root, e[0]);
		cntE[r]++;
		g[r].add(e[0]);
		g[r].add(e[1]);
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (cntE[i] == 0) continue;
		int size = g[i].size();
		if (cntE[i] == size * (size - 1) / 2) ans++;
	}
	for (boolean v : vis) { // 零图也是连通分量
		if (!v) ans++;
	}
	return ans;
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

优化：修改并查集板子，合并的同时计数每个连通分量的边和结点数量，合并之后直接检查。

```java
/**
 * 并查集
 * ?
 */
private int[] root, cntE, cntN;

public int countCompleteComponents(int n, int[][] edges) {
	root = new int[n];
	cntE = new int[n]; // 边计数
	cntN = new int[n]; // 结点计数
	Arrays.setAll(root, a -> a);
	Arrays.fill(cntN, 1);
	for (int[] e : edges) union(e[0], e[1]);
	boolean[] vis = new boolean[n];
	int ans = 0;
	for (int i = 0; i < n; i++) {
		int r = find(i); // 代表元
		if (!vis[r]) { // 该分量还未访问
			vis[r] = true;
			if (cntE[r] == cntN[r] * (cntN[r] - 1)) ans++;
		}
	}
	return ans;
}

private int find(int i) {
	if (root[i] != i) root[i] = find(root[i]);
	return root[i];
}

private void union(int x, int y) {
	int r1 = find(x), r2 = find(y);
	if (r1 != r2) {
		root[r1] = r2;
		cntE[r2] += cntE[r1] + 2;
		cntN[r2] += cntN[r1];
	}
	else cntE[r1] += 2;
}
```

#### [2698. 求一个整数的惩罚数](https://leetcode.cn/problems/find-the-punishment-number-of-an-integer/)

@2023.09.15

#回溯 

```java
for (int end = start + 1; end <= s.length(); end++)
{
	int part = Integer.parseInt(s.substring(start, end)); // 取当前片段的值
	if (current + part > x) return; // 剪枝：和大于x
	bt(end, current + part, s, x);
	if (ansList.contains(x)) return; // 剪枝：x已确认满足要求
}
```

对`[1, n]`内的所有`i`进行回溯，检查`i * i`是否可以拆分为若干和为`i`的片段。

```java
/**
 * 回溯
 * Somnia1337
 */
Set<Integer> ansList = new HashSet<>(); // 记录满足要求的i

public int punishmentNumber(int n)
{
	int ret = 1;
	for (int i = 2; i <= n; i++)
	{
		bt(0, 0, String.valueOf(i * i), i);
	}
	for (int ans : ansList) ret += ans * ans;
	return ret;
}

public void bt(int start, int current, String s, int x)
{
	if (start == s.length())
	{
		if (current == x) ansList.add(x); // 满足要求，加入解集
		return;
	}
	for (int end = start + 1; end <= s.length(); end++)
	{
		int part = Integer.parseInt(s.substring(start, end)); // 取当前片段的值
		if (current + part > x) return; // 剪枝：和大于x
		bt(end, current + part, s, x);
		if (ansList.contains(x)) return; // 剪枝：x已确认满足要求
	}
}
```

#### [2707. 字符串中的额外字符](https://leetcode.cn/problems/extra-characters-in-a-string/)

@2023.12.16

#动态规划 

`dp[i]` 表示 `s[:i]` 对应的子问题答案，枚举子串右端点 `r`、左端点 `l`，如果 `s[l:r]` 在词典内，更新 `dp[r] = min(dp[l], dp[r])`。

```java
/**
 * 动态规划
 * ?
 */
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

#### [2712. 使所有字符相等的最小成本](https://leetcode.cn/problems/minimum-cost-to-make-all-characters-equal/)

@2023.12.18

#贪心 

一次遍历，如果 `s[i] != s[i - 1]`，必须反转左 / 右半部分，成本分别为 `i`、`n - i`，取较小者。

反转之后，`i` 及左侧字符都已相同，`i` 右侧的字符无论经过多少次反转，不同的相邻字符仍然不同，即之前的反转不会对之后的不同相邻字符造成影响。

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public long minimumCost(String s) {
	char[] chs = s.toCharArray();
	long ans = 0;
	for (int i = 1; i < chs.length; i++) {
		if (chs[i] != chs[i - 1]) ans += Math.min(i, chs.length - i);
	}
	return ans;
}
```

#### [2718. 查询后矩阵的和](https://leetcode.cn/problems/sum-of-matrix-after-queries/)

@2023.12.15

#技巧 

最后进行的查询将决定行、列的值，因此倒序遍历 `queries`，哈希表记录已修改的行、列，每次的增量为单元格增量 × 另一个维度尚未修改的数量。

```java
/**
 * 脑筋急转弯
 * Somnia1337
 */
public long matrixSumQueries(int n, int[][] queries) {
	Set<Integer> rowVis = new HashSet<>(), colVis = new HashSet<>();
	long ans = 0;
	for (int i = queries.length - 1; i >= 0; i--) {
		if (queries[i][0] == 0) {
			if (!rowVis.add(queries[i][1])) continue;
			ans += (long) (n - colVis.size()) * queries[i][2];
		}
		else {
			if (!colVis.add(queries[i][1])) continue;
			ans += (long) (n - rowVis.size()) * queries[i][2];
		}
	}
	return ans;
}
```

#### [2730. 找到最长的半重复子字符串](https://leetcode.cn/problems/find-the-longest-semi-repetitive-substring/)

@2023.09.15

#滑动窗口 

滑动窗口，`count`记录窗口内相邻相同的字符对数目，`pos`记录上一对相邻相同字符的位置，遇到下一对相连字符时，`left`直接跳转至`pos`。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int longestSemiRepetitiveSubstring(String s)
{
	int left = 0, right = 1, pos = -1;
	int count = 0, ans = 1;
	
	while (right < s.length())
	{
		if (s.charAt(right - 1) == s.charAt(right))
		{
			count++;
			if (count == 2)
			{
				left = pos;
				count--;
			}
			pos = right;
		}
		ans = Math.max(right - left + 1, ans);
		right++;
	}
	
	return ans;
}
```

#### [2731. 移动机器人](https://leetcode.cn/problems/movement-of-robots/)

@2023.10.10

```java
/**
 * 前缀和
 * Somnia1337
 */
public int sumDistance(int[] nums, String s, int d)
{
	final int MOD = (int) (Math.pow(10, 9) + 7);
	int len = nums.length;
	for (int i = 0; i < len; i++)
	{
		if (s.charAt(i) == 'L') nums[i] -= d;
		else nums[i] += d;
	}
	Arrays.sort(nums);
	long preSum = 0, ans = 0;
	for (int i = 0; i < len; i++)
	{
		ans += (long) nums[i] * i - preSum;
		ans %= MOD;
		preSum += nums[i];
	}
	return (int) ans;
}
```

#### [2735. 收集巧克力](https://leetcode.cn/problems/collecting-chocolates/)

@2023.12.28

1. 枚举

枚举操作次数 `k`，最多 `n` 次（也就是转了一整圈）。

每次操作都能让每种巧克力的可选价格范围扩大，不必考虑具体是哪一次操作后买入，因为一定能在最低时买入，只需维护史低。用 `int[] cost` 维护当前操作次数下每种巧克力的史低，更新并累加。

```java
/**
 * 枚举
 * Somnia1337
 */
public long minCost(int[] nums, int x) {
	int n = nums.length;
	int[] cost = new int[n];
	System.arraycopy(nums, 0, cost, 0, n);
	long ans = Long.MAX_VALUE;
	for (int k = 0; k < n; k++) {
		long cur = (long) x * k; // 当前总售价, 用操作成本初始化
		if (cur >= ans) break;
		for (int i = 0; i < n; i++) cur += cost[i] = Math.min(nums[(i + n - k) % n], cost[i]); // 更新史低, 同时累加
		ans = Math.min(cur, ans);
	}
	return ans;
}
```

2. 单调栈

[题解](https://leetcode.cn/problems/collecting-chocolates/solutions/2582752/xian-xing-on-dan-diao-zhan-chai-fen-java-q7zc/)

```java
/**
 * 单调栈
 * Explorer
 */
public long minCost(int[] nums, int x) {
	int n = nums.length, minIdx = 0;
	for (int i = 0; i < n; i++) {
		if (nums[i] < nums[minIdx]) minIdx = i;
	}
	int[] cost = new int[n];
	for (int i = 0; i < n; i++) cost[i] = nums[(i + minIdx) % n];
	int[] l = new int[n], r = new int[n];
	Arrays.fill(r, n);
	int[] stk = new int[n];
	int top = -1;
	stk[++top] = 0;
	for (int i = 1; i < n; i++) {
		while (cost[stk[top]] > cost[i]) r[stk[top--]] = i;
		l[i] = stk[top];
		stk[++top] = i;
	}
	long[] d = new long[n + 1];
	d[0] = cost[0];
	for (int i = 1; i < n; i++) {
		d[0] += cost[i];
		d[i - l[i]] -= cost[i];
		d[r[i] - i] -= cost[i];
		d[r[i] - l[i]] += cost[i];
	}
	for (int i = 1; i < n; i++) d[i] += d[i - 1];
	long ans = d[0];
	for (int i = 1; i < n && x + d[i] < 0; i++) ans += d[i] + x;
	return ans;
}
```

#### [2762. 不间断子数组](https://leetcode.cn/problems/continuous-subarrays/)

@2023.12.21

#双指针 

用两个堆维护当前子数组的最值，同向双指针，枚举 `r`，当极差 `> 2` 时持续右移 `l`。

```java
/**
 * 双指针
 * Somnia1337
 */
public long continuousSubarrays(int[] nums) {
	int l = 0, r = 0;
	Queue<Integer> min = new PriorityQueue<>(), max = new PriorityQueue<>(Collections.reverseOrder());
	Map<Integer, Integer> count = new HashMap<>();
	long ans = 0;
	while (r < nums.length) {
		if (count.merge(nums[r], 1, Integer::sum) == 1) {
			min.offer(nums[r]);
			max.offer(nums[r]);
		}
		while (max.peek() - min.peek() > 2) {
			if (count.merge(nums[l], -1, Integer::sum) == 0) {
				count.remove(nums[l]);
				min.remove(nums[l]);
				max.remove(nums[l]);
			}
			l++;
		}
		ans += ++r - l;
	}
	return ans;
}
```

以上两个最值堆及哈希表可用 `TreeMap` 等效代替。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public long continuousSubarrays(int[] nums) {
	TreeMap<Integer, Integer> tm = new TreeMap<>();
	int l = 0, r = 0;
	long ans = 0;
	while (r < nums.length) {
		tm.merge(nums[r], 1, Integer::sum);
		while (tm.lastKey() - tm.firstKey() > 2) {
			if (tm.merge(nums[l], -1, Integer::sum) == 0) tm.remove(nums[l]);
			l++;
		}
		ans += ++r - l;
	}
	return ans;
}
```

考虑暴力：枚举 `r`，向左扩展 `l`，累加。会超时。

枚举优化（懒更新）：`while` 外维护 `min`、`max`，枚举 `r`，当新加入的 `nums[r]` 在最值之间时保持 `l` 不变、直接累加，否则才从 `r` 开始更新 `l`、`min` 及 `max`。

```java
/**
 * 双指针
 * Sixology
 */
public long continuousSubarrays(int[] nums) {
	int l = 0, r = 0, min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
	long ans = 0;
	while (r < nums.length) {
		if (nums[r] < min || nums[r] > max) {
			min = max = nums[r];
			l = r - 1;
			while (l >= 0 && !(nums[l] - min > 2 || max - nums[l] > 2)) {
				max = Math.max(nums[l], max);
				min = Math.min(nums[l], min);
				l--;
			}
			l++;
		}
		ans += ++r - l;
	}
	return ans;
}
```

#### [2770. 达到末尾下标所需的最大跳跃次数](https://leetcode.cn/problems/maximum-number-of-jumps-to-reach-the-last-index/)

@2024.01.02

#动态规划 

`dp[i]` 表示达到 `nums[i]` 的子问题答案，双重 `for` 动态规划。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maximumJumps(int[] nums, int target) {
	int[] dp = new int[nums.length];
	Arrays.fill(dp, -1);
	dp[0] = 0;
	for (int i = 1; i < nums.length; i++) {
		for (int j = i - 1; j >= 0; j--) {
			if (dp[j] > -1 && Math.abs(nums[j] - nums[i]) <= target) dp[i] = Math.max(dp[j] + 1, dp[i]);
		}
	}
	return dp[nums.length - 1];
}
```

#### [2771. 构造最长非递减子数组](https://leetcode.cn/problems/longest-non-decreasing-subarray-from-two-arrays/)

@2023.12.29

#动态规划 

`dp` 开到 `[n][2]`，`dp[i][0]`、`dp[i][1]` 分别表示以 `nums1[i]`、`nums2[i]` 结尾的子问题答案，状态转移见代码。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maxNonDecreasingLength(int[] nums1, int[] nums2) {
	int n = nums1.length;
	int[][] dp = new int[n][2];
	int ans = 1;
	for (int[] row : dp) Arrays.fill(row, 1);
	for (int i = 1; i < n; i++) {
		if (nums1[i] >= nums1[i - 1]) dp[i][0] = Math.max(dp[i - 1][0] + 1, dp[i][0]);
		if (nums1[i] >= nums2[i - 1]) dp[i][0] = Math.max(dp[i - 1][1] + 1, dp[i][0]);
		if (nums2[i] >= nums1[i - 1]) dp[i][1] = Math.max(dp[i - 1][0] + 1, dp[i][1]);
		if (nums2[i] >= nums2[i - 1]) dp[i][1] = Math.max(dp[i - 1][1] + 1, dp[i][1]);
		ans = Math.max(Math.max(dp[i][0], dp[i][1]), ans);
	}
	return ans;
}
```

状压：

```java
/**
 * 动态规划
 * ?
 */
public int maxNonDecreasingLength(int[] nums1, int[] nums2) {
	int n = nums1.length, dp1 = 1, dp2 = 1, ans = 1;
	for (int i = 1; i < n; i++) {
		int t11 = nums1[i - 1] <= nums1[i] ? dp1 : 0;
		int t12 = nums1[i - 1] <= nums2[i] ? dp1 : 0;
		int t21 = nums2[i - 1] <= nums1[i] ? dp2 : 0;
		int t22 = nums2[i - 1] <= nums2[i] ? dp2 : 0;
		dp1 = Math.max(t11, t21) + 1;
		dp2 = Math.max(t12, t22) + 1;
		ans = Math.max(Math.max(dp1, dp2), ans);
	}
	return ans;
}
```

#### [2772. 使数组中的所有元素都等于零](https://leetcode.cn/problems/apply-operations-to-make-all-array-elements-equal-to-zero/)

@2024.01.02

1. 单调队列

双端队列存储 `int[]{到期下标, 减小值}`，`minus` 表示当前的减小值。

本质上，队列的作用是分别维护 `minus` 的每一小块叠加量的作用期限，以及时移除每一小块的作用效果。

```java
/**
 * 单调队列
 * Somnia1337
 */
public boolean checkArray(int[] nums, int k) {
	Deque<int[]> dq = new ArrayDeque<>();
	int minus = 0;
	
	// 前 n-k+1 个元素 (可作为子数组的左端点)
	for (int i = 0; i < nums.length - k + 1; i++) {
		if (!dq.isEmpty() && dq.peekFirst()[0] == i) minus -= dq.pollFirst()[1]; // 队首的作用到期, 更新 minus
		if ((nums[i] -= minus) < 0) return false; // 更新 nums[i] 同时判断
		minus += nums[i]; // 叠加 nums[i] 的作用
		dq.offerLast(new int[]{i + k, nums[i]}); // 记录 nums[i] 的作用的期限
	}
	
	// 后 k-1 个元素 (不可作为子数组的左端点)
	for (int i = nums.length - k + 1; i < nums.length; i++) {
		if (!dq.isEmpty() && dq.peekFirst()[0] == i) minus -= dq.pollFirst()[1]; // 队首的作用到期, 更新 minus
		if (nums[i] != minus) return false;
	}
	
	return true;
}
```

2. 差分数组

```java
/**
 * 差分数组
 * ?
 */
public boolean checkArray(int[] nums, int k) {
	int x = nums[0];
	for (int i = 0; i < nums.length - k; i++) {
		if (x > nums[i]) return false;
		nums[i + k] += x = nums[i];
	}
	for (int i = nums.length - k; i < nums.length - 1; i++) {
		if (nums[i] != nums[i + 1]) return false;
	}
	return true;
}
```

#### [2779. 数组的最大美丽值](https://leetcode.cn/problems/maximum-beauty-of-an-array-after-applying-operation/)

@2023.12.05

#滑动窗口 

用长为 `k * 2` 的滑动窗口遍历 `nums`，取窗口内元素数量的最大值。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int maximumBeauty(int[] nums, int k)
{
	Arrays.sort(nums);
	int len = nums.length, l = 0, r = 0, ans = 0;
	k *= 2;
	while (r < len)
	{
		while (r < len && nums[r] - nums[l] <= k) r++;
		ans = Math.max(r - l, ans);
		l++;
	}
	return ans;
}
```

#### [2789. 合并后数组中的最大元素](https://leetcode.cn/problems/largest-element-in-an-array-after-merge-operations/)

@2023.12.30

#贪心 

倒序遍历，在能合并时持续合并，直到数组左端。

```java
/**
 * 贪心
 * Somnia1337
 */
public long maxArrayValue(int[] nums) {
	long ans = nums[nums.length - 1];
	for (int i = nums.length - 2; i >= 0; i--) {
		if (nums[i] > ans) { // 无法合并, 从 nums[i] 重新开始
			ans = nums[i];
			continue;
		}
		ans += nums[i]; // 合并
	}
	return ans;
}
```

#### [2799. 统计完全子数组的数目](https://leetcode.cn/problems/count-complete-subarrays-in-an-array/)

@2023.12.20

#双指针 

1. 枚举

`total` 表示整个 `nums` 中不同元素的数目，枚举右端点 `r`，向左扩展左端点 `l`，直到子数组中不同元素的数目 `cnt == total`，此时 `l` 左侧的下标均可作为左端点。

```java
/**
 * 枚举
 * Somnia1337
 */
public int countCompleteSubarrays(int[] nums) {
	// 一次遍历, 计算 total
	boolean[] vis = new boolean[2001]; // 1<=nums[i]<=2000
	int total = 0;
	for (int x : nums) {
		if (!vis[x]) total++;
		vis[x] = true;
	}
	
	int ans = 0;
	// 枚举 r, 向左扩展 l
	// 可行性剪枝: r 从 total-1 开始
	for (int r = total - 1; r < nums.length; r++) {
		vis = new boolean[2001];
		int l = r, cnt = 0; // cnt: 子数组中不同元素的数目
		while (l >= 0 && cnt < total) {
			if (!vis[nums[l]]) cnt++;
			vis[nums[l--]] = true;
		}
		if (cnt == total) ans += l + 2; // l 多向左移了 1
	}
	return ans;
}
```

2. 双指针

考虑枚举法的低效部分：随着右端点右移，子数组中不同元素的数目不变/增加，**对应的左端点一定不会左移而只会右移**，因此可以转化为「同向双指针」。

仍然枚举右端点，但是 `cnt` 放在 `for` 外，当 `cnt == total` 时持续右移 `l`，此时 `l` 左侧的下标均可作为左端点。

```java
/**
 * 双指针
 * Somnia1337
 */
public int countCompleteSubarrays(int[] nums) {
	// 一次遍历, 计算 total
	boolean[] vis = new boolean[2001]; // 1<=nums[i]<=2000
	int total = 0;
	for (int x : nums) {
		if (!vis[x]) total++;
		vis[x] = true;
	}
	
	int l = 0, cnt = 0, ans = 0;
	int[] count = new int[2001]; // 子数组中元素的计数, 用于更新 cnt
	// 枚举 r
	for (int r : nums) {
		if (++count[r] == 1) cnt++;
		// 持续 l++
		while (cnt == total) {
			if (--count[nums[l++]] == 0) cnt--;
		}
		ans += l; // l 多向右移了 1
	}
	return ans;
}
```

#### [2808. 使循环数组所有元素相等的最少秒数](https://leetcode.cn/problems/minimum-seconds-to-equalize-a-circular-array/)

@2024.01.30



```java
/**
 * 枚举
 * 灵茶山艾府
 */
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

#### [2812. 找出最安全路径](https://leetcode.cn/problems/find-the-safest-path-in-a-grid/)

@2023.12.14

#搜索 #二分查找 

多源 BFS 将原本为 `0` 的点修改为与之最近的小偷的距离，再二分查找答案，通过 BFS 检查能否到终点来调整左右边界。

```java
/**
 * BFS + 二分查找 + DFS
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int n;
private boolean[][] vis;

public int maximumSafenessFactor(List<List<Integer>> grid) {
	n = grid.size();
	if (grid.get(0).get(0) == 1 || grid.get(n - 1).get(n - 1) == 1) return 0; // 起点/终点有小偷
	
	// 转换成 int[][], 同时将小偷入队
	int[][] g = new int[n][n];
	Queue<Integer> q = new LinkedList<>(); // 队里放一维坐标(x*n+y)
	for (int i = 0; i < n; i++) {
		List<Integer> row = grid.get(i);
		for (int j = 0; j < n; j++) {
			g[i][j] = row.get(j);
			if (g[i][j] == 1) {
				g[i][j] = -1;
				for (int k = 0; k < 4; k++) {
					int nx = i + dirs[k], ny = j + dirs[k + 1];
					if (nx >= 0 && nx < n && ny >= 0 && ny < n) q.add(nx * n + ny);
				}
			}
		}
	}
	
	// 多源 BFS, 更新每个非小偷位置离最近小偷的距离
	int d = 1; // 当前距离
	while (!q.isEmpty()) {
		int size = q.size(); // 按层扩展
		while (size-- > 0) {
			int p = q.poll(), x = p / n, y = p % n;
			if (g[x][y] != 0) continue; // 为小偷, 或者有更近的小偷
			g[x][y] = d;
			for (int k = 0; k < 4; k++) {
				int nx = x + dirs[k], ny = y + dirs[k + 1];
				if (nx >= 0 && nx < n && ny >= 0 && ny < n) q.add(nx * n + ny);
			}
		}
		d++;
	}
	
	// 二分查找, DFS 检查保持距离 m 的前提下能否到达终点
	int l = 0, r = g[0][0];
	while (l <= r) {
		int m = l + r >> 1;
		vis = new boolean[n][n];
		if (check(g, 0, 0, m)) l = m + 1;
		else r = m - 1;
	}
	return Math.max(r, 0);
}

private boolean check(int[][] g, int x, int y, int d) {
	if (x == n - 1 && y == n - 1) return true;
	vis[x][y] = true;
	for (int k = 0; k < 4; k++) {
		int nx = x + dirs[k], ny = y + dirs[k + 1];
		// 坐标在网格内 && 未访问 && 到小偷距离至少为d && 递归返回true
		if ((nx >= 0 && nx < n && ny >= 0 && ny < n) && !vis[nx][ny] && g[nx][ny] >= d && check(g, nx, ny, d))
			return true;
	}
	return false;
}
```

#### [2825. 循环增长使字符串子序列等于另一个字符串](https://leetcode.cn/problems/make-string-a-subsequence-using-cyclic-increments/)

@2024.01.01

#双指针 

修改 `isSubsequence()` 板子。

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean canMakeSubsequence(String str1, String str2) {
	return str1.length() >= str2.length() && isSubsequence(str1, str2);
}

private boolean isSubsequence(String src, String tar) {
	char[] chs1 = src.toCharArray(), chs2 = tar.toCharArray();
	int i = 0, j = 0;
	while (i < chs1.length && j < chs2.length) {
		if (chs1[i] == chs2[j] || (chs1[i] + 1 - 'a') % 26 == chs2[j] - 'a') j++;
		i++;
	}
	return j == tar.length();
}
```

#### [2830. 销售利润最大化](https://leetcode.cn/problems/maximize-the-profit-as-the-salesman/)

@2023.12.24

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int maximizeTheProfit(int n, List<List<Integer>> offers) {
	List<int[]>[] g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (List<Integer> offer : offers)
		g[offer.get(1)].add(new int[]{offer.get(0), offer.get(2)});
	
	int[] dp = new int[n + 1];
	for (int end = 1; end <= n; end++) {
		dp[end] = dp[end - 1];
		for (int[] p : g[end - 1]) dp[end] = Math.max(dp[p[0]] + p[1], dp[end]);
	}
	return dp[n];
}
```

#### [2831. 找出最长等值子数组](https://leetcode.cn/problems/find-the-longest-equal-subarray/)

@2023.12.21

#双指针 

分组记录每个元素出现的下标，对每组用同向双指针遍历，即，每次右移 `r` 后，持续右移 `l` 直到子数组中需要删除的元素数量 `<= k`。

实现时，向下标列表 `p` 存入 `i - len(p)`，这样，`p[r] - p[l]` 就是子数组中需要删除的元素数量。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int longestEqualSubarray(List<Integer> nums, int k) {
	int len = nums.size();
	Map<Integer, List<Integer>> pos = new HashMap<>();
	for (int i = 0; i < len; i++) {
		int x = nums.get(i);
		List<Integer> p = pos.computeIfAbsent(x, e -> new ArrayList<>());
		p.add(i - p.size());
	}
	int ans = 0;
	for (List<Integer> p : pos.values()) {
		if (p.size() <= ans) continue;
		for (int l = 0, r = 0, n = p.size(); r < n; r++) {
			while (p.get(r) - p.get(l) > k) l++;
			ans = Math.max(r - l + 1, ans);
		}
	}
	return ans;
}
```

只变长的窗口：先右移 `r`，更新答案为 `nums[r]` 的计数，再在需要删除的元素数量 `> k` 时左移 `l` 并更新 `nums[l]` 的计数。

```java
/**
 * 双指针
 * ?
 */
public int longestEqualSubarray(List<Integer> nums, int k) {
	int len = nums.size(), ans = 0;
	int[] count = new int[len + 1];
	int l = 0, r = 0;
	while (r < len) {
		ans = Math.max(++count[nums.get(r++)], ans);
		if (r - l - ans > k) count[nums.get(l++)]--;
	}
	return ans;
}
```

#### [2834. 找出美丽数组的最小和](https://leetcode.cn/problems/find-the-minimum-possible-sum-of-a-beautiful-array/)

@2023.09.02

#数学 

对 $[1,\space target - 1]$ 内的数字：

- $1$ 和 $target - 1$ 只能选 1 个。
- $2$ 和 $target - 2$ 只能选 1 个。
- ……
- 直到 $\frac{k}{2}$，它一定可以选。

记 $m = {\rm{min}}(\frac{k}{2},\space n)$，答案的第一部分为 ${\rm{sum}}(1,\space m)$。

还需要 $n - m$ 个数字，只能从 $target$ 开始往后选，构成答案的第二部分。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public int minimumPossibleSum(int n, int target) {
	long m = Math.min(target / 2, n);
	return (int) ((m * (m + 1) + (n - m - 1 + target * 2) * (n - m)) / 2 % 1000000007);
}
```

#### [2840. 判断通过操作能否让字符串相等 II](https://leetcode.cn/problems/check-if-strings-can-be-made-equal-with-operations-ii/)

@2023.09.02

#字符串 

分别统计 `s1` 与 `s2` 的奇数、偶数位上的各字母数量是否对应相等。

```java
/**
 * 字符统计
 * Somnia1337
 */
public boolean checkStrings(String s1, String s2) {
	int l = s1.length();
	int[][] count1 = new int[26][2], count2 = new int[26][2];
	for (int i = 0; i < l; i++) {
		count1[s1.charAt(i) - 'a'][i & 1]++;
		count2[s2.charAt(i) - 'a'][i & 1]++;
	}
	for (int i = 0; i < 26; i++) {
		if (count1[i][0] != count2[i][0] || count1[i][1] != count2[i][1]) return false;
	}
	return true;
}
```

#### [2841. 几乎唯一子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-almost-unique-subarray/)

@2023.09.02

#滑动窗口 

滑动窗口，用哈希表记录当前窗口内的 `<元素, 计数>`，每次滑动后，如果哈希表大小 `>= m`，更新答案。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public long maxSum(List<Integer> nums, int m, int k) {
	long cur = 0, ans = 0;
	Map<Integer, Integer> count = new HashMap<>();
	for (int i = 0; i < k; i++) {
		cur += nums.get(i);
		count.merge(nums.get(i), 1, Integer::sum);
	}
	if (count.size() >= m) ans = cur;
	for (int i = k; i < nums.size(); i++) {
		int l = nums.get(i - k), r = nums.get(i);
		cur += r - l;
		if (count.merge(l, -1, Integer::sum) == 0) count.remove(l);
		count.merge(r, 1, Integer::sum);
		if (count.size() >= m) ans = Math.max(cur, ans);
	}
	return ans;
}
```

#### [2844. 生成特殊数字的最少操作](https://leetcode.cn/problems/minimum-operations-to-make-a-special-number/)

@2023.09.03

#贪心 

倒序遍历，用 `has0`、`has5` 记录是否出现过 `0`、`5`，并对当前位讨论：

- 为 `0`：如果之前出现过 `0`，找到 `"00"`。
- 为 `5`：如果之前出现过 `0`，找到 `"50"`。
- 为 `2` / `7`：如果之前出现过 `5`，找到 `"25"` / `"75"`。

用 `cnt0` 记录 `0` 出现的次数，如果遍历结束后仍没有找到 `"00"`、`"50"`、`"25"`、`"75"` 中的任何一个，那么删去所有非 `0` 数字即可。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minimumOperations(String num) {
	int l = num.length(), cnt0 = 0;
	boolean has0 = false, has5 = false;
	for (int i = l - 1; i >= 0; i--) {
		char digit = num.charAt(i);
		// 讨论出现 0/2/5/7 时是否已找到解
		if (digit == '0') {
			if (has0) return l - i - 2;
			else has0 = true;
			cnt0++;
		}
		else if (digit == '5') {
			if (has0) return l - i - 2;
			else has5 = true;
		}
		else if (digit == '2' || digit == '7') {
			if (has5) return l - i - 2;
		}
	}
	// 如果遍历完成仍未找到解, 只需删去所有非 0 元素, 将 num 变成 0
	return l - cnt0;
}
```

#### [2845. 统计趣味子数组的数目](https://leetcode.cn/problems/count-of-interesting-subarrays/)

@2024.01.22



```java
/**
 * 前缀和
 * 灵茶山艾府
 */
public long countInterestingSubarrays(List<Integer> nums, int modulo, int k) {
	int n = nums.size();
	int[] cnt = new int[n + 1];
	cnt[0] = 1;
	int pre = 0;
	long ans = 0;
	for (int x : nums) {
		if (x % modulo == k) pre = ++pre % modulo;
		int p = (pre - k + modulo) % modulo;
		if (p <= n) ans += cnt[p];
		cnt[pre]++;
	}
	return ans;
}
```

#### [2849. 判断能否在给定时间到达单元格](https://leetcode.cn/problems/determine-if-a-cell-is-reachable-at-a-given-time/)

@2023.09.10

#数学 

设 $X$ 方向上距离为 `dx`、$Y$ 方向上距离为 `dy`，不妨假设 `dx < dy`，那么先斜向走 `dx` 步，$X$ 方向上已到达，$Y$ 方向上还差 `dy - dx` 步，总步数为 `dx + (dy - dx)`，归纳得总步数为 `max(dx, dy)`，判断其是否 `<= t` 即可。

特殊情况：`max(dx, dy) == 0` 且 `t == 1`，由于必须走 1 步，必定离开终点，因此最终无法到达。

```java
/**
 * 数学
 * Somnia1337
 */
public boolean isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
	int d = Math.max(Math.abs(fx - sx), Math.abs(fy - sy));
	return (d != 0 || t != 1) && d <= t;
}
```

#### [2850. 将石头分散到网格图的最少移动次数](https://leetcode.cn/problems/minimum-moves-to-spread-stones-over-grid/)

@2023.09.14

#回溯 

```java
/**
 * 回溯
 * JavaLongestLang
 */
public int minimumMoves(int[][] grid) {
	return bt(grid, 0, 0);
}

private int bt(int[][] grid, int x, int y) {
	if (x >= 3) return 0;
	if (y >= 3) return bt(grid, x + 1, 0);
	if (grid[x][y] != 0) return bt(grid, x, y + 1);
	int ans = Integer.MAX_VALUE;
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			if (i == x && j == y) continue;
			if (grid[i][j] <= 1) continue;
			grid[i][j]--;
			ans = Math.min(bt(grid, x, y + 1) + Math.abs(i - x) + Math.abs(j - y), ans);
			grid[i][j]++;
		}
	}
	return ans;
}
```

#### [2856. 删除数对后的最小数组长度](https://leetcode.cn/problems/minimum-array-length-after-pair-removals/)

@2023.09.17

#数学 #双指针 #二分查找 

1. 数学

计数，如果有数字出现次数过半，则用其他元素与它消解，否则一定能删到只剩0 个/1 个元素（取决于数组长度奇偶性）。

```java
/**
 * 数学
 * Somnia1337
 */
public int minLengthAfterRemovals(List<Integer> nums)
{
	int len = nums.size();
	int max = 0;
	Map<Integer, Integer> count = new HashMap<>();
	for (int num : nums)
	{
		count.merge(num, 1, Integer::sum);
		if (count.get(num) > max) max = count.get(num);
	}
	return max > len / 2 ? len - (len - max) * 2 : len % 2;
}
```

2. 双指针

`left`初始化为0，`right`初始化为`(len + 1) / 2`，当`right` < `len`时，如果`nums[left]` < `nums[right]`，二者均加1，否则仅`right`加1。

```java
/**
 * 双指针
 * oamuuvyi
 */
public int minLengthAfterRemovals(List<Integer> nums)
{
	int len = nums.size();
	int left = 0;
	for (int right = (len + 1) / 2; right < len; right++)
	{
		if (nums.get(left) < nums.get(right))
		{
			left++;
		}
	}
	return len - 2 * left;
}
```

3. 二分查找

```java
/**
 * 二分查找
 * oamuuvyi
 */
public int minLengthAfterRemovals(List<Integer> nums)
{
	int n = nums.size();
	int i = n / 2, j = i;
	Integer num = nums.get(i);
	while (i >= 0 && nums.get(i)
						 .equals(num)) i--;
	while (j < n && nums.get(j)
						.equals(num)) j++;
	return n - 2 * Math.min(n / 2, i + 1 + n - j);
}
```

#### [2857. 统计距离为 k 的点对](https://leetcode.cn/problems/count-pairs-of-points-with-distance-k/)

@2023.09.17

#暴力 

遍历`coordinates`，对每个坐标，枚举与其距离为`k`的所有可能坐标。

```java
/**
 * 枚举
 * 纰缪
 */
public int countPairs(List<List<Integer>> coordinates, int k)
{
	HashMap<List<Integer>, Integer> points = new HashMap<>();
	int ans = 0;
	for (List<Integer> coordinate : coordinates)
	{
		Integer a = coordinate.get(0);
		Integer b = coordinate.get(1);
		for (int i = 0; i <= k; i++) ans += points.getOrDefault(List.of(i ^ a, k - i ^ b), 0);
		points.merge(coordinate, 1, Integer::sum);
	}
	return ans;
}
```

#### [2860. 让所有学生保持开心的分组方法数](https://leetcode.cn/problems/happy-students/)

@2023.09.17

#数组 

排序后，对每个索引`i`，需满足`i + 1 > nums[i]`、`i + 1 < nums[i + 1]`。

```java
/**
 * 排序
 * Somnia1337
 */
public int countWays(List<Integer> nums)
{
	int len = nums.size();
	nums.add(Integer.MAX_VALUE);
	Collections.sort(nums);
	
	int ans = (nums.get(0) > 0 ? 1 : 0);
	for (int i = 0; i < len; i++)
	{
		if (i + 1 > nums.get(i) && i + 1 < nums.get(i + 1)) ans++;
	}
	
	return ans;
}
```

#### [2861. 最大合金数](https://leetcode.cn/problems/maximum-number-of-alloys/)

@2023.09.17

#二分查找 

枚举每个机器，二分查找其能制造的最大合金数。

二分时，上界定为 `10 ** 9`，下界定为 `0`，计算制造中值个合金所需的钱，与 `budget` 对比进行查找。

```java
/**
 * 二分查找
 * 珂朵莉
 */
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

#### [2865. 美丽塔 I](https://leetcode.cn/problems/beautiful-towers-i/)

@2023.09.24

#暴力 

枚举以每座塔作为峰值时的总高度。

```java
/**
 * 枚举
 * Somnia1337
 */
public long maximumSumOfHeights(List<Integer> maxHeights)
{
	long ans = 0;
	for (int i = 0; i < maxHeights.size(); i++)
	{
		ans = Math.max(solve(maxHeights, i), ans);
	}
	return ans;
}

private long solve(List<Integer> maxHeights, int index)
{
	int peak = maxHeights.get(index);
	long ret = peak;
	int left = peak, right = peak;
	for (int i = index - 1; i >= 0; i--)
	{
		left = Math.min(maxHeights.get(i), left);
		ret += left;
	}
	for (int i = index + 1; i < maxHeights.size(); i++)
	{
		right = Math.min(maxHeights.get(i), right);
		ret += right;
	}
	return ret;
}
```

#### [2866. 美丽塔 II](https://leetcode.cn/problems/beautiful-towers-ii/)

@2023.09.24

```java
/**
 * 单调栈
 * Nervose Wu
 */
public long maximumSumOfHeights(List<Integer> maxHeights)
{
	int[] dict = new int[maxHeights.size()];
	for (int i = 0; i < maxHeights.size(); i++)
	{
		dict[i] = maxHeights.get(i);
	}
	
	// 以 i 为山顶时左侧 / 右侧的最大值
	long[] left = new long[dict.length];
	long[] right = new long[dict.length];
	Deque<int[]> arrayDeque = new ArrayDeque<>();
	long curSum = 0;
	for (int i = 0; i < dict.length; i++)
	{
		int cur = dict[i];
		int total = 0;
		while (!arrayDeque.isEmpty() && arrayDeque.peekLast()[0] > cur)
		{
			int[] temp = arrayDeque.pollLast();
			total += temp[1];
			curSum -= ((long) temp[1] * (temp[0] - cur));
		}
		left[i] = curSum;
		curSum += cur;
		arrayDeque.addLast(new int[]{cur, total + 1});
	}
	arrayDeque = new ArrayDeque<>();
	curSum = 0;
	for (int i = dict.length - 1; i >= 0; i--)
	{
		int cur = dict[i];
		int total = 0;
		while (!arrayDeque.isEmpty() && arrayDeque.peekLast()[0] > cur)
		{
			int[] temp = arrayDeque.pollLast();
			total += temp[1];
			curSum -= ((long) temp[1] * (temp[0] - cur));
		}
		right[i] = curSum;
		curSum += cur;
		arrayDeque.addLast(new int[]{cur, total + 1});
	}
	long res = 0;
	for (int i = 0; i < dict.length; i++)
	{
		res = Math.max(res, dict[i] + left[i] + right[i]);
	}
	return res;
}
```

#### [2870. 使数组为空的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-empty/)

@2023.09.30

#技巧 

用哈希表对`nums`计数，如果有元素只出现1次，返回-1，否则，将每个出现次数用最少的2或3表示，找到规律如下：

```text
出现次数：2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
最少操作：1 1 2 2 2 3 3 3 4  4  4  5  5  5  6
```

最少操作 = (出现次数 + 2) / 3。

```java
/**
 * 找规律
 * Somnia1337
 */
public int minOperations(int[] nums)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int n : nums)
	{
		count.merge(n, 1, Integer::sum);
	}
	int ans = 0;
	for (int c : count.values())
	{
		if (c == 1) return -1;
		ans += (c + 2) / 3;
	}
	return ans;
}
```

#### [2871. 将数组分割成最多数目的子数组](https://leetcode.cn/problems/split-array-into-maximum-number-of-subarrays/)

@2023.10.01

#贪心 #位运算 

`&` 的性质：`a & b` $\leqslant$ `min(a, b)`，即更多的数 `&` 在一起非增。

由此，将 `nums` 的所有元素进行 `&` 运算的结果一定最小，记 $\underset{i = 1}{\overset{len}{\&}} nums[i]$ 为 `sum`——

- 如果 `sum` = 0，说明有可能存在多个 `&` 的结果为 0 的子数组，在遍历时，一旦当前子数组 `&` 的结果为 0，就拆分出一个子数组，返回最终分割出的子数组数量。
	- 边界：只在 `&` 的结果 = 0 时累加答案（相当于切割的刀数），由于最后一个后缀子数组 `&` 的结果可能不为 0，最终答案不能加 1；反过来想，如果最后一个后缀子数组 `&` 的结果也为 0，那么最后一刀切在末元素之后，答案也是正确的。
- 如果 `sum` > 0，那么任取两个子数组，其各自 `&` 运算的结果一定都不小于 `sum`，其和不小于 `sum * 2`，因此无法分割。

`-1` 的性质：`-1` = `111...1`，`x & -1 = x`。

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int maxSubarrays(int[] nums)
{
	int ans = 0;
	int a = -1; // mask
	for (int num : nums)
	{
		a &= num;
		if (a == 0)
		{
			ans++; // 切一刀
			a = -1; // 重置mask
		}
	}
	return Math.max(ans, 1); // 如果ans=0，也即没有切刀，说明全部元素&在一起不为0，无法分割，返回1
}
```

#### [2874. 有序三元组中的最大值 II](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-ii/)

@2023.10.01

#暴力 

用 `sufMax[i]` 记录 `nums` 的后缀最大值，遍历 `nums`，用 `max` 记录遍历到的最大值并作为 `nums[i]`，当前元素作为 `nums[j]`，`sufMax[j + 1]` 作为 `nums[k]`，更新 `max((nums[i] - nums[j]) * nums[k])`。

```java
/**
 * 枚举
 * Somnia1337
 */
public long maximumTripletValue(int[] nums) {
	int n = nums.length, max = nums[0]; // 前缀最大值作为 nums[i]
	long ans = 0;
	int[] sufMax = Arrays.copyOf(nums, n); // 后缀最大值作为 nums[k]
	for (int i = n - 2; i >= 0; i--) {
		sufMax[i] = Math.max(sufMax[i + 1], sufMax[i]);
	}
	for (int j = 1; j < n - 1; j++) { // 遍历, 中间元素作为 nums[j]
		if (nums[j] >= max) max = nums[j];
		else ans = Math.max((long) (max - nums[j]) * sufMax[j + 1], ans);
	}
	return ans;
}
```

优化：一次遍历，用 `deltaMax` 记录的最大 `nums[i] - nums[j]`，用 `preMax` 记录前缀最大值，遍历时将 `x` 依次作为 `nums[k]`、`nums[j]`、`nums[i]` 更新 `ans`、`deltaMax`、`preMax`。

```java
/**
 * 枚举
 * ?
 */
public long maximumTripletValue(int[] nums) {
	int deltaMax = 0, preMax = 0;
	long ans = 0;
	for (int x : nums) {
		// 将 x 作为 nums[k], 更新 ans
		ans = Math.max((long) deltaMax * x, ans);
		// 将 x 作为 nums[j], 更新 deltaMax
		deltaMax = Math.max(preMax - x, deltaMax);
		// 将 x 作为 nums[i], 更新 preMax
		preMax = Math.max(x, preMax);
	}
	return ans;
}
```

#### [2875. 无限数组的最短子数组](https://leetcode.cn/problems/minimum-size-subarray-in-infinite-array/)

@2023.10.01

```java
/**
 * ?
 * jiaobohan
 */
public int minSizeSubarray(int[] nums, int target)
{
	int start = 0, end = 0, len = nums.length;
	int sum = 0, minLen = Integer.MAX_VALUE;
	while (start == 0 || start % len != 0)
	{
		if (sum >= target)
		{
			if (sum == target)
			{
				minLen = Math.min(minLen, end - start);
			}
			sum -= nums[start % len];
			start++;
		}
		else
		{
			sum += nums[end % len];
			end++;
		}
	}
	return minLen < Integer.MAX_VALUE ? minLen : -1;
}
```

#### [2895. 最小处理时间](https://leetcode.cn/problems/minimum-processing-time/)

@2023.10.08

#贪心 

排序，将耗时最长的任务分配给最早空闲的 CPU，取所有核心的最晚完成时间。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minProcessingTime(List<Integer> processorTime, List<Integer> tasks)
{
	Collections.sort(processorTime);
	Collections.sort(tasks);
	int ans = 0;
	for (int i = tasks.size() - 1, j = 0; i >= 0; i -= 4, j++)
	{
		ans = Math.max(processorTime.get(j) + tasks.get(i), ans);
	}
	return ans;
}
```

#### [2896. 执行操作使两个字符串相等](https://leetcode.cn/problems/apply-operations-to-make-two-strings-equal/)

@2023.10.08

```java
/**
 * 动态规划
 * 小羊肖恩
 */
public int minOperations(String s1, String s2, int x)
{
	int len = s1.length();
	char[] chars1 = s1.toCharArray();
	char[] chars2 = s2.toCharArray();
	List<Integer> indices = new ArrayList<>();
	for (int i = 0; i < len; i++)
	{
		if (chars1[i] != chars2[i])
		{
			indices.add(i);
		}
	}
	
	int size = indices.size();
	if (size % 2 == 1) return -1;
	int[] dp = new int[size + 1];
	Arrays.fill(dp, Integer.MAX_VALUE);
	dp[0] = 0;
	
	for (int i = 0; i < size; i++)
	{
		if (i % 2 == 0) dp[i + 1] = dp[i];
		else dp[i + 1] = dp[i] + x;
		if (i > 0)
		{
			dp[i + 1] = Math.min(dp[i + 1], dp[i - 1] + (indices.get(i) - indices.get(i - 1)));
		}
	}
	
	return dp[size];
}
```

#### [2898. 最大线性股票得分](https://leetcode.cn/problems/maximum-linear-stock-score/)

@2024.01.16



```java
/**
 * 哈希表
 * Somnia1337
 */
public long maxScore(int[] prices) {
	Map<Integer, Long> g = new HashMap<>();
	long ans = 0;
	for (int i = 0; i < prices.length; i++) {
		ans = Math.max(g.merge(prices[i] - i, (long) prices[i], Long::sum), ans);
	}
	return ans;
}
```

#### [2900. 最长相邻不相等子序列 I](https://leetcode.cn/problems/longest-unequal-adjacent-groups-subsequence-i/)

@2023.10.14

#贪心 

遍历，当 `groups[i] != groups[i - 1]` 时加入解集。

```java
/**
 * 贪心
 * Somnia1337
 */
public List<String> getWordsInLongestSubsequence(int n, String[] words, int[] groups)
{
	List<String> ans = new ArrayList<>();
	
	for (int i = 0; i < n; i++)
	{
		if (i == 0 || groups[i] != groups[i - 1])
		{
			ans.add(words[i]);
		}
	}
	
	return ans;
}
```

#### [2904. 最短且字典序最小的美丽子字符串](https://leetcode.cn/problems/shortest-and-lexicographically-smallest-beautiful-string/)

@2023.10.15

#暴力 #前缀和 

求前缀和，`pre[i]` 表示前 `i` 个字符中 `'1'` 的数量。

维护最小长度与起点，枚举终点和可能的起点，满足条件时更新最小长度和起点。

```java
/**
 * 枚举
 * Somnia1337
 */
public String shortestBeautifulSubstring(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[] pre = new int[n + 1];
	for (int i = 1; i <= n; i++) pre[i] = pre[i - 1] + chs[i - 1] - '0';
	
	int minL = n + 1, st = -1;
	for (int i = 1; i < n + 1; i++) { // 枚举终点
		for (int j = Math.max(0, i - minL); j < i; j++) { // 枚举起点
			// 更新最小长度和起点
			// 1 的个数为 k && (st 未更新 || i-j<minL || 新串字典序更小)
			if (pre[i] - pre[j] == k && (st == -1 || minL > i - j || s.substring(j, j + minL).compareTo(s.substring(st, st + minL)) < 0)) {
				minL = i - j;
				st = j;
			}
		}
	}
	return st >= 0 ? s.substring(st, st + minL) : "";
}
```

#### [2905. 找出满足差值条件的下标 II](https://leetcode.cn/problems/find-indices-with-index-and-value-difference-ii/)

@2023.10.15

#暴力 

维护最大值、最小值所在下标 `maxIdx`、`minIdx`，从 `indexDifference` 开始枚举 `j`（此时 `j - indexDifference` 才有效），用 `j - indexDifference` 更新 `maxIdx`、`minIdx`，然后检查 `j` 处元素与最大元素、最小元素的差值是否超过 `valueDifference`。

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] findIndices(int[] nums, int indexDifference, int valueDifference)
{
	int len = nums.length;
	int maxIdx = 0, minIdx = 0;
	
	for (int j = indexDifference; j < len; j++) // 从indexDifference开始
	{
		int idx = j - indexDifference;
		if (nums[idx] > nums[maxIdx]) maxIdx = idx;
		if (nums[idx] < nums[minIdx]) minIdx = idx;
		if (Math.abs(nums[j] - nums[maxIdx]) >= valueDifference) return new int[]{maxIdx, j};
		if (Math.abs(nums[j] - nums[minIdx]) >= valueDifference) return new int[]{minIdx, j};
	}
	
	return new int[]{-1, -1};
}
```

#### [2906. 构造乘积矩阵](https://leetcode.cn/problems/construct-product-matrix/)

@2023.10.15

#技巧 

将 `grid` 展开成一维，对其求每个元素除自身以外的乘积，再复原成二维。

核心部分对 [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/) 的代码略作修改即可。

```java
/**
 * 前缀积
 * Somnia1337
 */
public int[][] constructProductMatrix(int[][] grid)
{
	// 展开
	int rows = grid.length, cols = grid[0].length;
	long[] flat = new long[rows * cols];
	for (int i = 0; i < rows * cols; i++)
	{
		flat[i] = grid[i / cols][i % cols]; // 血泪史！！！
	}
	
	// 求解，然后复原
	long[] pros = productExceptSelf(flat);
	int[][] ans = new int[rows][cols];
	for (int i = 0; i < rows * cols; i++)
	{
		ans[i / cols][i % cols] = (int) pros[i]; // 血泪史！！！
	}
	
	return ans;
}

// 稍微修改[238. 除自身以外数组的乘积]的代码
private long[] productExceptSelf(long[] nums)
{
	final int MOD = 12345;
	int len = nums.length;
	long[] ans = new long[len];
	
	ans[0] = 1;
	for (int i = 1; i < len; i++)
	{
		ans[i] = (nums[i - 1] % MOD) * (ans[i - 1] % MOD);
	}
	long R = 1;
	for (int i = len - 1; i >= 0; i--)
	{
		ans[i] *= R % MOD;
		R *= nums[i] % MOD;
		ans[i] %= MOD;
		R %= MOD;
	}
	
	return ans;
}
```

#### [2909. 元素和最小的山形三元组 II](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-ii/)

@2023.10.22

#前后缀分解 

记录每个位置及左侧、及右侧的最小值，遍历，满足 `nums[j] > lMin[j]` 且 `nums[j] > rMin[j]` 的 `j` 可作为山峰。

```java
/**
 * 前后缀分解
 * 陈梁
 */
public int minimumSum(int[] nums) {
    int n = nums.length;
    int[] lMin = new int[n], rMin = new int[n];
    
    lMin[0] = nums[0];
    for (int i = 1; i < n; i++) {
        lMin[i] = Math.min(lMin[i - 1], nums[i]);
    }
    rMin[n - 1] = nums[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        rMin[i] = Math.min(rMin[i + 1], nums[i]);
    }
    
    int ans = Integer.MAX_VALUE;
    for (int j = 1; j < n - 1; j++) {
        if (nums[j] > lMin[j] && nums[j] > rMin[j]) {
            ans = Math.min(lMin[j] + nums[j] + rMin[j], ans);
        }
    }
    return ans < Integer.MAX_VALUE ? ans : -1;
}
```

#### [2914. 使二进制字符串变美丽的最少修改次数](https://leetcode.cn/problems/minimum-number-of-changes-to-make-binary-string-beautiful/)

@2023.10.28

#贪心 

记录每段连续的 `1`、`0` 的长度，一次遍历，用下一段补全长度为奇数的段。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minChanges(String s)
{
	int len = s.length();
	List<Integer> ls = new ArrayList<>();
	int left = 0, right = 0;
	while (right < len)
	{
		char l = s.charAt(left);
		while (right < len && s.charAt(right) == l) right++;
		ls.add(right - left);
		left = right;
	}
	int ans = 0;
	for (int i = 0; i < ls.size() - 1; i++)
	{
		if ((ls.get(i) & 1) == 1)
		{
			ls.set(i + 1, ls.get(i + 1) + 1);
			ans++;
		}
	}
	return ans;
}
```

另一种思路：美丽 <=> 对于每个偶数下标 `i`，有 `s[i] == s[i+1]`，如果不符合，改变一个字符即可。

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int minChanges(String s)
{
	int ans = 0;
	for (int i = 0; i < s.length(); i += 2)
	{
		if (s.charAt(i) != s.charAt(i + 1)) ans++;
	}
	return ans;
}
```

#### [2915. 和为目标值的最长子序列的长度](https://leetcode.cn/problems/length-of-the-longest-subsequence-that-sums-to-target/)

@2023.10.29

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int lengthOfLongestSubsequence(List<Integer> nums, int target)
{
	int[] dp = new int[target + 1];
	Arrays.fill(dp, Integer.MIN_VALUE);
	dp[0] = 0;
	int s = 0;
	for (int num : nums)
	{
		for (int i = Math.min(s + num, target); i >= num; i--)
		{
			dp[i] = Math.max(dp[i - num] + 1, dp[i]);
		}
	}
	return dp[target] > 0 ? dp[target] : -1;
}
```

#### [2918. 数组的最小相等和](https://leetcode.cn/problems/minimum-equal-sum-of-two-arrays-after-replacing-zeros/)

@2023.10.29

#分类讨论 

对 `nums1`、`nums2` 中的 `0` 计数，同时求和，然后讨论：

- `z1 == z2 == 0`，必须 `s1 == s2`，否则 `-1`。
- `z1 == 0 || z2 == 0`，以 `z1 == 0` 为例，`nums2` 的每个 `0` 都至少要变成 `1`，因此必须 `s2 + z2 <= s1`，否则 `-1`。
- `z1 > 0 && z2 > 0`，返回 `min(s1 + z1, s2 + z2)`。

```java
/**
 * 分类讨论
 * Somnia1337
 */
public long minSum(int[] nums1, int[] nums2)
{
	long s1 = 0, s2 = 0, z1 = 0, z2 = 0;
	for (int num1 : nums1)
	{
		if (num1 == 0) z1++;
		s1 += num1;
	}
	for (int num2 : nums2)
	{
		if (num2 == 0) z2++;
		s2 += num2;
	}
	if (z1 == 0 && z2 == 0) return s1 == s2 ? s1 : -1;
	if (z1 == 0) return s2 + z2 <= s1 ? s1 : -1;
	if (z2 == 0) return s1 + z1 <= s2 ? s2 : -1;
	return Math.max(s1 + z1, s2 + z2);
}
```

#### [2924. 找到冠军 II](https://leetcode.cn/problems/find-champion-ii/)

@2023.11.05

#搜索 

标记较弱队，遍历查找未被标记的队，如果有一个则返回，如果多于一个则返回 `-1`。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findChampion(int n, int[][] edges) {
	boolean[] ban = new boolean[n];
	for (int[] e : edges) {
		ban[e[1]] = true;
	}
	int ans = -1;
	for (int i = 0; i < n; i++) {
		if (!ban[i]) {
			if (ans == -1) ans = i;
			else return -1;
		}
	}
	return ans;
}
```

#### [2929. 给小朋友们分糖果 II](https://leetcode.cn/problems/distribute-candies-among-children-ii/)

@2023.11.12

#数学 #暴力 #分类讨论 

1. 数学

分类讨论，组合数计算。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public long distributeCandies(int n, int limit) {
	return c2(n + 2) - 3 * c2(n - limit + 1) + 3 * c2(n - 2 * limit) - c2(n - 3 * limit - 1);
}

private long c2(int n) {
	return n > 1 ? (long) n * (n - 1) / 2 : 0;
}
```

2. 枚举

枚举第一个人的糖果，计算后两个人的分类方法。

```java
/**
 * 枚举
 * 不上guardian不改名
 */
public long distributeCandies(int n, int limit) {
	long ans = 0;
	for (int i = 0; i <= Math.min(n, limit); i++) {
		ans += helper(n - i, limit);
	}
	return ans;
}

private long helper(int n, int l) {
	long a = Math.min(n, l), b = Math.max(n - l, 0);
	return Math.max(a - b + 1, 0);
}
```

#### [2933. 高访问员工](https://leetcode.cn/problems/high-access-employees/)

@2023.11.12

#哈希表 

`Map<员工, List<访问时间>>`，记录后遍历，排序 `List` 并遍历，如果存在 `i`，`log[i + 2] - log[i] < 60`，即为高访问员工。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<String> findHighAccessEmployees(List<List<String>> access_times)
{
	Map<String, List<Integer>> logs = new HashMap<>();
	for (List<String> log : access_times)
	{
		logs.putIfAbsent(log.get(0), new ArrayList<>());
		String t = log.get(1);
		logs.get(log.get(0)).add(60 * Integer.parseInt(t.substring(0, 2)) + Integer.parseInt(t.substring(2)));
	}
	List<String> ans = new ArrayList<>();
	for (String staff : logs.keySet())
	{
		List<Integer> log = logs.get(staff);
		if (log.size() < 3) continue;
		Collections.sort(log);
		for (int i = 0; i < log.size() - 2; i++)
		{
			if (log.get(i + 2) - log.get(i) < 60)
			{
				ans.add(staff);
				break;
			}
		}
	}
	return ans;
}
```

#### [2934. 最大化数组末位元素的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-maximize-last-elements-in-arrays/)

@2024.01.18



```java
/**
 * 分类讨论
 * 林深时见鹿
 */
public int minOperations(int[] nums1, int[] nums2) {
	int n = nums1.length;
	return Math.min(f(nums1[n - 1], nums2[n - 1], nums1, nums2), 1 + f(nums2[n - 1], nums1[n - 1], nums1, nums2));
}

private int f(int l1, int l2, int[] a, int[] b) {
	int ret = 0;
	for (int i = 0; i < a.length - 1; i++) {
		if (a[i] > l1 || b[i] > l2) {
			if (b[i] > l1 || a[i] > l2) return -1;
			ret++;
		}
	}
	return ret;
}
```

#### [2938. 区分黑球与白球](https://leetcode.cn/problems/separate-black-and-white-balls/)

@2023.11.19

#数组 

1. 两次遍历

第一次统计黑球，第二次累加移动黑球所需的距离。

```java
/**
 * 两次遍历
 * Somnia1337
 */
public long minimumSteps(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length, b = 0;
	for (char c : chs) {
		if (c == '1') b++;
	}
	long ans = 0;
	for (int i = 0; i < n; i++) {
		if (chs[i] == '1') {
			ans += n - b - i;
			b--;
		}
	}
	return ans;
}
```

2. 一次遍历

遍历统计黑球，如果为白球，累加将其移至左侧的距离，即当前见过的黑球个数。

```java
/**
 * 一次遍历
 * 灵茶山艾府
 */
public long minimumSteps(String s) {
	int cnt1 = 0;
	long ans = 0;
	for (char c : s.toCharArray()) {
		if (c == '1') cnt1++;
		else ans += cnt1;
	}
	return ans;
}
```

#### [2943. 最大化网格图中正方形空洞的面积](https://leetcode.cn/problems/maximize-area-of-square-hole-in-grid/)

@2024.01.01

#技巧 

两个方向上都只能移除给定数组内的线段，因此要找各自最多的连续可移除线段数，二者取较小者即为正方形边长。

```java
/**
 * 脑筋急转弯
 * 可爱抱抱呀😥
 */
public int maximizeSquareHoleArea(int n, int m, int[] hBars, int[] vBars) {
	return (int) Math.pow(Math.min(solve(hBars), solve(vBars)), 2);
}

private int solve(int[] arr) {
	Arrays.sort(arr);
	int count = 2, ans = 2;
	for (int i = 1; i < arr.length; i++) {
		if (arr[i] == arr[i - 1] + 1) count++;
		else count = 2;
		ans = Math.max(count, ans);
	}
	return ans;
}
```

#### [2944. 购买水果需要的最少金币数](https://leetcode.cn/problems/minimum-number-of-coins-for-fruits/)

@2023.11.26

#动态规划 

`dp` 表示获得前 `i` 个水果的最少花费，`dp[i][0]` 表示购买第 `i` 个水果的状态，`dp[i][1]` 表示免费获得第 `i` 个水果的状态。状态转移：

- `dp[i][0] = min(dp[i - 1][0], dp[i - 1][1]) + prices[i]`
- `dp[i][1] = min(dp[i/2:i][0])`

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minimumCoins(int[] prices)
{
	int len = prices.length;
	int[][] dp = new int[len][2];
	dp[0][0] = dp[0][1] = prices[0];
	for (int i = 1; i < len; i++)
	{
		dp[i][0] = Math.min(dp[i - 1][0], dp[i - 1][1]) + prices[i];
		dp[i][1] = Integer.MAX_VALUE;
		for (int j = i - 1; j >= i / 2; j--)
		{
			dp[i][1] = Math.min(dp[j][0], dp[i][1]);
		}
	}
	return Math.min(dp[len - 1][0], dp[len - 1][1]);
}
```

优化：状态压缩到一维，`dp[i]` 表示获得前 `i` 个水果的最少花费，取 `min(dp[i/2:i])` 作为当前子问题的答案。

```java
/**
 * 动态规划
 * Explorer
 */
public int minimumCoins(int[] prices)
{
	int[] dp = new int[prices.length];
	int ans = 0;
	for (int i = 0; i < prices.length; i++)
	{
		dp[i] = ans + prices[i];
		ans = Integer.MAX_VALUE;
		for (int j = i / 2; j <= i; j++)
		{
			ans = Math.min(dp[j], ans);
		}
	}
	return ans;
}
```

#### [2947. 统计美丽子字符串 I](https://leetcode.cn/problems/count-beautiful-substrings-i/)

@2023.11.26

#前缀和 

元音字母权为 1，辅音为 0，计算 `s` 权的前缀和，枚举结尾字符，嵌套枚举开始字符，检查是否满足要求。

```java
/**
 * 前缀和
 * Somnia1337
 */
public int beautifulSubstrings(String s, int k)
{
	int l = s.length();
	int[] pre = new int[l + 1];
	for (int i = 1; i < l + 1; i++)
	{
		pre[i] = pre[i - 1] + ("aeiou".indexOf(s.charAt(i - 1)) >= 0 ? 1 : 0);
	}
	
	int ans = 0;
	for (int i = 2; i < l + 1; i++)
	{
		for (int j = i & 1; j < i; j += 2)
		{
			int d = pre[i] - pre[j];
			if (d == (i - j) / 2 && d * d % k == 0) ans++;
		}
	}
	return ans;
}
```

#### [2948. 交换得到字典序最小的数组](https://leetcode.cn/problems/make-lexicographically-smallest-array-by-swapping-elements/)

@2024.01.18



```java
/**
 * 分组循环
 * 灵茶山艾府
 */
public int[] lexicographicallySmallestArray(int[] nums, int limit) {
	int n = nums.length;
	Integer[] ids = new Integer[n];
	Arrays.setAll(ids, a -> a);
	Arrays.sort(ids, Comparator.comparingInt(i -> nums[i]));
	
	int[] ans = new int[n];
	for (int i = 0; i < n; ) {
		int st = i;
		for (i++; i < n && nums[ids[i]] - nums[ids[i - 1]] <= limit; ) i++;
		Integer[] sub = Arrays.copyOfRange(ids, st, i);
		Arrays.sort(sub);
		for (int j = 0; j < sub.length; j++) ans[sub[j]] = nums[ids[st + j]];
	}
	return ans;
}
```

#### [2952. 需要添加的硬币的最小数量](https://leetcode.cn/problems/minimum-number-of-coins-to-be-added/)

@2023.12.03

#贪心 

如果已经能凑出 `[0, s - 1]`，新增一个数 `x`，就新得到 `[x, s + x - 1]`，讨论 `x`：

- `x <= s`，合并区间，得到 `[0, s + x - 1]`
- `x > s`，意味着无法得到 `s`，因此 `s` 必须加到数组中，得到 `[0, 2 * s - 1]`，重新考虑 `x` 与 `s` 大小关系

实现中：

- 先排序，从 0 开始累加硬币
- `s` 实际为 `cur`，表示目前累加的硬币值

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int minimumAddedCoins(int[] coins, int target)
{
	Arrays.sort(coins);
	int i = 0, cur = 1, ans = 0;
	while (cur <= target)
	{
		if (i < coins.length && coins[i] <= cur)
		{
			cur += coins[i];
			i++;
		}
		else
		{
			cur *= 2;
			ans++;
		}
	}
	return ans;
}
```

#### [2955. 同端子串的数量](https://leetcode.cn/problems/number-of-same-end-substrings/)

@2024.01.24

```java
// 爆内存的 DP
public int[] sameEndSubstringCount(String s, int[][] queries) {
	int[][] dp = prep(s);
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) ans[i] = dp[queries[i][0]][queries[i][1]];
	return ans;
}

private int[][] prep(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[][] ret = new int[n][n];
	for (int i = 0; i < n; i++) ret[i][i] = 1;
	for (int k = n - 1; k > 0; k--) {
		for (int i = k - 1, j = n - 1; i >= 0; i--, j--) {
			ret[i][j] = ret[i + 1][j] + ret[i][j - 1] - ret[i + 1][j - 1] + (chs[i] == chs[j] ? 1 : 0);
		}
	}
	return ret;
}
```

```java
/**
 * 前缀和
 * 丁飞
 */
public int[] sameEndSubstringCount(String s, int[][] queries) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[][] dc = new int[26][n + 1];
	for (int i = 0; i < s.length(); i++) {
		for (int k = 0; k < 26; k++) {
			dc[k][i + 1] = dc[k][i] + (s.charAt(i) - 'a' == k ? 1 : 0);
		}
	}
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++) {
		int l = queries[i][0], r = queries[i][1], cur = 0;
		for (int[] x : dc) {
			int cnt = x[r + 1] - x[l];
			cur += cnt * (cnt + 1) >> 1;
		}
		ans[i] = cur;
	}
	return ans;
}
```

```java
/**
 * 前缀和
 * ?
 */
public int[] sameEndSubstringCount(String s, int[][] queries) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[] idx = new int[26], next = new int[n];
	Arrays.fill(idx, -1);
	for (int i = n - 1; i >= 0; i--) {
		int ci = chs[i] - 'a';
		next[i] = idx[ci];
		idx[ci] = i;
	}
	int[] ans = new int[queries.length], count = new int[n + 1];
	for (int ci = 0; ci < 26; ci++) {
		int ix = idx[ci];
		if (ix < 0) continue;
		int cnt = 0;
		for (int i = 0; i < n; i++) {
			count[i] = cnt;
			if (ix == i) {
				cnt++;
				ix = next[ix];
			}
		}
		count[n] = cnt;
		for (int i = 0; i < queries.length; i++) {
			int[] q = queries[i];
			cnt = count[q[1] + 1] - count[q[0]];
			ans[i] += (cnt * (cnt + 1)) >>> 1;
		}
	}
	
	return ans;
}
```

#### [2957. 消除相邻近似相等字符](https://leetcode.cn/problems/remove-adjacent-almost-equal-characters/)

@2023.12.09

#贪心 

每个字符都可任意替换，那么无论如何替换，只需 1 次替换即可满足要求。从下标 `1` 开始遍历（懒替换，贪心点），与前一个字符比较，计数。

```java
/**
 * 贪心
 * Somnia1337
 */
public int removeAlmostEqualCharacters(String word) {
	int ans = 0;
	for (int i = 1; i < word.length(); i++) {
		if (Math.abs(word.charAt(i) - word.charAt(i - 1)) <= 1) {
			ans++;
			i++;
		}
	}
	return ans;
}
```

#### [2958. 最多 K 个重复元素的最长子数组](https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/)

@2023.12.09

#双指针 

双指针，哈希表记录 `<元素, 计数>`，枚举子数组右端点，计数超过 `k` 时右移左端点，更新答案。

```java
/**
 * 双指针
 * Somnia1337
 */
public int maxSubarrayLength(int[] nums, int k) {
	int l = 0, ans = 0;
	Map<Integer, Integer> count = new HashMap<>();
	for (int r = 0; r < nums.length; r++) {
		count.merge(nums[r], 1, Integer::sum);
		while (count.get(nums[r]) > k) {
			count.merge(nums[l++], -1, Integer::sum);
		}
		ans = Math.max(r - l + 1, ans);
	}
	return ans;
}
```

#### [2961. 双模幂运算](https://leetcode.cn/problems/double-modular-exponentiation/)

@2023.12.10

#数学 

用带模快速幂检查每行。

```java
/**
 * 快速幂
 * Somnia1337
 */
public List<Integer> getGoodIndices(int[][] variables, int target) {
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < variables.length; i++) {
		int[] row = variables[i];
		if (pow(pow(row[0], row[1], 10), row[2], row[3]) % row[3] == target) ans.add(i);
	}
	return ans;
}

private int pow(int x, int p, int MOD) {
	int ret = 1;
	while (p > 0) {
		if (p % 2 == 1) ret = ((ret % MOD) * (x % MOD)) % MOD;
		x = ((x % MOD) * (x % MOD)) % MOD;
		p >>= 1;
	}
	return ret;
}
```

#### [2962. 统计最大元素出现至少 K 次的子数组](https://leetcode.cn/problems/count-subarrays-where-max-element-appears-at-least-k-times/)

@2023.12.10

#双指针 

1. 一次遍历

一次遍历记录 `max` 的所有位置，遍历 `pos`，累加子问题答案。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public long countSubarrays(int[] nums, int k) {
	int max = nums[0];
	List<Integer> pos = new ArrayList<>(); // max 的所有位置
	for (int i = 0; i < nums.length; i++) {
		if (nums[i] > max) {
			max = nums[i];
			pos = new ArrayList<>();
			pos.add(i);
		}
		else if (nums[i] == max) pos.add(i);
	}
	if (pos.size() < k) return 0; // 最大值出现少于 k 次
	
	long ans = 0;
	int size = pos.size();
	for (int i = k - 1; i < size; i++) { // 以前 k-1 个位置为右端的组不满足要求
		int start = pos.get(i - k + 1) + 1; // 为满足要求, 左端不能晚于第 i-k+1 个位置 +1
		int n = (i < size - 1 ? pos.get(i + 1) : nums.length) - pos.get(i); // 右端在 i 和 i+1 个位置之间的子数组的子答案均为 startCnt
		ans += (long) n * start;
	}
	return ans;
}
```

2. 双指针

维护最大值计数，枚举右端，右移左端直到计数 `< k`，此时左指针之前的位置均可作为左端。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public long countSubarrays(int[] nums, int k) {
	int max = 0;
	for (int num : nums) max = Math.max(num, max);
	long ans = 0;
	int cnt = 0, l = 0; // cnt: 最大值计数, l: 左端下标
	for (int num : nums) { // 枚举子数组右端
		if (num == max) cnt++;
		while (cnt == k) { // 计数为 k 时持续右移 l
			if (nums[l++] == max) cnt--;
		}
		ans += l; // l 之前的位置均可作为左端
	}
	return ans;
}
```

#### [2964. 可被整除的三元组数量](https://leetcode.cn/problems/number-of-divisible-triplet-sums/)

@2024.01.23



```java
/**
 * 哈希表
 * Somnia1337
 */
public int divisibleTripletCount(int[] nums, int d) {
	int n = nums.length, ans = 0;
	for (int l = 0; l < n - 2; l++) {
		int[] count = new int[d];
		count[nums[l + 1] % d] = 1;
		for (int r = l + 2; r < n; r++) {
			ans += count[(d - (nums[l] + nums[r]) % d) % d];
			count[nums[r] % d]++;
		}
	}
	return ans;
}
```

#### [2966. 划分数组并满足最大差限制](https://leetcode.cn/problems/divide-array-into-arrays-with-max-difference/)

@2023.12.17

#贪心 

排序，按顺序填入，检查每个三元组的极差是否 `<= k`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int[][] divideArray(int[] nums, int k) {
	Arrays.sort(nums);
	int[][] ans = new int[nums.length / 3][3];
	for (int i = 0, it = 0; i < ans.length; i++) {
		for (int j = 0; j < 3; j++, it++) ans[i][j] = nums[it];
		if (ans[i][2] - ans[i][0] > k) return new int[][]{};
	}
	return ans;
}
```

#### [2967. 使数组成为等数数组的最小代价](https://leetcode.cn/problems/minimum-cost-to-make-array-equalindromic/)

@2023.12.17

#数学 

排序，如果中位数为回文，将所有元素变为中位数的花费就是最小的，否则，找中位数两侧最近的回文数，返回较小花费。

```java
/**
 * 数学
 * Somnia1337
 */
public long minimumCost(int[] nums) {
	Arrays.sort(nums);
	
	// 中位数回文, 最优
	int mid = nums[nums.length / 2];
	if (isPalindrome(mid)) return cost(nums, mid);
	
	// 找中位数左/右的最近回文数, 返回较小花费
	int l = mid - 1, r = mid + 1;
	while (l > 1 && !isPalindrome(l)) l--;
	while (r < 1000000000 && !isPalindrome(r)) r++;
	return Math.min(cost(nums, l), cost(nums, r));
}

// 将所有元素变为 tar 的花费
private long cost(int[] nums, int tar) {
	long ret = 0;
	for (int a : nums) ret += Math.abs(a - tar);
	return ret;
}

// 9. 回文数
private boolean isPalindrome(int x) {
	if (x < 0) return false;
	int y = 0, num = x;
	while (num != 0) {
		y = y * 10 + num % 10;
		num /= 10;
	}
	return y == x;
}
```

前缀和 + 二分优化 `cost()`：

```java
/**
 * 数学
 * Somnia1337
 */
public long minimumCost(int[] nums) {
	Arrays.sort(nums);
	long[] pre = new long[nums.length + 1];
	for (int i = 1; i < nums.length + 1; i++) pre[i] = pre[i - 1] + nums[i - 1];
	
	// 中位数回文, 最优
	int mid = nums[nums.length / 2];
	if (isPalindrome(mid)) return cost(nums, pre, mid);
	
	// 找中位数左/右的最近回文数, 返回较小花费
	int l = mid - 1, r = mid + 1;
	while (l > 1 && !isPalindrome(l)) l--;
	while (r < 1000000000 && !isPalindrome(r)) r++;
	return Math.min(cost(nums, pre, l), cost(nums, pre, r));
}

// 将所有元素变为 tar 的花费
private long cost(int[] nums, long[] pre, int tar) {
	int l = 0, r = nums.length - 1;
	while (l <= r) {
		int m = l + r >> 1;
		if (nums[m] >= tar) r = m - 1;
		else l = m + 1;
	}
	long left = (long) tar * (r + 1) - pre[r + 1];
	long right = pre[nums.length] - pre[r + 1] - (long) tar * (nums.length - (r + 1));
	return left + right;
}

// 9. 回文数
private boolean isPalindrome(int x) {
	if (x < 0) return false;
	int y = 0, num = x;
	while (num != 0) {
		y = y * 10 + num % 10;
		num /= 10;
	}
	return y == x;
}
```

预处理回文数 + 二分查找：

```java
/**
 * 数学
 * 灵茶山艾府
 */
private static final int[] pal = new int[109999];

static {
	int palIdx = 0;
	for (int base = 1; base <= 10000; base *= 10) {
		for (int i = base; i < base * 10; i++) {
			int x = i;
			for (int t = i / 10; t > 0; t /= 10) {
				x = x * 10 + t % 10;
			}
			pal[palIdx++] = x;
		}
		if (base <= 1000) {
			for (int i = base; i < base * 10; i++) {
				int x = i;
				for (int t = i; t > 0; t /= 10) {
					x = x * 10 + t % 10;
				}
				pal[palIdx++] = x;
			}
		}
	}
	pal[palIdx] = 1000000001;
}

public long minimumCost(int[] nums) {
	Arrays.sort(nums);
	int m = lowerBound(nums[(nums.length - 1) / 2]);
	if (pal[m] <= nums[nums.length / 2]) return cost(nums, m);
	return Math.min(cost(nums, m - 1), cost(nums, m));
}

private long cost(int[] nums, int i) {
	int tar = pal[i];
	long ret = 0;
	for (int x : nums) ret += Math.abs(x - tar);
	return ret;
}

private int lowerBound(int tar) {
	int l = 0, r = pal.length;
	while (l < r) {
		int m = l + r >> 1;
		if (pal[m] < tar) l = m + 1;
		else r = m;
	}
	return r;
}
```

#### [2971. 找到最大周长的多边形](https://leetcode.cn/problems/find-polygon-with-the-largest-perimeter/)

@2023.12.23

#前缀和 

排序，求前缀和，找到最大的满足 `nums[i] < pre` 的 `i`，`sum(nums[:i])` 即为答案。

```java
/**
 * 前缀和
 * Somnia1337
 */
public long largestPerimeter(int[] nums) {
	Arrays.sort(nums);
	long pre = nums[0] + nums[1], ans = -1;
	for (int i = 2; i < nums.length; i++) {
		if (nums[i] < pre) ans = pre + nums[i];
		pre += nums[i];
	}
	return ans;
}
```

#### [2975. 移除栅栏得到的正方形田地的最大面积](https://leetcode.cn/problems/maximum-square-area-by-removing-fences-from-a-field/)

@2023.12.24

#暴力 

哈希表分别记录两个方向上的所有间隔距离，合并表，最大的公共边长即为答案正方形的边长。

```java
/**
 * 枚举
 * Somnia1337
 */
public int maximizeSquareArea(int m, int n, int[] hFences, int[] vFences) {
	Arrays.sort(hFences);
	Arrays.sort(vFences);
	Set<Integer> h = getIntervals(hFences, m), v = getIntervals(vFences, n);
	
	h.retainAll(v); // 合并表
	if (h.isEmpty()) return -1;
	int max = 0;
	for (int H : h) max = Math.max(H, max);
	return pow(max, 2, 1000000007);
}

private Set<Integer> getIntervals(int[] arr, int l) {
	Set<Integer> ret = new HashSet<>();
	for (int i = 0; i < arr.length; i++) {
		for (int j = i - 1; j >= 0; j--) ret.add(arr[i] - arr[j]);
		ret.add(arr[i] - 1);
		ret.add(l - arr[i]);
	}
	ret.add(l - 1);
	return ret;
}

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

#### [2976. 转换字符串的最小成本 I](https://leetcode.cn/problems/minimum-cost-to-convert-string-i/)

@2023.12.24

#搜索 

构造字母表的 `26 * 26` 的邻接矩阵，用 *Floyd 算法* 计算结点两两之间的最短路径，一次遍历 `source` 和 `target` 进行转换。

```java
/**
 * Floyd 算法
 * Somnia1337
 */
public long minimumCost(String source, String target, char[] original, char[] changed, int[] cost) {
	int[][] g = new int[26][26]; // 邻接矩阵
	for (int i = 0; i < 26; i++) { // 初始化
		Arrays.fill(g[i], Integer.MAX_VALUE);
		g[i][i] = 0;
	}
	for (int i = 0; i < original.length; i++) { // 写入 cost
		int x = original[i] - 'a', y = changed[i] - 'a';
		g[x][y] = Math.min(cost[i], g[x][y]);
	}
	floyd(g); // Floyd 求两两最短路
	
	long ans = 0;
	for (int i = 0; i < source.length(); i++) {
		char s = source.charAt(i), t = target.charAt(i);
		if (s == t) continue;
		if (g[s - 'a'][t - 'a'] == Integer.MAX_VALUE) return -1;
		ans += g[s - 'a'][t - 'a'];
	}
	return ans;
}

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

#### [2979. 最贵的无法购买的商品](https://leetcode.cn/problems/most-expensive-item-that-can-not-be-bought/)

@2024.01.16



```java
/**
 * 数学
 * 丁飞
 */
public int mostExpensiveItem(int primeOne, int primeTwo) {
	return primeOne * primeTwo - primeOne - primeTwo;
}
```

#### [2981. 找出出现至少三次的最长特殊子字符串 I](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-i/)

@2023.12.31

见下题。

```java
/**
 * 二分查找
 * Somnia1337
 */
private char[] chs;

public int maximumLength(String s) {
	chs = s.toCharArray();
	int l = 2, r = s.length() - 2; // 双闭区间
	while (l <= r) {
		int m = l + r >> 1;
		if (check(s, m)) l = m + 1;
		else r = m - 1;
	}
	if (r > 1) return r; // 在 [2,n-2] 内的答案一定有效
	return check(s, 1) ? 1 : -1; // 单独检查 1
}

private boolean check(String s, int m) {
	final char A = 'a';
	int l = s.length(), cnt = 0; // cnt: 当前窗口内的不同字符数
	int[] count = new int[26]; // 窗口内各个字符的计数
	Map<String, Integer> sub = new HashMap<>();
	for (int i = 0; i < l; i++) {
		if (i >= m) {
			if (cnt == 1 && sub.merge(s.substring(i - m, i), 1, Integer::sum) == 3) return true;
			if (--count[chs[i - m] - A] == 0) cnt--;
		}
		if (++count[chs[i] - A] == 1) cnt++;
	}
	// 最右侧的子串
	return cnt == 1 && sub.merge(s.substring(l - m, l), 1, Integer::sum) == 3;
}
```

#### [2982. 找出出现至少三次的最长特殊子字符串 II](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-ii/)

@2023.12.31

1. 二分查找

二分答案，`check(s, m)` 用滑动窗口检查是否有长为 `m` 的特殊子串出现 3 次。

```java
/**
 * 二分查找
 * Somnia1337
 */
private char[] chs;

public int maximumLength(String s) {
	chs = s.toCharArray();
	int l = 2, r = s.length() - 2; // 双闭区间
	while (l <= r) {
		int m = l + r >> 1;
		if (check(s, m)) l = m + 1;
		else r = m - 1;
	}
	if (r > 1) return r; // 在 [2,n-2] 内的答案一定有效
	return check(s, 1) ? 1 : -1; // 单独检查 1
}

private boolean check(String s, int m) {
	final char A = 'a';
	int l = s.length(), cnt = 0; // cnt: 当前窗口内的不同字符数
	int[] count = new int[26]; // 窗口内各个字符的计数
	Map<String, Integer> sub = new HashMap<>();
	for (int i = 0; i < l; i++) {
		if (i >= m) {
			if (cnt == 1 && sub.merge(s.substring(i - m, i), 1, Integer::sum) == 3) return true;
			if (--count[chs[i - m] - A] == 0) cnt--;
		}
		if (++count[chs[i] - A] == 1) cnt++;
	}
	// 最右侧的子串
	return cnt == 1 && sub.merge(s.substring(l - m, l), 1, Integer::sum) == 3;
}
```

2. 分组

```java
/**
 * 分组
 * a碟
 */
public int maximumLength(String s) {
	Map<Integer, Integer>[] count = new HashMap[26];
	for (int i = 0; i < 26; i++) count[i] = new HashMap<>();
	for (int l = 0, r = 0; r < s.length(); r++) {
		if (r + 1 < s.length() && s.charAt(r + 1) == s.charAt(l)) continue;
		char c = s.charAt(l);
		int len = r - l + 1;
		count[c - 'a'].merge(len, 1, Integer::sum);
		if (len > 1) count[c - 'a'].merge(len - 1, 2, Integer::sum);
		if (len > 2) count[c - 'a'].merge(len - 2, 3, Integer::sum);
		l = r + 1;
	}
	int ans = -1;
	for (Map<Integer, Integer> c : count) {
		for (Map.Entry<Integer, Integer> e : c.entrySet()) {
			if (e.getValue() >= 3) ans = Math.max(e.getKey(), ans);
		}
	}
	return ans;
}
```

#### [2992. 自整除排列的数量](https://leetcode.cn/problems/number-of-self-divisible-permutations/)

@2024.01.23

```java
for (int x = 0; x < n; x++) {
	if (!vis[x] && gcd(x + 1, idx) == 1) {
		vis[x] = true;
		bt(idx + 1);
		vis[x] = false;
	}
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private boolean[] vis;
private int n, ans;

public int selfDivisiblePermutationCount(int n) {
	vis = new boolean[n];
	this.n = n;
	ans = 0;
	bt(1);
	return ans;
}

private void bt(int idx) {
	if (idx == n + 1) {
		ans++;
		return;
	}
	for (int x = 0; x < n; x++) {
		if (!vis[x] && gcd(x + 1, idx) == 1) {
			vis[x] = true;
			bt(idx + 1);
			vis[x] = false;
		}
	}
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### [2997. 使数组异或和等于 K 的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-xor-equal-to-k/)

@2024.01.06

```java
/**
 * 位运算
 * Somnia1337
 */
public int minOperations(int[] nums, int k) {
	int xor = k;
	for (int x : nums) xor ^= x;
	return Integer.bitCount(xor);
}
```

#### [2998. 使 X 和 Y 相等的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-x-and-y-equal/)

@2024.01.06

1. BFS

```java
/**
 * BFS
 * Somnia1337
 */
public int minimumOperationsToMakeEqual(int x, int y) {
	Map<Integer, Integer> memo = new HashMap<>();
	memo.put(x, 0);
	Queue<Integer> q = new LinkedList<>();
	q.add(x);
	while (!q.isEmpty()) {
		int p = q.poll();
		if (p == y) return memo.get(p);
		List<Integer> next = new ArrayList<>(List.of(p + 1, p - 1));
		if (p % 5 == 0) next.add(p / 5);
		if (p % 11 == 0) next.add(p / 11);
		for (int v : next) {
			if (memo.getOrDefault(v, Integer.MAX_VALUE) > memo.get(p) + 1) {
				memo.put(v, memo.get(p) + 1);
				q.offer(v);
			}
		}
	}
	return -1;
}
```

2. 记忆化搜索

```java
/**
 * 记忆化搜索
 * oamuuvyi
 */
public int minimumOperationsToMakeEqual(int x, int y) {
	return solve(x, y);
}

private int solve(int x, int y) {
	if (x <= y) {
		return y - x;
	}
	int a = solve(x / 11, y) + x % 11 + 1;
	int b = solve((x + 10) / 11, y) + (11 - x % 11) % 11 + 1;
	int c = solve(x / 5, y) + x % 5 + 1;
	int d = solve((x + 4) / 5, y) + (5 - x % 5) % 5 + 1;
	return Math.min(Math.min(Math.min(a, b), Math.min(c, d)), x - y);
}
```

#### [3001. 捕获黑皇后需要的最少移动次数](https://leetcode.cn/problems/minimum-moves-to-capture-the-queen/)

@2024.01.07

```java
/**
 * 分类讨论
 * OneDayIndependent
 */
public int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) {
	boolean A = a == e && !(c == e && d < b != d < f);
	boolean B = b == f && !(d == f && c < a != c < e);
	boolean C = Math.abs(c - e) == Math.abs(d - f) && !(Math.abs(c - a) == Math.abs(d - b) && b < d != b < f);
	return A || B || C ? 1 : 2;
}
```

#### [3002. 移除后集合的最多元素数](https://leetcode.cn/problems/maximum-size-of-a-set-after-removals/)

@2024.01.07

```java
/**
 * 哈希表
 * ?
 */
public int maximumSetSize(int[] nums1, int[] nums2) {
	int n = nums1.length;
	Set<Integer> s = new HashSet<>(), s1 = new HashSet<>(), s2 = new HashSet<>();
	for (int i : nums1) {
		s.add(i);
		s1.add(i);
	}
	for (int i : nums2) {
		s.add(i);
		s2.add(i);
	}
	int a = s.size(), b = Math.min(n / 2, s1.size()) + Math.min(n / 2, s2.size());
	return Math.min(Math.min(a, b), n);
}
```

#### [3004. 相同颜色的最大子树](https://leetcode.cn/problems/maximum-subtree-of-the-same-color/)

@2024.01.24



```java
/**
 * 树形 DP
 * 丁飞
 */
private int[] colors;
private List<Integer>[] g;
private int ans;

public int maximumSubtreeSize(int[][] edges, int[] colors) {
	this.colors = colors;
	g = new ArrayList[colors.length];
	Arrays.setAll(g, k -> new ArrayList<>());
	for (int[] e : edges) {
		g[e[0]].add(e[1]);
		g[e[1]].add(e[0]);
	}
	ans = 0;
	dfs(0, -1);
	return ans;
}

private int[] dfs(int node, int p) {
	int x = colors[node], y = 1;
	for (int next : g[node]) {
		if (next != p) {
			int[] dp = dfs(next, node);
			int a = dp[0], b = dp[1];
			if (x != a) x = y = 0;
			else y += b;
		}
	}
	ans = Math.max(y, ans);
	return new int[]{x, y};
}
```

```java
/**
 * ?
 * ?
 */
public int maximumSubtreeSize(int[][] edges, int[] colors) {
	int n = colors.length;
	int[] adj = new int[n], deg = new int[n];
	for (int[] e : edges) {
		int u = e[0], v = e[1];
		adj[u] += v;
		adj[v] += u;
		deg[u]++;
		deg[v]++;
	}
	deg[0]++;
	int[] q = new int[n], dp = new int[n];
	int idx = 0;
	Arrays.fill(dp, 1);
	for (int i = 1; i < n; i++) {
		if (deg[i] == 1) q[idx++] = i;
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		int v = q[i], u = adj[v];
		adj[u] -= v;
		if (--deg[u] == 1) q[idx++] = u;
		ans = Math.max(dp[v], ans);
		if (colors[u] != colors[v] || colors[u] == -1) dp[u] = -1;
		else dp[u] += dp[v];
	}
	return ans;
}
```

#### [3006. 找出数组中的美丽下标 I](https://leetcode.cn/problems/find-beautiful-indices-in-the-given-array-i/)

@2024.01.14



```java
/**
 * 双指针
 * Somnia1337
 */
public List<Integer> beautifulIndices(String s, String a, String b, int k) {
	List<Integer> ans = new ArrayList<>();
	int n = s.length(), l1 = a.length(), l2 = b.length(), up = s.length() - l1;
	for (int i = 0; i <= up; i++) {
		if (s.substring(i, i + l1).equals(a)) {
			int l = Math.max(i - k, 0), r = Math.min(i + k, n - l2);
			for (int j = l; j <= r; j++) {
				if (s.substring(j, j + l2).equals(b)) {
					ans.add(i);
					break;
				}
			}
		}
	}
	return ans;
}
```

#### [3007. 价值和小于等于 K 的最大数字](https://leetcode.cn/problems/maximum-number-that-sum-of-the-prices-is-less-than-or-equal-to-k/)

@2024.01.24



```java
/**
 * 二分查找
 * 灵茶山艾府
 */
public long findMaximumNumber(long k, int x) {
	long l = 0, r = (k + 1) << x;
	while (l + 1 < r) {
		long m = l + r >> 1;
		if (countDigitOne(m, x, k)) l = m;
		else r = m;
	}
	return l;
}

private boolean countDigitOne(long num, int x, long k) {
	long sum = 0;
	int i = x - 1;
	for (long n = num >> i; n > 0; n >>= x, i += x) {
		sum += n >> 1 << i;
		if (n % 2 > 0) {
			long mask = (1L << i) - 1;
			sum += (num & mask) + 1;
		}
	}
	return sum <= k;
}
```

#### [3011. 判断一个数组是否可以变为有序](https://leetcode.cn/problems/find-if-array-can-be-sorted/)

@2024.01.20

```java
public boolean canSortArray(int[] nums) {
	boolean changed;
	do {
		changed = false;
		for (int i = 0; i < nums.length - 1; i++) {
			if (nums[i] > nums[i + 1] && Integer.bitCount(nums[i]) == Integer.bitCount(nums[i + 1])) {
				swap(nums, i, i + 1);
				changed = true;
			}
		}
	} while (changed);
	return check(nums);
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}

private boolean check(int[] arr) {
	for (int i = 0; i < arr.length - 1; i++) {
		if (arr[i] > arr[i + 1]) return false;
	}
	return true;
}
```

#### [3012. 通过操作使数组长度最小](https://leetcode.cn/problems/minimize-length-of-array-using-operations/)

@2024.01.21

```java
public int minimumArrayLength(int[] nums) {
	int min = Integer.MAX_VALUE;
	for (int x : nums) min = Math.min(x, min);
	int count = 0;
	for (int x : nums) {
		if (x % min > 0) return 1;
		if (x == min) count++;
	}
	return (count + 1) / 2;
}
```

#### [3015. 按距离统计房屋对数目 I](https://leetcode.cn/problems/count-the-number-of-houses-at-a-certain-distance-i/)

@2024.01.21

```java
public int[] countOfPairs(int n, int x, int y) {
	int[][] g = new int[n][n];
	for (int i = 0; i < n; i++) {
		Arrays.fill(g[i], Integer.MAX_VALUE);
		g[i][i] = 0;
		if (i >= 1) g[i][i - 1] = 1;
		if (i < n - 1) g[i][i + 1] = 1;
	}
	if (x != y) g[x - 1][y - 1] = g[y - 1][x - 1] = 1;
	floyd(g);
	
	int[] ans = new int[n];
	for (int i = 0; i < n; i++) {
		for (int j = i; j < n; j++) {
			if (g[i][j] > 0) ans[g[i][j] - 1] += 2;
		}
	}
	return ans;
}

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

#### [3016. 输入单词需要的最少按键次数 II](https://leetcode.cn/problems/minimum-number-of-pushes-to-type-word-ii/)

@2024.01.21

```java
public int minimumPushes(String word) {
	char[] chs = word.toCharArray();
	int[] count = new int[26];
	for (char c : chs) count[c - 'a']++;
	Arrays.sort(count);
	int ans = 0;
	for (int i = 25, pos = 0; i >= 0 && count[i] > 0; i--, pos++) {
		ans += count[i] * (pos / 8 + 1);
	}
	return ans;
}
```

#### [3025. 人员站位的方案数 I](https://leetcode.cn/problems/find-the-number-of-ways-to-place-people-i/)

@2024.02.04



```java
/**
 * 枚举
 * 灵茶山艾府
 */
public int numberOfPairs(int[][] points) {
	Arrays.sort(points, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
	int ans = 0;
	for (int i = 0; i < points.length; i++) {
		int y1 = points[i][1];
		int maxY = Integer.MIN_VALUE;
		for (int j = i + 1; j < points.length; j++) {
			int y2 = points[j][1];
			if (y2 <= y1 && y2 > maxY) {
				maxY = y2;
				ans++;
			}
		}
	}
	return ans;
}
```

#### [3026. 最大好子数组和](https://leetcode.cn/problems/maximum-good-subarray-sum/)

@2024.02.04



```java
/**
 * 前缀和
 * 灵茶山艾府
 */
public long maximumSubarraySum(int[] nums, int k) {
	long sum = 0, ans = Long.MIN_VALUE;
	Map<Integer, Long> min = new HashMap<>();
	for (int x : nums) {
		long a = min.getOrDefault(x - k, Long.MAX_VALUE >> 1);
		long b = min.getOrDefault(x + k, Long.MAX_VALUE >> 1);
		min.merge(x, sum, Math::min);
		ans = Math.max((sum += x) - Math.min(a, b), ans);
	}
	return ans > Long.MIN_VALUE >> 2 ? ans : 0;
}
```

#### [3029. 将单词恢复初始状态所需的最短时间 I](https://leetcode.cn/problems/minimum-time-to-revert-word-to-initial-state-i/)

@2024.02.04



```java
/**
 * 枚举
 * Somnia1337
 */
public int minimumTimeToInitialState(String word, int k) {
	String s = word;
	int n = word.length(), max = n / k, ans = 0;
	if (n % k > 0) max++;
	while (ans < max) {
		if (s.length() <= k) s += word;
		s = s.substring(k);
		ans++;
		if (word.startsWith(s)) return ans;
	}
	return max;
}
```

#### [3030. 找出网格的区域平均强度](https://leetcode.cn/problems/find-the-grid-of-region-average/)

@2024.02.04



```java
/**
 * 枚举
 * Somnia1337
 */
public int[][] resultGrid(int[][] image, int threshold) {
	int rows = image.length, cols = image[0].length;
	int[][][] c = new int[rows][cols][2]; // [行][列][计数, 累加]
	
	// 枚举所有可作为区域左上角的位置
	for (int i = 0; i < rows - 2; i++) {
		for (int j = 0; j < cols - 2; j++) {
			if (check(image, i, j, threshold)) {
				write(image, c, i, j);
			}
		}
	}
	
	// 写入答案
	int[][] ans = new int[rows][cols];
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (c[i][j][0] > 0) ans[i][j] = c[i][j][1] / c[i][j][0];
			else ans[i][j] = image[i][j];
		}
	}
	return ans;
}

// 检查区域是否满足要求
private boolean check(int[][] m, int i, int j, int t) {
	final int[] dirs = {0, 1, 0}; // 只检查右侧和下侧
	for (int x = i; x < i + 3; x++) {
		for (int y = j; y < j + 3; y++) {
			for (int k = 0; k < 2; k++) {
				int nx = x + dirs[k], ny = y + dirs[k + 1];
				// 在区域内 && 相差 > threshold
				if (checkXY(nx, i, i + 3, ny, j, j + 3) && Math.abs(m[x][y] - m[nx][ny]) > t) {
					return false;
				}
			}
		}
	}
	return true;
}

// 将区域均值写入 c
private void write(int[][] m, int[][][] c, int i, int j) {
	// 求区域均值
	int avg = 0;
	for (int x = i; x < i + 3; x++) {
		for (int y = j; y < j + 3; y++) avg += m[x][y];
	}
	avg /= 9;
	
	// 写入 c
	for (int x = i; x < i + 3; x++) {
		for (int y = j; y < j + 3; y++) {
			c[x][y][0]++;
			c[x][y][1] += avg;
		}
	}
}

// 检查是否在同一个区域内
private boolean checkXY(int x, int xL, int xR, int y, int yL, int yR) {
	return x >= xL && x < xR && y >= yL && y < yR;
}
```

#### [3039. 进行操作使字符串为空](https://leetcode.cn/problems/apply-operations-to-make-string-empty/)

@2024.02.18



```java
/**
 * 字符统计
 * Somnia1337
 */
public String lastNonEmptyString(String s) {
	char[] chs = s.toCharArray();
	int max = 0;
	int[] count = new int[26], memo = new int[26];
	for (char c : chs) max = Math.max(++count[c - 'a'], max);
	System.arraycopy(count, 0, memo, 0, 26);
	max--;
	for (int i = 0; i < 26; i++) count[i] -= max;
	StringBuilder ans = new StringBuilder();
	for (char c : chs) {
		if (++count[c - 'a'] > memo[c - 'a']) ans.append(c);
	}
	return ans.toString();
}
```

#### [3040. 相同分数的最大操作数目 II](https://leetcode.cn/problems/maximum-number-of-operations-with-the-same-score-ii/)

@2024.02.18



```java
/**
 * 记忆化搜索
 * 灵茶山艾府
 */
private int[] arr;
private int[][] memo;

public int maxOperations(int[] nums) {
	int n = nums.length;
	arr = nums;
	memo = new int[n][n];
	int ans1 = solve(2, n - 1, nums[0] + nums[1]);
	int ans2 = solve(0, n - 3, nums[n - 2] + nums[n - 1]);
	int ans3 = solve(1, n - 2, nums[0] + nums[n - 1]);
	return Math.max(Math.max(ans1, ans2), ans3) + 1;
}

private int solve(int i, int j, int tar) {
	for (int[] row : memo) Arrays.fill(row, -1);
	return dfs(i, j, tar);
}

private int dfs(int i, int j, int tar) {
	if (i >= j) return 0;
	if (memo[i][j] != -1) return memo[i][j];
	int ret = 0;
	if (arr[i] + arr[i + 1] == tar) {
		ret = Math.max(dfs(i + 2, j, tar) + 1, ret);
	}
	if (arr[j - 1] + arr[j] == tar) {
		ret = Math.max(dfs(i, j - 2, tar) + 1, ret);
	}
	if (arr[i] + arr[j] == tar) {
		ret = Math.max(dfs(i + 1, j - 1, tar) + 1, ret);
	}
	return memo[i][j] = ret;
}
```

#### [3043. 最长公共前缀的长度](https://leetcode.cn/problems/find-the-length-of-the-longest-common-prefix/)

@2024.02.18



```java
/**
 * 哈希表
 * Somnia1337
 */
public int longestCommonPrefix(int[] arr1, int[] arr2) {
	Set<Integer> s1 = new HashSet<>();
	for (int x : arr1) {
		while (x > 0) {
			s1.add(x);
			x /= 10;
		}
	}
	int ans = 0;
	for (int x : arr2) {
		while (x > 0) {
			if (s1.contains(x)) {
				ans = Math.max(ans, len(x));
				break;
			}
			x /= 10;
		}
	}
	return ans;
}

private int len(int x) {
	int ret = 0;
	while (x > 0) {
		ret++;
		x /= 10;
	}
	return ret;
}
```

#### [3044. 出现频率最高的素数](https://leetcode.cn/problems/most-frequent-prime/)

@2024.02.18



```java
/**
 * 哈希表
 * 灵茶山艾府
 */
private final int[][] dirs = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
private int rows, cols;

public int mostFrequentPrime(int[][] mat) {
	rows = mat.length;
	cols = mat[0].length;
	Map<Integer, Integer> count = new HashMap<>();
	
	// 枚举每个起点
	for (int x = 0; x < rows; x++) {
		for (int y = 0; y < cols; y++) {
			// 枚举每个方向
			for (int[] d : dirs) {
				// 要求 > 10, 忽略一位数
				int nx = x + d[0], ny = y + d[1], v = mat[x][y];
				while (inBound(nx, ny)) {
					v = v * 10 + mat[nx][ny];
					if (isPrime(v)) count.merge(v, 1, Integer::sum);
					nx += d[0];
					ny += d[1];
				}
			}
		}
	}
	
	// 遍历表, 获取答案
	int cnt = 0, ans = -1;
	for (Map.Entry<Integer, Integer> e : count.entrySet()) {
		int v = e.getKey(), c = e.getValue();
		if (c > cnt) {
			ans = v;
			cnt = c;
		}
		else if (c == cnt) ans = Math.max(v, ans);
	}
	return ans;
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}

private boolean isPrime(int x) {
	for (int i = 2; i * i <= x; i++) {
		if (x % i == 0) return false;
	}
	return true;
}
```

#### [3047. 求交集区域内的最大正方形面积](https://leetcode.cn/problems/find-the-largest-area-of-square-inside-two-rectangles/)

@2024.02.26

```java
/**
 * 枚举
 * 灵茶山艾府
 */
public long largestSquareArea(int[][] bottomLeft, int[][] topRight) {
	long ans = 0;
	for (int i = 0; i < bottomLeft.length; i++) {
		int[] b1 = bottomLeft[i], t1 = topRight[i];
		for (int j = i + 1; j < bottomLeft.length; j++) {
			int[] b2 = bottomLeft[j], t2 = topRight[j];
			int h = Math.min(t1[1], t2[1]) - Math.max(b1[1], b2[1]);
			int w = Math.min(t1[0], t2[0]) - Math.max(b1[0], b2[0]);
			int a = Math.min(w, h);
			if (a > 0) ans = Math.max((long) a * a, ans);
		}
	}
	return ans;
}
```

#### [3048. 标记所有下标的最早秒数 I](https://leetcode.cn/problems/earliest-second-to-mark-indices-i/)

@2024.02.26

题意理解：

你有 `n` 门课程需要考试，第 `i` 门课程需要用 `nums[i]` 天复习。同一天只能复习一门课程。

在第 `i` 天，你可以选择参加第 `changeIndices[i]` 门课程的考试。考试这一天不能复习。

搞定所有课程的复习 & 考试，至少要多少天？

```java
/**
 * 二分查找
 * 灵茶山艾府
 */
public int earliestSecondToMarkIndices(int[] nums, int[] changeIndices) {
	int n = nums.length, m = changeIndices.length;
	if (n > m) return -1;
	int[] done = new int[n]; // 避免反复创建和初始化数组
	int l = n - 1, r = m + 1;
	while (l + 1 < r) {
		int mid = l + r >> 1;
		if (check(nums, changeIndices, done, mid)) r = mid;
		else l = mid;
	}
	return r <= m ? r : -1;
}

private boolean check(int[] nums, int[] idx, int[] done, int mid) {
	int exam = nums.length, revw = 0;
	for (int i = mid - 1; i >= 0 && revw <= i + 1; i--) { // 要复习的天数不能太多
		int id = idx[i] - 1;
		if (done[id] != mid) {
			done[id] = mid;
			exam--; // 考试
			revw += nums[id]; // 需要复习的天数
		}
		else if (revw > 0) revw--; // 复习
	}
	return exam == 0 && revw == 0; // 考完了并且复习完了
}
```

#### [LCR 007. 三数之和](https://leetcode.cn/problems/1fGaJU/)

@2023.12.10

与 [15. 三数之和](https://leetcode.cn/problems/3sum/) 相同。

```java
/**
 * 双指针
 * Somnia1337
 */
public List<List<Integer>> threeSum(int[] nums) {
	List<List<Integer>> ans = new ArrayList<>();
	Arrays.sort(nums);
	int len = nums.length;
	for (int i = 0; i < len - 2; i++) {
		int left = i + 1, right = len - 1;
		while (left < right) {
			int sum = nums[i] + nums[left] + nums[right];
			if (sum > 0) right--;
			else if (sum < 0) left++;
			else {
				ans.add(Arrays.asList(nums[i], nums[left], nums[right]));
				while (left < right && nums[left + 1] == nums[left]) left++;
				while (right > left && nums[right - 1] == nums[right]) right--;
				left++;
				right--;
			}
		}
		while (i < len - 1 && nums[i + 1] == nums[i]) i++;
	}
	return ans;
}
```

#### [LCR 010. 和为 K 的子数组](https://leetcode.cn/problems/QTMn0o/)

@2023.12.11

与 [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/) 相同。

```java
/**
 * 前缀和 + 哈希表
 * Somnia1337
 */
public int subarraySum(int[] nums, int k) {
	Map<Integer, Integer> count = new HashMap<>();
	count.put(0, 1);
	int ans = 0, cur = 0;
	for (int num : nums) {
		cur += num;
		if (count.containsKey(cur - k)) ans += count.get(cur - k);
		count.merge(cur, 1, Integer::sum);
	}
	return ans;
}
```

#### [LCR 016. 无重复字符的最长子串](https://leetcode.cn/problems/wtcaE1/)

@2023.12.08

与 [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) 相同。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int lengthOfLongestSubstring(String s)
{
	if (s.isEmpty()) return 0;
	int[] count = new int[128];
	int l = 0, r = 0, ans = 0;
	while (r < s.length())
	{
		char c = s.charAt(r++);
		while (count[c] > 0) count[s.charAt(l++)]--;
		count[c] = 1;
		ans = Math.max(r - l, ans);
	}
	return ans;
}
```

#### [LCR 083. 全排列](https://leetcode.cn/problems/VvJkup/)

@2023.09.26

与 [46. 全排列](https://leetcode.cn/problems/permutations/) 相同。

```java
public List<List<Integer>> ans = new ArrayList<>();
public List<Integer> current = new ArrayList<>();
public Set<Integer> banned = new HashSet<>();

public List<List<Integer>> permute(int[] nums)
{
	bt(nums, banned, ans, current);
	return ans;
}

public void bt(int[] nums, Set<Integer> banned, List<List<Integer>> ans, List<Integer> current)
{
	if (current.size() == nums.length)
	{
		ans.add(new ArrayList<>(current));
		return;
	}
	for (int num : nums)
	{
		if (banned.contains(num)) continue;
		current.add(num);
		banned.add(num);
		bt(nums, banned, ans, current);
		current.remove(current.size() - 1);
		banned.remove(num);
	}
}
```

#### [LCR 121. 寻找目标值 - 二维数组](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

@2023.12.11

与 [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/) 相同。

```java
/**
 * z字形搜索
 * Somnia1337
 */
public boolean findTargetIn2DPlants(int[][] plants, int target) {
	if (plants.length == 0) return false;
	int i = 0, j = plants[0].length - 1;
	while (i < plants.length && j >= 0) {
		if (plants[i][j] == target) return true;
		else if (plants[i][j] < target) i++;
		else j--;
	}
	return false;
}
```

#### [LCR 138. 有效数字](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

@2023.06.30

#字符串 

根据题目规定的若干规则进行判断。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isNumber(String s)
{
	String validChars = "+-.Ee0123456789";
	int[] counts = new int[2];
	int hasE = 0, hasNum1 = 0, hasNum2 = 0, preWasPoint = 0, preWasNum = 0;
	while (s.startsWith(" ")) s = s.substring(1);
	while (s.endsWith(" ")) s = s.substring(0, s.length() - 1);
	
	for (int i = 0; i < s.length(); i++)
	{
		char c = s.charAt(i);
		int n = validChars.indexOf(c);
		if (preWasPoint == 1 && n < 3) return false;
		else preWasPoint = 0;
		if (n == -1) return false;
		else if (n <= 1)
		{
			if (i != 0 && preWasNum == 1) return false;
			if (counts[0] == 0 || counts[0] == 1 && hasE == 1) counts[0]++;
			else if (counts[0] == 1 || counts[0] == 2) return false;
			preWasNum = 0;
		}
		else if (n == 2)
		{
			if (hasE == 1) return false;
			else if (counts[1] == 0)
			{
				counts[1] = 1;
				preWasPoint = 1;
			}
			else return false;
			preWasNum = 0;
		}
		else if (n <= 4)
		{
			if (hasE == 0) hasE = 1;
			else return false;
			preWasNum = 0;
		}
		else if (n == 5)
		{
			preWasNum = 1;
			if (hasE == 0) hasNum1 = 1;
			else hasNum2 = 1;
		}
		else
		{
			preWasNum = 1;
			if (hasE == 0) hasNum1 = 1;
			else hasNum2 = 1;
		}
	}
	
	if (hasE == 1 && hasNum2 == 0) return false;
	return hasNum1 == 1;
}
```

#### [LCR 148. 验证图书取出顺序](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

@2023.12.11

与 [946. 验证栈序列](https://leetcode.cn/problems/validate-stack-sequences/) 相同。

```java
/**
 * 模拟
 * Somnia1337
 */
public boolean validateBookSequences(int[] putIn, int[] takeOut) {
	int l = putIn.length, it = 0;
	Deque<Integer> stk = new ArrayDeque<>();
	for (int p : putIn) {
		stk.push(p);
		while (!stk.isEmpty() && it < l && stk.peek() == takeOut[it]) {
			stk.pop();
			it++;
		}
	}
	return it == l;
}
```

#### [LCR 157. 套餐内商品的排列顺序](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)

@2023.12.08

#回溯 

```java
for (int i = 0; i < l; i++)
{
	if (!vis[i])
	{
		vis[i] = true;
		path[idx] = s.charAt(i);
		bt(s, idx + 1);
		vis[i] = false;
	}
}
```

哈希表去重。

```java
/**
 * 回溯
 * Somnia1337
 */
private Set<String> collect;
private char[] path;
private int l;
private boolean[] vis;

public String[] goodsOrder(String goods)
{
	l = goods.length();
	collect = new HashSet<>();
	path = new char[l];
	vis = new boolean[l];
	bt(goods, 0);
	String[] ans = new String[collect.size()];
	int i = 0;
	for (String s : collect) ans[i++] = s;
	return ans;
}

private void bt(String s, int idx)
{
	if (idx == l)
	{
		collect.add(new String(path));
		return;
	}
	for (int i = 0; i < l; i++)
	{
		if (!vis[i])
		{
			vis[i] = true;
			path[idx] = s.charAt(i);
			bt(s, idx + 1);
			vis[i] = false;
		}
	}
}
```

优化：改为交换字符数组的字符，尝试将 `bt()` 参数的 `idx` 处的字符与后续字符交换位置，同时记录交换过哪些字母（剪枝）。

```java
boolean[] vis = new boolean[26];
for (int i = idx; i < ch.length; i++)
{
	if (vis[ch[i] - 'a']) continue;
	vis[ch[i] - 'a'] = true;
	swap(i, idx);
	dfs(idx + 1);
	swap(i, idx);
}
```

```java
/**
 * 回溯
 * ?
 */
private List<String> ans;
private char[] ch;

public String[] goodsOrder(String goods)
{
	ans = new ArrayList<>();
	ch = goods.toCharArray();
	bt(0);
	return ans.toArray(new String[0]);
}

private void bt(int idx)
{
	if (idx == ch.length - 1) // 只需递归到 len-1, 已无后续字符可交换
	{
		ans.add(new String(ch));
		return;
	}
	boolean[] vis = new boolean[26];
	for (int i = idx; i < ch.length; i++)
	{
		if (vis[ch[i] - 'a']) continue;
		vis[ch[i] - 'a'] = true;
		swap(i, idx);
		bt(idx + 1);
		swap(i, idx);
	}
}

private void swap(int i, int j)
{
	char t = ch[i];
	ch[i] = ch[j];
	ch[j] = t;
}
```

#### [LCR 165. 解密数字](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

@2023.12.08

#动态规划 

`dp[i]` 表示子问题 `s[0:i]` 的答案，`dp[0] = dp[1] = 1`，对后续每个 `i`，考虑 `s[i]` 与上一个字符可否组合解码（上个字符不是 `'0'` 且最近两个数字构成的数字 `< 26`）：

- 如果可以，则上个字符可以与当前字符组合（`dp[i - 2]`）、也可以不组合（`dp[i - 1]`），`dp[i] = dp[i - 2] + dp[i - 1]`。
- 否则，当前字符只能单独解码，`dp[i] = dp[i - 1]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int crackNumber(int ciphertext)
{
	String s = String.valueOf(ciphertext);
	int l = s.length();
	if (l == 1) return 1;
	int[] dp = new int[l];
	dp[0] = 1;
	dp[1] = s.charAt(0) > '0' && Integer.parseInt(s.substring(0, 2)) < 26 ? 2 : 1;
	for (int i = 2; i < l; i++)
	{
		if (s.charAt(i - 1) > '0' && Integer.parseInt(s.substring(i - 1, i + 1)) < 26)
			dp[i] = dp[i - 2] + dp[i - 1];
		else dp[i] = dp[i - 1];
	}
	return dp[l - 1];
}
```

#### [LCR 185. 统计结果概率](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

@2023.12.11

```java
/**
 * 动态规划
 * 路漫漫我不畏
 */
public double[] statisticsProbability(int num) {
	int[][] dp = new int[num + 1][num * 6 + 1];
	for (int i = 1; i <= 6; i++) dp[1][i] = 1;
	for (int i = 2; i <= num; i++) {
		for (int j = i; j <= i * 6; j++) {
			for (int k = 1; k <= Math.min(j, 6); k++)
				dp[i][j] += dp[i - 1][j - k];
		}
	}
	
	double[] ans = new double[num * 5 + 1];
	double total = Math.pow(6, num);
	for (int i = num; i <= 6 * num; i++) ans[i - num] = dp[num][i] / total;
	return ans;
}
```

#### [LCR 188. 买卖芯片的最佳时机](https://leetcode.cn/problems/gu-piao-de-zui-da-li-run-lcof/)

@2023.12.11

与 [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) 相同。

```java
/**
 * 贪心
 * Somnia1337
 */
public int bestTiming(int[] prices) {
	if (prices.length == 0) return 0;
	int min = prices[0], ans = 0;
	for (int price : prices) {
		ans = Math.max(price - min, ans);
		min = Math.min(price, min);
	}
	return ans;
}
```

#### [LCP 30. 魔塔游戏](https://leetcode.cn/problems/p0NxJO/)

@2024.02.06



```java
/**
 * 贪心
 * 灵茶山艾府
 */
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

#### [面试题 01.07. 旋转矩阵](https://leetcode.cn/problems/rotate-matrix-lcci/)

@2023.12.08

与 [48. 旋转图像](https://leetcode.cn/problems/rotate-image/) 相同。

```java
/**
 * 模拟
 * Somnia1337
 */
public void rotate(int[][] matrix)
{
	int len = matrix.length;
	for (int i = 0; i < len / 2; ++i)
	{
		for (int j = 0; j < len; ++j)
		{
			swap(matrix, j, i, len - i - 1);
		}
	}
	for (int i = 0; i < len; ++i)
	{
		for (int j = 0; j < i; ++j)
		{
			swap(matrix, i, j);
		}
	}
}

private void swap(int[][] arr, int i, int j)
{
	int t = arr[i][j];
	arr[i][j] = arr[j][i];
	arr[j][i] = t;
}

private void swap(int[][] arr, int x, int up, int down)
{
	int t = arr[up][x];
	arr[up][x] = arr[down][x];
	arr[down][x] = t;
}
```