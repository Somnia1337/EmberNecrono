# 🏆 周赛

### [# 361](https://leetcode.cn/contest/weekly-contest-361/)

@2023.09.03

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 2289/4170 54.9% | 4 | 0:18:51 +5 | 2289 0:18:51 | -4->1572 |

#### [2843. 统计对称整数的数目](https://leetcode.cn/problems/count-symmetric-integers/)

```java
/**
 * 枚举
 * Somnia1337
 */
public int countSymmetricIntegers(int low, int high) {
	int ans = 0;
	for (int x = low; x <= high; x++) {
		char[] digits = String.valueOf(x).toCharArray();
		int l = digits.length, half = 0;
		if ((l & 1) == 1) continue;
		for (int i = 0; i < l / 2; i++) half += digits[i] - digits[l - i - 1];
		if (half == 0) ans++;
	}
	return ans;
}
```

#### [2844. 生成特殊数字的最少操作](https://leetcode.cn/problems/minimum-operations-to-make-a-special-number/)

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

#### [2846. 边权重均等查询](https://leetcode.cn/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/)

### [# 362](https://leetcode.cn/contest/weekly-contest-362/)

@2023.09.10

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 2380/4800 49.6% | 7 | 1:10:53 +10 | 1051 0:09:21 | +5->1577 |

#### [2848. 与车相交的点](https://leetcode.cn/problems/points-that-intersect-with-cars/)

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int numberOfPoints(List<List<Integer>> nums) {
	Set<Integer> v = new HashSet<>();
	for (List<Integer> row : nums) {
		for (int i = row.get(0); i <= row.get(1); i++) v.add(i);
	}
	return v.size();
}
```

2. 差分数组

```java
/**
 * 差分数组
 * 雪景式
 */
public int numberOfPoints(List<List<Integer>> nums) {
	int[] dif = new int[102];
	for (List<Integer> row : nums) {
		dif[row.get(0)]++;
		dif[row.get(1) + 1]--;
	}
	int ans = 0;
	for (int i = 1; i < 102; i++) {
		dif[i] += dif[i - 1];
		if (dif[i] > 0) ans++;
	}
	return ans;
}
```

#### [2849. 判断能否在给定时间到达单元格](https://leetcode.cn/problems/determine-if-a-cell-is-reachable-at-a-given-time/)

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

```java
/**
 * 回溯
 * JavaLongestLang
 */
public int minimumMoves(int[][] grid)
{
	return dfs(grid, 0, 0);
}

private int dfs(int[][] grid, int x, int y)
{
	if (x >= 3) return 0;
	if (y >= 3) return dfs(grid, x + 1, 0);
	if (grid[x][y] != 0) return dfs(grid, x, y + 1);
	
	int ans = Integer.MAX_VALUE;
	for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 3; j++)
		{
			if (i == x && j == y) continue;
			if (grid[i][j] <= 1) continue;
			grid[i][j] -= 1;
			ans = Math.min(ans, dfs(grid, x, y + 1) + Math.abs(i - x) + Math.abs(j - y));
			grid[i][j] += 1;
		}
	}
	
	return ans;
}
```

#### [2851. 字符串转换](https://leetcode.cn/problems/string-transformation/)

### [# 363](https://leetcode.cn/contest/weekly-contest-363/)

@2023.09.17

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1285/4768 27.0% | 7 | 0:20:25 +5 | 1125 0:05:43 | +39->1630 |

#### [2859. 计算 K 置位下标对应元素的和](https://leetcode.cn/problems/sum-of-values-at-indices-with-k-set-bits/)

```java
/**
 * 库函数
 * Somnia1337
 */
public int sumIndicesWithKSetBits(List<Integer> nums, int k) {
	int ans = 0;
	for (int i = 0; i < nums.size(); i++) {
		if (Integer.bitCount(i) == k) ans += nums.get(i);
	}
	return ans;
}
```

#### [2860. 让所有学生保持开心的分组方法数](https://leetcode.cn/problems/happy-students/)

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

#### [2862. 完全子集的最大元素和](https://leetcode.cn/problems/maximum-element-sum-of-a-complete-subset-of-indices/)

```java
/**
 * 质因子分解
 * 珂朵莉
 */
public long maximumSum(List<Integer> nums)
{
	Map<Integer, Long> map = new HashMap<>();
	for (int i = 0; i < nums.size(); i++)
	{
		int k = solve(i + 1);
		map.merge(k, (long) nums.get(i), Long::sum);
	}
	
	return map.values()
			  .stream()
			  .mapToLong(Long::valueOf)
			  .max()
			  .getAsLong();
}

int solve(int v)
{
	int res = 1;
	for (int i = 2; i <= v / i; i++)
	{
		if (v % i == 0)
		{
			int cnt = 0;
			while (v % i == 0)
			{
				v /= i;
				cnt++;
			}
			if (cnt % 2 == 1) res *= i;
		}
	}
	if (v > 1) res *= v;
	return res;
}
```

### [# 364](https://leetcode.cn/contest/weekly-contest-364/)

@2023.09.24

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 974/4304 22.7% | 7 | 0:10:07 +0 | 909 0:06:59 | 不计入 |

#### [2864. 最大二进制奇数](https://leetcode.cn/problems/maximum-odd-binary-number/)

```java
/**
 * 字符统计
 * Somnia1337
 */
public String maximumOddBinaryNumber(String s)
{
	int count = 0;
	for (int i = 0; i < s.length(); i++)
	{
		if (s.charAt(i) == '1') count++;
	}
	return "1".repeat(count - 1) + "0".repeat(s.length() - count) + "1";
}
```

#### [2865. 美丽塔 I](https://leetcode.cn/problems/beautiful-towers-i/)

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

public long solve(List<Integer> maxHeights, int index)
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
	//以i为山顶时左侧/右侧的最大值
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

#### [2867. 统计树中的合法路径数目](https://leetcode.cn/problems/count-valid-paths-in-a-tree/)

### [# 365](https://leetcode.cn/contest/weekly-contest-365/)

@2023.10.01

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1440/2909 49.6% | 7 | 1:23:21 +35 | 1091 0:04:04 | -5->1635 |

#### [2873. 有序三元组中的最大值 I](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-i/)

暴力枚举。

```java
/**
 * 枚举
 * Somnia1337
 */
public long maximumTripletValue(int[] nums)
{
	int len = nums.length;
	long ans = 0;
	for (int i = 0; i < len - 2; i++)
	{
		for (int j = i + 1; j < len - 1; j++)
		{
			for (int k = j + 1; k < len; k++)
			{
				ans = Math.max((long) (nums[i] - nums[j]) * nums[k], ans);
			}
		}
	}
	return ans;
}
```

#### [2874. 有序三元组中的最大值 II](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-ii/)

```java
/**
 * 枚举
 * Somnia1337
 */
public long maximumTripletValue(int[] nums)
{
	int len = nums.length;
	long ans = 0;
	int max = nums[0];
	int[] sufMax = Arrays.copyOf(nums, len);
	for (int i = len - 2; i >= 0; i--)
	{
		sufMax[i] = Math.max(sufMax[i + 1], sufMax[i]);
	}
	for (int i = 0; i < len - 1; i++)
	{
		if (nums[i] >= max) max = nums[i];
		else ans = Math.max((long) (max - nums[i]) * sufMax[i + 1], ans);
	}
	return ans;
}
```

#### [2875. 无限数组的最短子数组](https://leetcode.cn/problems/minimum-size-subarray-in-infinite-array/)

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

#### [2876. 有向图访问计数](https://leetcode.cn/problems/count-visited-nodes-in-a-directed-graph/)

### [# 366](https://leetcode.cn/contest/weekly-contest-366/)

@2023.10.08

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 584/2790 21.0% | 7 | 0:06:56 +0 | 534 0:02:41 | +48->1683 |

#### [2894. 分类求和并作差](https://leetcode.cn/problems/divisible-and-non-divisible-sums-difference/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int differenceOfSums(int n, int m)
{
	int num1 = 0, num2 = 0;
	for (int i = 1; i <= n; i++)
	{
		if (i % m != 0) num1 += i;
		else num2 += i;
	}
	return num1 - num2;
}
```

#### [2895. 最小处理时间](https://leetcode.cn/problems/minimum-processing-time/)

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

#### [2897. 对数组执行操作使平方和最大](https://leetcode.cn/problems/apply-operations-on-array-to-maximize-sum-of-squares/)

```java
/**
 * 位运算
 * Somnia1337
 */
public int maxSum(List<Integer> nums, int k)
{
	final int MOD = (int) (Math.pow(10, 9) + 7);
	int[] count = new int[30];
	for (int num : nums)
	{
		for (int i = 0; i < 30; i++)
		{
			count[i] += num >> i & 1;
		}
	}
	long ans = 0;
	for (int i = 0; i < k; i++)
	{
		int x = 0;
		for (int j = 0; j < 30; j++)
		{
			if (count[j] > 0)
			{
				count[j]--;
				x |= 1 << j;
			}
		}
		ans += (long) Math.pow(x, 2);
		ans %= MOD;
	}
	return (int) ans;
}
```

