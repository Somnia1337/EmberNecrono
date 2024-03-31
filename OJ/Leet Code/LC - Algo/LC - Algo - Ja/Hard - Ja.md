```text
题库: 4 30 32 37 41 42 51 52 60 65 68 76 84 85 115 123 124 127  132 135 140 149 154 174 188 212 214 224 233 239 265 295 296 297 301 302 305 312 315 329 354 391 403 410 440 446 458 460 466 514 568 630 660 679 689 711 715 765 773 774 778 827 828 829 839 871 878 902 924 968 980 1044 1092 1147 1153 1255 1259 1289 1326 1349 1392 1402 1416 1425 1449 1499 1665 1671 1723 1745 1761 1793 1944 2003 2050 2111 2127 2132 2136 2147 2227 2251 2258 2262 2276 2344 2350 2360 2398 2435 2454 2581 2585 2603 2608 2646 2681 2719 2736 2809 2819 2846 2862 2867 2872 2897 2931 2963 2968 2972 2983 3003 3027 3031 3041
LCP: 24
```

#### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

@2023.10.25

```java
/**
 * 双指针
 * windliang
 */
public double findMedianSortedArrays(int[] nums1, int[] nums2)
{
	int l1 = nums1.length, l2 = nums2.length, len = l1 + l2;
	int l = -1, r = -1;
	int s1 = 0, s2 = 0;
	for (int i = 0; i <= len / 2; i++)
	{
		l = r;
		if (s1 < l1 && (s2 >= l2 || nums1[s1] < nums2[s2])) r = nums1[s1++];
		else r = nums2[s2++];
	}
	if ((len & 1) == 0) return (l + r) / 2.0;
	return r;
}
```

#### [30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

@2023.09.23

![[Snipaste_230923_151728.png|400]]

计数`words`中每个词的出现次数存储在`dict`，遍历`s`，对每段可能串联了所有单词的子串，计数每个单词并与`dict`比较，将有效位置加入解集。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public List<Integer> findSubstring(String s, String[] words)
{
	List<Integer> ans = new ArrayList<>();
	Map<String, Integer> dict = new HashMap<>();
	for (String word : words) // 计数每个单词的出现次数
	{
		dict.merge(word, 1, Integer::sum);
	}
	int len = words[0].length();
	int sum = words.length * len;
	
	for (int i = 0; i <= s.length() - sum; i++)
	{
		if (dict.containsKey(s.substring(i, i + len))) // 开头单词匹配
		{
			Map<String, Integer> temp = new HashMap<>(); // 计数当前子串
			int j = i;
			for (; j < i + sum; j += len)
			{
				String part = s.substring(j, j + len);
				temp.merge(part, 1, Integer::sum);
				if (!dict.containsKey(part) || temp.get(part) > dict.get(part)) break; // 当前单词非法，或者次数超过上限
			}
			if (j == i + sum) ans.add(i);
		}
	}
	
	return ans;
}
```

```java
/**
 * 滑动窗口
 * 力扣官方题解
 */
public List<Integer> findSubstring(String s, String[] words)
{
	List<Integer> ans = new ArrayList<>();
	int m = words.length, n = words[0].length(), ls = s.length();
	for (int i = 0; i < n; i++)
	{
		if (i + m * n > ls) break;
		Map<String, Integer> differ = new HashMap<>();
		for (int j = 0; j < m; j++)
		{
			String word = s.substring(i + j * n, i + (j + 1) * n);
			differ.put(word, differ.getOrDefault(word, 0) + 1);
		}
		for (String word : words)
		{
			differ.put(word, differ.getOrDefault(word, 0) - 1);
			if (differ.get(word) == 0)
			{
				differ.remove(word);
			}
		}
		for (int start = i; start < ls - m * n + 1; start += n)
		{
			if (start != i)
			{
				String word = s.substring(start + (m - 1) * n, start + m * n);
				differ.put(word, differ.getOrDefault(word, 0) + 1);
				if (differ.get(word) == 0)
				{
					differ.remove(word);
				}
				word = s.substring(start - n, start);
				differ.put(word, differ.getOrDefault(word, 0) - 1);
				if (differ.get(word) == 0)
				{
					differ.remove(word);
				}
			}
			if (differ.isEmpty())
			{
				ans.add(start);
			}
		}
	}
	return ans;
}
```

#### [32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/)

@2023.07.03

#栈 

1. 栈

```java
/**
 * 栈
 * Somnia1337
 */
public int longestValidParentheses(String s)
{
	int len = s.length();
	Stack<Integer> stack = new Stack<>();
	ArrayList<Integer> arrayList = new ArrayList<>();
	for (int i = 0; i < len; i++)
	{
		if (s.charAt(i) == '(') stack.push(i); // '('压栈
		else
		{
			if (stack.isEmpty())
			{
				arrayList.add(i); // 栈为空，记录无效的')'
			}
			else
			{
				stack.pop(); // 栈非空，弹栈
			}
		}
	}
	arrayList.add(-1); // 左界为-1
	arrayList.add(len); // 右界为len
	for (int i = 0; i < stack.size(); i++)
	{
		arrayList.add(stack.pop()); // 将栈中剩余的'('位置并入arrayList
	}
	arrayList.sort(Comparator.comparingInt(a -> a)); // 排序
	int ans = 0;
	for (int i = 0; i < arrayList.size() - 1; i++)
	{
		ans = Math.max(ans, arrayList.get(i + 1) - arrayList.get(i) - 1); // 遍历无效位置集，找到每两对无效位置之间的距离最大值
	}
	return ans;
}
```

```java
/**
 * 栈
 * 力扣官方题解
 */
public int longestValidParentheses(String s)
{
	int ans = 0;
	Deque<Integer> stack = new LinkedList<>();
	stack.push(-1); // 将左界-1压栈
	for (int i = 0; i < s.length(); i++)
	{
		if (s.charAt(i) == '(')
		{
			stack.push(i); // '('压栈
		}
		else
		{
			stack.pop(); // 无论如何先弹栈
			if (stack.isEmpty())
			{
				stack.push(i); // 栈为空，')'压栈
			}
			else
			{
				ans = Math.max(ans, i - stack.peek()); // 栈非空，更新最大长度
			}
		}
	}
	return ans;
}
```

2. 正反两次遍历

```java
/**
 * 正反两次遍历
 * 力扣官方题解
 */
public int longestValidParentheses(String s)
{
	int left = 0, right = 0, maxlength = 0; //记录'('、')'数量及最大长度
	for (int i = 0; i < s.length(); i++) //左->右遍历
	{
		if (s.charAt(i) == '(') left++;
		else right++;
		if (left == right) //'('、')'数量相等时更新最大长度
		{
			maxlength = Math.max(maxlength, 2 * right);
		}
		else if (right > left) //')'数量更多时重置'('、')'数量
			left = right = 0;
	}
	left = right = 0;
	for (int i = s.length() - 1; i >= 0; i--) //右->左遍历
	{
		if (s.charAt(i) == '(') left++;
		else right++;
		if (left == right) //'('、')'数量相等时更新最大长度
		{
			maxlength = Math.max(maxlength, 2 * left);
		}
		else if (left > right) //'('数量更多时重置'('、')'数量
			left = right = 0;
	}
	return maxlength;
}
```

#### [37. 解数独](https://leetcode.cn/problems/sudoku-solver/)

@2023.09.21

#回溯 

```java
/**
 * 回溯
 * Somnia1337
 */
private boolean[][] chks, rows, cols;
private boolean valid;

public void solveSudoku(char[][] board) {
	chks = new boolean[9][9];
	rows = new boolean[9][9];
	cols = new boolean[9][9];
	valid = false;
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (board[i][j] != '.') {
				int val = board[i][j] - '1';
				chks[i / 3 * 3 + j / 3][val] = rows[i][val] = cols[j][val] = true;
			}
		}
	}
	bt(board, 0, 0);
}

private void bt(char[][] b, int x, int y) {
	if (x == 9 && y == 0) {
		valid = true;
		return;
	}
	int nx = y != 8 ? x : x + 1, ny = (y + 1) % 9;
	if (b[x][y] != '.') {
		bt(b, nx, ny);
		return;
	}
	for (int num = 0; num < 9; num++) {
		int chk = x / 3 * 3 + y / 3;
		if (chks[chk][num] || rows[x][num] || cols[y][num]) continue;
		chks[chk][num] = rows[x][num] = cols[y][num] = true;
		b[x][y] = (char) (num + '1');
		bt(b, nx, ny);
		if (valid) return;
		b[x][y] = '.';
		chks[chk][num] = rows[x][num] = cols[y][num] = false;
	}
}
```

#### [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)

@2023.09.20

while 终止条件的最后一项 `nums[nums[i] - 1] != nums[i]` 一直没写对。

遇到 `nums[i]`，就持续将其置换，直到它位于自己该在的位置上。

```java
/**
 * 置换
 * 力扣官方题解
 */
public int firstMissingPositive(int[] nums) {
	int n = nums.length;
	for (int i = 0; i < n; i++) {
		while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
			swap(nums, nums[i] - 1, i);
		}
	}
	for (int i = 0; i < n; i++) {
		if (nums[i] != i + 1) return i + 1;
	}
	return n + 1;
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

@2023.09.26

1. 按行遍历

```java
/**
 * 按行遍历
 * Somnia1337
 */
public int trap(int[] height) {
	int ans = 0;
	for (int y = 1; ; y++) {
		int cur = -1;
		for (int i = 0; i < height.length; i++) {
			if (height[i] >= y) {
				if (cur > -1) {
					ans += i - cur - 1;
				}
				cur = i;
			}
		}
		if (cur == -1) break;
	}
	return ans;
}
```

2. 单调栈

```java
/**
 * 单调栈
 * Somnia1337
 */
public int trap(int[] height) {
	Deque<Integer> stk = new ArrayDeque<>();
	int ans = 0;
	for (int i = 0; i < height.length; i++) {
		while (!stk.isEmpty() && height[stk.peek()] < height[i]) {
			int idx = stk.pop();
			if (!stk.isEmpty()) {
				int h = Math.min(height[stk.peek()], height[i]) - height[idx];
				int w = i - stk.peek() - 1;
				ans += h * w;
			}
		}
		stk.push(i);
	}
	return ans;
}
```

3. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int trap(int[] height) {
	int n = height.length;
	int[] lMax = new int[n];
	lMax[0] = height[0];
	for (int i = 1; i < n; ++i) lMax[i] = Math.max(lMax[i - 1], height[i]);
	int[] rMax = new int[n];
	rMax[n - 1] = height[n - 1];
	for (int i = n - 2; i >= 0; --i) rMax[i] = Math.max(rMax[i + 1], height[i]);
	int ans = 0;
	for (int i = 0; i < n; ++i) ans += Math.min(lMax[i], rMax[i]) - height[i];
	return ans;
}
```

4. 双指针

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int trap(int[] height) {
	int l = 0, r = height.length - 1;
	int lMax = 0, rMax = 0;
	int ans = 0;
	while (l < r) {
		lMax = Math.max(height[l], lMax);
		rMax = Math.max(height[r], rMax);
		if (height[l] < height[r]) {
			ans += lMax - height[l];
			l++;
		}
		else {
			ans += rMax - height[r];
			r--;
		}
	}
	return ans;
}
```

#### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

@2023.09.17

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<String>> solveNQueens(int n)
{
	List<List<String>> ans = new ArrayList<>();
	List<Integer> current = new ArrayList<>();
	Set<Integer> bannedY = new HashSet<>();
	bt(n, 0, bannedY, ans, current);
	return ans;
}

public void bt(int n, int row, Set<Integer> bannedY, List<List<String>> ans, List<Integer> current)
{
	if (row == n)
	{
		List<String> ansList = new ArrayList<>();
		for (int pos : current)
		{
			String ansStr = ".".repeat(pos) + "Q" + ".".repeat(n - pos - 1);
			ansList.add(ansStr);
		}
		ans.add(ansList);
		return;
	}
	for (int i = 0; i < n; i++)
	{
		if (bannedY.contains(i) || !check(n, i, row, current)) continue;
		current.add(i);
		bannedY.add(i);
		bt(n, row + 1, bannedY, ans, current);
		current.remove(current.size() - 1);
		bannedY.remove(i);
	}
}

public boolean check(int n, int pos, int row, List<Integer> current)
{
	for (int i = 0; i < row; i++)
	{
		if (current.get(i) == pos) return false;
	}
	for (int i = Math.max(row - pos, 0); i < row; i++)
	{
		if (current.get(i) == pos - row + i) return false;
	}
	for (int i = Math.max(row - n + pos, 0); i < row; i++)
	{
		if (current.get(i) == pos + row - i) return false;
	}
	return true;
}
```

```java
/**
 * 回溯
 * 代码随想录
 */
List<List<String>> res = new ArrayList<>();

public List<List<String>> solveNQueens(int n)
{
	char[][] chessboard = new char[n][n];
	for (char[] c : chessboard)
	{
		Arrays.fill(c, '.');
	}
	backTrack(n, 0, chessboard);
	return res;
}

public void backTrack(int n, int row, char[][] chessboard)
{
	if (row == n)
	{
		res.add(Array2List(chessboard));
		return;
	}
	for (int col = 0; col < n; ++col)
	{
		if (isValid(row, col, n, chessboard))
		{
			chessboard[row][col] = 'Q';
			backTrack(n, row + 1, chessboard);
			chessboard[row][col] = '.';
		}
	}
}

public List<String> Array2List(char[][] chessboard)
{
	List<String> ret = new ArrayList<>();
	for (char[] c : chessboard)
	{
		ret.add(String.copyValueOf(c));
	}
	return ret;
}

public boolean isValid(int row, int col, int n, char[][] chessboard)
{
	for (int i = 0; i < row; ++i)
	{
		if (chessboard[i][col] == 'Q') return false;
	}
	for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
	{
		if (chessboard[i][j] == 'Q') return false;
	}
	for (int i = row - 1, j = col + 1; i >= 0 && j <= n - 1; i--, j++)
	{
		if (chessboard[i][j] == 'Q') return false;
	}
	return true;
}
```

#### [52. N 皇后 II](https://leetcode.cn/problems/n-queens-ii/)

@2023.09.29

#回溯 

```java
for (int i = 0; i < n; i++)
{
	if (!check(i, row)) continue;
	board[row] = i;
	bt(n, row + 1);
	board[row] = -1;
}
```

用 `int[]` 记录每一行的皇后位置，填入新一行的某位置时，检查之前所有行是否存在冲突。

```java
/**
 * 回溯
 * Somnia1337
 */
public int ans;
public int[] board;

public int totalNQueens(int n)
{
	board = new int[n];
	Arrays.fill(board, -1);
	bt(n, 0);
	return ans;
}

public void bt(int n, int row)
{
	if (row == n)
	{
		ans++;
		return;
	}
	for (int i = 0; i < n; i++)
	{
		if (!check(i, row)) continue;
		board[row] = i;
		bt(n, row + 1);
		board[row] = -1;
	}
}

public boolean check(int pos, int row)
{
	for (int i = 0; i < row; i++)
	{
		if (board[i] == pos || board[i] == pos - row + i || board[i] == pos + row - i) return false;
	}
	return true;
}
```

#### [60. 排列序列](https://leetcode.cn/problems/permutation-sequence/)

@2023.10.31

#数学 

递归，每次确认一位，更新剩余的可用数字及 `k`。

```java
/**
 * 数学
 * Somnia1337
 */
private StringBuilder ans;
private boolean[] vis;

public String getPermutation(int n, int k) {
	ans = new StringBuilder();
	vis = new boolean[n + 1];
	solve(n, n, k - 1);
	return ans.toString();
}

// 递归, x 为剩余的可用数字
private void solve(int n, int x, int k) {
	if (x == 1) { // 只剩 1 个
		for (int i = 1; i <= n; i++) {
			if (!vis[i]) {
				ans.append(i);
				return;
			}
		}
	}
	
	int perm = factorial(x - 1); // 确认当前位后的排列数
	int offset = k / perm; // 分组偏移量
	for (int i = 1; i <= n; i++) { // 遍历剩余可用数字, 寻找本位
		if (!vis[i]) offset--;
		if (offset < 0) { // 本位
			vis[i] = true;
			ans.append(i);
			break;
		}
	}
	solve(n, x - 1, k % perm);
}

// 计算阶乘, 即排列数
private int factorial(int n) {
	int ret = 1;
	while (n > 1) ret *= n--;
	return ret;
}
```

#### [65. 有效数字](https://leetcode.cn/problems/valid-number/)

@2023.07.01

```java
/**
 * 讨论
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

#### [68. 文本左右对齐](https://leetcode.cn/problems/text-justification/)

@2023.10.13

#模拟 

硬核模拟……

```java
/**
 * 模拟
 * Somnia1337
 */
public List<String> fullJustify(String[] words, int maxWidth)
{
	List<String> ans = new ArrayList<>();
	int it = 0;
	while (it < words.length) // 遍历words
	{
		int start = it, ln = 0, gaps = -1; // 记录本行第一个单词、本行单词长度和、单词之间的空隙数
		while (it < words.length && ln + gaps < maxWidth) // 单词长度和+空隙数<max才能继续，因为每个空隙至少1个空格
		{
			ln += words[it++].length();
			gaps++;
		}
		if (ln + gaps > maxWidth) // 超过max，丢弃最后一个单词
		{
			it--;
			ln -= words[it].length();
			gaps--;
		}
		
		if (it == words.length) // 这是最后一行
		{
			StringBuilder line = new StringBuilder();
			for (int i = start; i < it; i++)
			{
				line.append(words[i]);
				if (i < it - 1) line.append(" "); // 单词之间1个空格
			}
			line.append(" ".repeat(maxWidth - line.length())); // 补全长度
			ans.add(line.toString());
			break;
		}
		
		if (gaps == 0) // 本行只有1个单词
		{
			String line = words[start] + " ".repeat(maxWidth - ln); // 补全长度
			ans.add(line);
			continue;
		}
		
		int space = maxWidth - ln; // 总空格数
		int[] spaces = new int[gaps]; // 为每个空隙分配空格
		Arrays.fill(spaces, space / gaps);
		int left = space % gaps;
		for (int j = 0; left > 0; j++) // 无法均分的空格往左靠
		{
			spaces[j]++;
			left--;
		}
		StringBuilder line = new StringBuilder();
		for (int i = start; i < it; i++)
		{
			line.append(words[i]);
			if (line.length() == maxWidth) break; // 最后一个单词，之后没有空格
			line.append(" ".repeat(spaces[i - start])); // 填充空格
		}
		ans.add(line.toString());
	}
	return ans;
}
```

#### [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

@2023.09.21

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public String minWindow(String s, String t) {
	char[] chs1 = s.toCharArray(), chs2 = t.toCharArray();
	int n = chs1.length;
	Map<Character, Integer> tar = new HashMap<>();
	int min = Integer.MAX_VALUE, st = -1, ed = -1;
	for (char c2 : chs2) tar.merge(c2, 1, Integer::sum);
	int l = 0, r = 0;
	while (r < n) {
		char c = chs1[r];
		if (tar.containsKey(c)) {
			tar.merge(c, -1, Integer::sum);
			if (tar.get(c) < 0) {
				while (l < r) {
					if (!tar.containsKey(chs1[l])) l++;
					else if (tar.get(chs1[l]) < 0) tar.merge(chs1[l++], 1, Integer::sum);
					else break;
				}
			}
		}
		boolean valid = true;
		for (int v : tar.values()) {
			if (v > 0) {
				valid = false;
				break;
			}
		}
		if (valid) {
			while (!tar.containsKey(chs1[l])) l++;
			if (r - l + 1 < min) min = (ed = r) - (st = l) + 1;
		}
		r++;
	}
	return st != -1 ? s.substring(st, ed + 1) : "";
}
```

```java
/**
 * 滑动窗口
 * ?
 */
public String minWindow(String s, String t) {
	if (s == null || s.isEmpty() || t == null || t.isEmpty()) return "";
	int[] need = new int[128];
	for (int i = 0; i < t.length(); i++) need[t.charAt(i)]++;
	int l = 0, r = 0, size = Integer.MAX_VALUE, cnt = t.length(), st = 0;
	while (r < s.length()) {
		char c = s.charAt(r);
		if (need[c]-- > 0) cnt--;
		if (cnt == 0) {
			while (l < r && need[s.charAt(l)] < 0) {
				need[s.charAt(l)]++;
				l++;
			}
			if (r - l + 1 < size) {
				size = r - l + 1;
				st = l;
			}
			need[s.charAt(l)]++;
			l++;
			cnt++;
		}
		r++;
	}
	return size < Integer.MAX_VALUE ? s.substring(st, st + size) : "";
}
```

#### [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

@2023.10.25

```java
/**
 * 单调栈
 * liweiwei1419
 */
public int largestRectangleArea(int[] heights)
{
	int len = heights.length;
	Deque<Integer> stk = new ArrayDeque<>(len);
	
	int ans = 0;
	for (int i = 0; i < len; i++)
	{
		while (!stk.isEmpty() && heights[i] < heights[stk.peekLast()])
		{
			int h = heights[stk.pollLast()];
			while (!stk.isEmpty() && heights[stk.peekLast()] == h) stk.pollLast();
			int w = !stk.isEmpty() ? i - stk.peekLast() - 1 : i;
			ans = Math.max(h * w, ans);
		}
		stk.addLast(i);
	}
	
	while (!stk.isEmpty())
	{
		int h = heights[stk.pollLast()];
		while (!stk.isEmpty() && heights[stk.peekLast()] == h) stk.pollLast();
		int w = !stk.isEmpty() ? len - stk.peekLast() - 1 : len;
		ans = Math.max(h * w, ans);
	}
	
	return ans;
}
```

#### [85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)

@2023.10.16

#暴力 

`dp[i][j][0]` 和 `dp[i][j][1]` 分别记录前 `i * j` 内以 `(i, j)` 为终点的竖直、水平方向上的最多连续 `'1'` 的个数，然后向左枚举每个宽度及对应的高度，更新最大面积。

```java
/**
 * 枚举
 * justdoit149
 */
public int maximalRectangle(char[][] matrix) {
	int rows = matrix.length, cols = matrix[0].length;
	int[][][] dp = new int[rows][cols][2]; // 记录每个点在两个方向上的连续 '1'
	int ans = 0;
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (matrix[i][j] == '1') {
				// 更新 dp
				dp[i][j][0] = (i >= 1 ? dp[i - 1][j][0] + 1 : 1);
				dp[i][j][1] = (j >= 1 ? dp[i][j - 1][1] + 1 : 1);
				// 枚举宽度及对应的高度, 更新最大面积
				int min = dp[i][j][1];
				for (int k = i; k >= i - dp[i][j][0] + 1; k--) {
					min = Math.min(dp[k][j][1], min);
					ans = Math.max(min * (i - k + 1), ans);
				}
			}
		}
	}
	return ans;
}
```

#### [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

@2023.10.31

#动态规划 

`dp[i][j]` 表示子问题 `s[:i]` 与 `t[:j]` 的解。当 `j == 0` 时，`t[:j]` 为 `""`，因此 `dp[i][0]` 均初始化为 1。

递推时，取 `c1 = s[i]`、`c2 = t[j]`，至少有 `dp[i][j] = dp[i - 1][j]`，如果还有 `c1 == c2`，则再加上 `dp[i - 1][j - 1]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int numDistinct(String s, String t) {
    int n1 = s.length(), n2 = t.length();
    if (n1 < n2) return 0;
    int[][] dp = new int[n1 + 1][n2 + 1];
    for (int i = 0; i <= n1; i++) dp[i][0] = 1;
    for (int i = 1; i < n1 + 1; i++) {
        char c1 = s.charAt(i - 1);
        for (int j = 1; j < n2 + 1; j++) {
            char c2 = t.charAt(j - 1);
            dp[i][j] = dp[i - 1][j];
            if (c1 == c2) dp[i][j] += dp[i - 1][j - 1];
        }
    }
    return dp[n1][n2];
}
```

#### [123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

@2023.10.03

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int maxProfit(int[] prices)
{
	int len = prices.length;
	int buy1 = -prices[0], sell1 = 0;
	int buy2 = -prices[0], sell2 = 0;
	for (int i = 1; i < len; i++)
	{
		buy1 = Math.max(buy1, -prices[i]);
		sell1 = Math.max(sell1, buy1 + prices[i]);
		buy2 = Math.max(buy2, sell1 - prices[i]);
		sell2 = Math.max(sell2, buy2 + prices[i]);
	}
	return sell2;
}
```

#### [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

@2023.10.21

```java
/**
 * DFS
 * 张张
 */
private int max = Integer.MIN_VALUE;

public int maxPathSum(TreeNode root)
{
	dfs(root);
	return max;
}

private int dfs(TreeNode root)
{
	if (root == null) return 0;
	int l = dfs(root.left), r = dfs(root.right);
	max = Math.max(root.val + l + r, max);
	return Math.max(root.val + Math.max(l, r), 0);
}
```

```java
/**
 * DFS
 * 力扣官方题解
 */
private int max = Integer.MIN_VALUE;

public int maxPathSum(TreeNode root)
{
	dfs(root);
	return max;
}

private int dfs(TreeNode node)
{
	if (node == null) return 0;
	
	// 递归计算左右子节点的最大贡献值
	// 只有在最大贡献值大于 0 时，才会选取对应子节点
	int l = Math.max(dfs(node.left), 0);
	int r = Math.max(dfs(node.right), 0);
	
	// 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
	int priceNewPath = node.val + l + r;
	
	// 更新答案
	max = Math.max(max, priceNewPath);
	
	// 返回节点的最大贡献值
	return node.val + Math.max(l, r);
}
```

#### [127. 单词接龙](https://leetcode.cn/problems/word-ladder/)

@2023.10.20

```java
/**
 * BFS
 * 代码随想录
 */
public int ladderLength(String beginWord, String endWord, List<String> wordList)
{
	Set<String> dict = new HashSet<>(wordList);
	if (dict.isEmpty() || !dict.contains(endWord)) return 0;
	Queue<String> queue = new LinkedList<>();
	queue.offer(beginWord);
	Map<String, Integer> paths = new HashMap<>();
	paths.put(beginWord, 1);

	while (!queue.isEmpty())
	{
		String node = queue.poll();
		int path = paths.get(node);
		for (int i = 0; i < node.length(); i++)
		{
			char[] chars = node.toCharArray();
			for (char k = 'a'; k <= 'z'; k++)
			{
				chars[i] = k;
				String next = String.valueOf(chars);
				if (next.equals(endWord)) return path + 1;
				if (dict.contains(next) && !paths.containsKey(next))
				{
					paths.put(next, path + 1);
					queue.offer(next);
				}
			}
		}
	}
	return 0;
}
```

#### [132. 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/)

@2023.11.01

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int minCut(String s)
{
	int len = s.length();
	boolean[][] pal = new boolean[len][len];
	for (boolean[] row : pal) Arrays.fill(row, true);
	
	for (int i = len - 1; i >= 0; i--)
	{
		for (int j = i + 1; j < len; j++)
		{
			pal[i][j] = s.charAt(i) == s.charAt(j) && pal[i + 1][j - 1];
		}
	}
	
	int[] dp = new int[len];
	Arrays.fill(dp, Integer.MAX_VALUE);
	for (int i = 0; i < len; i++)
	{
		if (pal[0][i]) dp[i] = 0;
		else
		{
			for (int j = 0; j < i; j++)
			{
				if (pal[j + 1][i]) dp[i] = Math.min(dp[j] + 1, dp[i]);
			}
		}
	}
	
	return dp[len - 1];
}
```

#### [135. 分发糖果](https://leetcode.cn/problems/candy/)

@2023.07.08

```java
/**
 * 贪心 + 两次遍历
 * Somnia1337
 */
public int candy(int[] ratings)
{
	int len = ratings.length;
	int[] result = new int[len];
	Arrays.fill(result, 1);
	for (int i = 0; i < len - 1; i++)
	{
		if (ratings[i] > ratings[i + 1])
		{
			result[i] = result[i + 1] + 1;
		}
		else if (ratings[i] < ratings[i + 1])
		{
			result[i + 1] = result[i] + 1;
		}
	}
	for (int i = len - 1; i > 0; i--)
	{
		if (ratings[i] > ratings[i - 1])
		{
			result[i] = Math.max(result[i], result[i - 1] + 1);
		}
		else if (ratings[i] < ratings[i - 1])
		{
			result[i - 1] = Math.max(result[i - 1], result[i] + 1);
		}
	}
	int ans = 0;
	for (int n : result)
	{
		ans += n;
	}
	return ans;
}
```

优化：向右遍历时从1开始，向左遍历时从`len - 2`开始，每次与前一个相比较即可，省去一次比较。

```java
/**
 * 贪心 + 两次遍历
 * Somnia1337
 */
public int candy(int[] ratings)
{
	int len = ratings.length;
	int[] result = new int[len];
	Arrays.fill(result, 1);
	for (int i = 1; i < len; i++)
	{
		if (ratings[i] > ratings[i - 1])
		{
			result[i] = result[i - 1] + 1;
		}
	}
	for (int i = len - 2; i >= 0; i--)
	{
		if (ratings[i] > ratings[i + 1])
		{
			result[i] = Math.max(result[i], result[i + 1] + 1);
		}
	}
	int ans = 0;
	for (int n : result)
	{
		ans += n;
	}
	return ans;
}
```

```java
/**
 * 贪心 + 两次遍历
 * 力扣官方题解
 */
public int candy(int[] ratings)
{
	int n = ratings.length;
	int[] left = new int[n];
	for (int i = 0; i < n; i++)
	{
		if (i > 0 && ratings[i] > ratings[i - 1])
		{
			left[i] = left[i - 1] + 1;
		}
		else
		{
			left[i] = 1;
		}
	}
	int right = 0, ret = 0;
	for (int i = n - 1; i >= 0; i--)
	{
		if (i < n - 1 && ratings[i] > ratings[i + 1])
		{
			right++;
		}
		else
		{
			right = 1;
		}
		ret += Math.max(left[i], right);
	}
	return ret;
}
```

#### [140. 单词拆分 II](https://leetcode.cn/problems/word-break-ii/)

@2023.09.15

1. 回溯

```java
for (int r = idx + 1; r <= s.length(); r++) {
	if (!dict.contains(s.substring(idx, r))) continue;
	path.add(s.substring(idx, r));
	bt(s, r);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private Set<String> dict;
private List<String> ans, path;

public List<String> wordBreak(String s, List<String> wordDict) {
	dict = new HashSet<>(wordDict);
	ans = new ArrayList<>();
	path = new ArrayList<>();
	bt(s, 0);
	return ans;
}

private void bt(String s, int idx) {
	if (idx == s.length()) {
		ans.add(String.join(" ", path));
		return;
	}
	for (int r = idx + 1; r <= s.length(); r++) {
		if (!dict.contains(s.substring(idx, r))) continue;
		path.add(s.substring(idx, r));
		bt(s, r);
		path.remove(path.size() - 1);
	}
}
```

2. 字典树

```java
/**
 * 字典树
 * Somnia1337
 */
class Solution {
    private final char A = 'a';
    private TrieNode root;
    private List<String> path, ans;
	
    public List<String> wordBreak(String s, List<String> wordDict) {
        buildTrie(wordDict);
        ans = new ArrayList<>();
        path = new ArrayList<>();
        dfs(s, 0);
        return ans;
    }
	
    private void dfs(String s, int idx) {
        if (idx == s.length()) {
            ans.add(String.join(" ", path));
            return;
        }
        TrieNode node = root;
        for (int i = idx; i < s.length(); i++) {
            char c = s.charAt(i);
            if (node.children[c - A] == null) break;
            if ((node = node.children[c - A]).isWord) {
                path.add(s.substring(idx, i + 1));
                dfs(s, i + 1);
                path.remove(path.size() - 1);
            }
        }
    }
	
    private void buildTrie(List<String> words) {
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
	
    class TrieNode {
        TrieNode[] children;
        boolean isWord;
		
        public TrieNode() {
            this.children = new TrieNode[26];
            this.isWord = false;
        }
    }
}
```

#### [149. 直线上最多的点数](https://leetcode.cn/problems/max-points-on-a-line/)

@2023.10.24

1. 暴力

枚举所有 2 个点的组，它们一定共线，再枚举第 3 个点，记录与前两点共线的个数，更新最大值。

```java
/**
 * 暴力
 * Somnia1337
 */
public int maxPoints(int[][] points)
{
	int len = points.length;
	if (len <= 2) return len;
	
	int ans = 0;
	for (int i = 0; i < len - 2; i++)
	{
		int x1 = points[i][0], y1 = points[i][1];
		for (int j = i + 1; j < len - 1; j++)
		{
			int x2 = points[j][0], y2 = points[j][1];
			
			int cur = 2; // 已有 2 点共线
			for (int k = j + 1; k < len; k++)
			{
				int x3 = points[k][0], y3 = points[k][1];
				if ((y3 - y1) * (x2 - x1) == (y2 - y1) * (x3 - x1)) cur++; // 3 点共线
			}
			ans = Math.max(cur, ans);
		}
	}
	
	return ans;
}
```

2. 哈希表

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int maxPoints(int[][] points)
{
	int len = points.length;
	if (len <= 2) return len;
	int ans = 0;
	for (int i = 0; i < len - 1; i++)
	{
		if (ans >= len - i || ans > len / 2) break;
		Map<Integer, Integer> map = new HashMap<>();
		for (int j = i + 1; j < len; j++)
		{
			int x = points[i][0] - points[j][0];
			int y = points[i][1] - points[j][1];
			if (x == 0) y = 1;
			else if (y == 0) x = 1;
			else
			{
				if (y < 0)
				{
					x = -x;
					y = -y;
				}
				int gcd = gcd(Math.abs(x), Math.abs(y));
				x /= gcd;
				y /= gcd;
			}
			int key = y + x * 20001;
			map.put(key, map.getOrDefault(key, 0) + 1);
		}
		int max = 0;
		for (int num : map.values())
		{
			max = Math.max(max, num + 1);
		}
		ans = Math.max(max, ans);
	}
	return ans;
}

private int gcd(int a, int b)
{
	return b != 0 ? gcd(b, a % b) : a;
}
```

#### [154. 寻找旋转排序数组中的最小值 II](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/)

@2023.12.10

#二分查找 

```java
/**
 * 二分查找
 * Krahets
 */
public int findMin(int[] stock) {
	int l = 0, r = stock.length - 1;
	while (l < r) {
		int m = l + r >> 1;
		if (stock[m] > stock[r]) l = m + 1;
		else if (stock[m] < stock[r]) r = m;
		else r--; // 无法判断, -1 缩小范围
	}
	return stock[l];
}
```

#### [174. 地下城游戏](https://leetcode.cn/problems/dungeon-game/)

@2023.11.01

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int calculateMinimumHP(int[][] dungeon)
{
	int rows = dungeon.length, cols = dungeon[0].length;
	int[][] dp = new int[rows + 1][cols + 1];
	for (int i = 0; i <= rows; ++i) Arrays.fill(dp[i], Integer.MAX_VALUE);
	dp[rows][cols - 1] = dp[rows - 1][cols] = 1;
	for (int i = rows - 1; i >= 0; i--)
	{
		for (int j = cols - 1; j >= 0; j--)
		{
			int min = Math.min(dp[i + 1][j], dp[i][j + 1]);
			dp[i][j] = Math.max(min - dungeon[i][j], 1);
		}
	}
	return dp[0][0];
}
```

#### [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

@2023.10.04

```java
/**
 * 动态规划
 * 千万利器莫过于信念
 */
public int maxProfit(int k, int[] prices)
{
	int len = prices.length;
	if (len <= 1) return 0;
	k = Math.min(k, len / 2);
	
	int[][][] dp = new int[len][2][k + 1];
	for (int n = 0; n <= k; n++)
	{
		dp[0][0][n] = 0;
		dp[0][1][n] = -prices[0];
	}
	
	for (int i = 1; i < len; i++)
	{
		for (int n = 1; n <= k; n++)
		{
			dp[i][0][n] = Math.max(dp[i - 1][0][n], dp[i - 1][1][n] + prices[i]);
			dp[i][1][n] = Math.max(dp[i - 1][1][n], dp[i - 1][0][n - 1] - prices[i]);
		}
	}
	return dp[len - 1][0][k];
}
```

#### [212. 单词搜索 II](https://leetcode.cn/problems/word-search-ii/)

@2023.10.22

#搜索 

先遍历一次 `board`，用 `Map<Character, List<List<Integer>>>` 记录每个字母出现的所有位置。

然后遍历 `words`，对每个单词，获取首字母在 `board` 中的所有位置，进行搜索前，两次剪枝：

- 遍历 `word`，如果存在未在 `board` 中出现的字母，必然搜不到，不必继续。
- 针对题目测试用例，对于开头多个字母相同的情况，如 `"aaaaab"`，将其反转后进行搜索。因为反转不会影响搜索的结果，且先搜索 `'b'` 显然更优。

后一条尤为重要，仅此一个反转，从超时优化到 17 ms 99%。

```java
/**
 * 回溯
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;
private boolean[][] vis;
private boolean[] found; // 记录 words 中已经找到的单词

public List<String> findWords(char[][] board, String[] words) {
    rows = board.length;
    cols = board[0].length;
    int n = words.length;
    found = new boolean[n];
    
    // 记录每个字母在 board 中的所有位置
    List<int[]>[] pos = new List[26];
    Arrays.setAll(pos, e -> new ArrayList<>());
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            pos[board[i][j] - 'a'].add(new int[]{i, j});
        }
    }
    
    vis = new boolean[rows][cols];
    for (int k = 0; k < n; k++) {
        String word = words[k];
        
        // 检查是否有可能搜索到
        boolean valid = true;
        for (char c : word.toCharArray()) {
            // 如果 board 中根本未出现 word 的某个字母, 则不可能搜索到
            if (pos[c - 'a'].isEmpty()) {
                valid = false;
                break;
            }
        }
        if (!valid) continue;
        
        // 剪枝: 如果开头出现很多重复字母, 如 "aaaaab", 则反转 word
        if (word.length() >= 5 && word.startsWith(word.substring(0, 1).repeat(3))) {
            StringBuilder rev = new StringBuilder(word);
            word = rev.reverse().toString();
        }
        
        // 以首字母出现的所有位置为起点, 开始搜索
        for (int[] xy : pos[word.charAt(0) - 'a']) {
            dfs(board, word, xy[0], xy[1], 0, k);
            // 剪枝: 已经找到, 停止搜索
            if (found[k]) break;
        }
    }
    
    // 添加到解集
    List<String> ans = new ArrayList<>();
    for (int k = 0; k < n; k++) {
        if (found[k]) ans.add(words[k]);
    }
    return ans;
}

private void dfs(char[][] b, String t, int x, int y, int idx, int k) {
    if (idx == t.length()) {
        found[k] = true;
        return;
    }
    if (!inBound(x, y) || vis[x][y] || b[x][y] != t.charAt(idx)) return;
    
    vis[x][y] = true;
    for (int p = 0; p < 4; p++) {
        dfs(b, t, x + dirs[p], y + dirs[p + 1], idx + 1, k);
        if (found[k]) break;
    }
    vis[x][y] = false;
}

private boolean inBound(int x, int y) {
    return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [214. 最短回文串](https://leetcode.cn/problems/shortest-palindrome/)

@2023.10.31

```java
/**
 * 字符串哈希
 * 力扣官方题解
 */
public String shortestPalindrome(String s)
{
	int len = s.length();
	int base = 131, mod = 1000000007;
	int l = 0, r = 0, mul = 1, best = -1;
	for (int i = 0; i < len; i++)
	{
		l = (int) (((long) l * base + s.charAt(i)) % mod);
		r = (int) ((r + (long) mul * s.charAt(i)) % mod);
		if (l == r) best = i;
		mul = (int) ((long) mul * base % mod);
	}
	String add = (best == len - 1 ? "" : s.substring(best + 1));
	StringBuilder ans = new StringBuilder(add).reverse();
	return ans.append(s).toString();
}
```

#### [224. 基本计算器](https://leetcode.cn/problems/basic-calculator/)

@2023.10.21

```java
/**
 * 栈
 * ChatGPT
 */
public int calculate(String s)
{
	Stack<Integer> stack = new Stack<>();
	int num = 0;
	int ans = 0;
	int sign = 1;
	
	for (char c : s.toCharArray())
	{
		if (Character.isDigit(c))
		{
			num = num * 10 + (c - '0');
		}
		else if (c == '+')
		{
			ans += sign * num;
			num = 0;
			sign = 1;
		}
		else if (c == '-')
		{
			ans += sign * num;
			num = 0;
			sign = -1;
		}
		else if (c == '(')
		{
			stack.push(ans);
			stack.push(sign);
			ans = 0;
			sign = 1;
		}
		else if (c == ')')
		{
			ans += sign * num;
			num = 0;
			ans *= stack.pop();
			ans += stack.pop();
		}
	}
	
	ans += sign * num;
	return ans;
}
```

#### [233. 数字 1 的个数](https://leetcode.cn/problems/number-of-digit-one/)

@2023.11.02

```java
/**
 * 数学
 * 力扣官方题解
 */