### [# 367](https://leetcode.cn/contest/weekly-contest-367/)

@2023.10.15

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 526/4317 12.2% | 17 | 1:36:08 +30 | 1 0:09:45 | +53->1752 |

#### [2903. 找出满足差值条件的下标 I](https://leetcode.cn/problems/find-indices-with-index-and-value-difference-i/)

枚举每一对 `i`，`j`。

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] findIndices(int[] nums, int indexDifference, int valueDifference)
{
	int len = nums.length;
	for (int i = 0; i < len - indexDifference; i++)
	{
		for (int j = i + indexDifference; j < len; j++)
		{
			if (Math.abs(nums[j] - nums[i]) >= valueDifference) return new int[]{i, j};
		}
	}
	return new int[]{-1, -1};
}
```

#### [2904. 最短且字典序最小的美丽子字符串](https://leetcode.cn/problems/shortest-and-lexicographically-smallest-beautiful-string/)

求前缀和，`preSum[i]` 表示前 `i` 个字符中 `'1'` 的数量。

维护最小长度与起点，枚举终点与可能的起点，满足条件时更新最小长度与起点。

```java
/**
 * 枚举
 * Somnia1337
 */
public String shortestBeautifulSubstring(String s, int k)
{
	if (k == 1 && s.contains("1")) return "1";
	int len = s.length();
	char[] chars = s.toCharArray();
	
	// 求前缀和
	int[] preSum = new int[len + 1];
	preSum[0] = 0; // 前0个元素没有1
	for (int i = 1; i < len + 1; i++)
	{
		preSum[i] = preSum[i - 1] + chars[i - 1] - '0';
	}
	
	int minL = len + 1, start = -1;
	for (int i = 1; i < len + 1; i++) // 枚举终点
	{
		for (int j = Math.max(0, i - minL); j < i; j++) // 枚举起点
		{
			// 更新最小长度与起点
			// 条件：(1的个数为k)&(start未更新|i-j<minL|新串字典序小于旧串)
			if (preSum[i] - preSum[j] == k && (start == -1 || minL > i - j || s.substring(j, j + minL).compareTo(s.substring(start,start + minL)) < 0))
			{
				minL = i - j;
				start = j;
			}
		}
	}
	
	return start >= 0 ? s.substring(start, start + minL) : "";
}
```

#### [2905. 找出满足差值条件的下标 II](https://leetcode.cn/problems/find-indices-with-index-and-value-difference-ii/)

维护最大值、最小值所在下标 `maxIdx`、`minIdx`，从 `indexDifference` 开始枚举 `j`，用 `j - indexDifference` 更新 `maxIdx`、`minIdx`，然后检查 `j` 处元素与最大元素、最小元素的差值是否超过 `valueDifference`。

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] findIndices(int[] nums, int indexDifference, int valueDifference)
{
	int len = nums.length;
	int maxIdx = 0, minIdx = 0;
	for (int i = indexDifference; i < len; i++)
	{
		int idx = i - indexDifference;
		if (nums[idx] > nums[maxIdx]) maxIdx = idx;
		if (nums[idx] < nums[minIdx]) minIdx = idx;
		if (Math.abs(nums[i] - nums[maxIdx]) >= valueDifference) return new int[]{maxIdx, i};
		if (Math.abs(nums[i] - nums[minIdx]) >= valueDifference) return new int[]{minIdx, i};
	}
	return new int[]{-1, -1};
}
```

#### [2906. 构造乘积矩阵](https://leetcode.cn/problems/construct-product-matrix/)

将 `grid` 展开成一维，对其求每个元素除自身以外的乘积，再复原成二维。

核心部分调用 [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/) 即可。

```java
/**
 * 前缀积
 * Somnia1337
 */
public int[][] constructProductMatrix(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	long[] flat = new long[rows * cols];
	for (int i = 0; i < rows * cols; i++)
	{
		flat[i] = grid[i / cols][i % cols]; // 血泪史！！！
	}
	
	long[] pros = productExceptSelf(flat);
	int[][] ans = new int[rows][cols];
	for (int i = 0; i < rows * cols; i++)
	{
		ans[i / cols][i % cols] = (int) pros[i]; // 血泪史！！！
	}
	
	return ans;
}

public long[] productExceptSelf(long[] nums)
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

### [# 368](https://leetcode.cn/contest/weekly-contest-368/)

@2023.10.22

| 竞赛分数 |
| :--: |
| -54->1698 |

#### [2908. 元素和最小的山形三元组 I](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-i/)

同下一题。

```java
/**
 * 前后缀分解
 * 陈梁
 */
public int minimumSum(int[] nums)
{
	int len = nums.length;
	int[] lMin = new int[len];
	int[] rMin = new int[len];
	
	lMin[0] = nums[0];
	for (int i = 1; i < len; i++)
	{
		lMin[i] = Math.min(lMin[i - 1], nums[i]);
	}
	rMin[len - 1] = nums[len - 1];
	for (int i = len - 2; i >= 0; i--)
	{
		rMin[i] = Math.min(rMin[i + 1], nums[i]);
	}
	
	int ans = Integer.MAX_VALUE;
	for (int j = 1; j < len - 1; j++)
	{
		if (nums[j] > lMin[j] && nums[j] > rMin[j]) ans = Math.min(lMin[j] + nums[j] + rMin[j], ans);
	}
	
	return ans < Integer.MAX_VALUE ? ans : -1;
}
```

#### [2909. 元素和最小的山形三元组 II](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-ii/)

记录每个位置及左侧、及右侧的最小值，遍历，满足 `nums[j] > lMin[j]` 且 `nums[j] > rMin[j]` 的 `j` 可作为山峰。

```java
/**
 * 前后缀分解
 * 陈梁
 */
public int minimumSum(int[] nums)
{
	int len = nums.length;
	int[] lMin = new int[len];
	int[] rMin = new int[len];
	
	lMin[0] = nums[0];
	for (int i = 1; i < len; i++)
	{
		lMin[i] = Math.min(lMin[i - 1], nums[i]);
	}
	rMin[len - 1] = nums[len - 1];
	for (int i = len - 2; i >= 0; i--)
	{
		rMin[i] = Math.min(rMin[i + 1], nums[i]);
	}
	
	int ans = Integer.MAX_VALUE;
	for (int j = 1; j < len - 1; j++)
	{
		if (nums[j] > lMin[j] && nums[j] > rMin[j]) ans = Math.min(lMin[j] + nums[j] + rMin[j], ans);
	}
	
	return ans < Integer.MAX_VALUE ? ans : -1;
}
```

#### [2910. 合法分组的最少组数](https://leetcode.cn/problems/minimum-number-of-groups-to-create-a-valid-assignment/)

#### [2911. 得到 K 个半回文串的最少修改次数](https://leetcode.cn/problems/minimum-changes-to-make-k-semi-palindromes/)

### [# 369](https://leetcode.cn/contest/weekly-contest-369/)

@2023.10.29

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 878/4121 21.4% | 7 | 0:14:15 +0 | 761 0:05:21 | +31->1719 |

#### [2917. 找出数组中的 K-or 值](https://leetcode.cn/problems/find-the-k-or-of-an-array/)

```java
/**
 * 位运算
 * Somnia1337
 */
public int findKOr(int[] nums, int k) {
	int ans = 0;
	for (int i = 0; i <= 31; i++) {
		int cnt = 0;
		for (int x : nums) {
			if ((x & (1 << i)) == 1 << i) cnt++;
		}
		if (cnt >= k) ans |= 1 << i;
	}
	return ans;
}
```

#### [2918. 数组的最小相等和](https://leetcode.cn/problems/minimum-equal-sum-of-two-arrays-after-replacing-zeros/)

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

#### [2919. 使数组变美的最小增量运算数](https://leetcode.cn/problems/minimum-increment-operations-to-make-array-beautiful/)

#### [2920. 收集所有金币可获得的最大积分](https://leetcode.cn/problems/maximum-points-after-collecting-coins-from-all-nodes/)

### [# 370](https://leetcode.cn/contest/weekly-contest-370/)

@2023.11.05

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1124/3983 28.3% | 7 | 0:07:56 +0 | 1055 0:01:50 | +14->1733 |

#### [2923. 找到冠军 I](https://leetcode.cn/problems/find-champion-i/)

标记较弱队，遍历查找未被标记的队。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findChampion(int[][] grid)
{
	int n = grid.length;
	boolean[] ban = new boolean[n];
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (i == j) continue;
			if (grid[i][j] == 1) ban[j] = true;
			else ban[i] = true;
		}
	}
	for (int i = 0; i < n; i++)
	{
		if (!ban[i]) return i;
	}
	return -1;
}
```