public int countDigitOne(int n)
{
	long mulk = 1;
	int ans = 0;
	while (n >= mulk)
	{
		ans += (int) ((n / (mulk * 10)) * mulk + Math.min(Math.max(n % (mulk * 10) - mulk + 1, 0), mulk));
		mulk *= 10;
	}
	return ans;
}
```

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

@2023.09.16

```java
/**
 * 单调队列
 * 力扣官方题解
 */
public int[] maxSlidingWindow(int[] nums, int k) {
	int n = nums.length;
	Deque<Integer> dq = new ArrayDeque<>();
	for (int i = 0; i < k; i++) {
		while (!dq.isEmpty() && nums[dq.peek()] < nums[i]) dq.poll();
		dq.push(i);
	}
	int[] ans = new int[n - k + 1];
	ans[0] = nums[dq.peekLast()];
	for (int i = k; i < n; i++) {
		if (dq.peekLast() < i - k + 1) dq.pollLast();
		while (!dq.isEmpty() && nums[dq.peek()] < nums[i]) dq.poll();
		dq.push(i);
		ans[i - k + 1] = nums[dq.peekLast()];
	}
	return ans;
}
```

#### [265. 粉刷房子 II](https://leetcode.cn/problems/paint-house-ii/)

@2023.11.01

#动态规划 

对 [1289. 下降路径最小和 II](https://leetcode.cn/problems/minimum-falling-path-sum-ii/) 略改。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minCostII(int[][] costs)
{
	int rows = costs.length, cols = costs[0].length;
	int[][] dp = new int[rows][cols];
	System.arraycopy(costs[0], 0, dp[0], 0, cols);
	
	for (int i = 1; i < rows; i++)
	{
		int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
		int idx = 0;
		for (int k = 0; k < cols; k++)
		{
			if (dp[i - 1][k] <= min1)
			{
				min2 = min1;
				min1 = dp[i - 1][k];
				idx = k;
			}
			else if (dp[i - 1][k] < min2)
			{
				min2 = dp[i - 1][k];
			}
		}
		for (int j = 0; j < cols; j++)
		{
			dp[i][j] = costs[i][j] + (j == idx ? min2 : min1);
		}
	}
	
	int ans = Integer.MAX_VALUE;
	for (int j = 0; j < cols; j++)
	{
		ans = Math.min(dp[rows - 1][j], ans);
	}
	return ans;
}
```

#### [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/)

@2023.10.25

```java
/**
 * 堆
 * Krahets
 */
class MedianFinder
{
    private Queue<Integer> left, right;
	
    public MedianFinder()
    {
        left = new PriorityQueue<>();
        right = new PriorityQueue<>(Collections.reverseOrder());
    }
	
    public void addNum(int num)
    {
        if (left.size() != right.size())
        {
            left.offer(num);
            right.offer(left.poll());
        }
        else
        {
            right.offer(num);
            left.offer(right.poll());
        }
    }
	
    public double findMedian()
    {
        return left.size() != right.size() ? left.peek() : (left.peek() + right.peek()) / 2.0;
    }
}
```

#### [296. 最佳的碰头地点](https://leetcode.cn/problems/best-meeting-point/)

@2023.11.17

#数学 

`x`、`y` 两方向独立，分别寻找中位数即为最佳点，再次遍历累加距离。

```java
/**
 * 数学
 * Somnia1337
 */
public int minTotalDistance(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	List<Integer> x = new ArrayList<>(), y = new ArrayList<>();
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (grid[i][j] == 1)
			{
				x.add(i);
				y.add(j);
			}
		}
	}
	Collections.sort(x);
	Collections.sort(y);
	int X = x.get(x.size() / 2), Y = y.get(y.size() / 2);
	int ans = 0;
	for (int xp : x) ans += Math.abs(xp - X);
	for (int yp : y) ans += Math.abs(yp - Y);
	return ans;
}
```

#### [297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)

@2023.11.19

#序列化 

输入 `[1,2,3,null,null,4,5]` 将被序列化为 `1 2 n n 3 4 n n 5 n n `，解码时按空格拆分，用成员变量 `i` 维护当前访问下标，递归调用添加左右子节点。

```java
/**
 * 序列化
 * 力扣新星
 */
class Codec
{
    private int i = 0;
	
    public String serialize(TreeNode root)
    {
        if (root == null) return "n ";
        return root.val + " " + serialize(root.left) + serialize(root.right);
    }
	
    public TreeNode deserialize(String data)
    {
        String[] vals = data.split(" ");
        return helper(vals);
    }
	
    private TreeNode helper(String[] vals)
    {
        if (vals[i].equals("n"))
        {
            i++;
            return null;
        }
        TreeNode ret = new TreeNode(Integer.parseInt(vals[i]));
        i++;
        ret.left = helper(vals);
        ret.right = helper(vals);
        return ret;
    }
}
```

#### [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)

@2023.12.29

```java
/**
 * 回溯
 * 彤哥来刷题啦
 */
private List<String> ans;
private StringBuilder path;

public List<String> removeInvalidParentheses(String s) {
	int lEx = 0, rEx = 0; // 需要删除的左括号数和右括号数
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if (c == '(') lEx++;
		else if (c == ')') {
			if (lEx == 0) rEx++;
			else lEx--;
		}
	}
	if (lEx == 0 && rEx == 0) return List.of(s); // 本身合法
	
	ans = new ArrayList<>();
	path = new StringBuilder();
	dfs(s, 0, 0, lEx, rEx, 0);
	return ans;
}

private void dfs(String s, int idx, int diff, int lEx, int rEx, int state) {
	if (idx == s.length()) {
		String str = path.toString();
		if (diff == 0 && check(str)) ans.add(str);
		return;
	}
	char c = s.charAt(idx);
	if (c == '(') {
		// 最低位为 1 说明前面的 ( 没要, 那这个 ( 也不能要
		if ((state & 1) == 0) {
			// 要这个 (
			path.append(c);
			dfs(s, idx + 1, diff + 1, lEx, rEx, 0);
			path.deleteCharAt(path.length() - 1);
		}
		
		// 不要这个 (
		// 不要这个 (, 后面的 ( 也不能要
		if (lEx > 0) dfs(s, idx + 1, diff, lEx - 1, rEx, state | 1);
	}
	else if (c == ')') {
		// 要这个 )
		// 倒数第二位为 1 说明前面的 ) 没要, 那这个 ) 也不能要
		if (diff > 0 && (state & 2) == 0) {
			path.append(c);
			dfs(s, idx + 1, diff - 1, lEx, rEx, 0);
			path.deleteCharAt(path.length() - 1);
		}
		
		// 不要这个 )
		// 不要这个 ), 后面的 ) 也不能要
		if (rEx > 0) dfs(s, idx + 1, diff, lEx, rEx - 1, state | 2);
	}
	else {
		path.append(c);
		dfs(s, idx + 1, diff, lEx, rEx, 0);
		path.deleteCharAt(path.length() - 1);
	}
}

// 检查是否合法
private boolean check(String s) {
	int l = 0;
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if (c == '(') l++;
		else if (c == ')') {
			if (l == 0) return false;
			else l--;
		}
	}
	return l == 0;
}
```

#### [302. 包含全部黑色像素的最小矩形](https://leetcode.cn/problems/smallest-rectangle-enclosing-black-pixels/)

@2023.11.01

#搜索 

```java
/**
 * DFS
 * Somnia1337
 */
private int u, d, l, r, rows, cols;

public int minArea(char[][] image, int x, int y)
{
	rows = image.length;
	cols = image[0].length;
	u = rows;
	d = 0;
	l = cols;
	r = 0;
	dfs(image, x, y);
	return (r - l + 1) * (d - u + 1);
}

private void dfs(char[][] image, int x, int y)
{
	if (!(x >= 0 && x < rows && y >= 0 && y < cols) || image[x][y] != '1') return;
	image[x][y] = '0';
	u = Math.min(x, u);
	d = Math.max(x, d);
	l = Math.min(y, l);
	r = Math.max(y, r);
	dfs(image, x - 1, y);
	dfs(image, x + 1, y);
	dfs(image, x, y - 1);
	dfs(image, x, y + 1);
}
```

#### [305. 岛屿数量 II](https://leetcode.cn/problems/number-of-islands-ii/)

@2024.01.13

#并查集 

初始时需将 `root` 填充 `-1`，表示为水域。按顺序建岛，枚举新岛四周的岛屿，如果原本不属于同一个岛，则连接岛。

```java
/**
 * 并查集
 * Somnia1337
 */
public List<Integer> numIslands2(int m, int n, int[][] positions) {
	final int[] dirs = {0, 1, 0, -1, 0};
	int[] root = new int[m * n];
	Arrays.fill(root, -1); // -1 表示水域
	List<Integer> ans = new ArrayList<>();
	int cnt = 0; // 当前岛屿数量
	for (int[] pos : positions) {
		int x = pos[0], y = pos[1], p = x * n + y; // p: 展成一维的坐标
		if (root[p] < 0) { // 建岛的位置为水域(有岛上岛用例)
			root[p] = p; // 根节点为自身
			cnt++;
			for (int k = 0; k < 4; k++) { // 枚举四周的岛
				int nx = x + dirs[k], ny = y + dirs[k + 1], np = nx * n + ny;
				// 为有效岛
				// 这里的条件不能用 np 限定, 必须 nx 和 ny 都满足
				if (nx >= 0 && nx < m && ny >= 0 && ny < n && root[np] >= 0) {
					if (find(root, p) != find(root, np)) { // 原本不是同一个岛
						cnt--; // 岛数量 -1
						union(root, p, np); // 合并两岛
					}
				}
			}
		}
		ans.add(cnt);
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

#### [312. 戳气球](https://leetcode.cn/problems/burst-balloons/)

@2023.10.21

```java
/**
 * 动态规划
 * labuladong
 */
public int maxCoins(int[] nums)
{
	int len = nums.length;
	int[] points = new int[len + 2];
	points[0] = points[len + 1] = 1;
	System.arraycopy(nums, 0, points, 1, len);
	int[][] dp = new int[len + 2][len + 2];
	
	for (int i = len; i >= 0; i--)
	{
		for (int j = i + 1; j < len + 2; j++)
		{
			for (int k = i + 1; k < j; k++)
			{
				dp[i][j] = Math.max(dp[i][k] + dp[k][j] + points[i] * points[j] * points[k], dp[i][j]);
			}
		}
	}
	
	return dp[0][len + 1];
}
```

#### [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)

@2023.11.01

```java
/**
 * 二分查找
 * powcai
 */
public List<Integer> countSmaller(int[] nums)
{
	List<Integer> position = new ArrayList<>();
	List<Integer> queue = new ArrayList<>();
	for (int i = nums.length - 1; i >= 0; i--)
	{
		int num = nums[i];
		int pos = findPos(queue, num);
		position.add(pos);
		queue.add(pos, num);
	}
	
	List<Integer> ans = new ArrayList<>();
	for (int i = position.size() - 1; i >= 0; i--) ans.add(position.get(i));
	return ans;
}

private int findPos(List<Integer> queue, int num)
{
	int l = 0, r = queue.size() - 1;
	while (l <= r)
	{
		int m = l + r >> 1;
		if (queue.get(m) < num) l = m + 1;
		else r = m - 1;
	}
	return l;
}
```

#### [329. 矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/)

@2023.12.17

```java
/**
 * 记忆化搜索
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int rows, cols;
private int[][] vis;

public int longestIncreasingPath(int[][] matrix) {
	rows = matrix.length;
	cols = matrix[0].length;
	vis = new int[matrix.length][matrix[0].length];
	int ans = 1;
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (vis[i][j] == 0) ans = Math.max(dfs(matrix, i, j), ans);
		}
	}
	return ans;
}

private int dfs(int[][] m, int x, int y) {
	if (vis[x][y] > 0) return vis[x][y];
	int ret = 0;
	for (int k = 0; k < 4; k++) {
		int nx = x + dirs[k], ny = y + dirs[k + 1];
		if ((nx >= 0 && nx < rows && ny >= 0 && ny < cols) && m[nx][ny] < m[x][y])
			ret = Math.max(dfs(m, nx, ny), ret);
	}
	vis[x][y] = ret + 1;
	return ret + 1;
}
```

#### [354. 俄罗斯套娃信封问题](https://leetcode.cn/problems/russian-doll-envelopes/)

@2023.11.02

```java
/**
 * 动态规划 + 二分查找
 * Somnia1337
 */
public int maxEnvelopes(int[][] envelopes)
{
	int len = envelopes.length;
	Arrays.sort(envelopes, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
	int[] h = new int[len];
	for (int i = 0; i < len; i++) h[i] = envelopes[i][1];
	return solve(h);
}

private int solve(int[] nums)
{
	int ans = 0;
	int[] arr = new int[nums.length];
	for (int num : nums)
	{
		int l = 0, r = ans;
		while (l < r)
		{
			int m = (l + r) / 2;
			if (arr[m] >= num) r = m;
			else l = m + 1;
		}
		if (l == ans) ans++;
		arr[l] = num;
	}
	return ans;
}
```

#### [391. 完美矩形](https://leetcode.cn/problems/perfect-rectangle/)

@2023.11.24

哈希表记录每个顶点的出现次数，大矩形的顶点各出现 1 次，其余顶点均出现偶数次，且全部小矩形的面积和为大矩形面积。

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isRectangleCover(int[][] rectangles)
{
	Map<String, Integer> pos = new HashMap<>();
	int areaSum = 0;
	for (int[] rectangle : rectangles)
	{
		pos.merge(rectangle[0] + " " + rectangle[1], 1, Integer::sum);
		pos.merge(rectangle[2] + " " + rectangle[3], 1, Integer::sum);
		pos.merge(rectangle[0] + " " + rectangle[3], 1, Integer::sum);
		pos.merge(rectangle[2] + " " + rectangle[1], 1, Integer::sum);
		areaSum += (rectangle[2] - rectangle[0]) * (rectangle[3] - rectangle[1]);
	}
	List<String> corners = new ArrayList<>();
	for (String p : pos.keySet())
	{
		int cnt = pos.get(p);
		if (cnt == 1) corners.add(p);
		else if ((cnt & 1) == 1) return false;
	}
	if (corners.size() != 4) return false;
	
	int[][] cor = new int[4][2];
	for (int i = 0; i < 4; i++)
	{
		String[] corner = corners.get(i).split(" ");
		for (int j = 0; j < 2; j++) cor[i][j] = Integer.parseInt(corner[j]);
	}
	Arrays.sort(cor, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
	return areaSum == (cor[3][0] - cor[0][0]) * (cor[3][1] - cor[0][1]);
}
```

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public boolean isRectangleCover(int[][] rectangles)
{
	long area = 0;
	int minX = rectangles[0][0], minY = rectangles[0][1], maxX = rectangles[0][2], maxY = rectangles[0][3];
	Map<Point, Integer> cnt = new HashMap<>();
	for (int[] rect : rectangles)
	{
		int x = rect[0], y = rect[1], a = rect[2], b = rect[3];
		area += (long) (a - x) * (b - y);
		
		minX = Math.min(minX, x);
		minY = Math.min(minY, y);
		maxX = Math.max(maxX, a);
		maxY = Math.max(maxY, b);
		
		Point point1 = new Point(x, y);
		Point point2 = new Point(x, b);
		Point point3 = new Point(a, y);
		Point point4 = new Point(a, b);
		
		cnt.put(point1, cnt.getOrDefault(point1, 0) + 1);
		cnt.put(point2, cnt.getOrDefault(point2, 0) + 1);
		cnt.put(point3, cnt.getOrDefault(point3, 0) + 1);
		cnt.put(point4, cnt.getOrDefault(point4, 0) + 1);
	}
	
	Point pointMinMin = new Point(minX, minY);
	Point pointMinMax = new Point(minX, maxY);
	Point pointMaxMin = new Point(maxX, minY);
	Point pointMaxMax = new Point(maxX, maxY);
	if (area != (long) (maxX - minX) * (maxY - minY) || cnt.getOrDefault(pointMinMin, 0) != 1 || cnt.getOrDefault(pointMinMax, 0) != 1 || cnt.getOrDefault(pointMaxMin, 0) != 1 || cnt.getOrDefault(pointMaxMax, 0) != 1) return false;
	
	cnt.remove(pointMinMin);
	cnt.remove(pointMinMax);
	cnt.remove(pointMaxMin);
	cnt.remove(pointMaxMax);

	for (Map.Entry<Point, Integer> entry : cnt.entrySet())
	{
		int value = entry.getValue();
		if (value != 2 && value != 4) return false;
	}
	return true;
}

class Point
{
	int x;
	int y;

	public Point(int x, int y)
	{
		this.x = x;
		this.y = y;
	}

	@Override
	public int hashCode()
	{
		return x * 97 + y;
	}

	@Override
	public boolean equals(Object obj)
	{
		if (obj instanceof Point point2)
		{
			return this.x == point2.x && this.y == point2.y;
		}
		return false;
	}
}
```

#### [403. 青蛙过河](https://leetcode.cn/problems/frog-jump/)

@2023.11.24

#动态规划 

`dp[i][k]` 表示是否可以在上一步跳 `k` 到达石头 `i`，正序遍历 `stones`，嵌套倒序遍历之间的石头，记两石头的距离为 `k`，从 `j` 到 `i` 只能跳 `k - 1` / `k` / `k + 1` 步，状态转移 `dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1]`。

剪枝：最佳情况是跳 1、2、3 … 步，也即第 `j` 个石头最多跳 `j + 1` 步，因此当 `k > j + 1` 时可停止循环。

```java
/**
 * 动态规划
 * livorth
 */
public boolean canCross(int[] stones) {
    int n = stones.length;
    boolean[][] dp = new boolean[n][n];
    dp[0][0] = true;
    for (int i = 1; i < n; i++) {
        for (int j = i - 1; j >= 0; j--) {
            int k = stones[i] - stones[j];
            if (k > j + 1) break;
            dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
            if (i == n - 1 && dp[i][k]) return true;
        }
    }
    return false;
}
```

#### [410. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/)

@2023.11.02

#二分查找 #动态规划 

1. 二分查找

二分答案，检查对应的分块数，与 `k` 比较调整边界。

```java
/**
 * 二分查找
 * Somnia1337
 */
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

2. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
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

#### [440. 字典序的第K小数字](https://leetcode.cn/problems/k-th-smallest-in-lexicographical-order/)

@2023.12.28

#字典树 

将全部数字想象成一棵 10 叉树，如果 `preorder()` 这棵树，得到的序列就是按字典序排列的：

```text
	    1                  2                  3 ...
	  /   \              /   \
	10 ... 19          20 ... 29
   /  \      \
100 .. 109 .. 199
```

每次在树上可以选择向右或向下移动，总共移动 `k - 1` 步就找到了答案（数字从 `1` 开始）。具体地，记以当前所在结点为根的子树中有效结点的数量为 `total`：

- 如果 `total > k`，说明答案就在当前子树中，向下走 `1` 步。
- 如果 `total <= k`，说明答案在右侧某个兄弟结点的子树中，本树的结点都没有用了，向右走 `total` 步。

```java
/**
 * DFS
 * 郭郭
 */
public int findKthNumber(int n, int k) {
	long cur = 1; // 从 1 出发
	k--; // 消除 1 的偏差
	while (k > 0) {
		int total = getTotal(n, cur); // 当前结点子树中有效数字的数量
		if (total <= k) { // 向右走 total 步
			k -= total;
			cur++;
		}
		else { // 向下走 1 步
			k--;
			cur *= 10;
		}
	}
	return (int) cur; // 最后 cur 停在答案上
}

// 计算以 cur 为根的子树的有效结点数量
private int getTotal(int n, long cur) {
	long next = cur + 1, total = 0; // next: 当前节点右侧右边节点的值
	while (cur <= n) {
		total += Math.min(n - cur + 1, next - cur);
		next *= 10;
		cur *= 10;
	}
	return (int) total;
}
```

#### [446. 等差数列划分 II - 子序列](https://leetcode.cn/problems/arithmetic-slices-ii-subsequence/)

@2023.10.16

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int numberOfArithmeticSlices(int[] nums)
{
	int len = nums.length;
	Map<Long, Integer>[] count = new Map[len];
	
	int ans = 0;
	for (int i = 0; i < len; i++)
	{
		count[i] = new HashMap<>();
		for (int j = 0; j < i; j++)
		{
			long diff = (long) nums[i] - nums[j];
			int cnt = count[j].getOrDefault(diff, 0);
			ans += cnt;
			count[i].merge(diff, cnt + 1, Integer::sum);
		}
	}
	
	return ans;
}
```

#### [458. 可怜的小猪](https://leetcode.cn/problems/poor-pigs/)

@2023.11.19

#数学 

以 `minutesToTest = 60`，`minutesToDie = 15` 为例：

- 1 只猪在 `60` 分钟内可喝 `4` 次水，但是可以检验 `5` 桶，因为如果前 `4` 次都没死，第 `5` 桶水就有毒。
- 2 只猪可检验 `25` 桶水，一只喝每行的 `5` 桶混合，另一只喝每列的 `5` 桶混合。

![[458. 可怜的小猪.png|300]]

```java
/**
 * 数学
 * powcai
 */