对每行的 `0` 计数，冠军所在行 `k` 应满足除第 `k` 位为 `1` 外，其余均为 `0`。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findChampion(int[][] grid)
{
	int n = grid.length;
	for (int i = 0; i < n; i++)
	{
		int cnt = 0;
		for (int j = 0; j < n; j++)
		{
			if (grid[i][j] == 0)
			{
				cnt++;
				if (cnt == 2) break;
			}
		}
		if (cnt == 1) return i;
	}
	return -1;
}
```

#### [2924. 找到冠军 II](https://leetcode.cn/problems/find-champion-ii/)

标记较弱队，遍历查找未被标记的队，如果有一个则返回，如果多于一个则返回 `-1`。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findChampion(int n, int[][] edges)
{
	boolean[] ban = new boolean[n];
	for (int[] edge : edges)
	{
		ban[edge[1]] = true;
	}
	int ans = -1;
	for (int i = 0; i < n; i++)
	{
		if (!ban[i])
		{
			if (ans == -1) ans = i;
			else return -1;
		}
	}
	return ans;
}
```

#### [2925. 在树上执行操作以后得到的最大分数](https://leetcode.cn/problems/maximum-score-after-applying-operations-on-a-tree/)

#### [2926. 平衡子序列的最大和](https://leetcode.cn/problems/maximum-balanced-subsequence-sum/)

### [# 372](https://leetcode.cn/contest/weekly-contest-372/)

@2023.11.19

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1234/3920 31.5% | 7 | 0:22:10 +15 | 870 0:04:28 | +16->1749 |

#### [2937. 使三个字符串相等](https://leetcode.cn/problems/make-three-strings-equal/)

一次遍历，求三串的最长公共前缀长度，需要删除的字符数即长度和减去最长公共前缀长 \* 3。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findMinimumOperations(String s1, String s2, String s3)
{
	int l1 = s1.length(), l2 = s2.length(), l3 = s3.length();
	int l = Math.min(l1, Math.min(l2, l3)), it = 0;
	while (it < l && s1.charAt(it) == s2.charAt(it) && s2.charAt(it) == s3.charAt(it)) it++;
	return it > 0 ? l1 + l2 + l3 - it * 3 : -1;
}
```

#### [2938. 区分黑球与白球](https://leetcode.cn/problems/separate-black-and-white-balls/)

1. 两次遍历

第一次统计黑球，第二次累加移动黑球所需的距离。

```java
/**
 * 两次遍历
 * Somnia1337
 */
public long minimumSteps(String s)
{
	int l = s.length(), b = 0;
	for (int i = 0; i < l; i++)
	{
		if (s.charAt(i) == '1') b++;
	}
	long ans = 0;
	for (int i = 0; i < l; i++)
	{
		if (s.charAt(i) == '1')
		{
			ans += l - b - i;
			b--;
		}
	}
	return ans;
}
```

2. 一次遍历

遍历统计黑球，如果为白球，累加将其移至左侧的距离，即当前的黑球数。

```java
/**
 * 一次遍历
 * 灵茶山艾府
 */
public long minimumSteps(String s)
{
	long ans = 0;
	int cnt1 = 0;
	for (char c : s.toCharArray())
	{
		if (c == '1') cnt1++;
		else ans += cnt1;
	}
	return ans;
}
```

#### [2939. 最大异或乘积](https://leetcode.cn/problems/maximum-xor-product/)

#### [2940. 找到 Alice 和 Bob 可以相遇的建筑](https://leetcode.cn/problems/find-building-where-alice-and-bob-can-meet/)

### [# 374](https://leetcode.cn/contest/weekly-contest-374/)

@2023.12.03

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 990/4053 24.5% | 3 | 0:01:18 +0 | 948 0:00:38 | -5->1744 |

#### [2951. 找出峰值](https://leetcode.cn/problems/find-the-peaks/)

```java
/**
 * 枚举
 * Somnia1337
 */
public List<Integer> findPeaks(int[] mountain)
{
	List<Integer> ans = new ArrayList<>();
	for (int i = 1; i < mountain.length - 1; i++)
	{
		if (mountain[i] > mountain[i - 1] && mountain[i] > mountain[i + 1]) ans.add(i);
	}
	return ans;
}
```

#### [2952. 需要添加的硬币的最小数量](https://leetcode.cn/problems/minimum-number-of-coins-to-be-added/)

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

#### [2953. 统计完全子字符串](https://leetcode.cn/problems/count-complete-substrings/)

#### [2954. 统计感冒序列的数目](https://leetcode.cn/problems/count-the-number-of-infection-sequences/)

### [# 375](https://leetcode.cn/contest/weekly-contest-375/)

@2023.12.10

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 777/3518 22.1% | 18 | 1:32:17 +20 | 1 0:08:17 | +27->1803 |

#### [2960. 统计已测试设备](https://leetcode.cn/problems/count-tested-devices-after-test-operations/)

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public int countTestedDevices(int[] batteryPercentages) {
	int ans = 0, len = batteryPercentages.length;
	for (int i = 0; i < len; i++) {
		if (batteryPercentages[i] > 0) {
			ans++;
			for (int j = i + 1; j < len; j++) {
				batteryPercentages[j] = Math.max(batteryPercentages[j] - 1, 0);
			}
		}
	}
	return ans;
}
```

2. 脑筋急转弯

每到一个新设备时，`ans` 本身记录了之前测试的设备计数，也即本设备电量减少的次数，因此只有初始电量 `> ans` 才可测试本设备。

```java
/**
 * 脑筋急转弯
 * 灵茶山艾府
 */
public int countTestedDevices(int[] batteryPercentages) {
	int ans = 0;
	for (int b : batteryPercentages) {
		if (b > ans) ans++;
	}
	return ans;
}
```

#### [2961. 双模幂运算](https://leetcode.cn/problems/double-modular-exponentiation/)

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

#### [2963. 统计好分割方案的数目](https://leetcode.cn/problems/count-the-number-of-good-partitions/)

考虑不加限制条件时的总方案数：每两个元素间的空位都可以加隔板，$n$ 个元素则有 $2^{n - 1}$ 种方案。

加限制条件相当于禁用其中的一些位置，其不能有隔板。假设某元素重复出现的位置列表为 `[p0,...,pk]`，则 `p0` 到 `pk` 间的所有位置都不能有隔板。

`boolean[n - 1]` 标记每个位置是否被禁用，哈希表记录 `<元素, 下标[最左, 最右]>`，对于重复出现的元素，禁用其出现的两端之间的所有位置，最后计数没有被禁用的位置，用带模快速幂计算 $2^{cnt}$。

```java
/**
 * 快速幂
 * Somnia1337
 */
public int numberOfGoodPartitions(int[] nums) {
	boolean[] ban = new boolean[nums.length - 1]; // 禁用的位置
	Map<Integer, int[]> pos = new HashMap<>(); // <元素, 下标[最左, 最右]>
	for (int i = 0; i < nums.length; i++) {
		if (pos.containsKey(nums[i])) pos.get(nums[i])[1] = i; // 重复出现, 最右位置 i
		else pos.put(nums[i], new int[]{i, -1}); // 首次出现, 最左位置 i
	}
	for (int[] p : pos.values()) {
		if (p[1] > 0) { // 重复出现
			Arrays.fill(ban, p[0], p[1], true); // 禁用两端之间的位置
		}
	}
	int cnt = 0; // 未禁用的位置计数
	for (boolean b : ban) if (!b) cnt++;
	return pow(2, cnt, 1000000007); // 带模快速幂
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

可以用区间合并的思想（本质与隔板法无异），只记录每个元素的最右端位置，再次遍历，更新当前最右端位置，如果 `max == i`，说明此位置可放隔板。

```java
/**
 * 区间合并
 * 灵茶山艾府
 */