public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
	int ans = 0;
	while (Math.pow(minutesToTest / minutesToDie + 1, ans) < buckets) ans++;
	return ans;
}
```

#### [460. LFU 缓存](https://leetcode.cn/problems/lfu-cache/)

@2023.09.25

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
class LFUCache {
    private int cap, minFreq;
    private Map<Integer, Node> keyToNode;
    private Map<Integer, Node> freqToDummy;
	
    public LFUCache(int capacity) {
        cap = capacity;
        minFreq = 0;
        keyToNode = new HashMap<>();
        freqToDummy = new HashMap<>();
    }
	
    public int get(int key) {
        Node node = getNode(key);
        return node != null ? node.value : -1;
    }
	
    public void put(int key, int value) {
        Node node = getNode(key);
        if (node != null) { // 有这本书
            node.value = value; // 更新 value
            return;
        }
        if (keyToNode.size() == cap) { // 书太多了
            Node dummy = freqToDummy.get(minFreq);
            Node backNode = dummy.prev; // 最左边那摞书的最下面的书
            keyToNode.remove(backNode.key);
            remove(backNode); // 移除
            if (dummy.prev == dummy) freqToDummy.remove(minFreq); // 这摞书是空的, 移除
        }
        node = new Node(key, value); // 新书
        keyToNode.put(key, node);
        pushFront(1, node); // 放在「看过 1 次」的最上面
        minFreq = 1;
    }
	
    private Node getNode(int key) {
        if (!keyToNode.containsKey(key)) return null; // 没有这本书
        Node node = keyToNode.get(key); // 有这本书
        remove(node); // 把这本书抽出来
        Node dummy = freqToDummy.get(node.freq);
        if (dummy.prev == dummy) { // 抽出来后, 这摞书是空的
            freqToDummy.remove(node.freq); // 移除空链表
            if (minFreq == node.freq) minFreq++;
        }
        pushFront(++node.freq, node); // 放在右边这摞书的最上面
        return node;
    }
	
    // 创建双向链表
    private Node newList() {
        Node dummy = new Node(0, 0); // 哨兵节点
        dummy.prev = dummy;
        dummy.next = dummy;
        return dummy;
    }
	
    // 在链表头添加 1 个节点 (把 1 本书放在最上面)
    private void pushFront(int freq, Node x) {
        Node dummy = freqToDummy.computeIfAbsent(freq, k -> newList());
        x.prev = dummy;
        x.next = dummy.next;
        x.prev.next = x;
        x.next.prev = x;
    }
	
    // 删除 1 个节点 (抽出 1 本书)
    private void remove(Node x) {
        x.prev.next = x.next;
        x.next.prev = x.prev;
    }
	
    private static class Node {
        int key, value, freq = 1; // 新书只读了 1 次
        Node prev, next;
        
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
}
```

#### [466. 统计重复个数](https://leetcode.cn/problems/count-the-repetitions/)

@2024.01.02

```java
/**
 * 动态规划
 * puck1609
 */
public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
	char[] chs1 = s1.toCharArray(), chs2 = s2.toCharArray();
	int l2 = chs2.length;
	int[] dp = new int[l2];
	for (int i = 0; i < l2; i++) {
		int j = i;
		for (char c1 : chs1) {
			if (c1 == chs2[j]) {
				j = (j + 1) % l2;
				dp[i]++;
			}
		}
	}
	int cnt = 0, p = 0;
	for (int i = 0; i < n1; i++) {
		cnt += dp[p];
		p = (p + dp[p]) % l2;
	}
	return cnt / l2 / n2;
}
```

```java
/**
 * ?
 * ?
 */
public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
	char[] chs1 = s1.toCharArray(), chs2 = s2.toCharArray();
	int n = chs2.length, c1 = 0, c2 = 0, p = 0;
	Map<Integer, int[]> map = new HashMap<>();
	while (c1 < n1) {
		for (char c : chs1) {
			if (c == chs2[p]) p++;
			if (p == n) {
				p = 0;
				c2++;
			}
		}
		c1++;
		if (!map.containsKey(p)) map.put(p, new int[]{c1, c2});
		else {
			int[] pre = map.get(p);
			int circ1 = c1 - pre[0], circ2 = c2 - pre[1];
			c2 += circ2 * ((n1 - c1) / circ1);
			c1 += circ1 * ((n1 - c1) / circ1);
		}
	}
	return c2 / n2;
}
```

#### [514. 自由之路](https://leetcode.cn/problems/freedom-trail/)

@2024.01.29



```java
/**
 * 记忆化搜索
 * 灵茶山艾府
 */
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

#### [568. 最大休假天数](https://leetcode.cn/problems/maximum-vacation-days/)

@2024.02.05



```java
/**
 * 记忆化搜索
 * Chika Takami
 */
private int n;
private int[][] f, d, m;

public int maxVacationDays(int[][] flights, int[][] days) {
	n = days.length;
	f = flights;
	d = days;
	m = new int[n][days[0].length];
	return dfs(0, 0);
}

private int dfs(int t, int idx) {
	if (idx == m[0].length) return 0;
	if (m[t][idx] > 0) return m[t][idx];
	for (int i = 0; i < n; i++) {
		if (i == t || f[t][i] == 1) {
			m[t][idx] = Math.max(dfs(i, idx + 1) + d[i][idx], m[t][idx]);
		}
	}
	return m[t][idx];
}
```

#### [630. 课程表 III](https://leetcode.cn/problems/course-schedule-iii/description/)

@2023.09.11

#贪心 

反悔贪心，按 `lastDay` 升序排序，大顶堆维护所有已选课程的 `duration`，`day` 维护修读完所有已选课程的天数，遍历 `courses`：

- 如果当前课程 `duration + day <= lastDay`，贪心地修读。
- 否则，检查之前已选的 `duration` 最大的课程，如果当前课程更短，则进行反悔：取消之前那门课程，转而修读当前课程。这样，更可能用节省的时间修读更多课程。

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int scheduleCourse(int[][] courses) {
	Arrays.sort(courses, Comparator.comparingInt(a -> a[1]));
	Queue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
	int day = 0;
	for (int[] c : courses) {
		int duration = c[0], lastDay = c[1];
		if (day + duration <= lastDay) {
			day += duration;
			pq.offer(duration);
		}
		else if (!pq.isEmpty() && duration < pq.peek()) {
			day -= pq.poll() - duration;
			pq.offer(duration);
		}
	}
	return pq.size();
}
```

#### [660. 移除 9](https://leetcode.cn/problems/remove-9/)

@2023.09.14

#数学 

观察移除 9 后的数字：

```text
1, 2, 3, 4, 5, 6, 7, 8, 10,
11, 12, 13, 14, 15, 16, 17, 18, 20,
...
80, 81, 82, 83, 84, 85, 86, 87, 88,
100, 101, 102,
...
```

发现数字符合 9 进制。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int newInteger(int n) {
	return Integer.parseInt(Integer.toString(n, 9));
}
```

#### [679. 24 点游戏](https://leetcode.cn/problems/24-game/)

@2023.12.29

```java
/**
 * 回溯
 * airmelt
 */
private static final double TAR = 24;
private static final double EPISLON = 1e-6;

public boolean judgePoint24(int[] cards) {
	return helper(new double[]{cards[0], cards[1], cards[2], cards[3]});
}

private boolean helper(double[] arr) {
	if (arr.length == 1) return Math.abs(arr[0] - TAR) < EPISLON;
	for (int i = 0; i < arr.length; i++) {
		for (int j = i + 1; j < arr.length; j++) {
			double[] next = new double[arr.length - 1];
			for (int k = 0, pos = 0; k < arr.length; k++) if (k != i && k != j) next[pos++] = arr[k];
			for (double num : cal(arr[i], arr[j])) {
				next[next.length - 1] = num;
				if (helper(next)) return true;
			}
		}
	}
	return false;
}

private List<Double> cal(double a, double b) {
	List<Double> list = new ArrayList<>();
	list.add(a + b);
	list.add(a - b);
	list.add(b - a);
	list.add(a * b);
	if (!(Math.abs(b) < EPISLON)) list.add(a / b);
	if (!(Math.abs(a) < EPISLON)) list.add(b / a);
	return list;
}
```

#### [689. 三个无重叠子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-3-non-overlapping-subarrays/)

@2023.11.19

```java
/**
 * 动态规划
 * 宫水三叶
 */
public int[] maxSumOfThreeSubarrays(int[] nums, int k)
{
	int len = nums.length;
	long[] sum = new long[len + 1];
	for (int i = 1; i <= len; i++) sum[i] = sum[i - 1] + nums[i - 1];
	long[][] dp = new long[len + 10][4];
	for (int i = len - k + 1; i >= 1; i--)
	{
		for (int j = 1; j < 4; j++)
		{
			dp[i][j] = Math.max(dp[i + k][j - 1] + sum[i + k - 1] - sum[i - 1], dp[i + 1][j]);
		}
	}
	int[] ans = new int[3];
	int i = 1, j = 3, idx = 0;
	while (j > 0)
	{
		if (dp[i + 1][j] > dp[i + k][j - 1] + sum[i + k - 1] - sum[i - 1]) i++;
		else
		{
			ans[idx++] = i - 1;
			i += k;
			j--;
		}
	}
	return ans;
}
```

```java
/**
 * 动态规划
 * ?
 */
public int[] maxSumOfThreeSubarrays(int[] nums, int k)
{
	int len = nums.length, m = len - k + 1;
	int[] sums = new int[m];
	int sum = 0;
	for (int i = 0; i < k; i++) sum += nums[i];
	sums[0] = sum;
	for (int i = k; i < len; i++)
	{
		sum += nums[i];
		sum -= nums[i - k];
		sums[i - k + 1] = sum;
	}
	
	int[] l = new int[m];
	int maxIdx = 0;
	for (int i = 0; i < m; i++)
	{
		if (sums[i] > sums[maxIdx]) maxIdx = i;
		l[i] = maxIdx;
	}
	
	int[] r = new int[m];
	maxIdx = m - 1;
	for (int i = m - 1; i >= 0; i--)
	{
		if (sums[i] >= sums[maxIdx]) maxIdx = i;
		r[i] = maxIdx;
	}
	
	int max = 0;
	for (int i = k; i < m - k; i++)
	{
		int cur = sums[i] + sums[l[i - k]] + sums[r[i + k]];
		if (cur > max)
		{
			max = cur;
			maxIdx = i;
		}
	}
	
	return new int[]{l[maxIdx - k], maxIdx, r[maxIdx + k]};
}
```

#### [711. 不同岛屿的数量 II](https://leetcode.cn/problems/number-of-distinct-islands-ii/)

@2024.02.25

什么鸟题。

```java
/**
 * DFS + 图像哈希
 * xialeistudio
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private int[][] grid;
private int rows, cols;

public int numDistinctIslands2(int[][] grid) {
	this.grid = grid;
	rows = grid.length;
	cols = grid[0].length;
	Set<String> islands = new HashSet<>();
	for (int r = 0; r < grid.length; r++) {
		for (int c = 0; c < grid[0].length; c++) {
			if (grid[r][c] == 1) {
				List<int[]> list = new ArrayList<>();
				dfs(r, c, list);
				islands.add(hash(list));
			}
		}
	}
	return islands.size();
}

private String hash(List<int[]> shape) {
	List<UnaryOperator<int[]>> ops = new ArrayList<>();
	ops.add(p -> new int[]{p[0], p[1]});
	ops.add(p -> new int[]{p[1], -p[0]});
	ops.add(p -> new int[]{-p[0], -p[1]});
	ops.add(p -> new int[]{-p[1], p[0]});
	ops.add(p -> new int[]{p[0], -p[1]});
	ops.add(p -> new int[]{-p[0], p[1]});
	ops.add(p -> new int[]{p[1], p[0]});
	ops.add(p -> new int[]{-p[1], -p[0]});
	String ans = makeHash(shape, ops.get(0));
	for (int i = 1; i < ops.size(); i++) {
		String cur = makeHash(shape, ops.get(i));
		if (cur.compareTo(ans) < 0) ans = cur;
	}
	return ans;
}

private String makeHash(List<int[]> shape, UnaryOperator<int[]> op) {
	int[][] hash = new int[shape.size()][2];
	int minX = Integer.MAX_VALUE;
	int minY = Integer.MAX_VALUE;
	for (int i = 0; i < shape.size(); i++) {
		hash[i] = op.apply(shape.get(i));
		minX = Math.min(minX, hash[i][0]);
		minY = Math.min(minY, hash[i][1]);
	}
	for (int i = 0; i < hash.length; i++) {
		hash[i][0] -= minX;
		hash[i][1] -= minY;
	}
	Arrays.sort(hash, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
	return Arrays.deepToString(hash);
}

private void dfs(int x, int y, List<int[]> list) {
	if (!inBound(x, y)) return;
	if (grid[x][y] != 1) return;
	grid[x][y] = 0;
	list.add(new int[]{x, y});
	for (int k = 0; k < 4; k++) dfs(x + dirs[k], y + dirs[k + 1], list);
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [715. Range 模块](https://leetcode.cn/problems/range-module/)

@2023.11.12

```java
/**
 * 线段树
 * LFool⚡
 */
class RangeModule
{
    private int N = (int) 1e9;
    private Node root = new Node();
	
    public RangeModule()
    {
	
    }
	
    public void addRange(int left, int right)
    {
        update(root, 1, N, left, right - 1, 1);
    }
	
    public boolean queryRange(int left, int right)
    {
        return query(root, 1, N, left, right - 1);
    }
	
    public void removeRange(int left, int right)
    {
        update(root, 1, N, left, right - 1, -1);
    }
	
    public void update(Node node, int start, int end, int l, int r, int val)
    {
        if (l <= start && end <= r)
        {
            node.cover = val == 1;
            node.add = val;
            return;
        }
        int mid = (start + end) >> 1;
        pushDown(node);
        if (l <= mid) update(node.left, start, mid, l, r, val);
        if (r > mid) update(node.right, mid + 1, end, l, r, val);
        pushUp(node);
    }
	
    public boolean query(Node node, int start, int end, int l, int r)
    {
        if (l <= start && end <= r) return node.cover;
        int mid = (start + end) >> 1;
        pushDown(node);
        boolean ans = true;
        if (l <= mid) ans = ans && query(node.left, start, mid, l, r);
        if (r > mid) ans = ans && query(node.right, mid + 1, end, l, r);
        return ans;
    }
	
    private void pushUp(Node node)
    {
        node.cover = node.left.cover && node.right.cover;
    }
	
    private void pushDown(Node node)
    {
        if (node.left == null) node.left = new Node();
        if (node.right == null) node.right = new Node();
        if (node.add == 0) return;
        node.left.cover = node.add == 1;
        node.right.cover = node.add == 1;
        node.left.add = node.add;
        node.right.add = node.add;
        node.add = 0;
    }
	
    class Node
    {
        Node left, right;
        boolean cover;
        int add;
    }
}
```

#### [765. 情侣牵手](https://leetcode.cn/problems/couples-holding-hands/)

@2023.11.11

#贪心 

遍历每对位置的左边，用异或获取情侣的另一个人，如果右侧不是目标，就向后查找，交换。

贪心之处在于每次交换都能配对一对情侣，即每次交换都是有效的，因此交换次数一定最少。

```java
/**
 * 贪心
 * xPatrL
 */
public int minSwapsCouples(int[] row)
{
	int ans = 0;
	for (int i = 0; i < row.length; i += 2)
	{
		int x = row[i], y = x ^ 1;
		if (row[i + 1] != y)
		{
			for (int j = i + 2; j < row.length; j++)
			{
				if (row[j] == y)
				{
					swap(row, i + 1, j);
					ans++;
					break;
				}
			}
		}
	}
	return ans;
}

private void swap(int[] arr, int i, int j)
{
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [773. 滑动谜题](https://leetcode.cn/problems/sliding-puzzle/)

@2024.01.20



```java
/**
 * BFS
 * 张小马
 */
public int slidingPuzzle(int[][] board) {
	final int[][] dirs = {{1, 3}, {0, 2, 4}, {1, 5}, {0, 4}, {1, 3, 5}, {2, 4}};
	StringBuilder s = new StringBuilder();
	for (int i = 0; i < 6; i++) s.append((char) ('0' + board[i / 3][i % 3]));
	String cur = s.toString(), end = "123450";
	if (cur.equals(end)) return 0;
	Queue<String> q = new LinkedList<>();
	q.offer(cur);
	Set<String> vis = new HashSet<>();
	vis.add(cur);
	int step = 1;
	while (!q.isEmpty()) {
		int size = q.size();
		while (size-- > 0) {
			cur = q.poll();
			for (int i = 0; i < 6; i++) {
				if (cur.charAt(i) == '0') {
					for (int j : dirs[i]) {
						String next = getNext(cur, i, j);
						if (next.equals(end)) return step;
						if (!vis.add(next)) continue;
						q.offer(next);
					}
					break;
				}
			}
		}
		step++;
	}
	return -1;
}

private String getNext(String cur, int i, int j) {
	char[] nextVal = cur.toCharArray();
	swap(nextVal, i, j);
	return new String(nextVal);
}

private void swap(char[] arr, int i, int j) {
	char t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [774. 最小化去加油站的最大距离](https://leetcode.cn/problems/minimize-max-distance-to-gas-station/)

@2024.01.20



```java
/**
 * 二分查找
 * Somnia1337
 */
public double minmaxGasDist(int[] stations, int k) {
	final double SIGMA = 0.000001;
	double l = 0, r = Double.MIN_VALUE;
	for (int i = 1; i < stations.length; i++) r = Math.max(stations[i] - stations[i - 1], r);
	while (l - r < SIGMA) {
		double m = (l + r) / 2;
		if (check(stations, k, m)) r = m - SIGMA;
		else l = m + SIGMA;
	}
	return r;
}

private boolean check(int[] s, int k, double m) {
	for (int i = 1; i < s.length; i++) {
		if ((k -= (int) ((s[i] - s[i - 1]) / m)) < 0) return false;
	}
	return true;
}
```

#### [778. 水位上升的泳池中游泳](https://leetcode.cn/problems/swim-in-rising-water/)

@2023.10.16

#二分查找 #搜索 

二分查找时刻，DFS 检查在该时刻是否有通路通向终点。

```java
/**
 * 二分查找
 * Somnia1337
 */
private boolean[][] visited;

public int swimInWater(int[][] grid)
{
	int len = grid.length;
	int left = 0, right = len * len - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		visited = new boolean[len][len];
		if (dfs(grid, mid, 0, 0)) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}

private boolean dfs(int[][] grid, int time, int i, int j)
{
	if (!(i >= 0 && i < grid.length && j >= 0 && j < grid.length) || grid[i][j] > time || visited[i][j]) return false;
	if (i == grid.length - 1 && j == grid.length - 1) return true;
	visited[i][j] = true;
	return dfs(grid, time, i - 1, j) || dfs(grid, time, i + 1, j) || dfs(grid, time, i, j - 1) || dfs(grid, time, i, j + 1);
}
```

#### [827. 最大人工岛](https://leetcode.cn/problems/making-a-large-island/)

@2023.10.19

```java
/**
 * DFS
 * Somnia1337
 */
private int cur;

public int largestIsland(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	List<Integer> areas = new ArrayList<>(List.of(0, 0));
	int num = 2;
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (grid[i][j] == 1)
			{
				cur = 0;
				dfs(grid, i, j, num++);
				areas.add(cur);
			}
		}
	}
	
	int ans = 0;
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (grid[i][j] == 0)
			{
				Set<Integer> set = new HashSet<>();
				set.add(i >= 1 ? grid[i - 1][j] : 0);
				set.add(i < rows - 1 ? grid[i + 1][j] : 0);
				set.add(j >= 1 ? grid[i][j - 1] : 0);
				set.add(j < cols - 1 ? grid[i][j + 1] : 0);
				int curr = 0;
				for (int k : set) curr += areas.get(k);
				ans = Math.max(curr + 1, ans);
			}
		}
	}
	
	return ans > 0 ? ans : rows * cols;
}

private void dfs(int[][] grid, int x, int y, int num)
{
	if (x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] != 1) return;
	grid[x][y] = num;
	cur++;
	dfs(grid, x + 1, y, num);
	dfs(grid, x, y + 1, num);
	dfs(grid, x - 1, y, num);
	dfs(grid, x, y - 1, num);
}
```

```java
/**
 * DFS
 * Somnia1337
 */