public int numberOfGoodPartitions(int[] nums) {
	Map<Integer, Integer> rpos = new HashMap<>();
	for (int i = 0; i < nums.length; i++) {
		rpos.put(nums[i], i);
	}
	int cnt = 0, max = 0;
	for (int i = 0; i < nums.length - 1; i++) { // 不考虑最后一段区间
		max = Math.max(rpos.get(nums[i]), max); // 更新最右端
		if (max == i) cnt++; // 当前即最右端, 可放隔板
	}
	return pow(2, cnt, 1000000007);
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

### [# 376](https://leetcode.cn/contest/weekly-contest-376/)

@2023.12.17

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 231/3409 6.8% | 12 | 0:29:32 +5 | 217 0:06:40 | +69->1872 |

#### [2965. 找出缺失和重复的数字](https://leetcode.cn/problems/find-missing-and-repeated-values/)

辅助数组计数。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[] findMissingAndRepeatedValues(int[][] grid) {
	int n = grid.length;
	int[] count = new int[n * n + 1];
	for (int[] row : grid) {
		for (int x : row) count[x]++;
	}
	int[] ans = new int[2];
	for (int x = 1; x < n * n + 1; x++) {
		if (count[x] == 2) ans[0] = x;
		else if (count[x] == 0) ans[1] = x;
	}
	return ans;
}
```

#### [2966. 划分数组并满足最大差限制](https://leetcode.cn/problems/divide-array-into-arrays-with-max-difference/)

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

#### [2968. 执行操作使频率分数最大](https://leetcode.cn/problems/apply-operations-to-maximize-frequency-score/)

排序，答案对应的操作结果一定是某个子数组全部变为其自身的中位数。同向双指针，枚举右端点，在当前花费 `> k` 时移动左端点，更新答案。计算花费需用前缀和优化。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int maxFrequencyScore(int[] nums, long k) {
	Arrays.sort(nums);
	long[] pre = new long[nums.length + 1];
	for (int i = 1; i < nums.length + 1; i++) pre[i] = pre[i - 1] + nums[i - 1];
	int ans = 1;
	for (int l = 0, r = 1; r < nums.length; r++) {
		while (cost(nums, pre, l, r) > k) l++;
		ans = Math.max(r - l + 1, ans);
	}
	return ans;
}

private long cost(int[] nums, long[] pre, int l, int r) {
	int m = l + r >> 1, mid = nums[m];
	long left = (long) mid * (m - l) - (pre[m] - pre[l]);
	long right = pre[r + 1] - pre[m + 1] - (long) mid * (r - m);
	return left + right;
}
```

### [# 377](https://leetcode.cn/contest/weekly-contest-377/)

@2023.12.24

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 880/3148 28.0% | 8 | 0:53:55 +5 | 819 0:15:07 |  |

#### [2974. 最小数字游戏](https://leetcode.cn/problems/minimum-number-game/)

```java
/**
 * 排序
 * Somnia1337
 */
public int[] numberGame(int[] nums) {
	Arrays.sort(nums);
	int[] ans = new int[nums.length];
	for (int i = 0; i < nums.length; i += 2) {
		ans[i] = nums[i + 1];
		ans[i + 1] = nums[i];
	}
	return ans;
}
```

#### [2975. 移除栅栏得到的正方形田地的最大面积](https://leetcode.cn/problems/maximum-square-area-by-removing-fences-from-a-field/)

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

#### [2977. 转换字符串的最小成本 II](https://leetcode.cn/problems/minimum-cost-to-convert-string-ii/)

### [# 378](https://leetcode.cn/contest/weekly-contest-378/)

@2023.12.31

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 387/2747 14.1% | 12 | 0:37:05 +20 | 83 0:04:35 | +29->1932 |

#### [2980. 检查按位或是否存在尾随零](https://leetcode.cn/problems/check-if-bitwise-or-has-trailing-zeros/)

`a | b` 有尾随 `0` $\Rightarrow$ `a` 和 `b` 的末位都是 `0`。越多元素按位或，末尾为 `1` 的机会越多。

计数偶数，需要至少 2 个偶数。

```java
/**
 * 数学
 * Somnia1337
 */
public boolean hasTrailingZeros(int[] nums) {
	int cnt = 0;
	for (int x : nums) {
		if ((x & 1) == 0 && ++cnt == 2) return true;
	}
	return false;
}
```

#### [2981. 找出出现至少三次的最长特殊子字符串 I](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-i/)

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

#### [2983. 回文串重新排列查询](https://leetcode.cn/problems/palindrome-rearrangement-queries/)

```java
/**
 * 模拟
 * Frosty KowalevskiBGe
 */
private char[] chs;
private Set<Integer> diff;

public boolean[] canMakePalindromeQueries(String s, int[][] queries) {
	chs = s.toCharArray();
	diff = new HashSet<>();
	for (int i = 0, j = chs.length - 1; i < j; i++, j--) {
		if (chs[i] != chs[j]) diff.add(i);
	}
	boolean[] ans = new boolean[queries.length];
	for (int i = 0; i < ans.length; i++) ans[i] = check(queries[i]);
	return ans;
}

private boolean check(int[] q) {
	final char A = 'a';
	int[] lAc = new int[26], rAc = new int[26], lEx = new int[26], rEx = new int[26];
	for (int i = q[0], j = chs.length - q[0] - 1; i <= q[1]; i++, j--) {
		if (in(j, q[2], q[3])) {
			lAc[chs[i] - A]++;
			rAc[chs[j] - A]++;
		}
	}
	for (int i : diff) {
		int j = chs.length - i - 1;
		if (!in(i, q[0], q[1])) {
			if (in(j, q[2], q[3])) {
				rEx[chs[i] - A]++;
				rAc[chs[j] - A]++;
			}
			else if (chs[i] != chs[j]) return false;
		}
		else if (!in(j, q[2], q[3])) {
			lAc[chs[i] - A]++;
			lEx[chs[j] - A]++;
		}
	}
	for (int i = 0; i < 26; i++) {
		if ((lAc[i] -= lEx[i]) < 0 || (rAc[i] -= rEx[i]) < 0 || lAc[i] != rAc[i]) return false;
	}
	return true;
}

private boolean in(int i, int l, int r) {
	return i >= l && i <= r;
}
```

### [# 379](https://leetcode.cn/contest/weekly-contest-379/)

@2024.01.07

未参加

#### [3000. 对角线最长的矩形的面积](https://leetcode.cn/problems/maximum-area-of-longest-diagonal-rectangle/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int areaOfMaxDiagonal(int[][] dimensions) {
	int max = 0, ans = 0;
	for (int[] d : dimensions) {
		int l = d[0] * d[0] + d[1] * d[1];
		if (l > max) {
			ans = d[0] * d[1];
			max = l;
		}
		else if (l == max) ans = Math.max(d[0] * d[1], ans);
	}
	return ans;
}
```

#### [3001. 捕获黑皇后需要的最少移动次数](https://leetcode.cn/problems/minimum-moves-to-capture-the-queen/)

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

#### [3003. 执行操作后的最大分割数量](https://leetcode.cn/problems/maximize-the-number-of-partitions-after-operations/)

```java
/**
 * 前后缀分解
 * 灵茶山艾府
 */
private int seg = 1, mask = 0, size = 0;

public int maxPartitionsAfterOperations(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[] sufSeg = new int[n + 1], sufMask = new int[n + 1];
	for (int i = n - 1; i >= 0; i--) {
		update(chs[i], k);
		sufSeg[i] = seg;
		sufMask[i] = mask;
	}

	int ans = seg; // 不修改任何字母
	seg = 1;
	mask = 0;
	size = 0;
	for (int i = 0; i < n; i++) {
		int cur = seg + sufSeg[i + 1]; // 情况 3
		int unionN = Integer.bitCount(mask | sufMask[i + 1]);
		if (unionN < k) cur--; // 情况 1
		else if (unionN < 26 && size == k && Integer.bitCount(sufMask[i + 1]) == k) cur++; // 情况 2
		ans = Math.max(cur, ans);
		update(chs[i], k);
	}
	return ans;
}

private void update(char c, int k) {
	int bit = 1 << (c - 'a');
	if ((mask & bit) != 0) return;
	if (++size > k) {
		seg++;
		mask = bit;
		size = 1;
	}
	else mask |= bit;
}
```

### [# 380](https://leetcode.cn/contest/weekly-contest-380/)

@2024.01.14

未参加

#### [3005. 最大频率元素计数](https://leetcode.cn/problems/count-elements-with-maximum-frequency/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int maxFrequencyElements(int[] nums) {
	int[] count = new int[101];
	int max = 0;
	for (int x : nums) max = Math.max(++count[x], max);
	int ans = 0;
	for (int cnt : count) {
		if (cnt == max) ans += cnt;
	}
	return ans;
}
```

#### [3006. 找出数组中的美丽下标 I](https://leetcode.cn/problems/find-beautiful-indices-in-the-given-array-i/)

```java
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

#### [3008. 找出数组中的美丽下标 II](https://leetcode.cn/problems/find-beautiful-indices-in-the-given-array-ii/)

### [# 381](https://leetcode.cn/contest/weekly-contest-381/)

@2024.01.21

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 969/3737 26.0% | 12 | 0:57:14 +20 | 127 0:07:14 | -3->1919 |

#### [3014. 输入单词需要的最少按键次数 I](https://leetcode.cn/problems/minimum-number-of-pushes-to-type-word-i/)

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

#### [3015. 按距离统计房屋对数目 I](https://leetcode.cn/problems/count-the-number-of-houses-at-a-certain-distance-i/)

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

#### [3017. 按距离统计房屋对数目 II](https://leetcode.cn/problems/count-the-number-of-houses-at-a-certain-distance-ii/)

### [# 382](https://leetcode.cn/contest/weekly-contest-382/)

@2024.01.28

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 88/3134 2.9% | 11 | 0:19:49 +5 | 55 0:10:29 | +93->2012 |

#### [3014. 按键变更的次数](https://leetcode.cn/problems/number-of-changing-keys/)



```java
/**
 * 一次遍历
 * Somnia1337
 */
public int countKeyChanges(String s) {
	char[] chs = s.toLowerCase().toCharArray();
	int ans = 0;
	for (int i = 1; i < chs.length; i++) {
		if (chs[i] != chs[i - 1]) ans++;
	}
	return ans;
}
```

#### [3015. 子集中元素的最大数量](https://leetcode.cn/problems/find-the-maximum-number-of-elements-in-subset/)



```java
/**
 * 哈希表
 * Somnia1337
 */
public int maximumLength(int[] nums) {
	Map<Integer, Integer> count = new HashMap<>();
	for (int x : nums) count.merge(x, 1, Integer::sum);
	int ans1 = count.getOrDefault(1, 0), ans2 = 0;
	if ((ans1 & 1) == 0) ans1--;
	for (Map.Entry<Integer, Integer> e : count.entrySet()) {
		int x = e.getKey();
		if (x > 1 && e.getValue() > 1) {
			int cur = 2;
			boolean centered = false;
			for (int y = x * x; ; y *= y) {
				int cnt = count.getOrDefault(y, 0);
				if (cnt == 0) break;
				if (cnt == 1) {
					cur++;
					centered = true;
					break;
				}
				cur += 2;
			}
			if (!centered) cur--;
			ans2 = Math.max(cur, ans2);
		}
	}
	return Math.max(Math.max(ans1, ans2), 1);
}
```

#### [3016. Alice 和 Bob 玩鲜花游戏](https://leetcode.cn/problems/alice-and-bob-playing-flower-game/)

@2024.01.28



```java
/**
 * 数学
 * Somnia1337
 */
public long flowerGame(int n, int m) {
	long x1 = n + 1 >> 1, x2 = n - x1;
	long y1 = m + 1 >> 1, y2 = m - y1;
	return x1 * y2 + x2 * y1;
}
```

#### [3017. 给定操作次数内使剩余元素的或值最小](https://leetcode.cn/problems/minimize-or-of-remaining-elements-using-operations/)

### [# 383](https://leetcode.cn/contest/weekly-contest-383/)

@2024.02.04

未参加

#### [3028. 边界上的蚂蚁](https://leetcode.cn/problems/ant-on-the-boundary/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int returnToBoundaryCount(int[] nums) {
	int pos = 0, ans = 0;
	for (int x : nums) {
		if ((pos += x) == 0) ans++;
	}
	return ans;
}
```

#### [3029. 将单词恢复初始状态所需的最短时间 I](https://leetcode.cn/problems/minimum-time-to-revert-word-to-initial-state-i/)

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

#### [3031. 将单词恢复初始状态所需的最短时间 II](https://leetcode.cn/problems/minimum-time-to-revert-word-to-initial-state-ii/)

```java
/**
 * KMP
 * 灵茶山艾府
 */
public int minimumTimeToInitialState(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length, l = 0, r = 0;
	int[] z = new int[n];
	for (int i = 1; i < n; i++) {
		if (i <= r) z[i] = Math.min(z[i - l], r - i + 1);
		while (i + z[i] < n && chs[z[i]] == chs[z[i] + i]) {
			l = i;
			r = i + z[i];
			z[i]++;
		}
		if (i % k == 0 && z[i] >= n - i) return i / k;
	}
	return (n - 1) / k + 1;
}
```

### [# 384](https://leetcode.cn/contest/weekly-contest-384/)

@2024.02.11

未参加

#### [3033. 修改矩阵](https://leetcode.cn/problems/modify-the-matrix/)

```java
/**
 * 按列遍历
 * 灵茶山艾府
 */
public int[][] modifiedMatrix(int[][] matrix) {
	for (int j = 0; j < matrix[0].length; j++) {
		int max = 0;
		for (int[] row : matrix) max = Math.max(row[j], max);
		for (int[] row : matrix) {
			if (row[j] == -1) row[j] = max;
		}
	}
	return matrix;
}
```

#### [3034. 匹配模式数组的子数组数目 I](https://leetcode.cn/problems/number-of-subarrays-that-match-a-pattern-i/)

#### [3035. 回文字符串的最大数量](https://leetcode.cn/problems/maximum-palindromes-after-operations/)

```java
public int maxPalindromesAfterOperations(String[] words) {
	int n = words.length;
	int[] count = new int[26], lens = new int[n];
	for (int i = 0; i < n; i++) {
		for (char c : words[i].toCharArray()) count[c - 'a']++;
		lens[i] = words[i].length();
	}
	Arrays.sort(lens);
	for (int i = 0; i < n; i++) {
		if (!check(count, lens[i])) return i;
	}
	return n;
}

private boolean check(int[] c, int l) {
	Arrays.sort(c);
	int cnt = 0, tar = l / 2;
	for (int x : c) cnt += x / 2;
	if (cnt < tar) return false;
	for (int i = 25; i >= 0 && tar > 0; i--) {
		int rem = Math.min(c[i] / 2 * 2, tar * 2);
		c[i] -= rem;
		tar -= rem / 2;
	}
	if ((l & 1) == 1) {
		boolean del = false;
		for (int i = 0; i < 26; i++) {
			if ((c[i] & 1) == 1) {
				c[i]--;
				del = true;
				break;
			}
		}
		if (!del) c[25]--;
	}
	return true;
}
```

#### [3036. 匹配模式数组的子数组数目 II](https://leetcode.cn/problems/number-of-subarrays-that-match-a-pattern-ii/)

### [# 385](https://leetcode.cn/contest/weekly-contest-385/)

@2024.02.18

未参加

#### [3042. 统计前后缀下标对 I](https://leetcode.cn/problems/count-prefix-and-suffix-pairs-i/)

```java
/**
 * 枚举
 * Somnia1337
 */
public int countPrefixSuffixPairs(String[] words) {
	int ans = 0;
	for (int i = 0; i < words.length; i++) {
		for (int j = i + 1; j < words.length; j++) {
			if (words[j].startsWith(words[i]) && words[j].endsWith(words[i])) ans++;
		}
	}
	return ans;
}
```

#### [3043. 最长公共前缀的长度](https://leetcode.cn/problems/find-the-length-of-the-longest-common-prefix/)

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

#### [3045. 统计前后缀下标对 II](https://leetcode.cn/problems/count-prefix-and-suffix-pairs-ii/)

### [# 386](https://leetcode.cn/contest/weekly-contest-386/)

@2024.02.25

未参加

#### [3046. 分割数组](https://leetcode.cn/problems/split-the-array/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isPossibleToSplit(int[] nums) {
	int[] count = new int[101];
	for (int x : nums) {
		if (++count[x] >= 3) return false;
	}
	return true;
}
```

#### [3047. 求交集区域内的最大正方形面积](https://leetcode.cn/problems/find-the-largest-area-of-square-inside-two-rectangles/)

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

#### [3049. 标记所有下标的最早秒数 II](https://leetcode.cn/problems/earliest-second-to-mark-indices-ii/)

### [# 387](https://leetcode.cn/contest/weekly-contest-387/)

@2024.03.03

未参加

#### [100243. 将元素分配到两个数组中 I](https://leetcode.cn/problems/distribute-elements-into-two-arrays-i/)



```java
/**
 * 模拟
 * Somnia1337
 */
public int[] resultArray(int[] nums) {
	int n = nums.length;
	int[] a1 = new int[n], a2 = new int[n];
	a1[0] = nums[0];
	a2[0] = nums[1];
	int i = 0, j = 0;
	for (int k = 2; k < n; k++) {
		if (a1[i] > a2[j]) a1[++i] = nums[k];
		else a2[++j] = nums[k];
	}
	
	int[] ans = new int[n];
	int k = 0;
	i = 0;
	while (a1[i] > 0) ans[k++] = a1[i++];
	j = 0;
	while (a2[j] > 0) ans[k++] = a2[j++];
	return ans;
}
```

#### [100237. 元素和小于等于 k 的子矩阵的数目](https://leetcode.cn/problems/count-submatrices-with-top-left-element-and-sum-less-than-k/)



```java
/**
 * 前缀和 + 枚举
 * Somnia1337
 */
public int countSubmatrices(int[][] grid, int k) {
	if (grid[0][0] > k) return 0;
	int rows = grid.length, cols = grid[0].length;
	long[][] pre = new long[rows][cols];
	for (int i = 0; i < rows; i++) {
		long[] row = pre[i];
		row[0] = grid[i][0];
		for (int j = 1; j < cols; j++) {
			row[j] = row[j - 1] + grid[i][j];
		}
	}
	
	int ans = 0;
	for (int c = 0; c < cols; c++) {
		long sum = 0;
		for (int r = 0; r < rows; r++) {
			sum += pre[r][c];
			if (sum <= k) ans++;
			else break;
		}
	}
	return ans;
}
```

```java
/**
 * 二维前缀和
 * 灵茶山艾府
 */
public int countSubmatrices(int[][] grid, int k) {
	int rows = grid.length, cols = grid[0].length, ans = 0;
	int[][] pre = new int[rows + 1][cols + 1];
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j];
			if (pre[i + 1][j + 1] <= k) ans++;
		}
	}
	return ans;
}
```

#### [100234. 在矩阵上写出字母 Y 所需的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-write-the-letter-y-on-a-grid/)



```java
/**
 * 枚举
 * Somnia1337
 */