public int largestIsland(int[][] grid)
{
	int n = grid.length;
	int m = grid[0].length;
	int id = 2;
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < m; j++)
		{
			if (grid[i][j] == 1)
			{
				dfs(grid, n, m, i, j, id++);
			}
		}
	}
	int[] sizes = new int[id];
	int ans = 0;
	for (int[] row : grid)
	{
		for (int j = 0; j < m; j++)
		{
			if (row[j] > 1)
			{
				ans = Math.max(ans, ++sizes[row[j]]);
			}
		}
	}
	
	boolean[] visited = new boolean[id];
	int up, down, left, right, merge;
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < m; j++)
		{
			if (grid[i][j] == 0)
			{
				up = i > 0 ? grid[i - 1][j] : 0;
				down = i + 1 < n ? grid[i + 1][j] : 0;
				left = j > 0 ? grid[i][j - 1] : 0;
				right = j + 1 < m ? grid[i][j + 1] : 0;
				visited[up] = true;
				merge = 1 + sizes[up];
				if (!visited[down])
				{
					merge += sizes[down];
					visited[down] = true;
				}
				if (!visited[left])
				{
					merge += sizes[left];
					visited[left] = true;
				}
				if (!visited[right])
				{
					merge += sizes[right];
					visited[right] = true;
				}
				ans = Math.max(ans, merge);
				visited[up] = false;
				visited[down] = false;
				visited[left] = false;
				visited[right] = false;
			}
		}
	}
	return ans;
}

private void dfs(int[][] grid, int n, int m, int i, int j, int id)
{
	if (i < 0 || i == n || j < 0 || j == m || grid[i][j] != 1)
	{
		return;
	}
	grid[i][j] = id;
	dfs(grid, n, m, i + 1, j, id);
	dfs(grid, n, m, i - 1, j, id);
	dfs(grid, n, m, i, j + 1, id);
	dfs(grid, n, m, i, j - 1, id);
}
```

#### [828. 统计子串中的唯一字符](https://leetcode.cn/problems/count-unique-characters-of-all-substrings-of-a-given-string/)

@2023.11.26

#变化量 

![[828. 统计子串中的唯一字符.png|500]]

对于以最末一个 `'A'` 结尾的子串，其起点在——

- 绿区，第 1 次出现 `'A'`，唯一字符数加 1。
- 红区，第 2 次出现 `'A'`，唯一字符数减 1。
- 白区，第 2 次以上出现 `'A'`，唯一字符数不变。

用两个 `int[26]`，`l1` 和 `l2`，分别维护每个字母上次、上上次出现的位置，更新当前子串组的唯一字符数之和，累加答案。

```java
/**
 * 变化量
 * 灵茶山艾府
 */
public int uniqueLetterString(String s)
{
	int cur = 0, ans = 0;
	int[] l1 = new int[26], l2 = new int[26];
	Arrays.fill(l1, -1);
	Arrays.fill(l2, -1);
	for (int i = 0; i < s.length(); i++)
	{
		int d = s.charAt(i) - 'A';
		cur += i - 2 * l1[d] + l2[d];
		ans += cur;
		l2[d] = l1[d];
		l1[d] = i;
	}
	return ans;
}
```

#### [829. 连续整数求和](https://leetcode.cn/problems/consecutive-numbers-sum/)

@2024.01.15



```java
/**
 * 数学
 * 京城打工人
 */
public int consecutiveNumbersSum(int n) {
	int ans = 0;
	for (int k = 1; k * k < 2 * n; k++) {
		if (2 * n % k == 0 && ((2 * n / k - k + 1) & 1) == 0) ans++;
	}
	return ans;
}
```

#### [839. 相似字符串组](https://leetcode.cn/problems/similar-string-groups/)

@2024.01.13

#并查集 

数据范围仅 `300`，枚举每对字符串，如果相似则合并分量，支数即为答案。

```java
/**
 * 并查集
 * Somnia1337
 */
public int numSimilarGroups(String[] strs) {
	int n = strs.length;
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int i = 1; i < n; i++) {
		for (int j = i - 1; j >= 0; j--) {
			if (sim(strs[i], strs[j])) union(root, i, j);
		}
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (root[i] == i) ans++;
	}
	return ans;
}

// 检查两字符串是否相似
// 题目保证所有串互为字母异位词, 只需检查不同位置数 <= 2
private boolean sim(String a, String b) {
	if (a.equals(b)) return true;
	int diff = 0;
	for (int i = 0, l = a.length(); i < l; i++) {
		if (a.charAt(i) != b.charAt(i) && ++diff == 3) return false;
	}
	return true;
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

#### [871. 最低加油次数](https://leetcode.cn/problems/minimum-number-of-refueling-stops/)

@2024.01.20

用大顶堆维护每个加油站提供的油量，模拟，每次将油箱清空并加到里程，同时将经过的加油站的油量入堆，如果仍未到达终点，选择之前到达过的油量最多的加油站并计一次加油。

```java
/**
 * 堆
 * 宫水三叶
 */
public int minRefuelStops(int target, int startFuel, int[][] stations) {
	Queue<Integer> q = new PriorityQueue<>(Collections.reverseOrder());
	// rem: 剩余油量 // pos: 当前位置
	int i = 0, rem = startFuel, pos = 0, ans = 0;
	while (pos < target) {
		if (rem == 0) {
			if (!q.isEmpty()) {
				rem += q.poll(); // 加最多的油
				ans++;
			}
			else return -1; // 无法加油
		}
		pos += rem;
		rem = 0; // 油量清空, 变成里程
		while (i < stations.length && stations[i][0] <= pos) q.offer(stations[i++][1]); // 将经过的加油站入堆
	}
	return ans;
}
```

#### [878. 第 N 个神奇数字](https://leetcode.cn/problems/nth-magical-number/)

@2024.01.11

#二分查找 #数学 

![[878. 第 N 个神奇数字.png|400]]

```java
/**
 * 二分查找
 * 灵茶山艾府
 */
public int nthMagicalNumber(int n, int a, int b) {
	final int MOD = 1000000007;
	long L = (long) a / gcd(a, b) * b;
	long l = 1, r = (long) Math.min(a, b) * n;
	while (l <= r) {
		long m = l + ((r - l) >> 1);
		if (m / a + m / b - m / L >= n) r = m - 1;
		else l = m + 1;
	}
	return (int) (l % MOD);
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### [902. 最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)

@2023.09.18

1. 回溯

```java
for (String digit : digits)
{
	path.append(digit);
	if (path.charAt(path.length() - 1) < nStr.charAt(path.length() - 1))
	{
		ans += (int) Math.pow(digits.length, len - path.length());
	}
	else if (check(path.toString(), nStr))
	{
		bt(digits, nStr, len);
	}
	path.deleteCharAt(path.length() - 1);
}
```

位数小于`n`的部分直接计算`pow(digits.length, i)`，等于`n`的部分使用回溯。

剪枝：

- 如果当前位小于`n`的对应位，后续位可以任意组合而都小于`n`，可以直接计算。
- 如果当前位大于`n`的对应位，后续为无论如何组合都会大于`n`，无需继续搜索。

```java
/**
 * 回溯
 * Somnia1337
 */
public int ans = 0;
public StringBuilder path = new StringBuilder();

public int atMostNGivenDigitSet(String[] digits, int n)
{
	int d = digits.length;
	int len = String.valueOf(n)
					.length();
	for (int i = 1; i < len; i++)
	{
		ans += (int) Math.pow(d, i);
	}
	bt(digits, String.valueOf(n), len);
	return ans;
}

public void bt(String[] digits, String nStr, int len)
{
	if (path.length() == len)
	{
		ans++;
		return;
	}
	for (String digit : digits)
	{
		path.append(digit);
		if (path.charAt(path.length() - 1) < nStr.charAt(path.length() - 1))
		{
			ans += (int) Math.pow(digits.length, len - path.length());
		}
		else if (check(path.toString(), nStr))
		{
			bt(digits, nStr, len);
		}
		path.deleteCharAt(path.length() - 1);
	}
}

public boolean check(String path, String nStr)
{
	int i = 0;
	for (; i < path.length(); i++)
	{
		if (path.charAt(i) > nStr.charAt(i)) return false;
		else if (path.charAt(i) < nStr.charAt(i)) return true;
	}
	return true;
}
```

2. 数位DP

```java
/**
 * 数位DP
 * 力扣官方题解
 */
public int atMostNGivenDigitSet(String[] digits, int n)
{
	String s = Integer.toString(n);
	int m = digits.length, k = s.length();
	int[][] dp = new int[k + 1][2];
	dp[0][1] = 1;
	for (int i = 1; i <= k; i++)
	{
		for (String digit : digits)
		{
			if (digit.charAt(0) == s.charAt(i - 1))
			{
				dp[i][1] = dp[i - 1][1];
			}
			else if (digit.charAt(0) < s.charAt(i - 1))
			{
				dp[i][0] += dp[i - 1][1];
			}
			else
			{
				break;
			}
		}
		if (i > 1)
		{
			dp[i][0] += m + dp[i - 1][0] * m;
		}
	}
	return dp[k][0] + dp[k][1];
}
```

#### [924. 尽量减少恶意软件的传播](https://leetcode.cn/problems/minimize-malware-spread/)

@2024.01.13

#并查集 

并查集统计每个分量的阶和初始感染数，只有当一个分量的初始感染数为 `1` 时，保护这个结点才是有效的（如果有 `>= 2` 个初始感染，那么保护任一个都不会让这个分量免于全部感染）。

```java
/**
 * 并查集
 * Godfather
 */
public int minMalwareSpread(int[][] graph, int[] initial) {
	int n = graph.length;
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			// 连通分量
			if (graph[i][j] == 1) union(root, i, j);
		}
	}
	// 每个 root[i] 更新为根, 方便后续查询
	for (int i = 0; i < n; i++) root[i] = find(root, i);
	// size[i]: 以 i 为根的连通分量的阶
	// cntMal[i]: 以 i 为根的连通分量的初始感染数
	int[] size = new int[n], cntMal = new int[n];
	for (int i = 0; i < n; i++) size[root[i]]++;
	for (int x : initial) cntMal[root[x]]++;
	Arrays.sort(initial); // 排序, 使找到的第一个结点的索引最小
	int max = 0, ans = -1; // max: 移除某结点后不再感染的结点的最大值
	for (int x : initial) {
		// 要求该分量只有 1 个初始感染, 移除这个感染才是有效的, 否则仍然会全部感染
		// 该分量的阶 > max
		if (cntMal[root[x]] == 1 && size[root[x]] > max) {
			max = size[root[x]];
			ans = x;
		}
	}
	return max > 0 ? ans : initial[0];
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

#### [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

@2023.09.26

```java
int ans = 0;

public int minCameraCover(TreeNode root)
{
	if (solve(root) == 0) ans++;
	return ans;
}

public int solve(TreeNode root)
{
	if (root == null) return 2;
	int left = solve(root.left);
	int right = solve(root.right);
	if (left == 2 && right == 2)
	{
		return 0;
	}
	else if (left == 0 || right == 0)
	{
		ans++;
		return 1;
	}
	else return 2;
}
```

#### [980. 不同路径 III](https://leetcode.cn/problems/unique-paths-iii/)

@2024.01.30



```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dirs = new int[]{-1, 0, 1, 0, -1};
private int rows, cols, cnt, ans;
private boolean[][] vis;

public int uniquePathsIII(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	cnt = ans = 0;
	int x = 0, y = 0; // 起点
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] == 0) cnt++;
			else if (grid[i][j] == 1) {
				x = i;
				y = j;
			}
		}
	}
	vis = new boolean[rows][cols];
	dfs(grid, x, y, cnt);
	return ans;
}

private void dfs(int[][] grid, int x, int y, int rem) {
	if (grid[x][y] == 2 && rem == -1) ans++;
	for (int i = 0; i < 4; i++) {
		int nx = x + dirs[i], ny = y + dirs[i + 1];
		if (inBound(nx, ny) && !vis[nx][ny] && (grid[nx][ny] == 0 || grid[nx][ny] == 2)) {
			vis[nx][ny] = true;
			dfs(grid, nx, ny, rem - 1);
			vis[nx][ny] = false;
		}
	}
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [1044. 最长重复子串](https://leetcode.cn/problems/longest-duplicate-substring/)

@2023.10.16

#二分查找 #滑动窗口 #字符串 #哈希表 

二分查找子串的长度，检查是否有重复子串。

检查的部分用 *Rabin-Karp 算法*，也就是字符串哈希。

```java
/**
 * 字符串哈希
 * 宫水三叶
 */
private long[] hash, prime;
private int n;

public String longestDupSubstring(String s) {
	final int PRIME = 1313131;
	n = s.length();
	
	// 预处理哈希数组与质数幂数组
	hash = new long[n + 1];
	prime = new long[n + 1];
	prime[0] = 1;
	for (int i = 0; i < n; i++) {
		prime[i + 1] = prime[i] * PRIME;
		hash[i + 1] = hash[i] * PRIME + s.charAt(i);
	}
	
	// 二分
	int l = 0, r = n - 1, st = -1;
	while (l <= r) {
		int m = l + r >> 1, idx = check(s, m);
		if (idx >= 0) {
			st = idx;
			l = m + 1;
		}
		else r = m - 1;
	}
	return st >= 0 ? s.substring(st, st + l - 1) : "";
}

// 检查是否有长为 l 的重复子串
private int check(String s, int l) {
	Set<Long> vis = new HashSet<>();
	for (int i = 1, j = i + l - 1; j <= n; i++, j++) { // i 为起点, j 为终点
		long h = this.hash[j] - this.hash[i - 1] * prime[j - i + 1]; // 计算哈希值
		if (!vis.add(h)) return i - 1; // 哈希值重复, 说明有重复子串
	}
	return -1;
}
```

#### [1092. 最短公共超序列](https://leetcode.cn/problems/shortest-common-supersequence/)

@2024.01.10

#动态规划 

`str1`、`str2` 分别简记为 `s`、`t`，先求出两者的 LCS 对应的 `dp`（`dp[i][j]` 表示 `s[:i]` 和 `t[:j]` 的 LCS 长度），根据 `dp` 倒推答案。

双指针分别指向 `s`、`t` 的右端，倒序遍历，每次比较 `s[i]` 和 `t[j]`：

- 如果 `s[i] == t[j]`，追加 `s[i]` 并左移 `i` 和 `j`。
- 如果 `s[i] != t[j]`，比较 `dp[i][j]` 和 `dp[i-1][j]`、`dp[i][j-1]`：
	- 如果 `dp[i][j] == dp[i-1][j]`，追加 `s[i]` 并左移 `i`。
	- 如果 `dp[i][j] == dp[i][j-1]`，追加 `t[j]` 并左移 `j`。
	- 否则，随便追加一个的效果相同，不妨追加 `s[i]` 并左移 `i` 和 `j`。

```java
/**
 * 动态规划
 * ylb
 */
public String shortestCommonSupersequence(String str1, String str2) {
	char[] s = str1.toCharArray(), t = str2.toCharArray();
	int l1 = s.length, l2 = t.length;
	int[][] dp = new int[l1 + 1][l2 + 1];
	for (int i = 1; i <= l1; i++) {
		for (int j = 1; j <= l2; j++) {
			if (s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
			else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
		}
	}
	
	StringBuilder ans = new StringBuilder();
	int i = l1, j = l2;
	while (i > 0 || j > 0) {
		if (i == 0) ans.append(t[--j]);
		else if (j == 0) ans.append(s[--i]);
		else {
			if (dp[i][j] == dp[i - 1][j]) ans.append(s[--i]);
			else if (dp[i][j] == dp[i][j - 1]) ans.append(t[--j]);
			else {
				ans.append(s[--i]);
				j--;
			}
		}
	}
	return ans.reverse().toString();
}
```

#### [1147. 段式回文](https://leetcode.cn/problems/longest-chunked-palindrome-decomposition/)

@2024.01.04

#贪心 #字符串哈希 

从两侧开始，找到匹配的串就减掉。

```java
/**
 * 贪心
 * 灵茶山艾府
 */
public int longestDecomposition(String s) {
	if (s.isEmpty()) return 0;
	for (int i = 1, n = s.length(); i <= n / 2; i++) {
		if (s.substring(0, i).equals(s.substring(n - i))) {
			return longestDecomposition(s.substring(i, n - i)) + 2;
		}
	}
	return 1;
}
```

优化：字符串哈希。

```java
/**
 * 贪心
 * 啥时候才能全a啊
 */
private final long BASE = 131131;
private char[] chs;

public int longestDecomposition(String text) {
	chs = text.toCharArray();
	return solve(0, text.length() - 1);
}

private int solve(int l, int r) {
	if (l == r) return 1;
	if (l > r) return 0;
	long h1 = 0, h2 = 0, c = 1;
	while (l <= r) {
		h1 = h1 * BASE + chs[l];
		h2 += chs[r] * c;
		c *= BASE;
		l++;
		r--;
		if (h1 == h2) break;
	}
	if (h1 == h2) return solve(l, r) + 2;
	return 1;
}
```

#### [1153. 字符串转化](https://leetcode.cn/problems/string-transforms-into-another-string/)

@2024.01.09

#字符串 #哈希表 

`str1` 中某个字符 `c1` 对应的下标组为 `[i_1, ..., i_n]`，这组下标在 `str2` 中也必须对应某个相同的字符 `c2`。

另外，`str2` 的不同字符数必须少于 26 个，这样才有操作的空间。

```java
public boolean canConvert(String str1, String str2) {
	if (str1.equals(str2)) return true;
	Map<Character, Character> map = new HashMap<>();
	boolean[] vis = new boolean[26];
	int size = 0;
	for (int i = 0, l = str1.length(); i < l; i++) {
		char c1 = str1.charAt(i), c2 = str2.charAt(i);
		if (map.computeIfAbsent(c1, k -> c2) != c2) return false;
		if (!vis[c2 - 'a']) {
			vis[c2 - 'a'] = true;
			size++;
		}
	}
	return size < 26;
}
```

#### [1255. 得分最高的单词集合](https://leetcode.cn/problems/maximum-score-words-formed-by-letters/)

@2024.01.12

#回溯 

数据范围仅 `14`，回溯每一种单词集合。

```java
int[] cnt = cnt(w[idx]);
boolean added = add(cnt); // 是否成功加入 w[idx]
bt(w, idx + 1);
if (added) remove(cnt); // 如果成功加入, 现在需要移除
bt(w, idx + 1);
```

```java
/**
 * 回溯
 * Somnia1337
 */
private final char A = 'a';
private int[] score, max; // max: 剩余可用量
private int cur, ans;

public int maxScoreWords(String[] words, char[] letters, int[] score) {
	this.score = score;
	max = new int[26];
	for (char c : letters) max[c - A]++;
	cur = ans = 0;
	bt(words, 0);
	return ans;
}

private void bt(String[] w, int idx) {
	if (idx == w.length) {
		ans = Math.max(cur, ans);
		return;
	}
	int[] cnt = cnt(w[idx]);
	boolean added = add(cnt); // 是否成功加入 w[idx]
	bt(w, idx + 1);
	if (added) remove(cnt); // 如果成功加入, 现在需要移除
	bt(w, idx + 1);
}

// 统计 s 的字母频数
private int[] cnt(String s) {
	int[] ret = new int[26];
	for (char c : s.toCharArray()) ret[c - A]++;
	return ret;
}

// 尝试加入 s, 如果超出上限返回 false, 成功返回 true
private boolean add(int[] cnt) {
	for (int i = 0; i < 26; i++) {
		if (cnt[i] > max[i]) return false;
	}
	for (int i = 0; i < 26; i++) {
		max[i] -= cnt[i];
		cur += cnt[i] * score[i];
	}
	return true;
}

// 将 s 的影响从 max 移除
private void remove(int[] cnt) {
	for (int i = 0; i < 26; i++) {
		max[i] += cnt[i];
		cur -= cnt[i] * score[i];
	}
}
```

#### [1259. 不相交的握手](https://leetcode.cn/problems/handshakes-that-dont-cross/)

@2024.01.09

#数学 #动态规划 

卡特兰数解决的问题：

- 大小 $n \times n$ 的方格图，从左上角到达右下角，每次只能向下或向右且不能走到主对角线下侧，求总路径数。
- 在圆上选择 $2 n$ 个点，求两两相连得到的 $n$ 条线段不相交的方案数。
- 已知栈的入栈序列为 $1, 2, \cdots, n$，求不同的出栈序列数。
- 由 $n$ 个结点构造的不同的二叉树数量。

卡特兰数常用公式：

- $H_n = 1, (n = 0, 1)$，$H_n = \underset{i = 1}{\overset{n}{\sum}} H_{i - 1} H_{n - i}, (n \geqslant 2)$
- $H_n = {\large \frac{H_{n - 1} (4 n - 2)}{n + 1}}$

`n` 个人的圈（编号 `[1:n]`），第 `n` 个人只能和 `1` / `3` / `5` / ... / `n-1` 握手，否则连线的两侧都有奇数人，无法满足要求。

`dp[i]` 表示 `i` 人的子问题答案，一次握手将图分为两部分，组合方案数为 `dp[j-1] * dp[i-j-1]`，转移方程 `dp[i] = sum(dp[j-1] * dp[i-j-1])`。

```java
/**
 * 动态规划
 * ......
 */
public int numberOfWays(int n) {
	final int MOD = 1000000007;
	int[] dp = new int[n + 1];
	dp[0] = 1;
	for (int i = 2; i <= n; i += 2) {
		for (int j = 1; j < i; j += 2) {
			dp[i] = (int) (dp[i] + ((long) dp[j - 1] * dp[i - j - 1]) % MOD) % MOD;
		}
	}
	return dp[n];
}
```

#### [1289. 下降路径最小和 II](https://leetcode.cn/problems/minimum-falling-path-sum-ii/)

@2023.09.13

#动态规划 

与 [931. 下降路径最小和](https://leetcode.cn/problems/minimum-falling-path-sum/) 相似，区别在于，`dp[i][j]` 来自 `dp[i - 1]` 中除去 `dp[i - 1][j]` 以外的最小元素。

因此，遍历到行 `dp[i]` 时，需先计算 `dp[i - 1]` 中的最小以及第二小项。如果 `j = 最小项下标`，则 `dp[i][j]` 只能为 `grid[i][j] + 第二小项`，其余情况均为 `grid[i][j] + 最小项`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minFallingPathSum(int[][] grid) {
	int n = grid.length;
	int[][] dp = new int[n][n];
	for (int i = 0; i < n; i++) System.arraycopy(grid[i], 0, dp[i], 0, n);
	for (int i = 1; i < n; i++) {
		int idx = 0, min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
		for (int j = 0; j < n; j++) {
			if (dp[i - 1][j] <= min1) {
				min2 = min1;
				min1 = dp[i - 1][idx = j];
			}
			else if (dp[i - 1][j] < min2) min2 = dp[i - 1][j];
		}
		for (int j = 0; j < n; j++) {
			if (j == idx) dp[i][j] += min2;
			else dp[i][j] += min1;
		}
	}
	int ans = Integer.MAX_VALUE;
	for (int j = 0; j < n; j++) ans = Math.min(dp[n - 1][j], ans);
	return ans;
}
```

#### [1326. 灌溉花园的最少水龙头数目](https://leetcode.cn/problems/minimum-number-of-taps-to-open-to-water-a-garden/)

@2024.01.12

#贪心 

数组 `far` 维护记录每个位置的水流最远能够到达的位置，遍历，每次都贪心地取到最远位置。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minTaps(int n, int[] ranges) {
	int[] far = new int[n + 1]; // 每个位置的水流最远能到达的位置
	for (int i = 0; i < n + 1; i++) {
		int l = Math.max(i - ranges[i], 0), r = Math.min(i + ranges[i], n);
		for (int j = l; j <= r; j++) far[j] = Math.max(r, far[j]);
	}
	int ans = 0;
	for (int i = 0; i < n + 1; i++) {
		if (far[i] <= i) return -1; // 之前的水流无法覆盖当前位置, 不连续
		if (far[i] == n) return ans + 1; // 到达末尾
		ans++;
		i = far[i] - 1; // 贪心地取最远位置
	}
	return ans;
}
```

#### [1349. 参加考试的最大学生数](https://leetcode.cn/problems/maximum-students-taking-exam/)

@2023.12.26

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int maxStudents(char[][] seats) {
	int rows = seats.length, cols = seats[0].length;
	int[] a = new int[rows];
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (seats[i][j] == '.') a[i] |= 1 << j;
		}
	}
	int[][] dp = new int[rows][1 << cols];
	for (int j = 1; j < (1 << cols); j++) dp[0][j] = dp[0][j & ~((j & -j) * 3)] + 1;
	for (int i = 1; i < rows; i++) {
		for (int j = a[i]; j > 0; j = (j - 1) & a[i]) {
			dp[i][j] = dp[i - 1][a[i - 1]];
			for (int s = j; s > 0; s = (s - 1) & j) {
				if ((s & (s >> 1)) == 0) {
					int t = a[i - 1] & ~(s << 1 | s >> 1);
					dp[i][j] = Math.max(dp[i - 1][t] + dp[0][s], dp[i][j]);
				}
			}
		}
		dp[i][0] = dp[i - 1][a[i - 1]];
	}
	return dp[rows - 1][a[rows - 1]];
}
```

#### [1392. 最长快乐前缀](https://leetcode.cn/problems/longest-happy-prefix/)

@2024.01.13



```java
/**
 * KMP
 * kingwx001
 */
public String longestPrefix(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int[] next = new int[n];
	for (int l = 0, r = 1; r < n; r++) {
		while (l > 0 && chs[l] != chs[r]) l = next[l - 1];
		if (s.charAt(r) == s.charAt(l)) l++;
		next[r] = l;
	}
	return s.substring(0, next[n - 1]);
}
```

#### [1402. 做菜顺序](https://leetcode.cn/problems/reducing-dishes/)

@2023.10.22

1. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int maxSatisfaction(int[] satisfaction)
{
	int len = satisfaction.length;
	int[][] dp = new int[len + 1][len + 1];
	Arrays.sort(satisfaction);
	
	int ans = 0;
	for (int i = 1; i <= len; i++)
	{
		for (int j = 1; j <= i; j++)
		{
			dp[i][j] = dp[i - 1][j - 1] + satisfaction[i - 1] * j;
			if (j < i) dp[i][j] = Math.max(dp[i - 1][j], dp[i][j]);
			ans = Math.max(ans, dp[i][j]);
		}
	}
	
	return ans;
}
```

2. 贪心

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int maxSatisfaction(int[] satisfaction)
{
	Arrays.sort(satisfaction);
	for (int i = 0, j = satisfaction.length - 1; i < j; i++, j--)
	{
		int temp = satisfaction[i];
		satisfaction[i] = satisfaction[j];
		satisfaction[j] = temp;
	}
	
	int presum = 0, ans = 0;
	for (int s : satisfaction)
	{
		if (presum + s > 0)
		{
			presum += s;
			ans += presum;
		}
		else break;
	}
	
	return ans;
}
```

#### [1416. 恢复数组](https://leetcode.cn/problems/restore-the-array/)

@2024.01.06

1. 记忆化搜索

```java
/**
 * 记忆化搜索
 * Somnia1337
 */
private Map<Integer, Integer> memo;

public int numberOfArrays(String s, int k) {
	memo = new HashMap<>();
	return dfs(s, k, 0);
}

private int dfs(String s, int k, int idx) {
	if (memo.containsKey(idx)) return memo.get(idx);
	int n = s.length(), ret = 0;
	if (idx == n) return 1;
	if (s.charAt(idx) == '0') return 0;
	for (int i = idx; i < n; i++) {
		if (Long.parseLong(s.substring(idx, i + 1)) > k) break;
		ret = (ret + dfs(s, k, i + 1)) % 1000000007;
	}
	memo.put(idx, ret);
	return ret;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * ?
 */
public int numberOfArrays(String s, int k) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	long sum = 0;
	long[] dp = new long[n];
	for (int i = 0; i < n; i++) {
		if ((sum = sum * 10 + chs[i] - '0') <= 0 || sum > k) break;
		dp[i] = 1;
	}
	for (int i = 0; i < n; i++) {
		if (dp[i] == 0) continue;
		long num = 0;
		for (int j = i + 1; j < n; j++) {
			if ((num = num * 10 + chs[j] - '0') <= 0 || num > k) break;
			dp[j] = (dp[j] + dp[i]) % 1000000007;
		}
	}
	return (int) dp[n - 1];
}
```

#### [1425. 带限制的子序列和](https://leetcode.cn/problems/constrained-subsequence-sum/)

@2023.10.23

```java
/**
 * 动态规划
 * ?
 */
public int constrainedSubsetSum(int[] nums, int k)
{
	int n = nums.length, ans = nums[0];
	int[] dp = new int[n];
	dp[0] = nums[0];
	for (int i = 1, idx = 0; i < n; i++)
	{
		if (idx < i - k)
		{
			int max = Integer.MIN_VALUE;
			for (int j = idx + 1; j < i; j++)
			{
				if (dp[j] >= max)
				{
					max = dp[j];
					idx = j;
				}
			}
		}
		dp[i] = nums[i] + Math.max(0, dp[idx]);
		if (dp[i] >= dp[idx])
		{
			idx = i;
		}
		ans = Math.max(ans, dp[i]);
	}
	return ans;
}
```

#### [1449. 数位成本和为目标值的最大数字](https://leetcode.cn/problems/form-largest-integer-with-digits-that-add-up-to-target/)

@2024.01.06

```java
// 超时回溯
private int[][] join;
private StringBuilder path;
private List<String> candi;

public String largestNumber(int[] cost, int target) {
	join = new int[9][2];
	for (int i = 0; i < 9; i++) {
		join[i][0] = cost[i];
		join[i][1] = i + 1;
	}
	Arrays.sort(join, (a, b) -> {
		if (a[0] != b[0]) return a[0] - b[0];
		return b[1] - a[1];
	});
	for (int i = 1; i < 9; i++) {
		if (join[i][0] == join[i - 1][0]) join[i][1] = join[i - 1][1];
	}
	path = new StringBuilder();
	candi = new ArrayList<>();
	bt(target, 0);
	if (candi.isEmpty()) return "0";
	int max = 0;
	for (String s : candi) max = Math.max(s.length(), max);
	for (int i = 0; i < candi.size(); i++) {
		String s = candi.get(i);
		if (s.length() < max) continue;
		char[] chs = s.toCharArray();
		Arrays.sort(chs);
		StringBuilder re = new StringBuilder(new String(chs));
		candi.set(i, re.reverse().toString());
	}
	candi.sort((a, b) -> {
		if (a.length() != b.length()) return b.length() - a.length();
		return b.compareTo(a);
	});
	return candi.get(0);
}

private void bt(int tar, int idx) {
	if (idx == 9 || tar == 0) {
		if (tar == 0) {
			candi.add(path.toString());
			return;
		}
	}
	for (int i = idx; i < 9; i++) {
		if (join[i][0] > tar) return;
		int hold = join[i][0];
		path.append(join[i][1]);
		while (i < 9 - 1 && join[i + 1][0] == join[i][0]) i++;
		bt(tar - hold, i);
		path.delete(path.length() - 1, path.length());
	}
}
```

```java
/**
 * 动态规划
 * ?
 */
public String largestNumber(int[] cost, int target) {
	int[] dp = new int[target + 1];
	Arrays.fill(dp, Integer.MIN_VALUE);
	dp[0] = 0;
	for (int c : cost) {
		for (int j = c; j <= target; j++) dp[j] = Math.max(dp[j - c] + 1, dp[j]);
	}
	if (dp[target] < 0) return "0";
	StringBuilder ans = new StringBuilder();
	for (int i = 8, j = target; i >= 0; i--) {
		for (int c = cost[i]; j >= c && dp[j] == dp[j - c] + 1; j -= c) ans.append(i + 1);
	}
	return ans.toString();
}
```

#### [1499. 满足不等式的最大值](https://leetcode.cn/problems/max-value-of-equation/)

@2024.01.02

#数学 #堆 #单调队列 

变形：$y_i + y_j + \lvert x_i - x_j \rvert = (x_j + y_j) + (y_i - x_i)$。

枚举 `j`，问题转换为寻找满足 `i < j` 且 `x_j - k <= x_i` 的最大的 `y_i - x_i`。

1. 堆

堆维护元组 `(x_i, y_i-x_i)`。

```java
/**
 * 堆
 * Somnia1337
 */
public int findMaxValueOfEquation(int[][] points, int k) {
	Queue<int[]> pq = new PriorityQueue<>((a, b) -> {
		if (a[1] != b[1]) return b[1] - a[1];
		return a[0] - b[0];
	});
	int ans = Integer.MIN_VALUE;
	for (int[] p : points) {
		while (!pq.isEmpty() && p[0] - pq.peek()[0] > k) pq.poll();
		if (!pq.isEmpty()) {
			int[] pk = pq.peek();
			ans = Math.max(pk[1] + pk[0] + p[1] + Math.abs(p[0] - pk[0]), ans);
		}
		pq.offer(new int[]{p[0], p[1] - p[0]});
	}
	return ans;
}
```

2. 单调队列

单调队列维护如上元组，过程：

```
1. 将队首不满足 x_j-k<=x_i 的 x_i 出队
2. 如果队非空, 更新答案
3. y_j-x_j 不小于队尾时持续出队
4. j 入队
```

```java
/**
 * 单调队列
 * 灵茶山艾府
 */
public int findMaxValueOfEquation(int[][] points, int k) {
	Deque<int[]> q = new ArrayDeque<>();
	int ans = Integer.MIN_VALUE;
	for (int[] p : points) {
		int x = p[0], y = p[1];
		while (!q.isEmpty() && q.peekFirst()[0] < x - k) q.pollFirst();
		if (!q.isEmpty()) ans = Math.max(ans, x + y + q.peekFirst()[1]);
		while (!q.isEmpty() && q.peekLast()[1] <= y - x) q.pollLast();
		q.addLast(new int[]{x, y - x});
	}
	return ans;
}
```

#### [1665. 完成所有任务的最少初始能量](https://leetcode.cn/problems/minimum-initial-energy-to-finish-tasks/)

@2024.01.03

#贪心 

差值越大，做完任务剩余的能量越大，所以先完成差值大的任务。

```java
/**
 * 贪心
 * 颠倒的蝴蝶
 */
public int minimumEffort(int[][] tasks) {
	Arrays.sort(tasks, Comparator.comparingInt(a -> (a[1] - a[0])));
	int ans = 0;
	for (int[] t : tasks) {
		ans += t[0];
		ans = Math.max(t[1], ans);
	}
	return ans;
}
```

#### [1671. 得到山形数组的最少删除次数](https://leetcode.cn/problems/minimum-number-of-removals-to-make-mountain-array/)

@2023.12.22

#前后缀分解 

要使删除次数最少，即要山形子序列最长，可以枚举 `nums[i]` 作为峰顶计算。

山形子序列可视为一个递增子序列拼接一个递减子序列，`pre[i]`、`suf[i]` 分别表示以 `nums[i]` 为最右、最左元素时的最长递增、递减子序列长度，则以 `nums[i]` 为峰顶的最长山形子序列长度为 `pre[i] + suf[i] - 1`。

可以枚举计算 `pre[i]` 和 `suf[i]`。

```java
/**
 * 前后缀分解
 * 不上瓜店不改名
 */
public int minimumMountainRemovals(int[] nums) {
	int[] pre = new int[nums.length], suf = new int[nums.length];
	Arrays.fill(pre, 1);
	Arrays.fill(suf, 1);
	for (int i = nums.length - 1; i >= 0; i--) {
		for (int j = nums.length - 1; j > i; j--) {
			if (nums[j] < nums[i]) suf[i] = Math.max(suf[j] + 1, suf[i]);
		}
	}
	int max = 0;
	for (int i = 0; i < nums.length - 1; i++) {
		for (int j = 0; j < i; j++) {
			if (nums[j] < nums[i]) pre[i] = Math.max(pre[j] + 1, pre[i]);
		}
		if (pre[i] == 1 || suf[i] == 1) continue;
		max = Math.max(pre[i] + suf[i] - 1, max);
	}
	return nums.length - max;
}
```

优化：二分查找计算 `pre[i]` 和 `suf[i]`。

```java
/**
 * 前后缀分解
 * 灵茶山艾府
 */
public int minimumMountainRemovals(int[] nums) {
	int[] suf = new int[nums.length];
	List<Integer> g = new ArrayList<>();
	for (int i = nums.length - 1; i > 0; i--) {
		int x = nums[i], j = lowerBound(g, x);
		if (j == g.size()) g.add(x);
		else g.set(j, x);
		suf[i] = j + 1;
	}
	
	g = new ArrayList<>();
	int max = 0;
	for (int i = 0; i < nums.length - 1; i++) {
		int x = nums[i], j = lowerBound(g, x);
		if (j == g.size()) g.add(x);
		else g.set(j, x);
		int pre = j + 1;
		if (pre >= 2 && suf[i] >= 2) max = Math.max(pre + suf[i] - 1, max);
	}
	return nums.length - max;
}

private int lowerBound(List<Integer> g, int tar) {
	int l = 0, r = g.size();
	while (l < r) {
		int m = l + r >> 1;
		if (g.get(m) < tar) l = m + 1;
		else r = m;
	}
	return r;
}
```

#### [1723. 完成所有工作的最短时间](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/)

@2023.10.23

#二分查找 #回溯 

二分答案，`l` 初始化为 `max(jobs)`，`r` 初始化为 `sum(jobs)`。

回溯检查是否可行，剪枝：

- 优先分配耗时长的工作。
- 当 `workers[i]` 未分配工作时，不给 `workers[i + 1]` 分配。
- 如果将 `jobs[idx]` 分配给 `workers[j]` 后，其工作量刚好达到上限，且递归返回了 `false`，无需再尝试将 `jobs[idx]` 分配给其他人，可直接返回 `false`。

```java
/**
 * 二分查找 + 回溯
 * Somnia1337
 */
public int minimumTimeRequired(int[] jobs, int k) {
    int n = jobs.length;
    Arrays.sort(jobs); // 排序, 随后优先分配耗时长的工作
    int l = jobs[n - 1], r = 0; // l 初始化为 max(jobs)
    for (int job : jobs) r += job; // r 初始化为 sum(jobs)
    while (l <= r) {
        int m = l + r >> 1;
        int[] workers = new int[k];
        if (bt(jobs, workers, m, n - 1)) r = m - 1;
        else l = m + 1;
    }
    return l;
}

private boolean bt(int[] jobs, int[] workers, int time, int idx) {
    if (idx == -1) return true;
    for (int i = workers.length - 1; i >= 0; i--) {
        if (workers[i] + jobs[idx] > time) continue;
        workers[i] += jobs[idx];
        if (bt(jobs, workers, time, idx - 1)) return true;
        workers[i] -= jobs[idx];
        // 剪枝 3
        // 特别特别重要(2241ms -> 0ms)
        if (workers[i] == 0 || workers[i] + jobs[idx] == time) return false;
    }
    return false;
}
```

#### [1745. 分割回文串 IV](https://leetcode.cn/problems/palindrome-partitioning-iv/)

@2024.01.06

#动态规划 

动态规划预处理所有回文子串的区间，枚举左右两个分割点，判断是否分为 3 个回文串。

```java
/**
 * 动态规划
 * Somnia1337
 */
public boolean checkPartitioning(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	boolean[][] isPal = getPals(s);
	for (int a = 0; a < n - 2; a++) {
		if (!isPal[0][a]) continue;
		for (int b = n - 1; b > a + 1; b--) {
			if (isPal[b][n - 1] && isPal[a + 1][b - 1]) return true;
		}
	}
	return false;
}

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

#### [1761. 一个图中连通三元组的最小度数](https://leetcode.cn/problems/minimum-degree-of-a-connected-trio-in-a-graph/)

@2024.01.15



```java
/**
 * 枚举
 * ylb
 */
public int minTrioDegree(int n, int[][] edges) {
	boolean[][] g = new boolean[n][n];
	int[] d = new int[n];
	for (int[] e : edges) {
		int u = e[0] - 1, v = e[1] - 1;
		g[u][v] = g[v][u] = true;
		d[u]++;
		d[v]++;
	}
	int ans = Integer.MAX_VALUE;
	for (int i = 0; i < n; i++) {
		for (int j = i + 1; j < n; j++) {
			if (g[i][j]) {
				for (int k = j + 1; k < n; k++) {
					if (g[i][k] && g[j][k]) ans = Math.min(d[i] + d[j] + d[k] - 6, ans);
				}
			}
		}
	}
	return ans < Integer.MAX_VALUE ? ans : -1;
}
```

#### [1793. 好子数组的最大分数](https://leetcode.cn/problems/maximum-score-of-a-good-subarray/)

@2024.01.07

#双指针 

从 `nums[k]` 递减枚举子数组的最小值，从 `k` 开始分别向两侧枚举左右端点，满足条件后更新答案。

```java
/**
 * 双指针
 * Somnia1337
 */
public int maximumScore(int[] nums, int k) {
	int n = nums.length, l = k, r = k, ans = 0;
	for (int x = nums[k]; x * n > ans; x--) { // 最优性剪枝
		while (r < n && nums[r] >= x) r++;
		while (l >= 0 && nums[l] >= x) l--;
		ans = Math.max((r - l - 1) * x, ans);
	}
	return ans;
}
```

#### [1944. 队列中可以看到的人数](https://leetcode.cn/problems/number-of-visible-people-in-a-queue/)

@2024.01.05

#单调栈 

倒序遍历，递减单调栈维护身高，栈顶的 `heights[j] < heights[i]` 时，`j` 被 `i` 挡住、不可能再被 `i` 左侧的人看到，持续弹栈并累加 `ans[i]`。

```java
/**
 * 单调栈
 * 灵茶山艾府
 */
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

#### [2003. 每棵子树内缺失的最小基因值](https://leetcode.cn/problems/smallest-missing-genetic-value-in-each-subtree/)

@2023.10.31

```java
/**
 * DFS
 * 灵茶山艾府
 */
public int[] smallestMissingValueSubtree(int[] parents, int[] nums)
{
	int len = parents.length;
	int[] ans = new int[len];
	Arrays.fill(ans, 1);
	int node = -1;
	for (int i = 0; i < len; i++)
	{
		if (nums[i] == 1)
		{
			node = i;
			break;
		}
	}
	if (node < 0) return ans;
	
	List<Integer>[] g = new ArrayList[len];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int i = 1; i < len; ++i) g[parents[i]].add(i);
	
	Set<Integer> vis = new HashSet<>();
	int mex = 2;
	while (node >= 0)
	{
		dfs(node, g, vis, nums);
		while (vis.contains(mex)) mex++;
		ans[node] = mex;
		node = parents[node];
	}
	return ans;
}

private void dfs(int x, List<Integer>[] g, Set<Integer> vis, int[] nums)
{
	vis.add(nums[x]);
	for (int son : g[x])
	{
		if (!vis.contains(nums[son])) dfs(son, g, vis, nums);
	}
}
```

#### [2050. 并行课程 III](https://leetcode.cn/problems/parallel-courses-iii/)

@2024.01.20



```java
/**
 * 拓扑排序
 * Somnia1337
 */
public int minimumTime(int n, int[][] relations, int[] time) {
	int[] in = new int[n], t = new int[n];
	List<Integer>[] next = new List[n];
	Arrays.setAll(next, k -> new ArrayList<>());
	for (int[] rel : relations) {
		next[rel[0] - 1].add(rel[1] - 1);
		in[rel[1] - 1]++;
	}
	Queue<Integer> q = new LinkedList<>();
	for (int i = 0; i < n; i++) {
		if (in[i] == 0) q.offer(i);
	}
	int ans = 0;
	while (!q.isEmpty()) {
		int p = q.poll(), T = t[p] + time[p];
		ans = Math.max(T, ans);
		for (int nex : next[p]) {
			if (--in[nex] == 0) q.offer(nex);
			t[nex] = Math.max(T, t[nex]);
		}
	}
	return ans;
}
```

```java
/**
 * ?
 * ?
 */
public int minimumTime(int n, int[][] relations, int[] time) {
	int[] late = Arrays.copyOf(time, n);
	boolean flag = true;
	while (flag) {
		flag = false;
		for (int[] rel : relations) {
			if (late[rel[1] - 1] < late[rel[0] - 1] + time[rel[1] - 1]) {
				flag = true;
				late[rel[1] - 1] = late[rel[0] - 1] + time[rel[1] - 1];
			}
		}
		for (int i = relations.length - 1; i >= 0; i--) {
			if (late[relations[i][1] - 1] < late[relations[i][0] - 1] + time[relations[i][1] - 1]) {
				flag = true;
				late[relations[i][1] - 1] = late[relations[i][0] - 1] + time[relations[i][1] - 1];
			}
		}
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		ans = Math.max(late[i], ans);
	}
	return ans;
}
```

#### [2111. 使数组 K 递增的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-make-the-array-k-increasing/)

@2024.01.07

#二分查找 

分组求 LIS 的长度（微调板子为非递减），`n` 减掉每组 LIS 长度之和即为答案。

```java
/**
 * 二分查找
 * EDCTY
 */
public int kIncreasing(int[] arr, int k) {
	int n = arr.length, ans = n;
	for (int i = 0; i < k; i++) {
		int[] cur = new int[(n - i - 1) / k + 1];
		for (int j = i, it = 0; j < n; j += k, it++) cur[it] = arr[j];
		ans -= count(cur);
	}
	return ans;
}

private int count(int[] arr) {
	int ret = 1;
	for (int i = 1; i < arr.length; i++) {
		int idx = bis(arr, 0, ret - 1, arr[i]);
		arr[idx] = arr[i];
		ret = Math.max(idx + 1, ret);
	}
	return ret;
}

private int bis(int[] arr, int l, int r, int t) {
	while (l <= r) {
		int m = (l + r) >> 1;
		if (arr[m] <= t) l = m + 1;
		else r = m - 1;
	}
	return l;
}
```

#### [2127. 参加会议的最多员工数](https://leetcode.cn/problems/maximum-employees-to-be-invited-to-a-meeting/)

@2023.11.01

```java
/**
 * 拓扑排序
 * 灵茶山艾府
 */
public int maximumInvitations(int[] favorite) {
	int len = favorite.length;
	int[] in = new int[len];
	for (int f : favorite) in[f]++;
	int[] maxD = new int[len];
	Deque<Integer> q = new ArrayDeque<>();
	for (int i = 0; i < len; i++) {
		if (in[i] == 0) q.add(i);
	}
	while (!q.isEmpty()) {
		int p = q.poll(), f = favorite[p];
		maxD[f] = maxD[p] + 1;
		if (--in[f] == 0) q.add(f);
	}
	int max = 0, sum = 0;
	for (int i = 0; i < len; i++) {
		if (in[i] == 0) continue;
		in[i] = 0;
		int circ = 1;
		for (int x = favorite[i]; x != i; x = favorite[x]) {
			in[x] = 0;
			circ++;
		}
		if (circ == 2) sum += maxD[i] + maxD[favorite[i]] + 2;
		else max = Math.max(circ, max);
	}
	return Math.max(sum, max);
}
```

#### [2132. 用邮票贴满网格图](https://leetcode.cn/problems/stamping-the-grid/)

@2023.12.14

```java
/**
 * 差分数组
 * 灵茶山艾府
 */
public boolean possibleToStamp(int[][] grid, int stampHeight, int stampWidth) {
	int rows = grid.length, cols = grid[0].length;
	
	// 计算 grid 的二维前缀和
	int[][] pre = new int[rows + 1][cols + 1];
	for (int i = 1; i < rows + 1; i++) {
		for (int j = 1; j < cols + 1; j++) {
			pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + grid[i - 1][j - 1];
		}
	}
	
	// 计算二维差分
	// 为方便下一步, 在 d 的最上面和最左边各加了一行/列, 所以下标 +1
	int[][] d = new int[rows + 2][cols + 2];
	for (int i2 = stampHeight; i2 <= rows; i2++) {
		for (int j2 = stampWidth; j2 <= cols; j2++) {
			int i1 = i2 - stampHeight + 1, j1 = j2 - stampWidth + 1;
			if (pre[i2][j2] - pre[i2][j1 - 1] - pre[i1 - 1][j2] + pre[i1 - 1][j1 - 1] == 0) {
				d[i1][j1]++;
				d[i1][j2 + 1]--;
				d[i2 + 1][j1]--;
				d[i2 + 1][j2 + 1]++;
			}
		}
	}
	
	// 还原二维差分矩阵对应的计数矩阵(原地)
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			d[i + 1][j + 1] += d[i + 1][j] + d[i][j + 1] - d[i][j];
			if (grid[i][j] == 0 && d[i + 1][j + 1] == 0) return false;
		}
	}
	return true;
}
```

#### [2136. 全部开花的最早一天](https://leetcode.cn/problems/earliest-possible-day-of-full-bloom/)

@2023.09.30

#贪心 

由于总的种植时间为定值`sum(plantTime)`，因此不考虑`plantTime`，优先种植`growTime`较大的种子。根据`growTime`自定义排序种子，遍历维护最晚开花的时间即为答案。

```java
/**
 * 贪心
 * Somnia1337
 */
public int earliestFullBloom(int[] plantTime, int[] growTime)
{
	int len = plantTime.length;
	int[][] seeds = new int[len][2];
	for (int i = 0; i < len; i++)
	{
		seeds[i][0] = plantTime[i];
		seeds[i][1] = growTime[i];
	}
	Arrays.sort(seeds, Comparator.comparing(a -> -a[1]));
	int ans = 1, days = 0;
	for (int i = 0; i < len; i++)
	{
		days += seeds[i][0];
		ans = Math.max(days + seeds[i][1], ans);
	}
	return ans;
}
```

优化：只记录下标，用`Integer[]`代替`int[][]`，遍历时调用`seeds[i]`获取种子下标。

```java
/**
 * 贪心
 * Somnia1337
 */
public int earliestFullBloom(int[] plantTime, int[] growTime)
{
	int len = plantTime.length;
	Integer[] seeds = new Integer[len];
	for (int i = 0; i < len; i++) seeds[i] = i;
	Arrays.sort(seeds, Comparator.comparingInt(a -> -growTime[a]));
	int ans = 1, days = 0;
	for (int i = 0; i < len; i++)
	{
		days += plantTime[seeds[i]];
		ans = Math.max(days + growTime[seeds[i]], ans);
	}
	return ans;
}
```

#### [2147. 分隔长廊的方案数](https://leetcode.cn/problems/number-of-ways-to-divide-a-long-corridor/)

@2024.01.04

#数学 

统计每段连续可放隔板的区间长度，带模累乘。

```java
/**
 * 数学
 * Somnia1337
 */
public int numberOfWays(String corridor) {
	char[] chs = corridor.toCharArray();
	int l = chs.length, cnt = 0; // 座位计数
	for (char c : chs) {
		if (c == 'S') cnt++;
	}
	if (cnt == 0 || (cnt & 1) == 1) return 0;
	
	List<Integer> g = new ArrayList<>(); // 记录各个区间长度
	int s = 0, cur = 0; // s: 座位计数 // cur: 当前区间长度
	for (int i = 0; i < l - 1; i++) {
		if (chs[i] == 'S') {
			if (++s == 2) cur = 1; // 区间开始
			else if (s == 3) { // 区间结束
				s = 1;
				g.add(cur);
				cur = 0;
			}
		}
		else cur++;
	}
	return helper(g);
}

private int helper(List<Integer> g) {
	final int MOD = 1000000007;
	long ret = 1;
	for (int s : g) ret = ret * s % MOD;
	return (int) ret;
}
```

简化：在一次遍历的同时分段累乘。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public int numberOfWays(String corridor) {
	final int MOD = 1000000007;
	int cnt = 0, pre = 0;
	long ans = 1;
	for (int i = 0; i < corridor.length(); i++) {
		if (corridor.charAt(i) == 'S') {
			if (++cnt >= 3 && (cnt & 1) == 1) ans = ans * (i - pre) % MOD;
			pre = i;
		}
	}
	return cnt > 0 && (cnt & 1) == 0 ? (int) ans : 0;
}
```

#### [2227. 加密解密字符串](https://leetcode.cn/problems/encrypt-and-decrypt-strings/)

@2024.01.08

#技巧 

不同字符串的加密结果可能相同，因此直接解密较为困难。注意到允许的解密结果 `dictionary` 有限，可以预处理，将每个允许的结果进行加密并计数，得到密文后直接返回计数。

```java
/**
 * 正难则反
 * 灵茶山艾府
 */
class Encrypter {
    private String[] map = new String[26];
    private Map<String, Integer> count = new HashMap<>();
	
    public Encrypter(char[] keys, String[] values, String[] dictionary) {
        for (int i = 0; i < keys.length; i++) map[keys[i] - 'a'] = values[i];
        for (String d : dictionary) count.merge(encrypt(d), 1, Integer::sum);
    }
	
    public String encrypt(String word1) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < word1.length(); i++) {
            String s = map[word1.charAt(i) - 'a'];
            if (s == null) return "";
            ans.append(s);
        }
        return ans.toString();
    }
	
    public int decrypt(String word2) {
        return count.getOrDefault(word2, 0);
    }
}
```

#### [2251. 花期内花的数目](https://leetcode.cn/problems/number-of-flowers-in-full-bloom/)

@2023.09.28

```java
/**
 * 二分查找
 * Somnia1337
 */