public int minimumOperationsToWriteY(int[][] grid) {
	int n = grid.length;
	int[] yCnt = new int[3], oCnt = new int[3];
	for (int i = 0; i < n / 2; i++) {
		yCnt[grid[i][i]]++;
		yCnt[grid[i][n - i - 1]]++;
		grid[i][i] = grid[i][n - i - 1] = -1;
	}
	for (int m = n / 2, i = m; i < n; i++) {
		yCnt[grid[i][m]]++;
		grid[i][m] = -1;
	}
	for (int[] row : grid) {
		for (int j = 0; j < n; j++) {
			if (row[j] >= 0) oCnt[row[j]]++;
		}
	}
	
	int ySum = 0, oSum;
	for (int cnt : yCnt) ySum += cnt;
	oSum = n * n - ySum;
	
	int ans = Integer.MAX_VALUE;
	for (int y = 0; y <= 2; y++) {
		int yCost = ySum - yCnt[y], oCost = Integer.MAX_VALUE;
		for (int o = 0; o <= 2; o++) {
			if (o == y) continue;
			oCost = Math.min(oSum - oCnt[o], oCost);
		}
		ans = Math.min(yCost + oCost, ans);
	}
	return ans;
}
```

#### [100246. 将元素分配到两个数组中 II](https://leetcode.cn/problems/distribute-elements-into-two-arrays-ii/)

```java
/**
 * 二分查找
 * 15066212pp
 */
public int[] resultArray(int[] nums) {
	List<Integer> a1 = new ArrayList<>(), a2 = new ArrayList<>();
	a1.add(nums[0]);
	a2.add(nums[1]);
	List<Integer> sa1 = new ArrayList<>(), sa2 = new ArrayList<>();
	sa1.add(nums[0]);
	sa2.add(nums[1]);
	int k = 2;
	while (k < nums.length) {
		if (greaterCount(sa1, nums[k]) > greaterCount(sa2, nums[k])) {
			insert(sa1, nums[k]);
			a1.add(nums[k++]);
		}
		else if (greaterCount(sa1, nums[k]) < greaterCount(sa2, nums[k])) {
			insert(sa2, nums[k]);
			a2.add(nums[k++]);
		}
		else {
			if (a1.size() <= a2.size()) {
				insert(sa1, nums[k]);
				a1.add(nums[k++]);
			}
			else {
				insert(sa2, nums[k]);
				a2.add(nums[k++]);
			}
		}
	}
	int[] ans = new int[a1.size() + a2.size()];
	k = 0;
	for (int x : a1) ans[k++] = x;
	for (int x : a2) ans[k++] = x;
	return ans;
}

private void insert(List<Integer> a, int val) {
	int l = -1, r = a.size();
	while (l + 1 < r) {
		int m = l + r >> 1;
		if (a.get(m) >= val) r = m;
		else l = m;
	}
	a.add(r, val);
}

private int greaterCount(List<Integer> a, int val) {
	int l = -1, r = a.size();
	while (l + 1 < r) {
		int m = l + r >> 1;
		if (a.get(m) > val) r = m;
		else l = m;
	}
	return a.size() - r;
}
```

```python
"""
模拟
TsReaper
"""
class Solution:
    def resultArray(self, nums: List[int]) -> List[int]:
        def greaterCount(arr, val):
            return len(arr) - arr.bisect_right(val)
		
        arr1, arr2 = SortedList([nums[0]]), SortedList([nums[1]])
        res1, res2 = [nums[0]], [nums[1]]
        for i in range(2, len(nums)):
            c1, c2 = greaterCount(arr1, nums[i]), greaterCount(arr2, nums[i])
            if c1 > c2 or (c1 == c2 and len(arr1) <= len(arr2)):
                arr1.add(nums[i])
                res1.append(nums[i])
            else:
                arr2.add(nums[i])
                res2.append(nums[i])
        return res1 + res2
```

### [# 388](https://leetcode.cn/contest/weekly-contest-388/)

@2024.03.10

未参加

### [# 389](https://leetcode.cn/contest/weekly-contest-389/)

@2024.03.17

未参加

#### [100248. 字符串及其反转中是否存在同一子字符串](https://leetcode.cn/problems/existence-of-a-substring-in-a-string-and-its-reverse/)

暴力，哈希表记录。

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isSubstringPresent(String s) {
	Set<String> vis = new HashSet<>();
	int n = s.length();
	for (int i = 0; i < n - 1; i++) vis.add(s.substring(i, i + 2));
	s = rev(s);
	for (int i = 0; i < n - 1; i++) {
		if (vis.contains(s.substring(i, i + 2))) return true;
	}
	return false;
}

private String rev(String s) {
	StringBuilder ret = new StringBuilder(s);
	return ret.reverse().toString();
}
```

一次遍历：

```java
/**
 * 一次遍历
 * 灵茶山艾府
 */
public boolean isSubstringPresent(String s) {
	char[] chs = s.toCharArray();
	boolean[][] vis = new boolean[26][26];
	for (int i = 1; i < chs.length; i++) {
		int x = chs[i - 1] - 'a', y = chs[i] - 'a';
		vis[x][y] = true;
		if (vis[y][x]) return true;
	}
	return false;
}
```

#### [100236. 统计以给定字符开头和结尾的子字符串总数](https://leetcode.cn/problems/count-substrings-starting-and-ending-with-given-character/)

每个 `c` 都能与自己以及所有左侧的 `c` 组成一个符合要求的字符串，计数，公式求和。

```java
/**
 * 数学
 * Somnia1337
 */
public long countSubstrings(String s, char c) {
	long cnt = 0;
	for (char ch : s.toCharArray()) {
		if (ch == c) cnt++;
	}
	return cnt * (cnt + 1) / 2;
}
```

#### [100255. 成为 K 特殊字符串需要删除的最少字符数](https://leetcode.cn/problems/minimum-deletions-to-make-string-k-special/)



```java
/**
 * 正难则反
 * 灵茶山艾府
 */
public int minimumDeletions(String word, int k) {
	int[] count = new int[26];
	for (char c : word.toCharArray()) count[c - 'a']++;
	Arrays.sort(count);
	int save = 0;
	for (int i = 0; i < 26; i++) {
		int sum = 0;
		for (int j = i; j < 26; j++) {
			sum += Math.min(count[j], count[i] + k);
		}
		save = Math.max(sum, save);
	}
	return word.length() - save;
}
```

#### [100227. 拾起 K 个 1 需要的最少行动次数](https://leetcode.cn/problems/minimum-moves-to-pick-k-ones/)

# 🐱 双周赛

### [# 112](https://leetcode.cn/contest/biweekly-contest-112/)

@2023.09.02

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1121/2900 38.7% | 12 | 0:50:37 +15 | 573 0:07:10 | +1576->1576 |

#### [2839. 判断通过操作能否让字符串相等 I](https://leetcode.cn/problems/check-if-strings-can-be-made-equal-with-operations-i/)

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean canBeEqual(String s1, String s2) {
	int l = s1.length();
	if (l != s2.length()) return false;
	char[] chs1 = s1.toCharArray();
	for (int i = 0; i < l; i++) {
		if (chs1[i] != s2.charAt(i)) {
			if (i + 2 >= l || chs1[i + 2] != s2.charAt(i)) return false;
			swap(chs1, i, i + 2);
		}
	}
	return true;
}

private void swap(char[] arr, int i, int j) {
	char t = arr[i];
	arr[i] = arr[j];
	arr[j] = t;
}
```

#### [2840. 判断通过操作能否让字符串相等 II](https://leetcode.cn/problems/check-if-strings-can-be-made-equal-with-operations-ii/)

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

#### [2842. 统计一个字符串的 k 子序列美丽值最大的数目](https://leetcode.cn/problems/count-k-subsequences-of-a-string-with-maximum-beauty/)

### [# 113](https://leetcode.cn/contest/biweekly-contest-113/)

@2023.09.16

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1206/3028 39.9% | 3 | 0:07:23 +0 | 1137 0:00:29 | +14->1591 |

#### [2855. 使数组成为递增数组的最少右移次数](https://leetcode.cn/problems/minimum-right-shifts-to-sort-the-array/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int minimumRightShifts(List<Integer> nums)
{
	int len = nums.size();
	int lastNum = nums.get(0), start = -1;
	boolean valid = true;
	
	for (int i = 1; i < len; i++)
	{
		int num = nums.get(i);
		if (num < lastNum)
		{
			start = i;
			valid = false;
			lastNum = num;
			break;
		}
		lastNum = num;
	}
	if (valid) return 0;
	
	for (int i = start; i < start + len; i++)
	{
		int num = nums.get(i % len);
		if (num < lastNum) return -1;
		lastNum = num;
	}
	return len - start;
}
```

#### [2856. 删除数对后的最小数组长度](https://leetcode.cn/problems/minimum-array-length-after-pair-removals/)

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