public int[] fullBloomFlowers(int[][] flowers, int[] people)
{
	int[] ans = new int[people.length];
	List<Integer> start = new ArrayList<>();
	List<Integer> end = new ArrayList<>();
	for (int[] flower : flowers)
	{
		start.add(flower[0]);
		end.add(flower[1]);
	}
	Collections.sort(start);
	Collections.sort(end);
	for (int i = 0; i < people.length; i++)
	{
		int s = rightSearch(start, people[i]), e = leftSearch(end, people[i]);
		ans[i] = Math.max(s - e - 1, 0);
	}
	return ans;
}

public int leftSearch(List<Integer> list, int target)
{
	int left = 0, right = list.size() - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		if (list.get(mid) < target) left = mid + 1;
		else right = mid - 1;
	}
	return right;
}

public int rightSearch(List<Integer> list, int target)
{
	int left = 0, right = list.size() - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		if (list.get(mid) > target) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}
```

#### [2258. 逃离火灾](https://leetcode.cn/problems/escape-the-spreading-fire/)

@2023.11.09

```java
/**
 * BFS
 * zhitongvian
 */
private int[] gx = new int[]{0, 0, 1, -1};
private int[] gy = new int[]{1, -1, 0, 0};

public int maximumMinutes(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	int[][] bj = new int[rows][cols];
	bj[rows - 1][cols - 1] = 1;
	
	// 从安全屋出发，第0个位置的数表示火/人进入安全屋的方向，初始值为-1
	List<int[]> list = List.of(new int[]{-1, cols - 1, rows - 1});
	Deque<List<int[]>> dq = new ArrayDeque<>();
	dq.add(list);
	int man = Integer.MAX_VALUE, fire = Integer.MAX_VALUE, cut = 0;
	// 人到安全屋的最短距离情况下，可以从那几个方向进入安全屋
	// 火到安全屋的最短距离情况下，可以从那几个方向进入安全屋
	List<Integer> pz = new ArrayList<>(), fz = new ArrayList<>();
	while (!dq.isEmpty())
	{
		List<int[]> l1 = dq.poll(), l2 = new ArrayList<>();
		for (int[] arr : l1)
		{
			int z0 = arr[0], x0 = arr[1], y0 = arr[2];
			for (int i = 0; i < 4; i++)
			{
				int z1 = z0 == -1 ? i : z0, x1 = x0 + gx[i], y1 = y0 + gy[i];
				if (x1 == 0 && y1 == 0 && cut + 1 <= man)
				{
					man = cut + 1;
					pz.add(z1);
				}
				if (x1 >= 0 && x1 < cols && y1 >= 0 && y1 < rows && bj[y1][x1] == 0 && grid[y1][x1] != 2)
				{
					bj[y1][x1] = 1;
					l2.add(new int[]{z1, x1, y1});
					if (grid[y1][x1] == 1)
					{
						if (fire == Integer.MAX_VALUE || fire == cut + 1)
						{
							fire = cut + 1;
							fz.add(z1);
						}
					}
				}
			}
		}
		//搜索出人和火到安全屋的最短距离后，就可以停止搜索了
		if (fire != Integer.MAX_VALUE && man != Integer.MAX_VALUE) break;
		if (!l2.isEmpty()) dq.add(l2);
		cut++;
	}
	//判断人和火能不能从安全屋的不同方向进入安全屋，如果人和火是从同一方向进入安全屋的话，最终结果需要减1
	boolean valid = false;
	for (int z : pz)
	{
		if (!fz.contains(z))
		{
			valid = true;
			break;
		}
	}
	
	if (man == 0 || man > fire || (!valid && man == fire)) return -1;
	if (fire == Integer.MAX_VALUE) return 1000000000;
	return fire - man - (!valid ? 1 : 0);
}
```

#### [2262. 字符串的总引力](https://leetcode.cn/problems/total-appeal-of-a-string/)

@2023.11.02

#变化量 

考虑引力的变化量：从左往右遍历 `s`，将 `s[i]` 添加到以 `s[i - 1]` 结尾的子串的末尾后，这些以 `s[i - 1]` 结尾的子串的引力值将如何变化？

分类讨论：

- 如果之前未出现过 `s[i]`，那么它们均加 1。
- 如果之前出现过 `s[i]`，记其上次出现的下标为 `j`，则 `j` 之前的子串均加 1，`j` 到 `i` 之间的子串不加。

用 `int[26]` 记录每个字母上次出现的位置，初始化为 `-1`。

```java
/**
 * 变化量
 * Somnia1337
 */
public long appealSum(String s)
{
	int[] last = new int[26];
	Arrays.fill(last, -1);
	long ans = 0, pre = 0;
	for (int i = 0; i < s.length(); i++)
	{
		pre += i - last[s.charAt(i) - 'a'];
		last[s.charAt(i) - 'a'] = i;
		ans += pre;
	}
	return ans;
}
```

#### [2276. 统计区间中的整数数目](https://leetcode.cn/problems/count-integers-in-intervals/)

@2023.12.16

```java
/**
 * 珂朵莉树
 * 灵茶山艾府
 */
class CountIntervals {
    private TreeMap<Integer, Integer> m = new TreeMap<>();
    private int cnt;
	
    public CountIntervals() {}
	
    public void add(int left, int right) {
        for (Map.Entry<Integer, Integer> e = m.ceilingEntry(left); e != null && e.getValue() <= right; e = m.ceilingEntry(left)) {
            int l = e.getValue(), r = e.getKey();
            left = Math.min(l, left);
            right = Math.max(r, right);
            cnt -= r - l + 1;
            m.remove(r);
        }
        cnt += right - left + 1;
        m.put(right, left);
    }
	
    public int count() {
        return cnt;
    }
}
```

#### [2344. 使数组可以被整除的最少删除次数](https://leetcode.cn/problems/minimum-deletions-to-make-array-divisible/)

@2023.12.27

#数学 

求整个 `numsDivide` 的最大公因数 `tar`，排序 `nums`，逐个检查是否整除 `tar`。

```java
/**
 * 数学
 * Somnia1337
 */
public int minOperations(int[] nums, int[] numsDivide) {
	int tar = numsDivide[0];
	for (int i = 1; i < numsDivide.length; i++) tar = gcd(numsDivide[i], tar);
	Arrays.sort(nums);
	for (int i = 0; i < nums.length && nums[i] <= tar; i++) {
		if (tar % nums[i] == 0) return i;
	}
	return -1;
}

private int gcd(int x, int y) {
	return y == 0 ? x : gcd(y, x % y);
}
```

#### [2350. 不可能得到的最短骰子序列](https://leetcode.cn/problems/shortest-impossible-sequence-of-rolls/)

@2024.01.10

#贪心 

以 `rolls=[4,2,1,2,3,3,2,4,1], k=4` 为例，前 5 个元素 `[4,2,1,2,3]` 是包含每个骰子面至少 1 次的最小值（前 4 个元素就没有包含 `3`），这样就得到了所有长为 1 的序列；再从第 6 个元素向后，`[3,2,4,1]` 又包含了每个骰子面，这样，从 `[0:5]` 中选取一个、从 `[5:9]` 中选取一个，就能组合成所有长为 2 的序列；以此类推。

实现时，遍历，每次集齐所有骰子面后，答案 `+1`，再重新开始收集。

```java
/**
 * 贪心
 * Fomalhaut
 */
public int shortestSequence(int[] rolls, int k) {
	Set<Integer> vis = new HashSet<>();
	int ans = 1;
	for (int r : rolls) {
		vis.add(r);
		if (vis.size() == k) {
			ans++;
			vis.clear();
		}
	}
	return ans;
}
```

#### [2360. 图中的最长环](https://leetcode.cn/problems/longest-cycle-in-a-graph/)

@2024.01.11

#搜索 

初始时间戳 `clock = 1`，首次访问结点 `x` 时记录访问时间 `time[x] = clock` 、更新 `clock`，并记录当前时间 `start = clock`，从当前结点出发检查是否有环，如果找到了之前访问过的结点 `x`，且之前访问 `x` 的时间不早于 `start`，说明找到了一个新的环，其环长即为两次访问 `x` 的时间差 `clock - time[x]`。取所有环长的最大值即为答案。

```java
/**
 * DFS
 * 灵茶山艾府
 */
public int longestCycle(int[] edges) {
	int n = edges.length, ans = -1;
	int[] time = new int[n];
	for (int i = 0, clock = 1; i < n; i++) {
		if (time[i] > 0) continue;
		for (int x = i, start = clock; x >= 0; x = edges[x]) {
			if (time[x] > 0) {
				if (time[x] >= start) ans = Math.max(ans, clock - time[x]);
				break;
			}
			time[x] = clock++;
		}
	}
	return ans;
}
```

#### [2398. 预算内的最多机器人数目](https://leetcode.cn/problems/maximum-number-of-robots-within-budget/)

@2024.01.05

#二分查找 #双指针 #单调队列 

1. 二分查找

二分答案，定长 `m` 的滑动窗口检查是否有连续 `m` 个机器人的运行总开销在预算以内。

难点是快速计算每个窗口内的最大值，使用单调队列记录下标，遍历：

- 队首过期时出队。
- 持续将队尾出队，直到队尾对应的元素 `>` 当前元素。

```java
/**
 * 二分查找 + 单调队列
 * Somnia1337
 */