遍历`coordinates`，对每个坐标，枚举与其距离为`k`的所有可能坐标。

```java
/**
 * 枚举
 * 纰缪
 */
public int countPairs(List<List<Integer>> coordinates, int k)
{
	HashMap<List<Integer>, Integer> map = new HashMap<>();
	int count = 0;
	for (List<Integer> coordinate : coordinates)
	{
		Integer a = coordinate.get(0);
		Integer b = coordinate.get(1);
		// 遍历坐标时枚举横坐标异或结果，构造距离为`k`的点
		for (int i = 0; i <= k; i++)
		{
			count += map.getOrDefault(List.of(i ^ a, k - i ^ b), 0);
		}
		map.merge(coordinate, 1, Integer::sum);
	}
	return count;
}
```

#### [2858. 可以到达每一个节点的最少边反转次数](https://leetcode.cn/problems/minimum-edge-reversals-so-every-node-is-reachable/)

### [# 114](https://leetcode.cn/contest/biweekly-contest-114/)

@2023.09.30

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 866/2406 36.0% | 7 | 0:05:33 +0 | 864 0:04:38 | +10->1640 |

#### [2869. 收集元素的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-collect-elements/)

```java
/**
 * 哈希表
 * Somnia1337
 */
public int minOperations(List<Integer> nums, int k)
{
	Set<Integer> set = new HashSet<>();
	for (int i = 1; i <= k; i++) set.add(i);
	int ans = 0;
	for (int i = nums.size() - 1; i >= 0; i--)
	{
		set.remove(nums.get(i));
		ans++;
		if (set.isEmpty()) return ans;
	}
	return ans;
}
```

#### [2870. 使数组为空的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-empty/)

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
	for (int v : count.values())
	{
		if (v == 1) return -1;
		ans += (v + 2) / 3;
	}
	return ans;
}
```

#### [2871. 将数组分割成最多数目的子数组](https://leetcode.cn/problems/split-array-into-maximum-number-of-subarrays/)

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int maxSubarrays(int[] nums)
{
	int ans = 0;
	int a = -1; // -1=111...1，-1&X=X
	for (int x : nums)
	{
		a &= x;
		if (a == 0)
		{
			ans++; // 分割
			a = -1;
		}
	}
	return Math.max(ans, 1); // 如果ans=0，说明全部元素&在一起不为0，无法分割，返回1
}
```

#### [2872. 可以被 K 整除连通块的最大数目](https://leetcode.cn/problems/maximum-number-of-k-divisible-components/)

### [# 115](https://leetcode.cn/contest/biweekly-contest-115/)

@2023.10.14

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 836/2809 29.8% | 7 | 0:15:28 +5 | 780 0:03:51 | +16->1699 |

#### [2899. 上一个遍历的整数](https://leetcode.cn/problems/last-visited-integers/)

```java
/**
 * 模拟
 * Somnia1337
 */
public List<Integer> lastVisitedIntegers(List<String> words)
{
	List<Integer> ans = new ArrayList<>();
	List<Integer> nums = new ArrayList<>();
	int cur = 0;
	for (String word : words)
	{
		if (!word.equals("prev"))
		{
			nums.add(Integer.parseInt(word));
			cur = nums.size() - 1;
		}
		else
		{
			if (!nums.isEmpty() && cur >= 0) ans.add(nums.get(cur--));
			else ans.add(-1);
		}
	}
	return ans;
}
```

#### [2900. 最长相邻不相等子序列 I](https://leetcode.cn/problems/longest-unequal-adjacent-groups-subsequence-i/)

遍历，当 `groups[i] != groups[i - 1]` 时加入解集。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public List<String> getWordsInLongestSubsequence(int n, String[] words, int[] groups)
{
	List<String> ans = new ArrayList<>();
	int prev = groups[0];
	ans.add(words[0]);
	for (int i = 1; i < groups.length; i++)
	{
		if (groups[i] != prev)
		{
			ans.add(words[i]);
			prev = groups[i];
		}
	}
	return ans;
}
```

#### [2901. 最长相邻不相等子序列 II](https://leetcode.cn/problems/longest-unequal-adjacent-groups-subsequence-ii/)

#### [2902. 和带限制的子多重集合的数目](https://leetcode.cn/problems/count-of-sub-multisets-with-bounded-sum/)

### [# 116](https://leetcode.cn/contest/biweekly-contest-116/)

@2023.10.28

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 1021/2904 35.2% | 7 | 0:12:14 +0 | 968 0:04:10 | -10->1688 |

#### [2913. 子数组不同元素数目的平方和 I](https://leetcode.cn/problems/subarrays-distinct-element-sum-of-squares-i/)

```java
/**
 * 枚举
 * Somnia1337
 */
public int sumCounts(List<Integer> nums)
{
	int len = nums.size();
	if (len == 1) return 1;
	long ans = 0;
	for (int end = 0; end < len; end++)
	{
		Set<Integer> count = new HashSet<>();
		for (int start = end; start >= 0; start--)
		{
			count.add(nums.get(start));
			ans += (long) Math.pow(count.size(), 2);
		}
	}
	return (int) (ans % 1000000007);
}
```

#### [2914. 使二进制字符串变美丽的最少修改次数](https://leetcode.cn/problems/minimum-number-of-changes-to-make-binary-string-beautiful/)

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

#### [2915. 和为目标值的最长子序列的长度](https://leetcode.cn/problems/length-of-the-longest-subsequence-that-sums-to-target/)

#### [2916. 子数组不同元素数目的平方和 II](https://leetcode.cn/problems/subarrays-distinct-element-sum-of-squares-ii/)

### [# 119](https://leetcode.cn/contest/biweekly-contest-119/)

@2023.12.09

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 605/2472 24.5% | 12 | 0:13:30 +5 | 572 0:07:57 | +32->1776 |

#### [2956. 找到两个数组中的公共元素](https://leetcode.cn/problems/find-common-elements-between-two-arrays/)

```java
/**
 * 哈希表
 * Somnia1337
 */
public int[] findIntersectionValues(int[] nums1, int[] nums2) {
	Set<Integer> s1 = new HashSet<>(), s2 = new HashSet<>();
	for (int n1 : nums1) s1.add(n1);
	for (int n2 : nums2) s2.add(n2);
	int[] ans = new int[2];
	for (int n2 : nums2) {
		if (s1.contains(n2)) ans[1]++;
	}
	for (int n1 : nums1) {
		if (s2.contains(n1)) ans[0]++;
	}
	return ans;
}
```

#### [2957. 消除相邻近似相等字符](https://leetcode.cn/problems/remove-adjacent-almost-equal-characters/)

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

#### [2959. 关闭分部的可行集合数目](https://leetcode.cn/problems/number-of-possible-sets-of-closing-branches/)

### [# 120](https://leetcode.cn/contest/biweekly-contest-120/)

@2023.12.23

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 502/2542 19.8% | 12 | 1:28:50 +15 | 319 0:27:45 |  |

#### [2970. 统计移除递增子数组的数目 I](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-i/)

```java
/**
 * 枚举
 * Somnia1337
 */
public int incremovableSubarrayCount(int[] nums) {
	int len = nums.length, start = len, end = -1;
	for (int i = 1; i < len; i++) {
		if (nums[i] <= nums[i - 1]) {
			start = Math.min(i, start);
			end = Math.max(i - 1, end);
		}
	}
	
	// 没有逆序部分
	if (start == len) return len * (len + 1) >> 1;
	
	int r = end, ans = 0; // 从 end 向右枚举 r
	while (r < nums.length - 1) {
		int l = start; // 从 start 向左枚举 l
		while (l > 0 && nums[l - 1] >= nums[r + 1]) l--;
		ans += l + 1;
		r++;
	}
	// r==n 时, 右侧已无元素, start 及左侧均可作为左端点
	return ans + start + 1;
}
```

#### [2971. 找到最大周长的多边形](https://leetcode.cn/problems/find-polygon-with-the-largest-perimeter/)

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

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public long largestPerimeter(int[] nums) {
	Arrays.sort(nums);
	long sum = 0;
	for (int x : nums) sum += x;
	for (int i = nums.length - 1; i > 1; i--) {
		if (sum > nums[i] * 2L) return sum;
		sum -= nums[i];
	}
	return -1;
}
```

#### [2972. 统计移除递增子数组的数目 II](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-ii/)

逆序部分：非严格递增的部分

一次遍历，找到整个数组第一个逆序部分的起点（橙线，`start`）和最后一个逆序部分的终点（蓝线，`end`），所有“移除递增子数组”都必须包含 `start` 到 `end` 之间的所有元素（双闭区间）。

![[10033. 统计移除递增子数组的数目 II.png|400]]

1. 枚举

从蓝线向右枚举“移除组”的右端点 `r`，那么移除后，右侧部分的首元素为 `nums[r + 1]`，从橙线向左寻找可以作为左端点的位置 `l`，即第一个满足 `nums[l - 1] < nums[r + 1]` 的 `l`，累加答案。

```java
/**
 * 枚举
 * Somnia1337
 */
public long incremovableSubarrayCount(int[] nums) {
	int len = nums.length, start = len, end = -1;
	for (int i = 1; i < len; i++) {
		if (nums[i] <= nums[i - 1]) {
			start = Math.min(i, start);
			end = Math.max(i - 1, end);
		}
	}
	
	// 没有逆序部分
	if (start == len) return (long) len * (len + 1) >> 1;
	
	int r = end; // 从 end 向右枚举 r
	long ans = 0;
	while (r < nums.length - 1) {
		int l = start; // 从 start 向左移动 l
		while (l > 0 && nums[l - 1] >= nums[r + 1]) l--;
		ans += l + 1;
		r++;
	}
	// r==n 时, 右侧已无元素, start 及左侧均可作为左端点
	return ans + start + 1;
}
```

2. 双指针

由于 `r` 右移时，`l` 一定不会再左移，因此可改为同向双指针。

```java
/**
 * 双指针
 * Somnia1337
 */
public long incremovableSubarrayCount(int[] nums) {
	int len = nums.length, start = len, end = -1;
	for (int i = 1; i < len; i++) {
		if (nums[i] <= nums[i - 1]) {
			start = Math.min(i, start);
			end = Math.max(i - 1, end);
		}
	}
	
	// 没有逆序部分
	if (start == len) return (long) len * (len + 1) >> 1;
	
	int l = 1, r = end;
	long ans = 0;
	while (r < nums.length - 1) {
		while (l <= start && nums[l - 1] < nums[r + 1]) l++;
		ans += l; // l 多向右移了 1, 减掉
		r++;
	}
	// r==n 时, 右侧已无元素, start 及左侧均可作为左端点
	return ans + start + 1;
}
```

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public long incremovableSubarrayCount(int[] a) {
	int len = a.length, i = 0;
	while (i < len - 1 && a[i] < a[i + 1]) i++;
	if (i == len - 1) return (long) len * (len + 1) / 2;
	
	long ans = i + 2;
	for (int j = len - 1; j > 0 && (j == len - 1 || a[j] < a[j + 1]); j--) {
		while (i >= 0 && a[i] >= a[j]) i--;
		ans += i + 2;
	}
	return ans;
}
```

#### [2973. 树中每个节点放置的金币数目](https://leetcode.cn/problems/find-number-of-coins-to-place-in-tree-nodes/)

### [# 121](https://leetcode.cn/contest/biweekly-contest-121/)

@2024.01.06

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 525/2218 23.7% | 12 | 1:02:05 +10 | 236 0:09:00 |  |

#### [2996. 大于等于顺序前缀和的最小缺失整数](https://leetcode.cn/problems/smallest-missing-integer-greater-than-sequential-prefix-sum/)

```java
/**
 * 哈希表
 * Somnia1337
 */
public int missingInteger(int[] nums) {
	int sum = nums[0];
	for (int i = 1; i < nums.length && nums[i] == nums[i - 1] + 1; i++) sum += nums[i];
	Set<Integer> vis = new HashSet<>();
	for (int x : nums) vis.add(x);
	while (vis.contains(sum)) sum++;
	return sum;
}
```

#### [2997. 使数组异或和等于 K 的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-xor-equal-to-k/)

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

#### [2999. 统计强大整数的数目](https://leetcode.cn/problems/count-the-number-of-powerful-integers/)

### [# 122](https://leetcode.cn/contest/biweekly-contest-122/)

@2024.01.20

| 全国排名 | 得分 | 用时 | 同分首位 | 竞赛分数 |
| :--: | :--: | :--: | :--: | :--: |
| 673/2547 26.5% | 7 | 0:08:35 +0 | 661 0:03:24 | -13->1922 |

#### [3010. 将数组分成最小总代价的子数组 I](https://leetcode.cn/problems/divide-an-array-into-subarrays-with-minimum-cost-i/)

```java
public int minimumCost(int[] nums) {
	int m1 = Integer.MAX_VALUE, m2 = Integer.MAX_VALUE, ans = nums[0];
	for (int i = 1; i < nums.length; i++) {
		if (nums[i] < m1) {
			m2 = m1;
			m1 = nums[i];
		}
		else if (nums[i] < m2) m2 = nums[i];
	}
	return ans + m1 + m2;
}
```

#### [3011. 判断一个数组是否可以变为有序](https://leetcode.cn/problems/find-if-array-can-be-sorted/)

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

#### [3013. 将数组分成最小总代价的子数组 II](https://leetcode.cn/problems/divide-an-array-into-subarrays-with-minimum-cost-ii/)

### [# 123](https://leetcode.cn/contest/biweekly-contest-123/)

@2024.02.03

未参加

#### [3024. 三角形类型 II](https://leetcode.cn/problems/type-of-triangle-ii/)

```java
/**
 * 分类讨论
 * Somnia1337
 */
public String triangleType(int[] nums) {
	int a = nums[0], b = nums[1], c = nums[2];
	if (a == b && b == c) return "equilateral";
	if (!(a + b > c && a + c > b && b + c > a)) return "none";
	return a == b || a == c || b == c ? "isosceles" : "scalene";
}
```

#### [3025. 人员站位的方案数 I](https://leetcode.cn/problems/find-the-number-of-ways-to-place-people-i/)

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

#### [3027. 人员站位的方案数 II](https://leetcode.cn/problems/find-the-number-of-ways-to-place-people-ii/)

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

### [# 124](https://leetcode.cn/contest/biweekly-contest-124/)

@2024.02.17

未参加

#### [3038. 相同分数的最大操作数目 I](https://leetcode.cn/problems/maximum-number-of-operations-with-the-same-score-i/)

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int maxOperations(int[] nums) {
	int tar = nums[0] + nums[1], ans = 1;
	for (int i = 3; i < nums.length; i += 2) {
		if (nums[i] + nums[i - 1] != tar) break;
		ans++;
	}
	return ans;
}
```

#### [3039. 进行操作使字符串为空](https://leetcode.cn/problems/apply-operations-to-make-string-empty/)

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

#### [3041. 修改数组后最大化数组中的连续元素数目](https://leetcode.cn/problems/maximize-consecutive-elements-in-an-array-after-modification/)

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int maxSelectedElements(int[] nums) {
	Arrays.sort(nums);
	Map<Integer, Integer> f = new HashMap<>();
	for (int x : nums) {
		f.put(x + 1, f.getOrDefault(x, 0) + 1);
		f.put(x, f.getOrDefault(x - 1, 0) + 1);
	}
	int ans = 0;
	for (int v : f.values()) ans = Math.max(v, ans);
	return ans;
}
```

### [# 125](https://leetcode.cn/contest/biweekly-contest-125/)

@2024.03.02

未参加

#### [100231. 超过阈值的最少操作数 I](https://leetcode.cn/problems/minimum-operations-to-exceed-threshold-value-i/)



```java
/**
 * 一次遍历
 * Somnia1337
 */
public int minOperations(int[] nums, int k) {
	int ans = 0;
	for (int x : nums) {
		if (x < k) ans++;
	}
	return ans;
}
```

#### [100232. 超过阈值的最少操作数 II](https://leetcode.cn/problems/minimum-operations-to-exceed-threshold-value-ii/)



```java
/**
 * 模拟
 * Somnia1337
 */
public int minOperations(int[] nums, int k) {
	Queue<Long> pq = new PriorityQueue<>();
	for (int x : nums) pq.add((long) x);
	int ans = 0;
	while (pq.peek() < k) {
		long a = pq.poll(), b = pq.poll();
		pq.offer(a * 2 + b);
		ans++;
	}
	return ans;
}
```

#### [100226. 在带权树网络中统计可连接服务器对数目](https://leetcode.cn/problems/count-pairs-of-connectable-servers-in-a-weighted-tree-network/)



```java
/**
 * 枚举
 * 小羊肖恩
 */
private List<int[]>[] adj;
private int cnt;

public int[] countPairsOfConnectableServers(int[][] edges, int signalSpeed) {
	int n = edges.length + 1;
	adj = new List[n];
	Arrays.setAll(adj, e -> new ArrayList<>());
	for (int[] e : edges) {
		adj[e[0]].add(new int[]{e[1], e[2]});
		adj[e[1]].add(new int[]{e[0], e[2]});
	}
	int[] ans = new int[n];
	for (int i = 0; i < n; i++) {
		int acc = 0;
		for (int[] pair : adj[i]) {
			cnt = 0;
			dfs(pair[0], i, pair[1], signalSpeed);
			ans[i] += cnt * acc;
			acc += cnt;
		}
	}
	return ans;
}

private void dfs(int u, int p, int d, int sig) {
	if (p != -1 && d % sig == 0) cnt++;
	for (int[] pair : adj[u]) {
		if (pair[0] != p) dfs(pair[0], u, d + pair[1], sig);
	}
}
```

#### [100210. 最大节点价值之和](https://leetcode.cn/problems/find-the-maximum-sum-of-node-values/)