public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
	int l = 0, r = chargeTimes.length;
	while (l <= r) {
		int m = l + r >> 1;
		if (check(chargeTimes, runningCosts, m, budget)) l = m + 1;
		else r = m - 1;
	}
	return Math.max(r, 0);
}

private boolean check(int[] c, int[] r, int m, long b) {
	Deque<Integer> dq = new ArrayDeque<>();
	long run = 0;
	for (int i = 0; i < c.length; i++) {
		if (i >= m) {
			if (!dq.isEmpty() && dq.peekFirst() <= i - m) dq.pollFirst();
			run -= r[i - m];
		}
		while (!dq.isEmpty() && c[dq.peekLast()] <= c[i]) dq.pollLast();
		dq.offerLast(i);
		run += r[i];
		if (i >= m - 1 && !dq.isEmpty() && run * m + c[dq.peek()] <= b) return true;
	}
	return false;
}
```

2. 双指针

双指针的移动符合单调性：`r` 右移时，区间越短、开销在预算以内的可能性越大。

同向双指针，仍然用单调队列维护下标，枚举 `r`，右移 `l` 直到有效，再更新答案。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
	long run = 0;
	int l = 0, r = 0, ans = 0;
	Deque<Integer> dq = new ArrayDeque<>();
	while (r < chargeTimes.length) {
		run += runningCosts[r];
		while (!dq.isEmpty() && chargeTimes[r] >= chargeTimes[dq.peekLast()]) dq.pollLast();
		dq.offerLast(r);
		while (!dq.isEmpty() && chargeTimes[dq.peekFirst()] + (r - l + 1) * run > budget) { // 超出预算时持续右移 l
			if (dq.peekFirst() == l) dq.pollFirst();
			run -= runningCosts[l++];
		}
		ans = Math.max(++r - l, ans);
	}
	return ans;
}
```

#### [2435. 矩阵中和能被 K 整除的路径](https://leetcode.cn/problems/paths-in-matrix-whose-sum-is-divisible-by-k/)

@2024.01.09

#动态规划 

`dp` 开到 `[rows+1][cols+1][k]`，`dp[i][j][v]` 表示到达 `grid[i-1][j-1]` 的、余 `k` 得 `v` 的方案数，答案即为 `dp[rows][cols][0]`。

初始化 `dp[1][0][0]` 或 `dp[0][1][0]` 为 `1`，转移方程见代码。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int numberOfPaths(int[][] grid, int k) {
	final int MOD = 1000000007;
	int rows = grid.length, cols = grid[0].length;
	int[][][] dp = new int[rows + 1][cols + 1][k];
	dp[1][0][0] = 1;
	for (int i = 1; i < rows + 1; i++) {
		for (int j = 1; j < cols + 1; j++) {
			for (int v = 0; v < k; v++) {
				dp[i][j][(v + grid[i - 1][j - 1]) % k] = (dp[i - 1][j][v] + dp[i][j - 1][v]) % MOD;
			}
		}
	}
	return dp[rows][cols][0];
}
```

#### [2454. 下一个更大元素 IV](https://leetcode.cn/problems/next-greater-element-iv/)

@2023.12.12

#栈 

两个单减栈，元素先进 `q1`，遇到更大元素后进 `q2`，再次遇到更大元素后更新答案。

遍历时，先检查 `q2` 写入答案，再检查 `q1` 中需要转移到 `q2` 的元素，最后将当前元素下标压入 `q1`。

```java
/**
 * 栈
 * Somnia1337
 */
public int[] secondGreaterElement(int[] nums) {
	Deque<Integer> q1 = new ArrayDeque<>(), q2 = new ArrayDeque<>();
	int[] ans = new int[nums.length];
	Arrays.fill(ans, -1);
	for (int i = 0; i < nums.length; i++) {
		while (!q2.isEmpty() && nums[q2.peekLast()] < nums[i]) ans[q2.pollLast()] = nums[i];
		Deque<Integer> t = new ArrayDeque<>();
		while (!q1.isEmpty() && nums[q1.peekLast()] < nums[i]) t.add(q1.pollLast());
		while (!t.isEmpty()) q2.addLast(t.pollLast());
		q1.add(i);
	}
	return ans;
}
```

#### [2581. 统计可能的树根数目](https://leetcode.cn/problems/count-number-of-possible-root-nodes/)

@2024.02.29



```java
/**
 * 换根 DP
 * 灵茶山艾府
 */
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

#### [2585. 获得分数的方法数](https://leetcode.cn/problems/number-of-ways-to-earn-points/)

@2023.12.16

#动态规划 

`dp[i][j]` 表示前 `i` 种题目组成 `j` 分的子问题答案。

对第 `i` 种题目，枚举做对题目数 `k` $\in [1, {\rm{min}}(cnt, \frac{j}{cnt})]$，第二级子问题为前 `i - 1` 种题目组成 `j - k * pnt` 分（`pnt` 为第 `i` 种题目的分值），递推式 `dp[i][j] = sum(dp[i - 1][j - k * pnt])`。

优化：`dp[i]` 行只依赖于 `dp[i - 1]` 行，状压到一维，倒序遍历。

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int waysToReachTarget(int target, int[][] types) {
	final int MOD = 1000000007;
	int[] dp = new int[target + 1];
	dp[0] = 1;
	for (int[] t : types) {
		int cnt = t[0], pnt = t[1];
		for (int j = target; j > 0; j--) {
			for (int k = 1; k <= cnt && k <= j / pnt; k++) {
				dp[j] = (dp[j] + dp[j - k * pnt]) % MOD;
			}
		}
	}
	return dp[target];
}
```

#### [2603. 收集树中金币](https://leetcode.cn/problems/collect-coins-in-a-tree/)

@2023.09.21

```java
/**
 * 拓扑排序
 * 灵茶山艾府
 */
public int collectTheCoins(int[] coins, int[][] edges)
{
	int n = coins.length;
	List[] g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	var deg = new int[n];
	for (var e : edges)
	{
		int x = e[0], y = e[1];
		g[x].add(y);
		g[y].add(x); // 建图
		deg[x]++;
		deg[y]++; // 统计每个节点的度数（邻居个数）
	}
	int leftEdges = n - 1; // 剩余边数
	// 拓扑排序，去掉没有金币的子树
	var q = new ArrayDeque<Integer>();
	for (int i = 0; i < n; i++)
	{
		if (deg[i] == 1 && coins[i] == 0)
		{ // 没有金币的叶子
			q.add(i);
		}
	}
	while (!q.isEmpty())
	{
		leftEdges--; // 删除节点到其父节点的边
		for (Object y : g[q.poll()])
		{
			if (--deg[(int) y] == 1 && coins[(int) y] == 0)
			{ // 没有金币的叶子
				q.add((Integer) y);
			}
		}
	}
	for (int i = 0; i < n; i++)
	{
		if (deg[i] == 1 && coins[i] == 1)
		{ // 有金币的叶子（判断 coins[i] 是避免把没有金币的叶子也算进来）
			q.add(i);
		}
	}
	leftEdges -= q.size(); // 删除所有叶子（到其父节点的边）
	for (int x : q)
	{ // 遍历所有叶子
		for (Object y : g[x])
		{
			if (--deg[(int) y] == 1)
			{ // y 现在是叶子了
				leftEdges--; // 删除 y（到其父节点的边）
			}
		}
	}
	return Math.max(leftEdges * 2, 0);
}
```

#### [2608. 图中的最短环](https://leetcode.cn/problems/shortest-cycle-in-a-graph/)

@2024.01.03

```java
/**
 * BFS
 * 灵茶山艾府
 */
private List<Integer>[] g;
private int[] dist;

public int findShortestCycle(int n, int[][] edges) {
	g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int[] e : edges) {
		g[e[0]].add(e[1]);
		g[e[1]].add(e[0]);
	}
	dist = new int[n];
	int ans = Integer.MAX_VALUE;
	for (int node = 0; node < n; node++) ans = Math.min(bfs(node), ans);
	return ans < Integer.MAX_VALUE ? ans : -1;
}

private int bfs(int node) {
	int ans = Integer.MAX_VALUE;
	Arrays.fill(dist, -1);
	dist[node] = 0;
	Deque<int[]> q = new ArrayDeque<>();
	q.add(new int[]{node, -1});
	while (!q.isEmpty()) {
		int[] p = q.poll();
		int x = p[0], d = p[1];
		for (int y : g[x])
			// 第一次遇到
			if (dist[y] < 0) {
				dist[y] = dist[x] + 1;
				q.add(new int[]{y, x});
			}
			// 第二次遇到
			else if (y != d) ans = Math.min(dist[x] + dist[y] + 1, ans);
	}
	return ans;
}
```

#### [2646. 最小化旅行的价格总和](https://leetcode.cn/problems/minimize-the-total-price-of-the-trips/)

@2023.12.06

```java
/**
 * DFS
 * 灵茶山艾府
 */
private List<Integer>[] g;
private int[] price, cnt;
private int end;

public int minimumTotalPrice(int n, int[][] edges, int[] price, int[][] trips)
{
	g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int[] e : edges)
	{
		int x = e[0], y = e[1];
		g[x].add(y);
		g[y].add(x);
	}

	cnt = new int[n];
	for (int[] t : trips)
	{
		end = t[1];
		dfs(t[0], -1);
	}

	this.price = price;
	int[] res = dp(0, -1);
	return Math.min(res[0], res[1]);
}

private boolean dfs(int x, int fa)
{
	if (x == end)
	{
		cnt[x]++;
		return true; // 找到 end
	}
	for (int y : g[x])
	{
		if (y != fa && dfs(y, x))
		{
			cnt[x]++; // x 是 end 的祖先节点，也就在路径上
			return true;
		}
	}
	return false; // 未找到 end
}

// 类似 337. 打家劫舍 III
private int[] dp(int x, int fa)
{
	int notHalve = price[x] * cnt[x]; // x 不变
	int halve = notHalve / 2; // x 减半
	for (int y : g[x])
	{
		if (y != fa)
		{
			int[] res = dp(y, x); // 计算 y 不变/减半的最小价值总和
			notHalve += Math.min(res[0], res[1]); // x 不变，那么 y 可以不变，可以减半，取这两种情况的最小值
			halve += res[0]; // x 减半，那么 y 只能不变
		}
	}
	return new int[]{notHalve, halve};
}
```

#### [2681. 英雄的力量](https://leetcode.cn/problems/power-of-heroes/)

@2023.12.16

#贡献法 

注意：本题要求的子结构为子序列，而非子数组。

排序，枚举子序列的最大值。设数列 `[a,b,c,d,e]`，取以 `d` 结尾（即作为最大值）的子序列组，讨论最小值：

- 选 `a`，则 `[b,c]` 选择与否不影响结果，共 $2^2$ 种方案，力量和为 $2^2 \times d^2 \times a$。
- 选 `b`，则 `[c]` 选择与否不影响结果，共 $2^1$ 种方案，力量和为 $2^1 \times d^2 \times b$。
- 选 `c`，共 $2^0$ 种方案，力量和为 $1 \times d^2 \times c$。
- 选 `d` 本身，即子序列为 `[d]`，力量和为 $d^3$。

因此，以 `d` 结尾时，`d` 及其左侧元素对答案的贡献为 $d^3 + d^2 \times (2^2 \times a + 2^1 \times b + 2^0 \times c)$，记 $2^2 \times a + 2^1 \times b + 2^0 \times c$ 为 `s`，则为 $d^3 + d^2 \times s = d^2 \times (d + s)$。

再以 `e` 结尾，得 $e^3 + e^2 \times (2^3 \times a + 2^2 \times b + 2^1 \times c + 2^0 \times d) = 2 \times s + d$。

每次枚举后，累加答案并更新 `s`。

```java
/**
 * 贡献法
 * 灵茶山艾府
 */
public int sumOfPower(int[] nums) {
	final long MOD = 1000000007;
	Arrays.sort(nums);
	long ans = 0, s = 0;
	for (long x : nums) {
		ans = (ans + x * x % MOD * (x + s)) % MOD;
		s = (s * 2 + x) % MOD; // 递推 s, 增加左侧元素的贡献值
	}
	return (int) ans;
}
```

#### [2719. 统计整数数目](https://leetcode.cn/problems/count-of-integers/)

@2024.01.16



```java
/**
 * 数位 DP
 * 灵茶山艾府
 */
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

#### [2736. 最大和查询](https://leetcode.cn/problems/maximum-sum-queries/)

@2023.11.17

```java
/**
 * 树状数组
 * ylb
 */
public int[] maximumSumQueries(int[] nums1, int[] nums2, int[][] queries)
{
	int len = nums1.length;
	int[][] nums = new int[len][0];
	for (int i = 0; i < len; i++) nums[i] = new int[]{nums1[i], nums2[i]};
	Arrays.sort(nums, (a, b) -> b[0] - a[0]);
	Arrays.sort(nums2);
	int q = queries.length;
	Integer[] idx = new Integer[q];
	for (int i = 0; i < q; i++) idx[i] = i;
	Arrays.sort(idx, (i, j) -> queries[j][0] - queries[i][0]);
	int[] ans = new int[q];
	int j = 0;
	BinaryIndexedTree tree = new BinaryIndexedTree(len);
	for (int i : idx)
	{
		int x = queries[i][0], y = queries[i][1];
		for (; j < len && nums[j][0] >= x; j++)
		{
			int k = len - Arrays.binarySearch(nums2, nums[j][1]);
			tree.update(k, nums[j][0] + nums[j][1]);
		}
		int p = Arrays.binarySearch(nums2, y);
		int k = p >= 0 ? len - p : len + p + 1;
		ans[i] = tree.query(k);
	}
	return ans;
}

class BinaryIndexedTree
{
	private int n;
	private int[] c;
	
	public BinaryIndexedTree(int n)
	{
		this.n = n;
		c = new int[n + 1];
		Arrays.fill(c, -1);
	}
	
	public void update(int x, int v)
	{
		while (x <= n)
		{
			c[x] = Math.max(c[x], v);
			x += x & -x;
		}
	}
	
	public int query(int x)
	{
		int mx = -1;
		while (x > 0)
		{
			mx = Math.max(mx, c[x]);
			x -= x & -x;
		}
		return mx;
	}
}
```

#### [2809. 使数组和小于等于 x 的最少时间](https://leetcode.cn/problems/minimum-time-to-make-array-sum-at-most-x/)

@2024.01.19



```java
/**
 * 动态规划
 * ?
 */
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

#### [2819. 购买巧克力后的最小相对损失](https://leetcode.cn/problems/minimum-relative-loss-after-buying-chocolates/)

@2024.02.14



```java
/**
 * 二分查找
 * 力扣官方题解
 */
public long[] minimumRelativeLosses(int[] prices, int[][] queries) {
	int n = prices.length;
	Arrays.sort(prices);
	long[] pre = new long[n + 1];
	for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + prices[i];
	long[] ans = new long[queries.length];
	for (int i = 0; i < queries.length; i++) {
		int k = queries[i][0], m = queries[i][1];
		if (m == n) {
			int x = bisect(prices, k);
			ans[i] = pre[x] + (long) k * (n - x) * 2 - (pre[n] - pre[x]);
			continue;
		}
		int need = n - m, idx = m;
		int l = 0, r = m - 1;
		while (l <= r) {
			int mid = l + r >> 1;
			if (k - prices[mid] > prices[mid + need] - k) l = mid + 1;
			else {
				r = mid - 1;
				idx = mid;
			}
		}
		long L = pre[idx];
		long R = (long) k * (n - idx - need) * 2 - (pre[n] - pre[idx + need]);
		ans[i] = L + R;
	}
	return ans;
}

private int bisect(int[] arr, int tar) {
	int ret = arr.length, l = 0, r = arr.length - 1;
	while (l <= r) {
		int m = l + r >> 1;
		if (arr[m] > tar) {
			ret = m;
			r = m - 1;
		}
		else l = m + 1;
	}
	return ret;
}
```

#### [2846. 边权重均等查询](https://leetcode.cn/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/)

@2024.01.26



```java
/**
 * ?
 * ?
 */
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

#### [2862. 完全子集的最大元素和](https://leetcode.cn/problems/maximum-element-sum-of-a-complete-subset-of-indices/)

@2023.09.17

#数学 

```java
/**
 * 质因子拆解
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

#### [2867. 统计树中的合法路径数目](https://leetcode.cn/problems/count-valid-paths-in-a-tree/)

@2024.02.27



```java
/**
 * DFS
 * 灵茶山艾府
 */
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

#### [2872. 可以被 K 整除连通块的最大数目](https://leetcode.cn/problems/maximum-number-of-k-divisible-components/)

@2024.01.22



```java
/**
 * DFS
 * 灵茶山艾府
 */
private List<Integer>[] g;
private int[] values;
private int k, ans;

public int maxKDivisibleComponents(int n, int[][] edges, int[] values, int k) {
	g = new ArrayList[n];
	Arrays.setAll(g, e -> new ArrayList<>());
	for (int[] e : edges) {
		int x = e[0], y = e[1];
		g[x].add(y);
		g[y].add(x);
	}
	this.values = values;
	this.k = k;
	ans = 0;
	dfs(0, -1); // 将结点 0 视为根, 它没有父结点
	return ans;
}

// 参数 p 表示 node 的父结点
private long dfs(int node, int p) {
	long sum = values[node];
	for (int next : g[node]) {
		// 有向树的体现: 遍历每个结点的非父邻居
		if (next != p) sum += dfs(next, node);
	}
	if (sum % k == 0) ans++;
	return sum;
}
```

#### [2897. 对数组执行操作使平方和最大](https://leetcode.cn/problems/apply-operations-on-array-to-maximize-sum-of-squares/)

@2023.10.08

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

#### [2931. 购买物品的最大开销](https://leetcode.cn/problems/maximum-spending-after-buying-items/)

@2024.01.18



```java
/**
 * 枚举
 * Somnia1337
 */
public long maxSpending(int[][] values) {
	int rows = values.length, cols = values[0].length;
	int[] ptr = new int[rows];
	Arrays.fill(ptr, cols - 1);
	long ans = 0;
	for (int day = 1; day <= rows * cols; day++) {
		int min = Integer.MAX_VALUE, idx = cols - 1;
		for (int i = 0; i < rows; i++) {
			if (ptr[i] == -1) continue;
			if (values[i][ptr[i]] < min) {
				min = values[i][ptr[i]];
				idx = i;
			}
		}
		ans += (long) values[idx][ptr[idx]--] * day;
	}
	return ans;
}
```

```java
/**
 * 排序
 * 灵茶山艾府
 */
public long maxSpending(int[][] values) {
	int rows = values.length, cols = values[0].length;
	int[] arr = new int[rows * cols];
	for (int i = 0; i < rows * cols; i += cols) System.arraycopy(values[i / cols], 0, arr, i, cols);
	Arrays.sort(arr);
	long ans = 0;
	for (int i = 0; i < arr.length; i++) ans += (long) arr[i] * (i + 1);
	return ans;
}
```

#### [2963. 统计好分割方案的数目](https://leetcode.cn/problems/count-the-number-of-good-partitions/)

@2023.12.10

#数学 #区间（待整理） 

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

#### [2968. 执行操作使频率分数最大](https://leetcode.cn/problems/apply-operations-to-maximize-frequency-score/)

@2023.12.17

#双指针 

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

#### [2972. 统计移除递增子数组的数目 II](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-ii/)

@2023.12.23

#双指针 

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
	while (r < len - 1) {
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

#### [2983. 回文串重新排列查询](https://leetcode.cn/problems/palindrome-rearrangement-queries/)

@2023.12.31

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

#### [3003. 执行操作后的最大分割数量](https://leetcode.cn/problems/maximize-the-number-of-partitions-after-operations/)

@2024.01.08

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

#### [3027. 人员站位的方案数 II](https://leetcode.cn/problems/find-the-number-of-ways-to-place-people-ii/)

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

#### [3031. 将单词恢复初始状态所需的最短时间 II](https://leetcode.cn/problems/minimum-time-to-revert-word-to-initial-state-ii/)

@2024.02.04



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

#### [3041. 修改数组后最大化数组中的连续元素数目](https://leetcode.cn/problems/maximize-consecutive-elements-in-an-array-after-modification/)

@2024.02.18



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

#### [LCP 24. 数字游戏](https://leetcode.cn/problems/5TxKeK/)

@2024.02.01



```java
/**
 * 贪心
 * 灵茶山艾府
 */
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