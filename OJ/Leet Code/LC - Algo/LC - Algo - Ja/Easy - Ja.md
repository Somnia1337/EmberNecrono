```text
题库: 1 9 14 20 26 27 28 35 58 66 67 69 70 88 118 119 121 125 136 163 168 169 171 190 191 202 205 217 219 225 228 231 232 242 243 252 258 263 266 268 278 283 290 292 293 303 326 338 342 344 345 346 349 350 367 374 383 387 389 392 401 409 412 414 415 434 441 448 455 459 461 463 476 482 485 492 495 496 500 504 506 507 509 520 521 541 551 557 561 566 575 594 598 599 605 628 643 645 657 661 674 680 682 693 696 697 704 709 717 724 728 734 744 746 747 748 762 766 771 796 804 806 812 819 821 824 830 832 836 844 859 860 867 868 883 884 888 892 896 905 908 914 917 922 925 929 941 942 944 953 961 976 977 989 997 999 1002 1005 1009 1013 1018 1021 1025 1037 1046 1047 1051 1071 1078 1089 1103 1108 1122 1128 1137 1154 1160 1185 1189 1200 1207 1217 1221 1232 1252 1260 1266 1267 1281 1287 1295 1299 1304 1309 1313 1317 1323 1331 1332 1337 1342 1346 1365 1370 1374 1380 1385 1389 1394 1399 1403 1408 1413 1417 1422 1431 1436 1437 1446 1450 1455 1460 1464 1470 1475 1480 1486 1491 1496 1502 1640 1732 1768 1784 2085 2103 2215 2235 2485 2500 2511 2520 2525 2558 2562 2578 2582 2586 2591 2605 2609 2652 2656 2660 2678 2682 2696 2697 2706 2744 2760 2765 2769 2788 2824 2828 2833 2839 2843 2848 2855 2859 2864 2869 2873 2894 2899 2903 2908 2913 2917 2923 2928 2932 2937 2942 2946 2951 2956 2960 2965 2970 2974 2980 2996 3000 3005 3010 3014 3024 3028 3033 3038 3042 3046
LCR: 120 122 125 126 128 146 147 161 169 173 182 186
LCP: 6 50
面试: 01.01 01.06 08.06
```

#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

@2023.06.12

如果先全部存入哈希表，将产生哈希冲突。

为了避免冲突，应先检查是否已找到答案，若否，再存入哈希表。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int[] twoSum(int[] nums, int target)
{
	Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
	
	for (int i = 0; i < nums.length; ++i)
	{
		if (hashtable.containsKey(target - nums[i])) //检查是否已为答案
			return new int[]{hashtable.get(target - nums[i]), i};
		hashtable.put(nums[i], i); //存入哈希表
	}
	
	return new int[0];
}
```

#### [9. 回文数](https://leetcode.cn/problems/palindrome-number/)

@2023.06.13

1. 字符串

取`x`的字符串，从两端向中间逐位检查是否相同。

```java
/**
 * 字符串
 * Somnia1337
 */
public boolean isPalindrome(int x)
{
	String s = String.valueOf(x);
	int l = s.length();
	
	for (int i = 0; i < l / 2 + 1; i++)
	{
		if (s.charAt(i) != s.charAt(l - 1 - i)) return false;
	}
	
	return true;
}
```

2. 反转一半

对`x`进行“余10除10”取其反转数，检查二者是否相等。

由于可能超过int范围，因此只反转`x`的一半，检查它与另一半是否相等。

边界：

- 负数不是回文，因为'-'不是数字。
- 除0外，所有个位为0的数字不是回文，因为`x`的最高位没有0占位。

```java
/**
 * 反转一半
 * 力扣官方题解
 */
public boolean isPalindrome(int x)
{
	if (x < 0 || (x % 10 == 0 && x != 0))
	{
		return false;
	}
	
	int revertedNumber = 0;
	
	while (x > revertedNumber) //只反转后一半，以免超出int范围
	{
		revertedNumber = revertedNumber * 10 + x % 10;
		x /= 10;
	}
	
	return x == revertedNumber || x == revertedNumber / 10;
}
```

#### [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

@2023.10.13

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int romanToInt(String s)
{
	String[] symbol = new String[]{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
	int[] nums = new int[]{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
	Map<String, Integer> map = new HashMap<>();
	for (int i = 0; i < symbol.length; i++)
	{
		map.put(symbol[i], nums[i]);
	}
	int ans = 0;
	int left = s.length() - 1, right = s.length();
	while (left >= 0)
	{
		while (left >= 0 && map.containsKey(s.substring(left, right))) left--;
		left++;
		ans += map.get(s.substring(left, right));
		right = left;
		left--;
	}
	return ans;
}
```

2. 模拟

可以完全照搬 [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/) 进行模拟。

```java
/**
 * 模拟
 * Acidicock
 */
public int romanToInt(String s)
{
	String[] symbol = new String[]{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
	int[] nums = new int[]{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
	int ans = 0;
	for (int i = 0; i < symbol.length; i++)
	{
		while (s.startsWith(symbol[i]))
		{
			ans += nums[i];
			s = s.substring(symbol[i].length());
		}
		if (s.isEmpty()) break;
	}
	return ans;
}
```

#### [14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

@2023.06.14

1. 纵向扫描

反复遍历所有字符串，比较第i个字符，出现不同时，返回相同的部分。

```java
/**
 * 纵向扫描
 * Somnia1337
 */
public String longestCommonPrefix(String[] strs)
{
	StringBuilder ret = new StringBuilder();
	int len0 = strs[0].length();
	
	for (int i = 0; i < len0; i++) //最多遍历次数为第1个串的长度
	{
		char c = strs[0].charAt(i); //取第1个串的第i个字符
		for (String s : strs) //遍历其余字符串，比较第i个字符与c
		{
			if (s.length() == i || s.charAt(i) != c) return ret.toString();
		}
		ret.append(c);
	}
	
	return ret.toString();
}
```

优化：无需存储公共前缀字符串，只需在出现不同字符时返回长度为当前遍历次数的子串，因为这部分子串已经检查过，为公共前缀。

```java
/**
 * 纵向扫描
 * 力扣官方题解
 */
public String longestCommonPrefix(String[] strs)
{
	if (strs == null || strs.length == 0) return "";
	
	int length = strs[0].length();
	int count = strs.length;
	
	for (int i = 0; i < length; i++) //最多遍历次数为第1个串的长度
	{
		char c = strs[0].charAt(i);
		for (int j = 1; j < count; j++)
		{
			if (i == strs[j].length() || strs[j].charAt(i) != c) return strs[0].substring(0, i); //返回长度为当前遍历次数的子串
		}
	}
	
	return strs[0]; //没有找到不同字符，返回第1个串
}
```

2. 横向扫描

先假设第1个串的全部为最长公共前缀，然后遍历余下字符串，不断更新最长公共前缀。

```java
/**
 * 横向扫描
 * 力扣官方题解
 */
public String longestCommonPrefix(String[] strs)
{
	if (strs == null || strs.length == 0)
	{
		return "";
	}
	
	String prefix = strs[0]; //假设第1个串的全部为最长公共前缀
	int count = strs.length;
	
	for (int i = 1; i < count; i++)
	{
		prefix = longestCommonPrefix(prefix, strs[i]); //更新最长公共前缀
		if (prefix.isEmpty()) break; //最长公共前缀为空时停止更新
	}
	return prefix;
}

//返回两个串的最长公共前缀
public String longestCommonPrefix(String str1, String str2)
{
	int length = Math.min(str1.length(), str2.length());
	int index = 0;
	
	while (index < length && str1.charAt(index) == str2.charAt(index)) index++;
	
	return str1.substring(0, index);
}
```

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

@2023.06.14

用栈存储所有左括号，遇到右括号时，如果…

- 栈为空，匹配失败。
- 栈不为空，检查弹出的左括号是否匹配。

```java
/**
 * 栈
 * Somnia1337
 */
public boolean isValid(String s) {
	Deque<Character> stk = new ArrayDeque<>();
	for (int i = 0; i < s.length(); i++) { // 遍历 s
		char c = s.charAt(i);
		if (c == '(' || c == '[' || c == '{') stk.push(c); // 左括号, 压栈
		else if (stk.isEmpty()) return false; // 右括号且栈为空, 匹配失败
		else { // 右括号且栈非空, 检查弹出的左括号是否匹配
			char p = stk.pop();
			if (p == '(' && c != ')' || p == '[' && c != ']' || p == '{' && c != '}') return false;
		}
	}
	return stk.isEmpty();
}
```

优化：将字符集存储在哈希表中，简化匹配操作。

```java
/**
 * 栈
 * 力扣官方题解
 */
public boolean isValid(String s) {
	int n = s.length();
	if (n % 2 == 1) return false; // 只有长为偶数时才可能匹配
	Map<Character, Character> pair = new HashMap<>() {{
		put(')', '(');
		put(']', '[');
		put('}', '{');
	}}; // 将字符集存储在哈希表中, 右括号为键, 左括号为值
	Deque<Character> stk = new ArrayDeque<>();
	for (int i = 0; i < n; i++) {
		char c = s.charAt(i);
		if (pair.containsKey(c)) { // 右括号
			if (stk.isEmpty() || stk.peek() != pair.get(c)) return false; // 栈为空 || 栈顶元素不匹配
			stk.pop(); // 匹配, 弹栈
		}
		else stk.push(c); // 左括号, 压栈
	}
	return stk.isEmpty();
}
```

#### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

@2023.06.14

数组`nums`有序，因此相同元素的下标连续。

定义快指针`fast`与慢指针`slow`，

- `fast`指向遍历数组时每次循环到达的下标位置。
- `slow`在`fast`遍历至不同值时更新，指向下一个不同值将填入的位置。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int removeDuplicates(int[] nums)
{
	int n = nums.length;
	
	if (n == 0) return 0;
	
	int fast = 1, slow = 1; //从下标1开始检查
	
	while (fast < n)
	{
		if (nums[fast] != nums[fast - 1]) //fast处元素与其前一个元素不同
		{
			nums[slow] = nums[fast]; //fast处元素复制到slow处
			++slow; //右移slow
		}
		++fast;
	}
	
	return slow; //slow之前的元素不包含重复项
}
```

#### [27. 移除元素](https://leetcode.cn/problems/remove-element/)

@2023.06.15

```java
/**
 * （快慢）双指针
 * Somnia1337
 */
public int removeElement(int[] nums, int val)
{
	int len = nums.length;
	
	if (len == 0) return 0;
	
	int fast = 0, slow = 0;
	
	while (fast < len)
	{
		if (nums[fast] != val) //fast处的元素不是删除目标
		{
			nums[slow] = nums[fast]; //fast处元素复制到slow处
			slow++; //右移slow
		}
		fast++;
	}
	
	return slow; //slow之前的元素不包含删除目标项
}
```

当`val`出现在`nums`开头时，其后的每个元素都需要移动。

优化：注意到题目允许元素顺序改变，改为使用左右双指针，当`nums[left]` = `val`时，将`nums[right]`复制到`nums[left]`。

```java
/**
 * （左右）双指针
 * 力扣官方题解
 */
public int removeElement(int[] nums, int val)
{
	int left = 0;
	int right = nums.length;
	
	while (left < right)
	{
		if (nums[left] == val) //left处的元素为删除目标
		{
			nums[left] = nums[right - 1]; //right处元素复制到left处
			right--; //左移right
		}
		else left++; //left处的元素不是删除目标，右移left
	}
	
	return left; //left之前的元素不包含删除目标项
}
```

#### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

@2023.06.30

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int strStr(String haystack, String needle)
{
	int len1 = haystack.length(), len2 = needle.length();
	char c = needle.charAt(0);
	for (int i = 0; i < len1; i++)
		if (haystack.charAt(i) == c && i + len2 <= len1 && haystack.substring(i, i + len2).equals(needle)) return i;
	return -1;
}
```

#### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

@2023.06.15

题目要求时间复杂度为$O(\log N)$，只能使用二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int searchInsert(int[] nums, int target)
{
	int left = 0, right = nums.length - 1;
	
	while (left <= right)
	{
		int mid = (left + right) / 2;
		if (nums[mid] == target) return mid;
		else if (nums[mid] < target) left = mid + 1;
		else right = mid - 1;
	}
	
	return left;
}
```

#### [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

@2023.06.15

要求最后一个单词的长度，无需处理前面的单词，只需反向遍历。

```java
/**
 * 反向遍历
 * Somnia1337
 */
public int lengthOfLastWord(String s)
{
	int len = s.length();
	int ans = 0;
	
	for (int i = len - 1; i >= 0; i--)
	{
		if (s.charAt(i) != ' ') ans++; //记录有效字符
		else if (ans > 0) return ans; //当有效字符数不为0，再次遇到空格时返回
	}
	
	return ans;
}
```

#### [66. 加一](https://leetcode.cn/problems/plus-one/)

@2023.06.15

1. 分析尾随9

题目关键在于`digits`尾随9的个数——

- 没有9：将尾元素 + 1。
- 有若干9：找到首个非9值，将其 + 1，其后元素置0。
- 所有位均为9：新建比原数组长1位的数组，首元素置1，其余元素置0。

```java
/**
 * 分析尾随9
 * 力扣官方题解
 */
public int[] plusOne(int[] digits)
{
	int n = digits.length;
	
	for (int i = n - 1; i >= 0; --i) //反向遍历
	{
		if (digits[i] != 9) //首个非9值
		{
			++digits[i]; //将其+1
			for (int j = i + 1; j < n; ++j) //其后元素置0
			{
				digits[j] = 0;
			}
			return digits;
		}
	}
	
	int[] ans = new int[n + 1]; //如果之前没有执行return，说明数组全为9，创建新数组
	ans[0] = 1; //将新数组首元素置1
	
	return ans;
}
```

2. 加1判0

倒序遍历`digits`，对各位重赋值`(digits[i] + 1) % 10`后判断是否为0（本质也是判9，但更简洁易懂），若是则继续，若否则返回。

```java
/**
 * 加1判0
 * inuter
 */
public int[] plusOne(int[] digits)
{
	int len = digits.length;
	
	for (int i = len - 1; i >= 0; i--)
	{
		digits[i] = (digits[i] + 1) % 10; //如果该位为9，则会置0，否则值+1
		if (digits[i] != 0) return digits; //如果当前位非9，返回
	}
	
	digits = new int[len + 1]; //如果之前没有执行return，说明数组全为9
	digits[0] = 1; //将新数组首元素置1
	
	return digits;
}
```

#### [67. 二进制求和](https://leetcode.cn/problems/add-binary/)

@2023.06.15

1. 进制转换

利用Java自带的高精度，转换为十进制直接计算。

```java
/**
 * 进制转换
 * 力扣官方题解
 */
public String addBinary(String a, String b)
{
	return Integer.toBinaryString(Integer.parseInt(a, 2) + Integer.parseInt(b, 2));
}
```

2. 模拟

模拟二进制数加法的竖式计算。

```java
/**
 * 模拟
 * 力扣官方题解
 */
public String addBinary(String a, String b)
{
	StringBuffer ans = new StringBuffer();
	int n = Math.max(a.length(), b.length()), carry = 0;
	
	for (int i = 0; i < n; ++i)
	{
		carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
		carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
		ans.append((char) (carry % 2 + '0'));
		carry /= 2;
	}
	
	if (carry > 0)
	{
		ans.append('1');
	}
	ans.reverse();
	
	return ans.toString();
}
```

#### [69. x 的平方根](https://leetcode.cn/problems/sqrtx/)

@2023.06.15

暴力超时。

这是一道常见的面试题，一般思路有：

- 通过其它的数学函数代替平方根函数得到精确结果，取整数部分作为答案。
- 通过数学方法得到近似结果，直接作为答案。

1. 数学变换

$$\sqrt{x} = x^{1/2} = (e^{\ln x})^{1/2} = e^{\frac{1}{2} \ln x}$$

由于浮点数的误差，计算完成后需检查真正的答案是`ans`还是`ans + 1`。

```java
/**
 * 数学变换
 * 力扣官方题解
 */
public int mySqrt(int x)
{
	if (x == 0) return 0;
	
	int ans = (int) Math.exp(0.5 * Math.log(x));
	
	return (long) (ans + 1) * (ans + 1) <= x ? ans + 1 : ans;
}
```

2. 二分查找

在0~`x`内二分查找。

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public int mySqrt(int x)
{
	int l = 0, r = x, ans = -1;
	
	while (l <= r)
	{
		int mid = l + (r - l) / 2; //防止超出int范围
		if ((long) mid * mid <= x)
		{
			ans = mid;
			l = mid + 1;
		}
		else
		{
			r = mid - 1;
		}
	}
	
	return ans;
}
```

3. 牛顿迭代

泰勒。

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

@2023.06.16

1. 动态规划

```java
/**
 * 动态规划（记忆化）
 * Somnia1337
 */
public int climbStairs(int n)
{
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	
	return climbStairs(n, hashmap);
}

public int climbStairs(int n, HashMap<Integer, Integer> memo)
{
	if (n == 1) return 1;
	else if (n == 2) return 2;
	if (!memo.containsKey(n))
	{
		int value = climbStairs(n - 2, memo) + climbStairs(n - 1, memo);
		memo.put(n, value);
	}
	
	return memo.get(n);
}
```

优化：使用滚动数组。

![](https://assets.leetcode-cn.com/solution-static/70/70_fig1.gif)

```java
/**
 * 动态规划（滚动数组）
 * 力扣官方题解
 */
public int climbStairs(int n)
{
	int p = 0, q = 0, r = 1;
	
	for (int i = 1; i <= n; ++i)
	{
		p = q;
		q = r;
		r = p + q;
	}
	
	return r;
}
```

2. 数学

矩阵快速幂可以将动态规划的$O(N)$优化为$O(\log N)$。

$$\begin{bmatrix} 1&1 \\ 1&0 \end{bmatrix} \begin{bmatrix} f(n) \\ f(n - 1) \end{bmatrix} = \begin{bmatrix} f(n) + f(n - 1) \\ f(n) \end{bmatrix} = \begin{bmatrix} f(n + 1) \\ f(n) \end{bmatrix}$$

$$\begin{bmatrix} f(n + 1) \\ f(n) \end{bmatrix} = {\begin{bmatrix} 1&1 \\ 1&0 \end{bmatrix}}^n \begin{bmatrix} f(1) \\ f(0) \end{bmatrix}$$

```java
/**
 * 数学（矩阵快速幂）
 * 力扣官方题解
 */
public int climbStairs(int n)
{
	int[][] q = {{1, 1}, {1, 0}};
	int[][] res = pow(q, n);
	
	return res[0][0];
}

public int[][] pow(int[][] a, int n)
{
	int[][] ret = {{1, 0}, {0, 1}};
	
	while (n > 0)
	{
		if ((n & 1) == 1)
		{
			ret = multiply(ret, a);
		}
		n >>= 1;
		a = multiply(a, a);
	}
	
	return ret;
}

public int[][] multiply(int[][] a, int[][] b)
{
	int[][] c = new int[2][2];
	
	for (int i = 0; i < 2; i++)
	{
		for (int j = 0; j < 2; j++)
		{
			c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
		}
	}
	
	return c;
}
```

也可以通过数学变换求得通项公式。
$$f(x) = \frac{1}{\sqrt{5}} [(\frac{1 + \sqrt{5}}{2})^n - (\frac{1 - \sqrt{5}}{2})^n]$$

```java
/**
 * 数学（通项公式）
 * 力扣官方题解
 */
public int climbStairs(int n)
{
	double sqrt5 = Math.sqrt(5);
	double fibn = Math.pow((1 + sqrt5) / 2, n + 1) - Math.pow((1 - sqrt5) / 2, n + 1);
	return (int) Math.round(fibn / sqrt5);
}
```

#### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

@2023.09.06

1. 辅助数组

```java
/**
 * 辅助数组
 * Somnia1337
 */
public void merge(int[] nums1, int m, int[] nums2, int n) {
	int[] ans = new int[m + n];
	int i1 = 0, i2 = 0, j = 0;
	while (j < m + n) {
		if (i2 == n || i1 < m && nums1[i1] < nums2[i2]) {
			ans[j] = nums1[i1];
			i1++;
		}
		else {
			ans[j] = nums2[i2];
			i2++;
		}
		j++;
	}
	for (j = 0; j < m + n; j++) nums1[j] = ans[j];
}
```

2. 双指针

```java
/**
 * 双指针
 * 不上guardian不改名
 */
public void merge(int[] nums1, int m, int[] nums2, int n) {
	int i1 = m - 1, i2 = n - 1, j = m + n - 1;
	while (i1 >= 0 && i2 >= 0) {
		if (nums1[i1] >= nums2[i2]) nums1[j--] = nums1[i1--];
		else nums1[j--] = nums2[i2--];
	}
	while (i2 >= 0) nums1[j--] = nums2[i2--];
}
```

#### [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

@2023.06.16

杨辉三角的性质：

- 每行对称，从1开始先增后减。
- 第$n$行（首行为$0$行）有$n + 1$项，前$n$行共有$\frac{n (n + 1)}{2}$项。
- 第$n$行的第$m$项可表示为组合数$C_n^m$，对应$(a + b)^n$展开式中各项系数。
- 每项等于其左上与右上的两项之和，即$C_n^m = C_{n - 1}^m + C_{n - 1}^{m - 1}$。

利用最后一条性质可以逐行计算杨辉三角。

```java
/**
 * 逐行计算
 * 力扣官方题解
 */
public List<List<Integer>> generate(int numRows)
{
	List<List<Integer>> ret = new ArrayList<List<Integer>>();
	
	for (int i = 0; i < numRows; ++i)
	{
		List<Integer> row = new ArrayList<Integer>();
		for (int j = 0; j <= i; ++j)
		{
			if (j == 0 || j == i)
			{
				row.add(1);
			}
			else
			{
				row.add(ret.get(i - 1)
						   .get(j - 1) + ret.get(i - 1)
											.get(j));
			}
		}
		ret.add(row);
	}
	
	return ret;
}
```

#### [119. 杨辉三角 II](https://leetcode.cn/problems/pascals-triangle-ii/)

@2023.06.17

公式$C_n^m = \frac{n!}{m! (n - m)!}$在阶乘过大时产生溢出错误。

1. 递推

```java
/**
 * 递推（逐行计算）
 * 力扣官方题解
 */
public List<Integer> getRow(int rowIndex)
{
	List<List<Integer>> C = new ArrayList<List<Integer>>();
	
	for (int i = 0; i <= rowIndex; ++i)
	{
		List<Integer> row = new ArrayList<Integer>();
		for (int j = 0; j <= i; ++j)
		{
			if (j == 0 || j == i)
			{
				row.add(1);
			}
			else
			{
				row.add(C.get(i - 1)
						 .get(j - 1) + C.get(i - 1)
										.get(j));
			}
		}
		C.add(row);
	}
	
	return C.get(rowIndex);
}
```

优化：使用滚动数组，每次倒序计算每行，杨辉三角的递推式表明当前行的第$i$项只与上一行的第$i$项及第$i - 1$项相关，倒序计算至第$i$项时，第$i - 1$项仍为上一行的值，即只用一个一维数组轮流表示杨辉三角的各行。

```java
/**
 * 递推（滚动数组）
 * 力扣官方题解
 */
public List<Integer> getRow(int rowIndex)
{
	List<Integer> row = new ArrayList<Integer>();
	row.add(1);
	
	for (int i = 1; i <= rowIndex; ++i)
	{
		row.add(0);
		for (int j = i; j > 0; --j)
		{
			row.set(j, row.get(j) + row.get(j - 1));
		}
	}
	
	return row;
}
```

2. 线性递推

使用行内递推公式$C_n^m = C_n^{m - 1} \times \frac{n - m + 1}{m}$，结合任何行的首项$C_n^0 = 1$，直接计算每行的所有项。

```java
/**
 * 线性递推
 * 力扣官方题解
 */
public List<Integer> getRow(int rowIndex)
{
	List<Integer> row = new ArrayList<Integer>();
	row.add(1);
	
	for (int i = 1; i <= rowIndex; ++i)
	{
		row.add((int) ((long) row.get(i - 1) * (rowIndex - i + 1) / i));
	}
	
	return row;
}
```

#### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

@2023.06.17

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxProfit(int[] prices) {
	int min = prices[0], ans = 0;
	for (int price : prices) {
		ans = Math.max(price - min, ans);
		min = Math.min(price, min);
	}
	return ans;
}
```

#### [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

@2023.06.17

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean isPalindrome(String s)
{
	int len = s.length();
	s = s.toLowerCase();
	int left = 0, right = len - 1;
	
	while (left < right)
	{
		//使left指向下一个有效字符
		while (left < right && !(s.charAt(left) >= 48 && s.charAt(left) <= 57 || s.charAt(left) >= 97 && s.charAt(left) <= 122)) left++;
		//使right指向下一个有效字符
		while (right > left && !(s.charAt(right) >= 48 && s.charAt(right) <= 57 || s.charAt(right) >= 97 && s.charAt(right) <= 122)) right--;
		if (s.charAt(left) != s.charAt(right)) return false;
		left++;
		right--;
	}
	
	return true;
}
```

可以调用方法Character.isLetterOrDigit检查一个字符是否为字母或数字。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean isPalindrome(String s)
{
	int n = s.length();
	int left = 0, right = n - 1;
	
	while (left < right)
	{
		while (left < right && !Character.isLetterOrDigit(s.charAt(left)))
		{
			++left;
		}
		while (left < right && !Character.isLetterOrDigit(s.charAt(right)))
		{
			--right;
		}
		if (left < right)
		{
			if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right)))
			{
				return false;
			}
			++left;
			--right;
		}
	}
	
	return true;
}
```

#### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

@2023.06.18

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int singleNumber(int[] nums)
{
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	
	for (int n : nums)
	{
		hashmap.merge(n, 1, Integer::sum);
	}
	for (int n : nums)
	{
		if (hashmap.get(n) == 1) return n;
	}
	
	return -1;
}
```

2. 位运算

异或$\oplus$的性质：

- 任何数异或0 = 自身（$a \oplus 0 = a$）。
- 任何数异或自身 = 0（$a \oplus a = 0$）。
- 交换律、结合律。

假设数组元素为$a_1$~$a_n$，只出现一次的元素为$a_n$，则所有元素与0异或的结果为
$$(a_1 \oplus a_1) \oplus \cdots \oplus (a_{n - 1} \oplus a_{n - 1}) \oplus a_n \oplus 0 = a_n \oplus 0 = a_n$$
这样，通过直接计算得到了只出现一次的元素。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int singleNumber(int[] nums)
{
	int single = 0;
	
	for (int num : nums)
	{
		single ^= num;
	}
	
	return single;
}
```

#### [163. 缺失的区间](https://leetcode.cn/problems/missing-ranges/)

@2023.10.03

遍历 `nums`，如果 `num` > `lower`，将 `[lower, num - 1]` 加入解集，并更新 `lower` 为 `num + 1`。

```java
/**
 * 一次遍历
 * 事无巨细鹏
 */
public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper)
{
	List<List<Integer>> ans = new ArrayList<>();
	for (int num : nums)
	{
		if (num > lower)
		{
			ans.add(Arrays.asList(lower, num - 1));
		}
		lower = num + 1;
	}
	if (lower <= upper)
	{
		ans.add(Arrays.asList(lower, upper));
	}
	return ans;
}
```

#### [168. Excel表列名称](https://leetcode.cn/problems/excel-sheet-column-title/)

@2023.06.18

两张哈希表写了快一小时一直有问题🤡👈🤣

1. 进制

从进制角度考虑的关键在于，这个进制不是从0，而是从1开始（十进制的一位数为0~9，而此题中一位字母对应值为1~26，而不是0~25）。

深夜（00:52）AC。

```java
/**
 * 进制
 * Somnia1337
 */
public String convertToTitle(int columnNumber)
{
	StringBuilder sb = new StringBuilder();
	
	while (columnNumber > 0)
	{
		char c = (char) (columnNumber % 26 + 'A');
		if (c == 'A')
		{
			c = 'Z';
			columnNumber -= 1;
		}
		sb.append(String.valueOf(c));
		columnNumber /= 26;
	}
	
	return sb.reverse()
			 .toString();
}
```

2. 数学

$$columnNumber = \sum_{i = 0}^{n - 1} a_i \times 26^i$$

本题即给出`columnNumber`求解$a_i$。

分离$a_0$，提出其余项公因数26
$$cN = a_0 + 26 \times \sum_{i = 1}^{n - 1} a_i \times 26^{i - 1}$$
两边同时 - 1
$$cN - 1 = (a_0 - 1) + 26 \times \sum_{i = 1}^{n - 1} a_i \times 26^{i - 1}$$
$a_0$为$(cN - 1) \% 26$，得到$a_0$后，令$cN^\prime = \frac{cN - a_0}{26}$，有
$$cN^\prime = a_1 + 26 \times \sum_{i = 2}^{n - 1} a_i \times 26^{i - 2}$$
以此类推，直到新的$cN$为0。

对于整数$n$，若$n \% 26 = 0$，则有
$$\frac{n}{26} = [\frac{n + r}{26}]$$
其中，$0 \leqslant r \leqslant 25$，因此有
$$cN^\prime = \frac{cN - a_0}{26} = [\frac{(cN - a_0) + (a_0 - 1)}{26}] = [\frac{cN - 1}{26}]$$

```java
/**
 * 数学
 * 力扣官方题解
 */
public String convertToTitle(int columnNumber)
{
	StringBuilder sb = new StringBuilder();
	
	while (columnNumber != 0)
	{
		columnNumber--;
		sb.append((char) (columnNumber % 26 + 'A'));
		columnNumber /= 26;
	}
	
	return sb.reverse()
			 .toString();
}
```

#### [169. 多数元素](https://leetcode.cn/problems/majority-element/)

@2023.06.19

1. 排序

存在一个值的个数大于数组长度的一半，那么对`nums`排序后，`nums[nums.length / 2]`即为该值，因为无论在前一半还是后一半，都能覆盖中间位置。

```java
/**
 * 排序
 * Somnia1337
 */
public int majorityElement(int[] nums)
{
	Arrays.sort(nums);
	
	return nums[nums.length / 2];
}
```

2. Boyer-Moore投票

> 1. 维护一个候选众数`candidate`及其出现次数`count`，初始时`candidate`任意，`count`为0。
> 2. 遍历`nums`，对每个`n`，如果此时`count`为0则将`candidate`置`n`，然后判断`n`：
> 	1. `n == candidate`，则`count++`。
> 	2. `n != candidate`，则`count--`。
> 3. 返回`candidate`。

例如：

```text
nums:     [7, 7, 5, 7, 5, 1 | 5, 7 | 5, 5, 7, 7 | 7, 7, 7, 7]
candidate: 7  7  7  7  7  7   5  5   5  5  5  5   7  7  7  7
count:     1  2  1  2  1  0   1  0   1  2  1  0   1  2  3  4
value:     1  2  1  2  1  0  -1  0  -1 -2 -1  0   1  2  3  4
//value指到当前的这一步遍历为止，众数出现的次数比非众数多出现的次数
```

每一步遍历中，`count`与`value`只可能相等（`candidate`为众数）或为相反数（`candidate`不是众数）。

```java
/**
 * Boyer-Moore投票
 * 力扣官方题解
 */
public int majorityElement(int[] nums)
{
	int count = 0;
	Integer candidate = null;
	
	for (int num : nums)
	{
		if (count == 0)
		{
			candidate = num;
		}
		count += (num == candidate) ? 1 : -1;
	}
	
	return candidate;
}
```

可以形象理解为，众数为一队士兵，其他数为另一队，两队每次战斗都会各损失一名士兵，最后剩下的士兵为众数（众数队士兵更多）。

```java
/**
 * Boyer-Moore投票
 * 鹤滨
 */
public int majorityElement(int[] nums)
{
	int winner = nums[0];
	int count = 1;
	
	for (int i = 1; i < nums.length; i++)
	{
		if (winner == nums[i])
		{
			count++;
		}
		else if (count == 0)
		{
			winner = nums[i];
			count++;
		}
		else
		{
			count--;
		}
	}
	
	return winner;
}
```

#### [171. Excel 表列序号](https://leetcode.cn/problems/excel-sheet-column-number/)

@2023.06.19

```java
/**
 * 进制
 * Somnia1337
 */
public int titleToNumber(String columnTitle)
{
	StringBuilder sb = new StringBuilder(columnTitle);
	String cT = sb.reverse()
				  .toString();
	int ret = 0;
	
	for (int i = 0; i < cT.length(); i++)
	{
		ret += (cT.charAt(i) - 64) * Math.pow((double) 26, (double) i);
	}
	
	return ret;
}
```

#### [190. 颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)

@2023.07.08

1. 库函数

```java
/**
 * 库函数
 * Yadong
 */
public int reverseBits(int n)
{
	return Integer.reverse(n);
}
```

2. 位运算

```java
/**
 * 位运算（逐位颠倒）
 * 力扣官方题解
 */
public int reverseBits(int n)
{
	int rev = 0;
	
	for (int i = 0; i < 32 && n != 0; ++i)
	{
		rev |= (n & 1) << (31 - i);
		n >>>= 1;
	}
	
	return rev;
}
```

优化：分治。

- 原数据为：12345678
- 第一轮奇偶位交换：21436587
- 第二轮每两位交换：43218765
- 第三轮每四位交换：87654321

```java
/**
 * 位运算（分治）
 * 力扣官方题解
 */
private static final int M1 = 0x55555555; //01010101010101010101010101010101
private static final int M2 = 0x33333333; //00110011001100110011001100110011
private static final int M4 = 0x0f0f0f0f; //00001111000011110000111100001111
private static final int M8 = 0x00ff00ff; //00000000111111110000000011111111

public int reverseBits(int n)
{
	n = n >>> 1 & M1 | (n & M1) << 1;
	n = n >>> 2 & M2 | (n & M2) << 2;
	n = n >>> 4 & M4 | (n & M4) << 4;
	n = n >>> 8 & M8 | (n & M8) << 8;
	return n >>> 16 | n << 16;
}
```

#### [191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)

@2023.07.08

1. 字符串

```java
/**
 * 字符串
 * Somnia1337
 */
public int hammingWeight(int n)
{
	String s = Integer.toBinaryString(n);
	int cnt = 0;
	
	for (int i = 0; i < s.length(); i++)
	{
		if (s.charAt(i) == '1') cnt++;
	}
	
	return cnt;
}
```

2. 位运算

运算`n & (n - 1)`的结果为将`n`的最低位1变为0的值（例如，$6 \& (6 - 1) = 4$，$6 = (101)_2$，$4 = (100)_2$）。可以在`n` > 0时循环进行`n & (n - 1)`，循环的次数即为`n`中位1的个数。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int hammingWeight(int n)
{
	int ret = 0;
	
	while (n != 0)
	{
		n &= n - 1;
		ret++;
	}
	
	return ret;
}
```

3. 库函数

```java
/**
 * 库函数
 * nicolas
 */
public int hammingWeight(int n)
{
	return Integer.bitCount(n);
}
```

#### [202. 快乐数](https://leetcode.cn/problems/happy-number/)

@2023.06.19

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isHappy(int n)
{
	HashSet<Integer> hashset = new HashSet<>();
	
	while (true)
	{
		int newN = 0;
		while (n > 0)
		{
			newN += Math.pow(n % 10, 2);
			n /= 10;
		}
		n = newN;
		while (newN % 10 == 0) newN /= 10;
		if (newN == 1) return true;
		else if (hashset.contains(newN)) return false;
		hashset.add(newN);
	}
}
```

2. 双指针

反复调用`getNext(n)`得到的是隐式的链表，可以通过快慢双指针检查该链表是否存在环，`slowRunner`每次前进1步，`fastRunner`每次前进两步。如果没有环，`fastRunner`将先于`slowRunner`到达1，否则两指针将在某处相遇。

![[Snipaste_ 230619_140117.png|300]]

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean isHappy(int n)
{
	int slowRunner = n;
	int fastRunner = getNext(n);
	
	while (fastRunner != 1 && slowRunner != fastRunner)
	{
		slowRunner = getNext(slowRunner);
		fastRunner = getNext(getNext(fastRunner));
	}
	
	return fastRunner == 1;
}

public int getNext(int n)
{
	int totalSum = 0;
	
	while (n > 0)
	{
		int d = n % 10;
		n = n / 10;
		totalSum += d * d;
	}
	
	return totalSum;
}
```

#### [205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

@2023.06.19

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isIsomorphic(String s, String t)
{
	int len = s.length();
	HashMap<Character, Character> hashmap = new HashMap<>();
	
	for (int i = 0; i < len; i++)
	{
		char c1 = s.charAt(i), c2 = t.charAt(i);
		//如果get(c1)!=c2，或者不含键c1却含值c2，均可说明c1与c2对应错乱
		if (hashmap.containsKey(c1) && hashmap.get(c1) != c2 || !(hashmap.containsKey(c1)) && hashmap.containsValue(c2)) return false;
		hashmap.put(c1, c2);
	}
	
	return true;
}
```

2. 一次遍历

调用`s.indexOf(s.charAt(i))`将返回该字符在`s`中最早出现的位置，如果`s`与`t`不同构，必存在一对字符，他们在两个字符串中最早出现的位置不同。

```java
/**
 * 一次遍历
 * Hello
 */
public boolean isIsomorphic(String s, String t)
{
	for (int i = 0; i < s.length(); i++)
	{
		if (s.indexOf(s.charAt(i)) != t.indexOf(t.charAt(i))) return false;
	}
	
	return true;
}
```

#### [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)

@2023.06.19

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean containsDuplicate(int[] nums)
{
	HashSet<Integer> hashmap = new HashSet<>();
	
	for (int n : nums)
	{
		if (hashmap.contains(n)) return true;
		hashmap.add(n);
	}
	
	return false;
}
```

2. 排序

```java
/**
 * 排序
 * 力扣官方题解
 */
public boolean containsDuplicate(int[] nums)
{
	Arrays.sort(nums);
	int n = nums.length;
	
	for (int i = 0; i < n - 1; i++)
	{
		if (nums[i] == nums[i + 1]) return true;
	}
	
	return false;
}
```

#### [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

@2023.06.19

1. 暴力

```java
/**
 * 暴力
 * Somnia1337
 */
public boolean containsNearbyDuplicate(int[] nums, int k)
{
	int len = nums.length;
	if (len == 1) return false;
	
	for (int i = 0; i < len; i++)
	{
		int j = i - k;
		if (j < 0) j = 0;
		for (; j < i; j++)
		{
			if (nums[j] == nums[i]) return true;
		}
	}
	
	return false;
}
```

逆天runtime。

![[Snipaste_ 230619_155740.png]]

2. 哈希表

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public boolean containsNearbyDuplicate(int[] nums, int k)
{
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	int length = nums.length;
	
	for (int i = 0; i < length; i++)
	{
		int num = nums[i];
		if (map.containsKey(num) && i - map.get(num) <= k) return true;
		map.put(num, i); //将键num对应的值更新为num出现的最右位置
	}
	
	return false;
}
```

3. 滑动窗口

```java
/**
 * 滑动窗口
 * 力扣官方题解
 */
public boolean containsNearbyDuplicate(int[] nums, int k)
{
	Set<Integer> set = new HashSet<Integer>();
	int length = nums.length;
	
	for (int i = 0; i < length; i++)
	{
		if (i > k)
		{
			set.remove(nums[i - k - 1]);
		}
		if (!set.add(nums[i])) return true;
	}
	
	return false;
}
```

#### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

@2024.03.03



```java
/**
 * 
 * songhouhou
 */
class MyStack {
    private Queue<Integer> q;
    
    public MyStack() {
        q = new LinkedList<>();
    }
    
    public void push(int x) {
        q.offer(x);
        int size = q.size();
        while (--size > 0) q.offer(q.poll());
    }
    
    public int pop() {
        return q.poll();
    }
    
    public int top() {
        return q.peek();
    }
    
    public boolean empty() {
        return q.isEmpty();
    }
}
```

#### [228. 汇总区间](https://leetcode.cn/problems/summary-ranges/)

@2023.06.19

```java
/**
 * 双指针
 * Somnia1337
 */
public List<String> summaryRanges(int[] nums)
{
	int len = nums.length;
	ArrayList<String> list = new ArrayList<>();
	
	for (int i = 0; i < len; i++)
	{
		int start = nums[i], end = nums[i];
		while (i < len - 1 && nums[i + 1] == nums[i] + 1)
		{
			end = nums[i + 1];
			i++;
		}
		if (start != end)
		{
			list.add(start + "->" + end);
		}
		else
		{
			list.add(String.valueOf(start));
		}
	}
	
	return list;
}
```

#### [231. 2 的幂](https://leetcode.cn/problems/power-of-two/)

@2023.06.19

1. 枚举

```java
/**
 * 枚举
 * Somnia1337
 */
public boolean isPowerOfTwo(int n)
{
	if (n <= 0) return false;
	
	HashMap<Integer, Boolean> hashmap = new HashMap<>();
	
	for (int i = 0; i <= 30; i++)
	{
		hashmap.put((int) Math.pow(2, i), true);
	}
	
	return hashmap.containsKey(n);
}
```

2. 数学

在int范围内，2的最大幂为$2^{30} = 1073741824$，用它对`n`取余，如果余0则`n`也为2的幂。

```java
/**
 * 数学
 * 力扣官方题解
 */
static final int BIG = 1 << 30;

public boolean isPowerOfTwo(int n)
{
	return n > 0 && BIG % n == 0;
}
```

3. 位运算

当且仅当`n`为2的幂时，满足`n & (n - 1) == 0`，因为此时`n`的二进制最高位为1、其余位为0，而`n - 1`的二进制最高位为0，其余位为1，二者与运算应为0。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public boolean isPowerOfTwo(int n)
{
	return n > 0 && (n & (n - 1)) == 0;
}
```

或者`n`与`-n`与运算，结果应为`n`（负数的二进制表示含若干前导1，例如$16 = (10000)_2$，$-16 = (11111111111111111111111111110000)_2$）。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public boolean isPowerOfTwo(int n)
{
	return n > 0 && (n & -n) == n;
}
```

4. 库函数

```java
/**
 * 库函数
 * Somnia1337
 */
public boolean isPowerOfTwo(int n)
{
	int zeros = Integer.numberOfTrailingZeros(n);
	return n >> zeros == 1;
}
```

```java
/**
 * 库函数
 * Somnia1337
 */
public boolean isPowerOfTwo(int n)
{
	return n > 0 && Integer.bitCount(n) == 1;
}
```

#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

@2024.03.04



```java
/**
 * 模拟
 * ylb
 */
class MyQueue {
    private Deque<Integer> stk1;
    private Deque<Integer> stk2;
    
    public MyQueue() {
        stk1 = new ArrayDeque<>();
        stk2 = new ArrayDeque<>();
    }
    
    public void push(int x) {
        stk1.push(x);
    }
    
    public int pop() {
        move();
        return stk2.pop();
    }
    
    public int peek() {
        move();
        return stk2.peek();
    }
    
    public boolean empty() {
        return stk1.isEmpty() && stk2.isEmpty();
    }
    
    private void move() {
        while (stk2.isEmpty()) {
            while (!stk1.isEmpty()) {
                stk2.push(stk1.pop());
            }
        }
    }
}
```

#### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

@2023.06.19

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isAnagram(String s, String t)
{
	int len1 = s.length(), len2 = t.length();
	
	if (len1 != len2) return false;
	
	HashMap<Character, Integer> hashmapS = new HashMap<>();
	HashMap<Character, Integer> hashmapT = new HashMap<>();
	
	for (int i = 0; i < len1; i++)
	{
		hashmapS.merge(s.charAt(i), 1, Integer::sum);
		hashmapT.merge(t.charAt(i), 1, Integer::sum);
	}
	
	return hashmapS.equals(hashmapT);
}
```

2. 排序

如果是异位词，排序后应该相同。

```java
/**
 * 排序
 * 力扣官方题解
 */
public boolean isAnagram(String s, String t)
{
	if (s.length() != t.length()) return false;
	
	char[] str1 = s.toCharArray();
	char[] str2 = t.toCharArray();
	
	Arrays.sort(str1);
	Arrays.sort(str2);
	
	return Arrays.equals(str1, str2);
}
```

#### [243. 最短单词距离](https://leetcode.cn/problems/shortest-word-distance/)

@2023.10.19

用 `pos1`、`pos2` 记录 `word1`、`word2` 的位置，遍历 `wordsDict`，更新两单词的位置，并更新最短距离。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int shortestDistance(String[] wordsDict, String word1, String word2) {
	int n = wordsDict.length, a = -1, b = n, ans = n;
	for (int i = 0; i < n; i++) {
		String word = wordsDict[i];
		if (word.equals(word1)) {
			a = i;
			if (b < n) ans = Math.min(Math.abs(b - a), ans);
		}
		if (word.equals(word2)) {
			b = i;
			if (a >= 0) ans = Math.min(Math.abs(b - a), ans);
		}
	}
	return ans;
}
```

#### [252. 会议室](https://leetcode.cn/problems/meeting-rooms/)

@2023.10.19

排序，检查会议时间是否有重叠。

```java
/**
 * 排序
 * Somnia1337
 */
public boolean canAttendMeetings(int[][] intervals)
{
	Arrays.sort(intervals, Comparator.comparing(a -> a[0]));
	for (int i = 0; i < intervals.length - 1; i++)
	{
		if (intervals[i][1] > intervals[i + 1][0]) return false;
	}
	return true;
}
```

#### [258. 各位相加](https://leetcode.cn/problems/add-digits/)

@2023.06.20

1. 找规律

```java
/**
 * 找规律
 * Somnia1337
 */
public int addDigits(int num)
{
	return num == 0 ? 0 : (num % 9 == 0 ? 9 : num % 9);
}
```

2. 数学

```java
/**
 * 数学
 * 力扣官方题解
 */
public int addDigits(int num)
{
	return (num - 1) % 9 + 1;
}
```

#### [263. 丑数](https://leetcode.cn/problems/ugly-number/)

@2023.06.20

```java
/**
 * 暴力
 * Somnia1337
 */
public boolean isUgly(int n)
{
	if (n == 0) return false;
	
	while (n % 30 == 0) n /= 30;
	while (n % 15 == 0) n /= 15;
	while (n % 10 == 0) n /= 10;
	while (n % 6 == 0) n /= 6;
	while (n % 5 == 0) n /= 5;
	while (n % 3 == 0) n /= 3;
	while (n % 2 == 0) n /= 2;
	
	return n == 1;
}
```

```java
/**
 * 暴力
 * 力扣官方题解
 */
public boolean isUgly(int n)
{
	if (n <= 0) return false;
	
	int[] factors = {2, 3, 5};
	
	for (int factor : factors)
	{
		while (n % factor == 0) n /= factor;
	}
	
	return n == 1;
}
```

#### [266. 回文排列](https://leetcode.cn/problems/palindrome-permutation/)

@2023.10.05

```java
/**
 * 字符统计
 * Somnia1337
 */
public boolean canPermutePalindrome(String s)
{
	int len = s.length();
	int[] count = new int[26];
	for (int i = 0; i < len; i++) count[s.charAt(i) - 'a']++;
	boolean hasOdd = false;
	for (int i = 0; i < 26; i++)
	{
		if (count[i] % 2 == 1)
		{
			if (hasOdd) return false;
			hasOdd = true;
		}
	}
	return true;
}
```

#### [268. 丢失的数字](https://leetcode.cn/problems/missing-number/)

@2023.06.20

1. 数学

将0-n求和，减去数组实际和，得到缺失元素。

```java
/**
 * 数学
 * Somnia1337
 */
public int missingNumber(int[] nums)
{
	int target = nums.length * (nums.length + 1) / 2;
	int actual = 0;
	for (int n : nums) actual += n;
	return target - actual;
}
```

2. 位运算

类似[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)方法2，扩充原数组，添加所有数字，那么原先丢失的数字现在只出现一次，其余数字均出现两次，对全体异或即可得到目标数字。

优化：无需真正扩充原数组，只需对变量循环异或所有数字即可。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int missingNumber(int[] nums)
{
	int xor = 0;
	int n = nums.length;
	
	for (int i = 0; i < n; i++)
	{
		xor ^= nums[i];
	}
	for (int i = 0; i <= n; i++)
	{
		xor ^= i;
	}
	
	return xor;
}
```

#### [278. 第一个错误的版本](https://leetcode.cn/problems/first-bad-version/)

@2024.02.22



```java
/**
 * 二分查找
 * Somnia1337
 */
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l <= r) {
            int m = l + (r - l >> 1);
            if (isBadVersion(m)) r = m - 1;
            else l = m + 1;
        }
        return l;
    }
}
```

#### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

@2023.06.20

1. 双指针

```java
/**
 * 双指针
 * 力扣官方题解
 */
public void moveZeroes(int[] nums)
{
	int n = nums.length, left = 0, right = 0;
	
	while (right < n)
	{
		if (nums[right] != 0)
		{
			swap(nums, left, right);
			left++;
		}
		right++;
	}
}

public void swap(int[] nums, int left, int right)
{
	int temp = nums[left];
	nums[left] = nums[right];
	nums[right] = temp;
}
```

2. 记录0的个数

遍历一次，维护一个变量`k`，记录已遇到0的次数，遇到非0时将其前移`k`位。

```java
/**
 * 记录0的个数
 * Banana
 */
public void moveZeroes(int[] nums)
{
	for (int i = 0, k = 0; i < nums.length; i++)
	{
		if (nums[i] == 0) k++;
		else
		{
			if (k > 0)
			{
				nums[i - k] = nums[i];
				nums[i] = 0;
			}
		}
	}
}
```

#### [290. 单词规律](https://leetcode.cn/problems/word-pattern/)

@2023.06.20

```java
/**
 * 哈希表
 * 力扣官方题解
 * @improver Somnia1337
 */
public boolean wordPattern(String pattern, String str)
{
	HashMap<String, Character> hashmap = new HashMap<>();
	int m = str.length();
	int i = 0;
	
	for (int p = 0; p < pattern.length(); ++p)
	{
		char ch = pattern.charAt(p);
		if (i >= m) return false;
		
		int j = i;
		while (j < m && str.charAt(j) != ' ') j++;
		
		String tmp = str.substring(i, j);
		if (hashmap.containsKey(tmp) && hashmap.get(tmp) != ch || hashmap.containsValue(ch) && !hashmap.containsKey(tmp)) return false;
		hashmap.put(tmp, ch);
		
		i = j + 1;
	}
	
	return i >= m;
}
```

也可以利用put的返回值来判断是否冲突：

- put的key不存在时，返回null。
- put的key存在时，将其映射至新value，并返回旧value。

```java
/**
 * 哈希表
 * windliang
 */
public boolean wordPattern(String pattern, String str)
{
	String[] words = str.split(" ");
	
	if (words.length != pattern.length()) return false;
	
	Map index = new HashMap();
	
	for (int i = 0; i < words.length; ++i)
	{
		if (index.put(pattern.charAt(i), i) != index.put(words[i], i)) return false;
	}
	
	return true;
}
```

#### [292. Nim 游戏](https://leetcode.cn/problems/nim-game/)

@2023.06.20

```java
/**
 * 找规律
 * Somnia1337
 */
public boolean canWinNim(int n) {
	return n % 4 != 0;
}
```

#### [293. 翻转游戏](https://leetcode.cn/problems/flip-game/)

@2023.10.19

枚举每个可能的位置。

```java
/**
 * 枚举
 * Somnia1337
 */
public List<String> generatePossibleNextMoves(String currentState)
{
	List<String> ans = new ArrayList<>();
	char[] chars = currentState.toCharArray();
	for (int i = 0; i < currentState.length() - 1; i++)
	{
		if (chars[i] == '+' && chars[i + 1] == '+')
		{
			chars[i] = chars[i + 1] = '-';
			ans.add(new String(chars));
			chars[i] = chars[i + 1] = '+';
		}
	}
	return ans;
}
```

#### [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)

@2023.10.23

前缀和。

```java
class NumArray
{
    private final int[] pSum;
	
    public NumArray(int[] nums)
    {
        pSum = new int[nums.length + 1];
        for (int i = 1; i < nums.length + 1; i++)
        {
            pSum[i] = pSum[i - 1] + nums[i - 1];
        }
    }
	
    public int sumRange(int left, int right)
    {
        return pSum[right + 1] - pSum[left];
    }
}
```

#### [326. 3 的幂](https://leetcode.cn/problems/power-of-three/)

@2023.06.20

```java
/**
 * 最大值取余
 * Somnia1337
 */
public boolean isPowerOfThree(int n)
{
	return n > 0 && 1162261467 % n == 0;
}
```

#### [338. 比特位计数](https://leetcode.cn/problems/counting-bits/)

@2023.07.08

1. 库函数

```java
/**
 * 库函数
 * Somnia1337
 */
public int[] countBits(int n)
{
	int[] ans = new int[n + 1];
	for (int i = 0; i < n + 1; i++)
	{
		ans[i] = Integer.bitCount(i);
	}
	return ans;
}
```

2. Brian Kernighan算法

计数原理同[191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)。

```java
/**
 * Brian Kernighan算法
 * 力扣官方题解
 */
public int[] countBits(int n)
{
	int[] bits = new int[n + 1];
	for (int i = 0; i <= n; i++)
	{
		bits[i] = countOnes(i);
	}
	return bits;
}

public int countOnes(int x)
{
	int ones = 0;
	while (x > 0)
	{
		x &= (x - 1);
		ones++;
	}
	return ones;
}
```

3. 动态规划

```java
/**
 * 动态规划（最高有效位）
 * 力扣官方题解
 */
public int[] countBits(int n)
{
	int[] bits = new int[n + 1];
	int highBit = 0;
	for (int i = 1; i <= n; i++)
	{
		if ((i & (i - 1)) == 0)
		{
			highBit = i;
		}
		bits[i] = bits[i - highBit] + 1;
	}
	return bits;
}
```

```java
/**
 * 动态规划（最低有效位）
 * 力扣官方题解
 */
public int[] countBits(int n)
{
	int[] bits = new int[n + 1];
	for (int i = 1; i <= n; i++)
	{
		bits[i] = bits[i >> 1] + (i & 1);
	}
	return bits;
}
```

```java
/**
 * 动态规划（最低设置位）
 * 力扣官方题解
 */
public int[] countBits(int n)
{
	int[] bits = new int[n + 1];
	for (int i = 1; i <= n; i++)
	{
		bits[i] = bits[i & (i - 1)] + 1;
	}
	return bits;
}
```

#### [342. 4的幂](https://leetcode.cn/problems/power-of-four/)

@2023.06.20

1. 暴力

```java
/**
 * 暴力
 * Somnia1337
 */
public boolean isPowerOfFour(int n)
{
	for (int i = 0; i <= 15; i++) if (Math.pow(4, i) == n) return true;
	return false;
}
```

2. 取模

如果`n`为4的幂，`n % 3` = 1；如果`n`为2的幂而不为4的幂，`n`可表示为$4^x \times 2$，则`n % 3` = 2。

```java
/**
 * 取模
 * 力扣官方题解
 */
public boolean isPowerOfFour(int n)
{
	return n > 0 && (n & (n - 1)) == 0 && n % 3 == 1;
}
```

3. 位运算

如果`n`为4的幂，其二进制表示中仅有一个1，且该1后面有偶数个0。

构造一个1/0交错的值$(10101010101010101010101010101010)_2$（十六进制为$(AAAAAAAA)_{16}$），将其与`n`按位与，如果为0则说明为4的幂。

例如，对于$4^2 = 16 = (10000)_2$，按位与的结果为0。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public boolean isPowerOfFour(int n)
{
	return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
}
```

#### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

@2023.06.23

```java
/**
 * 双指针
 * Somnia1337
 */
public void reverseString(char[] s)
{
	int len = s.length;
	for (int i = 0; i < len / 2; i++)
		swap(s, i, len - i - 1);
}

public void swap(char[] arr, int i, int j)
{
	char temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```

#### [345. 反转字符串中的元音字母](https://leetcode.cn/problems/reverse-vowels-of-a-string/)

@2023.06.23

```java
/**
 * 双指针
 * Somnia1337
 */
public String reverseVowels(String s)
{
	int len = s.length();
	if (len == 1) return s;
	int left = 0, right = len - 1;
	char[] ch = s.toCharArray();
	while (left < right)
	{
		while (left < right && !isVowel(ch[left])) left++;
		while (right > left && !isVowel(ch[right])) right--;
		swap(ch, left, right);
		left++;
		right--;
	}
	return new String(ch);
}

public boolean isVowel(char c)
{
	return "AEIOUaeiou".indexOf(c) >= 0;
}

public void swap(char[] arr, int i, int j)
{
	char temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```

#### [346. 数据流中的移动平均值](https://leetcode.cn/problems/moving-average-from-data-stream/)

@2024.02.22



```java
/**
 * 链表
 * Somnia1337
 */
class MovingAverage {
    private List<Integer> arr;
    private int size, sum;
    
    public MovingAverage(int size) {
        arr = new LinkedList<>();
        this.size = size;
    }
    
    public double next(int val) {
        arr.add(val);
        sum += val;
        if (arr.size() > size) sum -= arr.remove(0);
        return (double) sum / arr.size();
    }
}
```

#### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

@2023.06.24

1. 哈希表

```java
/**
 * 哈希表
 * Rorke76753
 * @improver Somnia1337
 */
public int[] intersection(int[] nums1, int[] nums2)
{
	HashSet<Integer> set1 = new HashSet<>();
	HashSet<Integer> set2 = new HashSet<>();
	for (int i : nums1)
		set1.add(i);
	for (int i : nums2)
		set2.add(i);
	set2.retainAll(set1); //使set2仅保留set1包含的元素
	return set2.stream()
			   .mapToInt(i -> i)
			   .toArray();
}
```

2. 排序 + 双指针

排序后，加入答案数组的值递增，创建变量`pre`记录上一次加入答案数组的值。

将两个指针分别指向两个数组头，比较值——

- 不同：右移指向的值较小的指针。
- 相同且不等于`pre`：加入答案数组，右移双指针。

```java
/**
 * 排序 + 双指针
 * 力扣官方题解
 */
public int[] intersection(int[] nums1, int[] nums2)
{
	Arrays.sort(nums1);
	Arrays.sort(nums2);
	int length1 = nums1.length, length2 = nums2.length;
	int[] intersection = new int[length1 + length2];
	int index = 0, index1 = 0, index2 = 0;
	while (index1 < length1 && index2 < length2)
	{
		int num1 = nums1[index1], num2 = nums2[index2];
		if (num1 == num2)
		{
			//保证加入元素的唯一性
			if (index == 0 || num1 != intersection[index - 1])
			{
				intersection[index++] = num1;
			}
			index1++;
			index2++;
		}
		else if (num1 < num2)
		{
			index1++;
		}
		else
		{
			index2++;
		}
	}
	return Arrays.copyOfRange(intersection, 0, index);
}
```

#### [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

@2023.06.24

1. 排序 + 双指针

```java
/**
 * 排序 + 双指针
 * Somnia1337
 */
public int[] intersect(int[] nums1, int[] nums2)
{
	Arrays.sort(nums1);
	Arrays.sort(nums2);
	int len1 = nums1.length, len2 = nums2.length;
	int[] ret = new int[Math.min(len1, len2)];
	int index = 0, index1 = 0, index2 = 0;
	while (index1 < len1 && index2 < len2)
	{
		int num1 = nums1[index1], num2 = nums2[index2];
		if (num1 == num2)
		{
			ret[index++] = num1;
			index1++;
			index2++;
		}
		else if (num1 < num2)
		{
			index1++;
		}
		else
		{
			index2++;
		}
	}
	return Arrays.copyOfRange(ret, 0, index);
}
```

2. 哈希表

遍历`nums1`，在哈希表中记录每个值及出现次数，然后遍历`nums2`，检查哈希表中是否存在该值且次数大于0，若是则将该数字添加到答案，并减少哈希表记录的次数。

优化：先遍历较短的数组（减少额外空间占用），然后遍历较长的数组得到交集。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int[] intersect(int[] nums1, int[] nums2)
{
	if (nums1.length > nums2.length)
	{
		return intersect(nums2, nums1);
	}
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	for (int num : nums1)
	{
		int count = map.getOrDefault(num, 0) + 1;
		map.put(num, count);
	}
	int[] intersection = new int[nums1.length];
	int index = 0;
	for (int num : nums2)
	{
		int count = map.getOrDefault(num, 0);
		if (count > 0)
		{
			intersection[index++] = num;
			count--;
			if (count > 0)
			{
				map.put(num, count);
			}
			else
			{
				map.remove(num);
			}
		}
	}
	return Arrays.copyOfRange(intersection, 0, index);
}
```

#### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

@2023.06.24

1. 二分查找

移动左右区间时，新区间不包含`mid`，所以当`left` = `right`时需单独检查`mid = (left + right) / 2`。

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public boolean isPerfectSquare(int num)
{
	int left = 0, right = num;
	while (left <= right)
	{
		int mid = (right - left) / 2 + left;
		long square = (long) mid * mid;
		if (square < num)
		{
			left = mid + 1;
		}
		else if (square > num)
		{
			right = mid - 1;
		}
		else
		{
			return true;
		}
	}
	return false;
}
```

2. 牛顿迭代

泰勒。

#### [374. 猜数字大小](https://leetcode.cn/problems/guess-number-higher-or-lower/)

@2023.09.06

需防溢出。

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int l = 1, r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (guess(m) <= 0) r = m;
            else l = m + 1;
        }
        return l;
    }
}
```

#### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

@2023.06.24

判断 `magazine` 中每个字母出现的频数均大于在 `ransomNote` 中出现的频数。

```java
/**
 * 字符统计
 * Somnia1337
 */
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

#### [387. 字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/)

@2023.06.24

1. 字符统计

遍历`s`两次，第一次在数组中记录每个字母的频率，第二次从数组中找到首个频率为1的字母。

```java
/**
 * 字符统计
 * Somnia1337
 */
public int firstUniqChar(String s)
{
	int len = s.length();
	char[] chars = new char[26];
	for (int i = 0; i < len; i++)
		chars[s.charAt(i) - 'a']++;
	for (int i = 0; i < len; i++)
		if (chars[s.charAt(i) - 'a'] == 1) return i;
	return -1;
}
```

2. 字符串操作

```java
/**
 * 字符串操作
 * Somnia1337
 */
public int firstUniqChar(String s)
{
	int len = s.length();
	StringBuilder sb = new StringBuilder(s);
	String sReversed = sb.reverse()
						 .toString();
	HashSet<Character> hashset = new HashSet<>();
	for (int i = 0; i < len; i++)
	{
		char c = s.charAt(i);
		if (hashset.add(c) && sReversed.indexOf(c) + i == len - 1) return i;
	}
	return -1;
}
```

3. 索引统计

遍历`s`一次，将数据存入哈希表，键为字符，值为它首次出现的索引（如果该字符只出现一次）或者−1（如果该字符出现多次）。

再遍历哈希表一次，找出其中不为−1的最小值，即为第一个不重复字符的索引。如果不存在，则返回-1。

```java
/**
 * 索引统计
 * 力扣官方题解
 */
public int firstUniqChar(String s)
{
	Map<Character, Integer> position = new HashMap<Character, Integer>();
	int n = s.length();
	for (int i = 0; i < n; ++i)
	{
		char ch = s.charAt(i);
		if (position.containsKey(ch))
		{
			position.put(ch, -1);
		}
		else
		{
			position.put(ch, i);
		}
	}
	int first = n;
	for (Map.Entry<Character, Integer> entry : position.entrySet())
	{
		int pos = entry.getValue();
		if (pos != -1 && pos < first)
		{
			first = pos;
		}
	}
	if (first == n)
	{
		first = -1;
	}
	return first;
}
```

#### [389. 找不同](https://leetcode.cn/problems/find-the-difference/)

@2023.06.24

1. 字符统计

```java
/**
 * 字符统计
 * Somnia1337
 */
public char findTheDifference(String s, String t)
{
	int lenS = s.length(), lenT = t.length();
	char[] chars = new char[26];
	for (int i = 0; i < lenS; i++)
		chars[s.charAt(i) - 'a']++;
	for (int i = 0; i < lenT; i++)
		chars[t.charAt(i) - 'a']--;
	for (int i = 0; i < 26; i++)
		if (chars[i] > 0) return (char) (i + 'a');
	return 'a' - 1;
}
```

2. 求和

```java
/**
 * 求和
 * 力扣官方题解
 */
public char findTheDifference(String s, String t)
{
	int as = 0, at = 0;
	for (int i = 0; i < s.length(); ++i)
	{
		as += s.charAt(i);
	}
	for (int i = 0; i < t.length(); ++i)
	{
		at += t.charAt(i);
	}
	return (char) (at - as);
}
```

3. 位运算

```java
/**
 * 位运算
 * 力扣官方题解
 * @improver Somnia1337
 */
public char findTheDifference(String s, String t)
{
	int ret = 0;
	for (char ch : (s + t).toCharArray())
		ret ^= ch;
	return (char) ret;
}
```

#### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

@2023.06.24

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean isSubsequence(String s, String t)
{
	int lenS = s.length(), lenT = t.length();
	if (lenS > lenT) return false;
	int pS = 0, pT = 0;
	while (pS < lenS)
	{
		char ch = s.charAt(pS);
		while (pT < lenT && t.charAt(pT) != ch) pT++;
		pS++;
		pT++;
		if (lenT - pT < lenS - pS) return false;
	}
	return true;
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean isSubsequence(String s, String t)
{
	int n = s.length(), m = t.length();
	int i = 0, j = 0;
	while (i < n && j < m)
	{
		if (s.charAt(i) == t.charAt(j))
		{
			i++;
		}
		j++;
	}
	return i == n;
}
```

#### [401. 二进制手表](https://leetcode.cn/problems/binary-watch/)

@2023.07.08

1. 枚举

```java
/**
 * 枚举
 * 力扣官方题解
 */
public List<String> readBinaryWatch(int turnedOn)
{
	List<String> ans = new ArrayList<String>();
	for (int h = 0; h < 12; ++h)
	{
		for (int m = 0; m < 60; ++m)
		{
			if (Integer.bitCount(h) + Integer.bitCount(m) == turnedOn)
			{
				ans.add(h + ":" + (m < 10 ? "0" : "") + m);
			}
		}
	}
	return ans;
}
```

2. 回溯

```java
/**
 * 回溯
 * 郁郁雨
 */
int[] hours = new int[]{1, 2, 4, 8, 0, 0, 0, 0, 0, 0};
int[] minutes = new int[]{0, 0, 0, 0, 1, 2, 4, 8, 16, 32};
List<String> res = new ArrayList<>();

public List<String> readBinaryWatch(int num)
{
	backtrack(num, 0, 0, 0);
	return res;
}

public void backtrack(int num, int index, int hour, int minute)
{
	if (hour > 11 || minute > 59)
		return;
	if (num == 0)
	{
		StringBuilder sb = new StringBuilder();
		sb.append(hour)
		  .append(':');
		if (minute < 10)
		{
			sb.append('0');
		}
		sb.append(minute);
		res.add(sb.toString());
		return;
	}
	for (int i = index; i < 10; i++)
	{
		backtrack(num - 1, i + 1, hour + hours[i], minute + minutes[i]);
	}
}
```

#### [409. 最长回文串](https://leetcode.cn/problems/longest-palindrome/)

@2023.06.25

```java
/**
 * 字符统计 + 贪心
 * Somnia1337
 */
public int longestPalindrome(String s)
{
	int[] arr = new int[58];
	for (char ch : s.toCharArray())
		arr[ch - 'A']++;
	int ret = 0, v = 0;
	for (int f : arr)
	{
		if (f % 2 == 0) ret += f;
		else if (v == 0)
		{
			ret += f;
			v = 1;
		}
		else ret += f - 1;
	}
	return ret;
}
```

#### [412. Fizz Buzz](https://leetcode.cn/problems/fizz-buzz/)

@2023.06.25

```java
/**
 * 暴力
 * Somnia1337
 */
public List<String> fizzBuzz(int n)
{
	ArrayList<String> answer = new ArrayList<>();
	for (int i = 1; i <= n; i++)
	{
		if (i % 15 == 0) answer.add("FizzBuzz");
		else if (i % 5 == 0) answer.add("Buzz");
		else if (i % 3 == 0) answer.add("Fizz");
		else answer.add(String.valueOf(i));
	}
	return answer;
}
```

#### [414. 第三大的数](https://leetcode.cn/problems/third-maximum-number/)

@2023.06.25

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public int thirdMax(int[] nums)
{
	int len = nums.length;
	Arrays.sort(nums);
	int cnt = 1;
	for (int i = nums.length - 2; i >= 0; i--)
	{
		if (nums[i] < nums[i + 1]) cnt++;
		if (cnt == 3) return nums[i];
	}
	return nums[len - 1];
}
```

2. 集合

遍历数组，每遍历一个数就将其插入有序集合，如果有序集合的大小超过3，就删除集合中的最小元素。

遍历结束后，如果有序集合的大小为3，返回其中的最小值；如果有序集合的大小不足3，返回有序集合中的最大值。

```java
/**
 * 集合
 * 力扣官方题解
 */
public int thirdMax(int[] nums)
{
	TreeSet<Integer> s = new TreeSet<Integer>();
	for (int num : nums)
	{
		s.add(num);
		if (s.size() > 3)
		{
			s.remove(s.first());
		}
	}
	return s.size() == 3 ? s.first() : s.last();
}
```

3. 贪心

用3个变量表示前三大的值，均初始化为`Long.MIN_VALUE`，遍历一次`nums`，更新三者的值，如果完毕后仍有`Long.MIN_VALUE`，返回三者最大，否则返回三者最小。

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int thirdMax(int[] nums)
{
	long a = Long.MIN_VALUE, b = Long.MIN_VALUE, c = Long.MIN_VALUE;
	for (long num : nums)
	{
		if (num > a)
		{
			c = b;
			b = a;
			a = num;
		}
		else if (a > num && num > b)
		{
			c = b;
			b = num;
		}
		else if (b > num && num > c)
		{
			c = num;
		}
	}
	return c == Long.MIN_VALUE ? (int) a : (int) c;
}
```

除了初始化为`Long.MIN_VALUE`，还可以初始化为null。

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int thirdMax(int[] nums)
{
	Integer a = null, b = null, c = null;
	for (int num : nums)
	{
		if (a == null || num > a)
		{
			c = b;
			b = a;
			a = num;
		}
		else if (a > num && (b == null || num > b))
		{
			c = b;
			b = num;
		}
		else if (b != null && b > num && (c == null || num > c))
		{
			c = num;
		}
	}
	return c == null ? a : c;
}
```

#### [415. 字符串相加](https://leetcode.cn/problems/add-strings/)

@2023.06.25

```java
/**
 * 模拟
 * Somnia1337
 */
public String addStrings(String num1, String num2)
{
	int len1 = num1.length(), len2 = num2.length(), len = Math.max(len1, len2);
	if (len1 < len) num1 = "0".repeat(len2 - len1) + num1;
	else if (len2 < len) num2 = "0".repeat(len1 - len2) + num2;
	StringBuilder sb = new StringBuilder();
	char carry = '0';
	for (int i = len - 1; i >= 0; i--)
	{
		char ch1 = num1.charAt(i), ch2 = num2.charAt(i), ch = (char) (ch1 - '0' + ch2 - '0' + carry);
		carry = '0';
		while (ch > '9')
		{
			ch -= 10;
			carry += 1;
		}
		sb.append(ch);
	}
	sb.append(carry);
	String ret = sb.reverse()
				   .toString();
	if (ret.charAt(0) == '0') ret = ret.replaceFirst("^0+", "");
	return ret.equals("") ? "0" : ret;
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public String addStrings(String num1, String num2)
{
	int i = num1.length() - 1, j = num2.length() - 1, add = 0;
	StringBuffer ans = new StringBuffer();
	while (i >= 0 || j >= 0 || add != 0)
	{
		int x = i >= 0 ? num1.charAt(i) - '0' : 0;
		int y = j >= 0 ? num2.charAt(j) - '0' : 0;
		int result = x + y + add;
		ans.append(result % 10);
		add = result / 10;
		i--;
		j--;
	}
	//计算完以后的答案需要翻转过来
	ans.reverse();
	return ans.toString();
}
```

#### [434. 字符串中的单词数](https://leetcode.cn/problems/number-of-segments-in-a-string/)

@2023.06.25

1. 一次遍历

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int countSegments(String s)
{
	int ret = 0, v1 = 0, v2 = 0;
	for (int i = 0; i < s.length(); i++)
	{
		char ch = s.charAt(i);
		if (ch != ' ')
		{
			v1 = 1;
			v2 = 1;
		}
		else v2--;
		if (v1 == 1 && (v2 == 0 || i == s.length() - 1 && v2 == 1))
		{
			ret++;
			v2--;
		}
	}
	return ret;
}
```

要统计单词数，只需统计单词的开头个数，单词的开头满足：

- 该字符不为空格，
- 该字符为首个字符 / 该字符的上一个字符为空格。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public int countSegments(String s)
{
	int segmentCount = 0;
	for (int i = 0; i < s.length(); i++)
	{
		if ((i == 0 || s.charAt(i - 1) == ' ') && s.charAt(i) != ' ')
		{
			segmentCount++;
		}
	}
	return segmentCount;
}
```

2. 拆分字符串

```java
/**
 * 拆分字符串
 * Jayden Shi
 */
public int countSegments(String s)
{
	s = s.trim();
	if (s.length() == 0)
	{
		return 0;
	}
	String[] a = s.split("\\s+");
	return a.length;
}
```

#### [441. 排列硬币](https://leetcode.cn/problems/arranging-coins/)

@2023.06.25

1. 暴力

```java
/**
 * 暴力
 * Somnia1337
 */
public int arrangeCoins(int n)
{
	int ret = 0;
	for (int i = 1; ; i++)
	{
		n -= i;
		if (n < 0) return ret;
		ret++;
	}
}
```

```java
/**
 * 暴力
 * yangsanity
 * @improver Somnia1337
 */
public int arrangeCoins(int n)
{
	int ret = 0;
	while (n - ret > 0) n -= ++ret;
	return ret;
}
```

2. 二分查找

结合等差数列求和公式，可用二分查找计算`n`可构成的完整行数。

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public int arrangeCoins(int n)
{
	int left = 1, right = n;
	while (left < right)
	{
		int mid = (right - left + 1) / 2 + left;
		if ((long) mid * (mid + 1) <= (long) 2 * n)
		{
			left = mid;
		}
		else
		{
			right = mid - 1;
		}
	}
	return left;
}
```

3. 数学

结合等差数列求和公式，列出方程
$$\frac{x (x + 1)}{2} = n$$

解得
$$x_1 = \frac{-1 - \sqrt{8 n + 1}}{2},\quad x_2 = \frac{-1 + \sqrt{8 n + 1}}{2}$$

$x_1 < 0$，舍去，$[x_2]$即为所求。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int arrangeCoins(int n)
{
	return (int) ((Math.sqrt((long) 8 * n + 1) - 1) / 2);
}
```

#### [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

@2023.06.25

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> findDisappearedNumbers(int[] nums)
{
	HashSet<Integer> hashset = new HashSet<>();
	ArrayList<Integer> ret = new ArrayList<>();
	for (int n : nums)
	{
		hashset.add(n);
	}
	for (int i = 0; i < nums.length; i++)
	{
		if (!hashset.contains(i + 1))
		{
			ret.add(i + 1);
		}
	}
	return ret;
}
```

2. 标记

遍历`nums`，每遇到一个数`x`，调用`nums[x - 1] += n`。由于`nums`中的值范围为$[1,\space n]$，增加后的值应该都大于`n`，再遍历一次`nums`，若`nums[i] <= n`，说明缺失了`i + 1`。

```java
/**
 * 标记
 * 力扣官方题解
 */
public List<Integer> findDisappearedNumbers(int[] nums)
{
	int n = nums.length;
	for (int num : nums)
	{
		int x = (num - 1) % n;
		nums[x] += n;
	}
	List<Integer> ret = new ArrayList<Integer>();
	for (int i = 0; i < n; i++)
	{
		if (nums[i] <= n)
		{
			ret.add(i + 1);
		}
	}
	return ret;
}
```

#### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

@2023.06.25

```java
/**
 * 排序 + 双指针 + 贪心
 * Somnia1337
 */
public int findContentChildren(int[] g, int[] s)
{
	Arrays.sort(g);
	Arrays.sort(s);
	int pG = 0, pS = 0, ans = 0;
	while (pG < g.length && pS < s.length)
	{
		while (pS < s.length - 1 && s[pS] < g[pG]) pS++;
		if (s[pS] <= g[pG]) ans++;
		pS++;
		pG++;
	}
	return ans;
}
```

#### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

@2023.06.25

1. 暴力

```java
/**
 * 暴力
 * Somnia1337
 */
public boolean repeatedSubstringPattern(String s) {
	int len = s.length();
	for (int i = 1; i <= len / 2; i++) {
		if (len % i != 0) continue;
		int times = len / i;
		if (s.equals(s.substring(0, i).repeat(times))) {
			return true;
		}
	}
	return false;
}
```

2. 拼接 + 子串

将两个 `s` 拼接，去头尾各 1 个字符，如果仍包含 `s` 则说明 `s` 可由子串重复若干次构成。

```java
/**
 * 拼接 + 子串
 * Goodlucky
 */
public boolean repeatedSubstringPattern(String s) {
	return (s + s).substring(1, s.length() * 2 - 1).contains(s);
}
```

#### [461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)

@2023.06.25

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public int hammingDistance(int x, int y)
{
	String s1 = Integer.toBinaryString(x);
	String s2 = Integer.toBinaryString(y);
	int l1 = s1.length(), l2 = s2.length(), len = Math.max(l1, l2), ans = 0;
	
	for (int i = 0; i < len; i++)
	{
		char c1 = i < l1 ? s1.charAt(l1 - i - 1) : '0';
		char c2 = i < l2 ? s2.charAt(l2 - i - 1) : '0';
		if (c1 != c2) ans++;
	}
	
	return ans;
}
```

2. 库函数

```java
/**
 * 库函数
 * 力扣官方题解
 */
public int hammingDistance(int x, int y)
{
	return Integer.bitCount(x ^ y);
}
```

3. 位计数

```java
/**
 * 位计数
 * 力扣官方题解
 */
public int hammingDistance(int x, int y)
{
	int s = x ^ y, ret = 0;
	while (s != 0)
	{
		ret += s & 1;
		s >>= 1;
	}
	return ret;
}
```

4. Brian Kernighan算法

待学习

#### [463. 岛屿的周长](https://leetcode.cn/problems/island-perimeter/)

@2023.07.08

1. 统计

```java
/**
 * 统计
 * Somnia1337
 */
public int islandPerimeter(int[][] grid)
{
	int rows = grid.length, columns = grid[0].length;
	int landCnt = 0, connectCnt = 0;
	for (int[] row : grid)
	{
		for (int i = 0; i < columns; i++)
		{
			if (row[i] == 1)
			{
				landCnt++;
				if (i < columns - 1) connectCnt += row[i + 1] == 1 ? 1 : 0;
			}
		}
	}
	for (int i = 0; i < columns; i++)
	{
		for (int j = 0; j < rows - 1; j++)
		{
			if (grid[j][i] == 1 && grid[j + 1][i] == 1)
			{
				connectCnt++;
			}
		}
	}
	return landCnt * 4 - connectCnt * 2;
}
```

2. DFS

```java
/**
 * DFS
 * 力扣官方题解
 */
static int[] dx = {0, 1, 0, -1};
static int[] dy = {1, 0, -1, 0};

public int islandPerimeter(int[][] grid)
{
	int n = grid.length, m = grid[0].length;
	int ans = 0;
	for (int i = 0; i < n; ++i)
	{
		for (int j = 0; j < m; ++j)
		{
			if (grid[i][j] == 1)
			{
				ans += dfs(i, j, grid, n, m);
			}
		}
	}
	return ans;
}

public int dfs(int x, int y, int[][] grid, int n, int m)
{
	if (x < 0 || x >= n || y < 0 || y >= m || grid[x][y] == 0)
	{
		return 1;
	}
	if (grid[x][y] == 2)
	{
		return 0;
	}
	grid[x][y] = 2;
	int res = 0;
	for (int i = 0; i < 4; ++i)
	{
		int tx = x + dx[i];
		int ty = y + dy[i];
		res += dfs(tx, ty, grid, n, m);
	}
	return res;
}
```

#### [476. 数字的补数](https://leetcode.cn/problems/number-complement/)

@2023.06.26

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public int findComplement(int num)
{
	String numBinary = Integer.toBinaryString(num);
	while (numBinary.startsWith("0")) numBinary = numBinary.substring(1);
	StringBuilder sb = new StringBuilder();
	for (int i = 0; i < numBinary.length(); i++)
		sb.append('1' - numBinary.charAt(i));
	String comBinary = sb.toString();
	return Integer.parseInt(comBinary, 2);
}
```

2. 位运算

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int findComplement(int num)
{
	int highbit = 0;
	for (int i = 1; i <= 30; ++i)
	{
		if (num >= 1 << i)
		{
			highbit = i;
		}
		else
		{
			break;
		}
	}
	int mask = highbit == 30 ? 0x7fffffff : (1 << (highbit + 1)) - 1;
	return num ^ mask;
}
```

3. 2的幂特性

2的幂满足形式$100\cdots0$，减1后满足形式$011\cdots1$。

一个数与其补数相加满足形式$11\cdots1$，刚好为2的幂减1。

寻找严格大于`num`的2的最小幂，将其减1，再减去`num`即为补数。

```java
/**
 * 2的幂特性
 * 可爱抱抱呀😥
 */
public int findComplement(int num)
{
	long ans = 1;
	while (ans <= num) ans *= 2;
	return (int) ans - 1 - num;
}
```

#### [482. 密钥格式化](https://leetcode.cn/problems/license-key-formatting/)

@2023.06.26

```java
/**
 * 数学
 * Somnia1337
 */
public String licenseKeyFormatting(String s, int k)
{
	String[] clear = s.split("-");
	int len = 0;
	for (String st : clear) len += st.length();
	int rem = len % k == 0 ? k : len % k;
	int part = (len - rem) / k + 1;
	StringBuilder sb = new StringBuilder();
	for (int i = 0, j = 0, it = 0; i < part; i++)
	{
		for (; it < rem + i * k && j < s.length(); j++)
		{
			char ch = s.charAt(j);
			if (ch != '-')
			{
				sb.append(ch);
				it++;
			}
		}
		if (i < part - 1) sb.append('-');
	}
	return sb.toString()
			 .toUpperCase();
}
```

```java
/**
 * 数学
 * 力扣官方题解
 */
public String licenseKeyFormatting(String s, int k)
{
	StringBuilder ans = new StringBuilder();
	int cnt = 0;
	for (int i = s.length() - 1; i >= 0; i--)
	{
		if (s.charAt(i) != '-')
		{
			cnt++;
			ans.append(Character.toUpperCase(s.charAt(i)));
			if (cnt % k == 0)
			{
				ans.append("-");
			}
		}
	}
	if (ans.length() > 0 && ans.charAt(ans.length() - 1) == '-')
	{
		ans.deleteCharAt(ans.length() - 1);
	}
	return ans.reverse()
			  .toString();
}
```

#### [485. 最大连续 1 的个数](https://leetcode.cn/problems/max-consecutive-ones/)

@2023.06.26

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findMaxConsecutiveOnes(int[] nums)
{
	int max = 0, cnt = 0;
	for (int i = 0; i < nums.length; i++)
	{
		if (i < nums.length - 1 && nums[i] == 1) cnt++;
		else if (i == nums.length - 1 && nums[i] == 1)
		{
			cnt++;
			max = Math.max(cnt, max);
		}
		else
		{
			max = Math.max(cnt, max);
			cnt = 0;
		}
	}
	return max;
}
```

可以简化为if-else，最终再判断一次`cnt`与`max`即可。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public int findMaxConsecutiveOnes(int[] nums)
{
	int maxCount = 0, count = 0;
	int n = nums.length;
	for (int i = 0; i < n; i++)
	{
		if (nums[i] == 1)
		{
			count++;
		}
		else
		{
			maxCount = Math.max(maxCount, count);
			count = 0;
		}
	}
	maxCount = Math.max(maxCount, count);
	return maxCount;
}
```

#### [492. 构造矩形](https://leetcode.cn/problems/construct-the-rectangle/)

@2023.06.26

```java
/**
 * 暴力
 * Somnia1337
 */
public int[] constructRectangle(int area)
{
	int mid = (int) Math.sqrt(area);
	if (mid * mid == area) return new int[]{mid, mid};
	mid++;
	while (mid < area)
	{
		if (area % mid == 0) return new int[]{mid, area / mid};
		mid++;
	}
	return new int[]{area, 1};
}
```

优化：从`mid`开始向下寻找（最多$\sqrt{area}$次），而不是向上（最多$area - \sqrt{area}$次）。

```java
/**
 * 暴力
 * 力扣官方题解
 */
public int[] constructRectangle(int area)
{
	int w = (int) Math.sqrt(area);
	while (area % w != 0)
	{
		--w;
	}
	return new int[]{area / w, w};
}
```

#### [495. 提莫攻击](https://leetcode.cn/problems/teemo-attacking/)

@2023.07.08

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findPoisonedDuration(int[] timeSeries, int duration)
{
	int ans = 0;
	for (int i = 0; i < timeSeries.length - 1; i++)
	{
		ans += Math.min(timeSeries[i + 1] - timeSeries[i], duration);
	}
	return ans + duration;
}
```

#### [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

@2023.06.27

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int[] nextGreaterElement(int[] nums1, int[] nums2)
{
	int len1 = nums1.length, len2 = nums2.length;
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	for (int i = 0; i < len2; i++) hashmap.put(nums2[i], i);
	int[] ret = new int[len1];
	for (int i = 0; i < len1; i++)
	{
		int index = hashmap.get(nums1[i]);
		for (int j = index; j < len2; j++)
			if (nums2[j] > nums1[i])
			{
				ret[i] = nums2[j];
				break;
			}
		if (ret[i] == 0) ret[i] = -1;
	}
	return ret;
}
```

2. 单调栈 + 哈希表

```java
/**
 * 单调栈 + 哈希表
 * 力扣官方题解
 */
public int[] nextGreaterElement(int[] nums1, int[] nums2)
{
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	Deque<Integer> stack = new ArrayDeque<Integer>();
	for (int i = nums2.length - 1; i >= 0; --i)
	{
		int num = nums2[i];
		while (!stack.isEmpty() && num >= stack.peek())
		{
			stack.pop();
		}
		map.put(num, stack.isEmpty() ? -1 : stack.peek());
		stack.push(num);
	}
	int[] res = new int[nums1.length];
	for (int i = 0; i < nums1.length; ++i)
	{
		res[i] = map.get(nums1[i]);
	}
	return res;
}
```

#### [500. 键盘行](https://leetcode.cn/problems/keyboard-row/)

@2023.06.27

```java
/**
 * 遍历
 * Somnia1337
 */
public String[] findWords(String[] words)
{
	String s1 = "qwertyuiopQWERTYUIOP";
	String s2 = "asdfghjklASDFGHJKL";
	String s3 = "zxcvbnmZXCVBNM";
	ArrayList<String> arrayList = new ArrayList<>();
	for (String word : words)
	{
		int len = word.length(), i = 0;
		for (i = 0; i < len && s1.indexOf(word.charAt(i)) >= 0; ) i++;
		if (i == len)
		{
			arrayList.add(word);
			continue;
		}
		for (i = 0; i < len && s2.indexOf(word.charAt(i)) >= 0; ) i++;
		if (i == len)
		{
			arrayList.add(word);
			continue;
		}
		for (i = 0; i < len && s3.indexOf(word.charAt(i)) >= 0; ) i++;
		if (i == len)
		{
			arrayList.add(word);
			continue;
		}
	}
	String[] ret = new String[arrayList.size()];
	for (int i = 0; i < arrayList.size(); i++)
		ret[i] = arrayList.get(i);
	return ret;
}
```

#### [504. 七进制数](https://leetcode.cn/problems/base-7/)

@2023.06.27

```java
/**
 * 模拟
 * Somnia1337
 */
public String convertToBase7(int num)
{
	if (num == 0) return "0";
	StringBuilder sb = new StringBuilder();
	int op = num;
	if (num < 0) op = -num;
	while (op > 0)
	{
		sb.append(String.valueOf(op % 7));
		op /= 7;
	}
	if (num < 0) sb.append('-');
	return sb.reverse()
			 .toString();
}
```

#### [506. 相对名次](https://leetcode.cn/problems/relative-ranks/)

@2023.06.27

```java
/**
 * 排序
 * Somnia1337
 */
public String[] findRelativeRanks(int[] score)
{
	int len = score.length;
	int[] sorted = Arrays.copyOf(score, len);
	Arrays.sort(sorted);
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	for (int i = 0; i < len; i++)
		hashmap.put(sorted[i], i);
	String[] ret = new String[len];
	for (int i = 0; i < len; i++)
	{
		int rate = len - hashmap.get(score[i]);
		if (rate == 1) ret[i] = "Gold Medal";
		else if (rate == 2) ret[i] = "Silver Medal";
		else if (rate == 3) ret[i] = "Bronze Medal";
		else ret[i] = String.valueOf(rate);
	}
	return ret;
}
```

可以用二维数组避免使用哈希表。

```java
/**
 * 排序
 * 力扣官方题解
 */
public String[] findRelativeRanks(int[] score)
{
	int n = score.length;
	String[] desc = {"Gold Medal", "Silver Medal", "Bronze Medal"};
	int[][] arr = new int[n][2];
	for (int i = 0; i < n; ++i)
	{
		arr[i][0] = score[i];
		arr[i][1] = i;
	}
	Arrays.sort(arr, (a, b) -> b[0] - a[0]);
	String[] ans = new String[n];
	for (int i = 0; i < n; ++i)
	{
		if (i >= 3)
		{
			ans[arr[i][1]] = Integer.toString(i + 1);
		}
		else
		{
			ans[arr[i][1]] = desc[i];
		}
	}
	return ans;
}
```

#### [507. 完美数](https://leetcode.cn/problems/perfect-number/)

@2023.06.27

1. 枚举

```java
/**
 * 枚举
 * Somnia1337
 */
public boolean checkPerfectNumber(int num)
{
	if (num == 1) return false;
	int limit = (int) Math.sqrt(num);
	int sum = 1;
	for (int i = 2; i < limit + 1; i++)
	{
		if (num % i == 0) sum += (i + num / i);
		if (sum > num) return false;
	}
	if (sum == num) return true;
	return false;
}
```

2. 打表

```java
/**
 * 打表
 * 力扣官方题解
 */
public boolean checkPerfectNumber(int num)
{
	return num == 6 || num == 28 || num == 496 || num == 8128 || num == 33550336;
}
```

#### [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

@2023.06.27

```java
/**
 * 动态规划
 * Somnia1337
 */
public int fib(int n)
{
	if (n == 0) return 0;
	int a = 0, b = 1;
	for (int i = 1; i < n; i++)
	{
		int temp = a;
		a = b;
		b = temp + a;
	}
	return b;
}
```

#### [520. 检测大写字母](https://leetcode.cn/problems/detect-capital/)

@2023.06.27

```java
/**
 * 题意理解
 * Somnia1337
 */
public boolean detectCapitalUse(String word)
{
	return word.length() == 1 || word.toLowerCase() == word || word.toUpperCase() == word || word.substring(1).toLowerCase().equals(word.substring(1));
}
```

据题意，可以简化为2种情况：

- 除首字母外的子串全为小写，首字母大小写任意。
- 全部字母为大写。

```java
/**
 * 题意理解
 * 可爱抱抱呀😥
 */
public boolean detectCapitalUse(String word)
{
	return word.equals(word.toUpperCase()) || word.substring(1).equals(word.substring(1).toLowerCase());
}
```

#### [521. 最长特殊序列 Ⅰ](https://leetcode.cn/problems/longest-uncommon-subsequence-i/description/)

@2023.06.27

看了题解不知道是题sb还是自己sb…

```java
/**
 * 脑筋急转弯
 * 力扣官方题解
 * @improver Somnia1337
 */
public int findLUSlength(String a, String b)
{
	return a.equals(b) ? -1 : Math.max(a.length(), b.length());
}
```

#### [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

@2023.06.27

一个题断断续续写了一个小时，难受。

```java
/**
 * 模拟
 * Somnia1337
 */
public String reverseStr(String s, int k)
{
	char[] chars = s.toCharArray();
	int len = s.length(), rem = len % (2 * k), part = len / (2 * k);
	for (int i = 0; i < part; i++)
		swap(chars, i * 2 * k, i * 2 * k + k);
	if (rem > 0 && rem < k) swap(chars, part * 2 * k, chars.length);
	else if (rem >= k) swap(chars, part * 2 * k, part * 2 * k + k);
	return new String(chars);
}

public void swap(char[] chars, int start, int end)
{
	for (int i = start, j = 1; i < (start + end) / 2; i++, j++)
	{
		char temp = chars[i];
		chars[i] = chars[end - j];
		chars[end - j] = temp;
	}
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public String reverseStr(String s, int k)
{
	int n = s.length();
	char[] arr = s.toCharArray();
	for (int i = 0; i < n; i += 2 * k)
	{
		reverse(arr, i, Math.min(i + k, n) - 1);
	}
	return new String(arr);
}

public void reverse(char[] arr, int left, int right)
{
	while (left < right)
	{
		char temp = arr[left];
		arr[left] = arr[right];
		arr[right] = temp;
		left++;
		right--;
	}
}
```

#### [551. 学生出勤记录 I](https://leetcode.cn/problems/student-attendance-record-i/)

@2023.06.27

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean checkRecord(String s)
{
	int aCnt = 0, lCnt = 0;
	for (int i = 0; i < s.length(); i++)
	{
		char ch = s.charAt(i);
		if (ch == 'A')
		{
			lCnt = 0;
			aCnt++;
			if (aCnt == 2) return false;
		}
		else if (ch == 'L')
		{
			lCnt++;
			if (lCnt == 3) return false;
		}
		else lCnt = 0;
	}
	return true;
}
```

#### [557. 反转字符串中的单词 III](https://leetcode.cn/problems/reverse-words-in-a-string-iii/)

@2023.06.27

```java
/**
 * 双指针
 * Somnia1337
 */
public String reverseWords(String s)
{
	int left = 0, right = 0;
	char[] chars = s.toCharArray();
	while (right < s.length())
	{
		while (right < s.length() && chars[right] != ' ') right++;
		swap(chars, left, right);
		right++;
		left = right;
	}
	return new String(chars);
}

public void swap(char[] chars, int start, int end)
{
	for (int i = start, j = 1; i < (start + end) / 2; i++, j++)
	{
		char temp = chars[i];
		chars[i] = chars[end - j];
		chars[end - j] = temp;
	}
}
```

#### [561. 数组拆分](https://leetcode.cn/problems/array-partition/description/)

@2023.06.27

用Stream小露一手。

```java
/**
 * 排序
 * Somnia1337
 */
public int arrayPairSum(int[] nums)
{
	Arrays.sort(nums);
	return IntStream.range(0, nums.length)
					.filter(i -> i % 2 == 0)
					.map(i -> nums[i])
					.sum();
}
```

只不过数据不好看🤡

![[Snipaste_ 230627_155124.png]]

不闹了。

```java
/**
 * 排序
 * Somnia1337
 */
public int arrayPairSum(int[] nums)
{
	Arrays.sort(nums);
	int sum = 0;
	for (int i = 0; i < nums.length; i += 2) sum += nums[i];
	return sum;
}
```

#### [566. 重塑矩阵](https://leetcode.cn/problems/reshape-the-matrix/)

@2023.06.27

1. 一次遍历

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[][] matrixReshape(int[][] mat, int r, int c)
{
	int[][] ret = new int[r][c];
	if (mat.length * mat[0].length != r * c) return mat;
	for (int i = 0, j = 0, m = 0, n = 0; i < mat.length && j < mat[0].length; )
	{
		ret[m][n++] = mat[i][j++];
		if (j == mat[0].length)
		{
			i++;
			j = 0;
		}
		if (n == c)
		{
			m++;
			n = 0;
		}
	}
	return ret;
}
```

2. 数学

二维数组的本质还是一行，可以只关注列数`c`与`n`，定义变量`x`从0至数组长度，`ans[x / c][x % c] = nums[x / n][x % n]`就能巧妙地遍历两个不同形状二维数组的每个元素。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int[][] matrixReshape(int[][] nums, int r, int c)
{
	int m = nums.length;
	int n = nums[0].length;
	if (m * n != r * c)
	{
		return nums;
	}
	int[][] ans = new int[r][c];
	for (int x = 0; x < m * n; ++x)
	{
		ans[x / c][x % c] = nums[x / n][x % n];
	}
	return ans;
}
```

#### [575. 分糖果](https://leetcode.cn/problems/distribute-candies/)

@2023.06.27

```java
/**
 * 贪心 + 哈希表
 * Somnia1337
 */
public int distributeCandies(int[] candyType)
{
	HashMap<Integer, Boolean> hashmap = new HashMap<>();
	for (int cT : candyType)
		hashmap.put(cT, true);
	return Math.min(hashmap.keySet()
						   .size(), candyType.length / 2);
}
```

#### [594. 最长和谐子序列](https://leetcode.cn/problems/longest-harmonious-subsequence/)

@2023.06.27

1. 哈希表

自己用哈希表做的超时了也不会改。

学了一招`hashmap.keySet()`（第575题是在本题之后做的）。

```java
/**
 * 哈希表
 * 力扣官方题解
 * @improver Somnia1337
 */
public int findLHS(int[] nums)
{
	HashMap<Integer, Integer> cnt = new HashMap<>();
	int res = 0;
	for (int num : nums)
		cnt.merge(num, 1, Integer::sum);
	for (int key : cnt.keySet())
		if (cnt.containsKey(key + 1)) res = Math.max(res, cnt.get(key) + cnt.get(key + 1));
	return res;
}
```

2. 排序

```java
/**
 * 排序
 * 力扣官方题解
 */
public int findLHS(int[] nums)
{
	Arrays.sort(nums);
	int begin = 0;
	int res = 0;
	for (int end = 0; end < nums.length; end++)
	{
		while (nums[end] - nums[begin] > 1)
		{
			begin++;
		}
		if (nums[end] - nums[begin] == 1)
		{
			res = Math.max(res, end - begin + 1);
		}
	}
	return res;
}
```

#### [598. 范围求和 II](https://leetcode.cn/problems/range-addition-ii/description/)

@2023.06.27

错了4次，第一次暴力爆内存，后几次思路有尛问题。

![[Snipaste_ 230627_234117.png|200]]

其实就是维护所有操作涉及的最小交集，属于是脑筋急转弯了。

```java
/**
 * 维护最小交集
 * Somnia1337
 */
public int maxCount(int m, int n, int[][] ops)
{
	if (ops.length == 0) return m * n;
	int a = ops[0][0], b = ops[0][1];
	for (int x = 0; x < ops.length; x++)
	{
		a = Math.min(a, ops[x][0]);
		b = Math.min(b, ops[x][1]);
	}
	return a * b;
}
```

#### [599. 两个列表的最小索引总和](https://leetcode.cn/problems/minimum-index-sum-of-two-lists/)

@2023.06.28

```java
/**
 * 哈希表
 * Somnia1337
 */
public String[] findRestaurant(String[] list1, String[] list2)
{
	int len1 = list1.length, len2 = list2.length;
	HashMap<String, Integer> map1 = new HashMap<>();
	HashMap<String, Integer> map2 = new HashMap<>();
	for (int i = 0; i < len1; i++)
		map1.put(list1[i], i);
	Set<String> set = map1.keySet();
	for (int i = 0; i < len2; i++)
		if (set.contains(list2[i])) map2.put(list2[i], i + map1.get(list2[i]));
	int ans = len1 + len2, cnt = 0;
	for (String key : map2.keySet())
	{
		int value = map2.get(key);
		if (value < ans)
		{
			ans = value;
			cnt = 1;
		}
		else if (value == ans) cnt++;
	}
	String[] ret = new String[cnt];
	int i = 0;
	for (String key : map2.keySet())
	{
		if (map2.get(key) == ans) ret[i++] = key;
	}
	return ret;
}
```

其实只需遍历`list2`一次，一直维护一个最小索引和以及一个答案列表，只需在遇到更小索引和后清空原答案即可。

优化：遍历`list2`时如果当前索引已经大于最小索引和，则可直接退出。

```java
/**
 * 哈希表
 * 力扣官方题解
 * @improver 龅牙叔, Somnia1337
 */
public String[] findRestaurant(String[] list1, String[] list2)
{
	Map<String, Integer> index = new HashMap<String, Integer>();
	for (int i = 0; i < list1.length; i++)
	{
		index.put(list1[i], i);
	}
	List<String> ret = new ArrayList<String>();
	int indexSum = Integer.MAX_VALUE;
	for (int i = 0; i < list2.length; i++)
	{
		if (index.containsKey(list2[i]))
		{
			int j = index.get(list2[i]);
			if (i + j < indexSum)
			{
				ret.clear();
				ret.add(list2[i]);
				indexSum = i + j;
			}
			else if (i + j == indexSum)
			{
				ret.add(list2[i]);
			}
			else if (i > indexSum) return ret.toArray(new String[ret.size()]);
		}
	}
	return ret.toArray(new String[ret.size()]);
}
```

#### [605. 种花问题](https://leetcode.cn/problems/can-place-flowers/)

@2023.06.28

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean canPlaceFlowers(int[] flowerbed, int n)
{
	int len = flowerbed.length;
	int cnt = 0, stage = 0, cur = 0;
	for (int i = 0; i < len; i++)
	{
		if (flowerbed[i] == 1)
		{
			if (stage == 0)
			{
				stage = 1;
				cnt += cur / 2;
				cur = 0;
			}
			else
			{
				cnt += (cur - 1) / 2;
				cur = 0;
			}
		}
		else cur++;
		if (i == len - 1) cnt += (cur - stage + 1) / 2;
	}
	return n <= cnt;
}
```

优化：根据题意，花不能连续，所以每当遍历至1可以跳过下一格（下一格必定为0且必定不能种花）。能种花的格子需满足其下一格也为0或者为最后一个（由于遇到1就跳过下一格，所以遇到的0的上一格也必为0）。

```java
/**
 * 一次遍历
 * 我
 * @improver Somnia1337
 */
public boolean canPlaceFlowers(int[] flowerbed, int n)
{
	for (int i = 0; i < flowerbed.length; i++)
	{
		if (flowerbed[i] == 1)
		{
			i++;
		}
		else if (i == flowerbed.length - 1 || flowerbed[i + 1] == 0)
		{
			n--;
			i++;
		}
	}
	return n <= 0;
}
```

#### [628. 三个数的最大乘积](https://leetcode.cn/problems/maximum-product-of-three-numbers/)

@2023.06.28

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public int maximumProduct(int[] nums)
{
	int len = nums.length;
	if (len == 3) return nums[0] * nums[1] * nums[2];
	Arrays.sort(nums);
	return nums[len - 1] > 0 ? nums[len - 1] * Math.max(nums[0] * nums[1], nums[len - 2] * nums[len - 3]) : nums[len - 1] * Math.min(nums[0] * nums[1], nums[len - 2] * nums[len - 3]);
}
```

2. 线性扫描

由于只需找出最大的3个值与最小的2个值，可以进行线性扫描，降低复杂度。

```java
/**
 * 线性扫描
 * 力扣官方题解
 */
public int maximumProduct(int[] nums)
{
	// 最小的和第二小的
	int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
	// 最大的、第二大的和第三大的
	int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
	for (int x : nums)
	{
		if (x < min1)
		{
			min2 = min1;
			min1 = x;
		}
		else if (x < min2)
		{
			min2 = x;
		}
		if (x > max1)
		{
			max3 = max2;
			max2 = max1;
			max1 = x;
		}
		else if (x > max2)
		{
			max3 = max2;
			max2 = x;
		}
		else if (x > max3)
		{
			max3 = x;
		}
	}
	return Math.max(min1 * min2 * max1, max1 * max2 * max3);
}
```

#### [643. 子数组最大平均数 I](https://leetcode.cn/problems/maximum-average-subarray-i/)

@2023.06.28

交了3次都超时，通过率唰唰掉…

分段计算将导致很多元素被重复计算了很多次，而使用滑动窗口，每次只需计算加头减尾，相当于一次遍历就能得到所有子组和。

```java
/**
 * 滑动窗口
 * 力扣官方题解
 */
public double findMaxAverage(int[] nums, int k)
{
	int sum = 0;
	int n = nums.length;
	for (int i = 0; i < k; i++)
	{
		sum += nums[i];
	}
	int maxSum = sum;
	for (int i = k; i < n; i++)
	{
		sum = sum - nums[i - k] + nums[i];
		maxSum = Math.max(maxSum, sum);
	}
	return 1.0 * maxSum / k;
}
```

#### [645. 错误的集合](https://leetcode.cn/problems/set-mismatch/)

@2023.06.28

1. 排序 + 位运算

```java
/**
 * 排序 + 位运算
 * Somnia1337
 */
public int[] findErrorNums(int[] nums)
{
	Arrays.sort(nums);
	int[] ret = new int[2];
	for (int i = 0; i < nums.length - 1; i++)
	{
		if (nums[i] == nums[i + 1])
		{
			nums[i + 1] = 0;
			ret[0] = nums[i];
			break;
		}
	}
	int x = 0;
	for (int i = 0; i < nums.length; i++)
	{
		x ^= (i + 1);
		x ^= (nums[i]);
	}
	ret[1] = x;
	return ret;
}
```

2. 哈希表

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int[] findErrorNums(int[] nums)
{
	int[] errorNums = new int[2];
	int n = nums.length;
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	for (int num : nums)
	{
		map.put(num, map.getOrDefault(num, 0) + 1);
	}
	for (int i = 1; i <= n; i++)
	{
		int count = map.getOrDefault(i, 0);
		if (count == 2)
		{
			errorNums[0] = i;
		}
		else if (count == 0)
		{
			errorNums[1] = i;
		}
	}
	return errorNums;
}
```

3. 数学

```java
/**
 * 数学
 * 清风Python
 * @translator ChatGPT
 */
public int[] findErrorNums(int[] nums)
{
	int ln = nums.length;
	Set<Integer> set = new HashSet<>();
	int total = 0;
	for (int num : nums)
	{
		set.add(num);
		total += num;
	}
	int[] result = new int[2];
	result[0] = Arrays.stream(nums)
					  .sum() - set.stream()
								  .mapToInt(Integer::intValue)
								  .sum();
	result[1] = (1 + ln) * ln / 2 - set.stream()
									   .mapToInt(Integer::intValue)
									   .sum();
	return result;
}
```

#### [657. 机器人能否返回原点](https://leetcode.cn/problems/robot-return-to-origin/)

@2023.06.28

```java
/**
 * 模拟
 * Somnia1337
 */
public boolean judgeCircle(String moves)
{
	int v = 0, h = 0;
	for (int i = 0; i < moves.length(); i++)
	{
		char ch = moves.charAt(i);
		if (ch == 'R') v++;
		else if (ch == 'L') v--;
		else if (ch == 'U') h++;
		else if (ch == 'D') h--;
	}
	return v == 0 && h == 0;
}
```

#### [661. 图片平滑器](https://leetcode.cn/problems/image-smoother/)

@2023.07.10

1. 一次遍历

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[][] imageSmoother(int[][] img)
{
	int rows = img.length, columns = img[0].length;
	int[][] ret = new int[rows][columns];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < columns; j++)
		{
			int sum = 0, cnt = 0;
			for (int m = Math.max(i - 1, 0); m <= Math.min(i + 1, rows - 1); m++)
			{
				for (int n = Math.max(j - 1, 0); n <= Math.min(j + 1, columns - 1); n++)
				{
					sum += img[m][n];
					cnt++;
				}
			}
			ret[i][j] = sum / cnt;
		}
	}
	return ret;
}
```

2. 前缀和

```java
/**
 * 前缀和
 * 宫水三叶
 */
public int[][] imageSmoother(int[][] img)
{
	int m = img.length, n = img[0].length;
	int[][] sum = new int[m + 10][n + 10];
	for (int i = 1; i <= m; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + img[i - 1][j - 1];
		}
	}
	int[][] ans = new int[m][n];
	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			int a = Math.max(0, i - 1), b = Math.max(0, j - 1);
			int c = Math.min(m - 1, i + 1), d = Math.min(n - 1, j + 1);
			int cnt = (c - a + 1) * (d - b + 1);
			int tot = sum[c + 1][d + 1] - sum[a][d + 1] - sum[c + 1][b] + sum[a][b];
			ans[i][j] = tot / cnt;
		}
	}
	return ans;
}
```

#### [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

@2023.06.28

```java
/**
 * 贪心
 * Somnia1337
 */
public int findLengthOfLCIS(int[] nums)
{
	int max = 0, cur = 1;
	for (int i = 0; i < nums.length - 1; i++)
	{
		if (nums[i + 1] > nums[i]) cur++;
		else
		{
			max = Math.max(max, cur);
			cur = 1;
		}
	}
	max = Math.max(max, cur);
	return max;
}
```

#### [680. 验证回文串 II](https://leetcode.cn/problems/valid-palindrome-ii/)

@2023.06.29

```java
/**
 * 双指针 + 递归
 * Somnia1337
 */
public boolean validPalindrome(String s)
{
	return validPalindrome(s, 0);
}

public boolean validPalindrome(String s, int v)
{
	int len = s.length();
	int left = 0, right = len - 1;
	while (left < right)
	{
		char ch1 = s.charAt(left), ch2 = s.charAt(right);
		if (ch1 != ch2)
		{
			if (v == 1) return false;
			v = 1;
			if (left == right - 1) return true;
			else if (s.charAt(right - 1) == ch1 || s.charAt(left + 1) == ch2)
				return validPalindrome(s.substring(left, right), 1) || validPalindrome(s.substring(left + 1, right + 1), 1);
			else return false;
		}
		left++;
		right--;
	}
	return true;
}
```

```java
/**
 * 双指针 + 递归
 * 力扣官方题解
 */
public boolean validPalindrome(String s)
{
	int low = 0, high = s.length() - 1;
	while (low < high)
	{
		char c1 = s.charAt(low), c2 = s.charAt(high);
		if (c1 == c2)
		{
			++low;
			--high;
		}
		else
		{
			return validPalindrome(s, low, high - 1) || validPalindrome(s, low + 1, high);
		}
	}
	return true;
}

public boolean validPalindrome(String s, int low, int high)
{
	for (int i = low, j = high; i < j; ++i, --j)
	{
		char c1 = s.charAt(i), c2 = s.charAt(j);
		if (c1 != c2)
		{
			return false;
		}
	}
	return true;
}
```

#### [682. 棒球比赛](https://leetcode.cn/problems/baseball-game/)

@2023.07.09

```java
/**
 * 模拟
 * Somnia1337
 */
public int calPoints(String[] operations)
{
	ArrayList<Integer> points = new ArrayList<>();
	int cur = 0;
	for (String s : operations)
	{
		if (s.equals("C"))
		{
			points.remove(cur - 1);
			cur--;
		}
		else if (s.equals("D"))
		{
			points.add(points.get(cur - 1) * 2);
			cur++;
		}
		else if (s.equals("+"))
		{
			points.add(points.get(cur - 1) + points.get(cur - 2));
			cur++;
		}
		else
		{
			points.add(Integer.parseInt(s));
			cur++;
		}
	}
	int ans = 0;
	for (int point : points)
	{
		ans += point;
	}
	return ans;
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public int calPoints(String[] ops)
{
	int ret = 0;
	List<Integer> points = new ArrayList<Integer>();
	for (String op : ops)
	{
		int n = points.size();
		switch (op.charAt(0))
		{
			case '+':
				ret += points.get(n - 1) + points.get(n - 2);
				points.add(points.get(n - 1) + points.get(n - 2));
				break;
			case 'D':
				ret += 2 * points.get(n - 1);
				points.add(2 * points.get(n - 1));
				break;
			case 'C':
				ret -= points.get(n - 1);
				points.remove(n - 1);
				break;
			default:
				ret += Integer.parseInt(op);
				points.add(Integer.parseInt(op));
				break;
		}
	}
	return ret;
}
```

#### [693. 交替位二进制数](https://leetcode.cn/problems/binary-number-with-alternating-bits/)

@2023.06.29

1. 字符串

```java
/**
 * 字符串
 * Somnia1337
 */
public boolean hasAlternatingBits(int n)
{
	String s = Integer.toBinaryString(n);
	while (s.startsWith("0")) s = s.substring(1);
	for (int i = 0; i < s.length() - 1; i++)
		if (s.charAt(i) + s.charAt(i + 1) != '0' + '1') return false;
	return true;
}
```

效果意外地好。

![[Snipaste_ 230629_072741.png]]

2. 位运算

`a`为`n`与`n`的所有位右移一位异或的结果，如果`n`所有位交替，`a`才为$011\cdots1$。`a + 1`为$1000\cdots0$，与`a`与运算结果为0。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public boolean hasAlternatingBits(int n)
{
	int a = n ^ (n >> 1);
	return (a & (a + 1)) == 0;
}
```

#### [696. 计数二进制子串](https://leetcode.cn/problems/count-binary-substrings/)

@2023.06.29

这是简单题？？？

题解NB！！！

将字符串按0、1的数量分组形成数组`counts`（如"00111011"分组为\[2, 3, 1, 2]），相邻的两个值为0、1（或1、0）的数量，他们能形成的满足条件的子串数量为`Math.min(a, b)`，遍历所有值对，求和即为答案。

```java
/**
 * 分组计数
 * 力扣官方题解
 */
public int countBinarySubstrings(String s)
{
	List<Integer> counts = new ArrayList<Integer>();
	int ptr = 0, n = s.length();
	while (ptr < n)
	{
		char c = s.charAt(ptr);
		int count = 0;
		while (ptr < n && s.charAt(ptr) == c)
		{
			++ptr;
			++count;
		}
		counts.add(count);
	}
	int ans = 0;
	for (int i = 1; i < counts.size(); ++i)
	{
		ans += Math.min(counts.get(i), counts.get(i - 1));
	}
	return ans;
}
```

#### [697. 数组的度](https://leetcode.cn/problems/degree-of-an-array/)

@2023.06.29

连续做不出，心态爆炸！

记录众数`x`，那么符合条件的子数组必包含所有`x`，其最短长度为`x`首次、末次出现的位置之差。

可能有多个众数，需要记录每个数出现的次数及首末位置，通过哈希表将每个数映射至一个长度为3的数组。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int findShortestSubArray(int[] nums)
{
	Map<Integer, int[]> map = new HashMap<Integer, int[]>();
	int n = nums.length;
	for (int i = 0; i < n; i++)
	{
		if (map.containsKey(nums[i]))
		{
			map.get(nums[i])[0]++;
			map.get(nums[i])[2] = i;
		}
		else
		{
			map.put(nums[i], new int[]{1, i, i});
		}
	}
	int maxNum = 0, minLen = 0;
	for (Map.Entry<Integer, int[]> entry : map.entrySet())
	{
		int[] arr = entry.getValue();
		if (maxNum < arr[0])
		{
			maxNum = arr[0];
			minLen = arr[2] - arr[1] + 1;
		}
		else if (maxNum == arr[0])
		{
			if (minLen > arr[2] - arr[1] + 1)
			{
				minLen = arr[2] - arr[1] + 1;
			}
		}
	}
	return minLen;
}
```

#### [704. 二分查找](https://leetcode.cn/problems/binary-search/)

@2023.06.29

```java
/**
 * 二分查找
 * Somnia1337
 */
public int search(int[] nums, int target)
{
	int len = nums.length;
	int left = 0, right = len - 1;
	while (left <= right)
	{
		int mid = (left + right) / 2;
		if (nums[mid] == target) return mid;
		else if (nums[mid] > target) right = mid - 1;
		else left = mid + 1;
	}
	return -1;
}
```

#### [709. 转换成小写字母](https://leetcode.cn/problems/to-lower-case/)

@2023.06.29

```java
/**
 * 字符串操作
 * Somnia1337
 */
public String toLowerCase(String s)
{
	return s.toLowerCase();
}
```

#### [717. 1 比特与 2 比特字符](https://leetcode.cn/problems/1-bit-and-2-bit-characters/)

@2023.06.29

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isOneBitCharacter(int[] bits)
{
	int len = bits.length;
	for (int i = 0; i < len; i++)
	{
		if (i == len - 1) return true;
		else if (bits[i] == 1) i++;
	}
	return false;
}
```

#### [724. 寻找数组的中心下标](https://leetcode.cn/problems/find-pivot-index/)

@2023.06.29

1. 滑动窗口

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int pivotIndex(int[] nums)
{
	int sum1 = 0, sum2 = 0;
	for (int i = 1; i < nums.length; i++) sum2 += nums[i];
	for (int i = 0; i < nums.length; i++)
	{
		if (sum1 == sum2) return i;
		else if (i == nums.length - 1) return -1;
		sum1 += nums[i];
		sum2 -= nums[i + 1];
	}
	return -1;
}
```

2. 前缀和

即全部元素和为`total`，遍历至第`i`个元素时其左侧元素和为`sum`，右侧元素和为`total - sum - nums[i]`，左右两侧相等的条件为`sum` = `total - sum - nums[i]`，即`sum * 2 + nums[i]` = `total`。

```java
/**
 * 前缀和
 * 力扣官方题解
 */
public int pivotIndex(int[] nums)
{
	int total = Arrays.stream(nums)
					  .sum();
	int sum = 0;
	for (int i = 0; i < nums.length; ++i)
	{
		if (2 * sum + nums[i] == total)
		{
			return i;
		}
		sum += nums[i];
	}
	return -1;
}
```

#### [728. 自除数](https://leetcode.cn/problems/self-dividing-numbers/)

@2023.06.29

```java
/**
 * 暴力
 * Somnia1337
 */
public List<Integer> selfDividingNumbers(int left, int right)
{
	ArrayList<Integer> ans = new ArrayList<>();
	for (int i = left; i <= right; i++)
	{
		int j = i, v = 1;
		while (j > 0)
		{
			if (j % 10 == 0 || i % (j % 10) != 0)
			{
				v = 0;
				break;
			}
			j /= 10;
		}
		if (v == 1) ans.add(i);
	}
	return ans;
}
```

#### [734. 句子相似性](https://leetcode.cn/problems/sentence-similarity/)

@2024.01.07

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean areSentencesSimilar(String[] sentence1, String[] sentence2, List<List<String>> similarPairs) {
	if (sentence1.length != sentence2.length) return false;
	Map<String, Set<String>> sim = new HashMap<>();
	for (List<String> p : similarPairs) {
		sim.computeIfAbsent(p.get(0), k -> new HashSet<>()).add(p.get(1));
		sim.computeIfAbsent(p.get(1), k -> new HashSet<>()).add(p.get(0));
	}
	for (int i = 0; i < sentence1.length; i++) {
		if (!sentence1[i].equals(sentence2[i]) && (!sim.containsKey(sentence1[i]) || !sim.get(sentence1[i]).contains(sentence2[i])))
			return false;
	}
	return true;
}
```

#### [744. 寻找比目标字母大的最小字母](https://leetcode.cn/problems/find-smallest-letter-greater-than-target/)

@2023.06.29

```java
/**
 * 一次遍历
 * Somnia1337
 */
public char nextGreatestLetter(char[] letters, char target)
{
	char ans = '{';
	for (char ch : letters)
		if (ch > target && ch < ans) ans = ch;
	return ans == '{' ? letters[0] : ans;
}
```

#### [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

@2023.06.29

只需从高到低建立每级楼梯到楼顶的最小花费，然后从0级/1级中选择更小的即可。

```java
/**
 * 动态规划 + 哈希表
 * Somnia1337
 */
public int minCostClimbingStairs(int[] cost)
{
	int len = cost.length;
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	for (int i = len - 1; i >= 0; i--)
	{
		if (i >= len - 2) hashmap.put(i, cost[i]);
		else hashmap.put(i, cost[i] + Math.min(hashmap.get(i + 1), hashmap.get(i + 2)));
	}
	return Math.min(hashmap.get(0), hashmap.get(1));
}
```

在这里使用哈希表完全没有必要，因为哈希表的目的是将检索数据的复杂度降至$O(1)$，但这里的数据都是有序遍历并存储的，使用数组就能满足$O(1)$检索。

```java
/**
 * 动态规划 + 动态数组
 * 力扣官方题解
 */
public int minCostClimbingStairs(int[] cost)
{
	int n = cost.length;
	int prev = 0, curr = 0;
	for (int i = 2; i <= n; i++)
	{
		int next = Math.min(curr + cost[i - 1], prev + cost[i - 2]);
		prev = curr;
		curr = next;
	}
	return curr;
}
```

```java
/**
 * 动态规划 + 原地修改
 * liyinze1
 */
public int minCostClimbingStairs(int[] cost)
{
	int len = cost.length;
	for (int i = 2; i < len; i++)
	{
		cost[i] += Math.min(cost[i - 1], cost[i - 2]);
	}
	return Math.min(cost[len - 1], cost[len - 2]);
}
```

```java
/**
 * 动态规划
 * 代码随想录
 */
public int minCostClimbingStairs(int[] cost)
{
	int len = cost.length;
	int[] dp = new int[len + 1];
	dp[0] = 0;
	dp[1] = 0;
	for (int i = 2; i <= len; i++)
	{
		dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
	}
	return dp[len];
}
```

#### [747. 至少是其他数字两倍的最大数](https://leetcode.cn/problems/largest-number-at-least-twice-of-others/)

@2023.06.29

1. 排序 + 哈希表

```java
/**
 * 排序 + 哈希表
 * Somnia1337
 */
public int dominantIndex(int[] nums)
{
	int len = nums.length;
	if (len == 1) return 0;
	HashMap<Integer, Integer> hashmap = new HashMap<>();
	for (int i = 0; i < len; i++) hashmap.put(nums[i], i);
	Arrays.sort(nums);
	int max = nums[len - 1], sec = nums[len - 2];
	return max >= sec * 2 ? hashmap.get(max) : -1;
}
```

2. 一次遍历

其实只需找到最大元素与次大元素，无需排序与哈希表，一次遍历即可。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public int dominantIndex(int[] nums)
{
	int m1 = -1, m2 = -1;
	int index = -1;
	for (int i = 0; i < nums.length; i++)
	{
		if (nums[i] > m1)
		{
			m2 = m1;
			m1 = nums[i];
			index = i;
		}
		else if (nums[i] > m2)
		{
			m2 = nums[i];
		}
	}
	return m1 >= m2 * 2 ? index : -1;
}
```

#### [748. 最短补全词](https://leetcode.cn/problems/shortest-completing-word/)

@2023.06.29

```java
/**
 * 字符统计
 * Somnia1337
 */
public String shortestCompletingWord(String licensePlate, String[] words)
{
	licensePlate = licensePlate.toLowerCase();
	int[] fLP = new int[26];
	for (int i = 0; i < licensePlate.length(); i++)
	{
		char c = licensePlate.charAt(i);
		if (c - 'a' >= 0 && 'z' - c >= 0) fLP[c - 'a']++;
	}
	ArrayList<String> validList = new ArrayList<>();
	for (String word : words)
	{
		int[] fW = new int[26];
		for (int i = 0; i < word.length(); i++) fW[word.charAt(i) - 'a']++;
		if (isValid(fLP, fW)) validList.add(word);
	}
	int minLen = Integer.MAX_VALUE;
	String ret = "";
	for (int i = 0; i < validList.size(); i++)
	{
		String cur = validList.get(i);
		if (cur.length() < minLen)
		{
			minLen = cur.length();
			ret = cur;
		}
	}
	return ret;
}

public boolean isValid(int[] std, int[] x)
{
	for (int i = 0; i < std.length; i++)
		if (x[i] < std[i]) return false;
	return true;
}
```

#### [762. 二进制表示中质数个计算置位](https://leetcode.cn/problems/prime-number-of-set-bits-in-binary-representation/)

@2023.06.29

1. 字符串

```java
/**
 * 字符串
 * Somnia1337
 */
public int countPrimeSetBits(int left, int right)
{
	HashSet<Integer> primes = new HashSet<>();
	primes.add(2);
	primes.add(3);
	primes.add(5);
	primes.add(7);
	primes.add(11);
	primes.add(13);
	primes.add(17);
	primes.add(19);
	primes.add(23);
	primes.add(29);
	primes.add(31);
	int ans = 0;
	for (int i = left; i <= right; i++)
	{
		String binary = Integer.toBinaryString(i);
		int cur = 0;
		for (int j = 0; j < binary.length(); j++)
			if (binary.charAt(j) == '1') cur++;
		if (primes.contains(cur)) ans++;
	}
	return ans;
}
```

2. 库方法 + 数学 + 位运算

使用库方法Integer.bitCount计算1的个数。

```java
/**
 * 库方法 + 数学 + 位运算
 * 力扣官方题解
 */
public int countPrimeSetBits(int left, int right)
{
	int ans = 0;
	for (int x = left; x <= right; ++x)
	{
		if (isPrime(Integer.bitCount(x)))
		{
			++ans;
		}
	}
	return ans;
}

private boolean isPrime(int x) {
	if (x < 2) return false;
	for (int i = 2; i * i <= x; i++) {
		if (x % i == 0) return false;
	}
	return true;
}
```

优化：数据范围内的二进制位不超过19，19内的质数有2, 3, 5, 7, 11, 13, 17, 19，可以用一个二进制数`mask` = $10100010100010101100$表示，其中为1的位代表该位为质数。设1的个数为`c`，若`pow(2, c)`与`mask`按位与运算不为0，说明`c`为质数。

```java
/**
 * 质数判断优化
 * 力扣官方题解
 */
public int countPrimeSetBits(int left, int right)
{
	int ans = 0;
	for (int x = left; x <= right; ++x)
	{
		if (((1 << Integer.bitCount(x)) & 665772) != 0)
		{
			++ans;
		}
	}
	return ans;
}
```

#### [766. 托普利茨矩阵](https://leetcode.cn/problems/toeplitz-matrix/)

@2023.07.09

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isToeplitzMatrix(int[][] matrix)
{
	int rows = matrix.length, columns = matrix[0].length;
	for (int i = 1; i < rows; i++)
	{
		for (int j = 1; j < columns; j++)
		{
			if (matrix[i][j] != matrix[i - 1][j - 1])
			{
				return false;
			}
		}
	}
	return true;
}
```

#### [771. 宝石与石头](https://leetcode.cn/problems/jewels-and-stones/)

@2023.06.29

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int numJewelsInStones(String jewels, String stones)
{
	int ans = 0;
	for (int i = 0; i < stones.length(); i++)
		if (jewels.indexOf(stones.charAt(i)) >= 0) ans++;
	return ans;
}
```

#### [796. 旋转字符串](https://leetcode.cn/problems/rotate-string/)

@2023.06.29

1. 双指针

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean rotateString(String s, String goal)
{
	int p1 = 0, p2 = 0, len1 = s.length(), len2 = goal.length();
	if (len1 != len2) return false;
	char ch1 = s.charAt(0);
	while (p2 < len2)
	{
		p1 = 0;
		while (goal.charAt(p2 % len2) != ch1)
		{
			p2++;
			if (p2 > len2) return false;
		}
		int v = 1, temp = p2;
		for (int i = 0; i < len2; i++)
			if (s.charAt(p1++) != goal.charAt(p2++ % len2))
			{
				v = 0;
				p2 = temp + 1;
				break;
			}
		if (v == 1) return true;
	}
	return false;
}
```

2. 模拟

```java
/**
 * 模拟
 * 力扣官方题解
 */
public boolean rotateString(String s, String goal)
{
	int m = s.length(), n = goal.length();
	if (m != n)
	{
		return false;
	}
	for (int i = 0; i < n; i++)
	{
		boolean flag = true;
		for (int j = 0; j < n; j++)
		{
			if (s.charAt((i + j) % n) != goal.charAt(j))
			{
				flag = false;
				break;
			}
		}
		if (flag)
		{
			return true;
		}
	}
	return false;
}
```

3. 字符串拼接

```java
/**
 * 字符串拼接
 * 力扣官方题解
 */
public boolean rotateString(String s, String goal)
{
	return s.length() == goal.length() && (s + s).contains(goal);
}
```

#### [804. 唯一摩尔斯密码词](https://leetcode.cn/problems/unique-morse-code-words/)

@2023.06.29

```java
/**
 * 哈希集
 * Somnia1337
 */
public int uniqueMorseRepresentations(String[] words)
{
	String[] codes = {
			".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."
	};
	HashSet<String> hashset = new HashSet<>();
	for (String s : words)
	{
		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < s.length(); i++)
			sb.append(codes[s.charAt(i) - 'a']);
		hashset.add(sb.toString());
	}
	return hashset.size();
}
```

#### [806. 写字符串需要的行数](https://leetcode.cn/problems/number-of-lines-to-write-string/)

@2023.07.09

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[] numberOfLines(int[] widths, String s)
{
	int lineCnt = 1, curLen = 0;
	for (char c : s.toCharArray())
	{
		int len = widths[c - 'a'];
		if (100 - curLen < len)
		{
			lineCnt++;
			curLen = len;
			continue;
		}
		curLen += len;
	}
	return new int[]{lineCnt, curLen};
}
```

#### [812. 最大三角形面积](https://leetcode.cn/problems/largest-triangle-area/description/)

@2023.07.13

```java
/**
 * 暴力
 * Somnia1337
 */
public double largestTriangleArea(int[][] points)
{
	double ans = 0;
	for (int i = 0; i < points.length - 2; i++)
	{
		for (int j = i + 1; j < points.length - 1; j++)
		{
			for (int k = j + 1; k < points.length; k++)
			{
				double a = Math.sqrt(Math.pow(points[i][0] - points[j][0], 2) + Math.pow(points[i][1] - points[j][1], 2));
				double b = Math.sqrt(Math.pow(points[i][0] - points[k][0], 2) + Math.pow(points[i][1] - points[k][1], 2));
				double c = Math.sqrt(Math.pow(points[j][0] - points[k][0], 2) + Math.pow(points[j][1] - points[k][1], 2));
				double p = (a + b + c) / 2;
				double area = Math.sqrt(p * (p - a) * (p - b) * (p - c));
				if (area > ans) ans = area;
			}
		}
	}
	return ans;
}
```

#### [819. 最常见的单词](https://leetcode.cn/problems/most-common-word/)

@2023.07.09

```java
/**
 * 哈希表
 * Somnia1337
 * @improver ChatGPT
 */
public String mostCommonWord(String paragraph, String[] banned)
{
	paragraph = paragraph.toLowerCase();
	String[] words = paragraph.split("[ ,;.?!']+");
	HashSet<String> bannedSet = new HashSet<>(Arrays.asList(banned));
	HashMap<String, Integer> counts = new HashMap<>();
	for (String word : words)
	{
		if (!bannedSet.contains(word))
		{
			counts.merge(word, 1, Integer::sum);
		}
	}
	int maxCnt = 0;
	String ans = "";
	for (Map.Entry<String, Integer> entry : counts.entrySet())
	{
		if (entry.getValue() > maxCnt)
		{
			maxCnt = entry.getValue();
			ans = entry.getKey();
		}
	}
	return ans;
}
```

#### [821. 字符的最短距离](https://leetcode.cn/problems/shortest-distance-to-a-character/)

@2023.06.29

对`s`一次遍历，用数组记录`c`出现的所有位置，然后遍历该数组，直接计算`s`中每个字符与`c`的最短距离。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[] shortestToChar(String s, char c)
{
	int len = s.length();
	int[] ret = new int[len];
	int[] pos = new int[len];
	int j = 0;
	for (int i = 0; i < len; i++) if (s.charAt(i) == c) pos[j++] = i;
	for (int r = 0; r < j; r++)
	{
		int start = pos[r];
		int end = r != j - 1 ? pos[r + 1] : len;
		for (int t = start; t < end; t++) ret[t] = Math.min(t - start, end - t);
	}
	for (int i = 0; i < pos[0]; i++) ret[i] = pos[0] - i;
	for (int i = pos[j - 1]; i < len; i++) ret[i] = i - pos[j - 1];
	return ret;
}
```

#### [824. 山羊拉丁文](https://leetcode.cn/problems/goat-latin/)

@2023.07.09

```java
/**
 * 模拟
 * Somnia1337
 */
public String toGoatLatin(String sentence)
{
	String vowels = "aeiouAEIOU";
	String[] words = sentence.split(" ");
	StringBuilder sb = new StringBuilder();
	int len = words.length;
	for (int i = 0; i < len; i++)
	{
		char c = words[i].charAt(0);
		if (vowels.indexOf(c) >= 0)
		{
			sb.append(words[i]);
		}
		else
		{
			sb.append(words[i].substring(1))
			  .append(c);
		}
		sb.append("ma")
		  .append("a".repeat(i + 1));
		if (i < len - 1)
		{
			sb.append(" ");
		}
	}
	return sb.toString();
}
```

#### [830. 较大分组的位置](https://leetcode.cn/problems/positions-of-large-groups/)

@2023.06.29

```java
/**
 * 分组计数
 * Somnia1337
 */
public List<List<Integer>> largeGroupPositions(String s)
{
	ArrayList<Integer> counts = new ArrayList<>();
	ArrayList<List<Integer>> ret = new ArrayList<>();
	int ptr = 0, n = s.length();
	while (ptr < n)
	{
		char c = s.charAt(ptr);
		int count = 0;
		while (ptr < n && s.charAt(ptr) == c)
		{
			++ptr;
			++count;
		}
		counts.add(count);
	}
	int curSum = 0;
	for (int i = 0; i < counts.size(); i++)
	{
		int count = counts.get(i);
		if (count >= 3)
		{
			ArrayList<Integer> temp = new ArrayList<>();
			temp.add(curSum);
			temp.add(curSum + count - 1);
			ret.add(temp);
		}
		curSum += count;
	}
	return ret;
}
```

#### [832. 翻转图像](https://leetcode.cn/problems/flipping-an-image/)

@2023.07.09

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int[][] flipAndInvertImage(int[][] image)
{
	int rows = image.length, columns = image[0].length;
	int[][] ret = new int[rows][columns];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < columns; j++)
		{
			ret[i][j] = 1 - image[i][columns - j - 1];
		}
	}
	return ret;
}
```

#### [836. 矩形重叠](https://leetcode.cn/problems/rectangle-overlap/)

@2023.07.10

```java
/**
 * 几何
 * Somnia1337
 */
public boolean isRectangleOverlap(int[] rec1, int[] rec2)
{
	return rec1[0] < rec2[2] && rec1[1] < rec2[3] && rec2[0] < rec1[2] && rec2[1] < rec1[3];
}
```

#### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

@2023.07.04

1. 栈

```java
/**
 * 栈
 * Somnia1337
 */
public boolean backspaceCompare(String s, String t)
{
	Stack<Character> stack = new Stack<>();
	int len1 = s.length(), len2 = t.length();
	for (int i = 0; i < len1; i++)
	{
		char c1 = s.charAt(i);
		if (c1 == '#')
		{
			if (!stack.isEmpty()) stack.pop();
		}
		else stack.push(c1);
	}
	int backCount = 0;
	for (int i = len2 - 1; i >= 0; i--)
	{
		char c2 = t.charAt(i);
		if (c2 == '#') backCount++;
		else
		{
			if (backCount > 0) backCount--;
			else if (stack.isEmpty() || stack.pop() != c2) return false;
		}
	}
	return stack.isEmpty();
}
```

2. 字符串

```java
/**
 * 字符串
 * 力扣官方题解
 */
public boolean backspaceCompare(String S, String T)
{
	return build(S).equals(build(T));
}

public String build(String str)
{
	StringBuffer ret = new StringBuffer();
	int length = str.length();
	for (int i = 0; i < length; ++i)
	{
		char ch = str.charAt(i);
		if (ch != '#')
		{
			ret.append(ch);
		}
		else
		{
			if (ret.length() > 0)
			{
				ret.deleteCharAt(ret.length() - 1);
			}
		}
	}
	return ret.toString();
}
```

#### [859. 亲密字符串](https://leetcode.cn/problems/buddy-strings/)

@2023.07.04

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean buddyStrings(String s, String goal)
{
	if (s.equals(goal))
	{
		int len = s.length();
		int[] counts = new int[26];
		for (int i = 0; i < len; i++)
		{
			int pos = s.charAt(i) - 'a';
			if (counts[pos] == 1) return true;
			counts[pos]++;
		}
		return false;
	}
	else
	{
		int len1 = s.length(), len2 = goal.length();
		if (len1 != len2) return false;
		int v = 0;
		for (int i = 0; i < len1; i++)
		{
			if (s.charAt(i) != goal.charAt(i))
			{
				if (v == 1) return false;
				v = 1;
				if (i == len1 - 1) return false;
				i++;
				int j = i;
				while (j < len1 && s.charAt(j) == goal.charAt(j)) j++;
				if (j == len1 || s.charAt(i - 1) != goal.charAt(j) || s.charAt(j) != goal.charAt(i - 1))
					return false;
				i = j;
			}
		}
	}
	return true;
}
```

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public boolean buddyStrings(String s, String goal)
{
	if (s.length() != goal.length())
	{
		return false;
	}
	if (s.equals(goal))
	{
		int[] count = new int[26];
		for (int i = 0; i < s.length(); i++)
		{
			count[s.charAt(i) - 'a']++;
			if (count[s.charAt(i) - 'a'] > 1)
			{
				return true;
			}
		}
		return false;
	}
	else
	{
		int first = -1, second = -1;
		for (int i = 0; i < goal.length(); i++)
		{
			if (s.charAt(i) != goal.charAt(i))
			{
				if (first == -1) first = i;
				else if (second == -1) second = i;
				else return false;
			}
		}
		return (second != -1 && s.charAt(first) == goal.charAt(second) && s.charAt(second) == goal.charAt(first));
	}
}
```

#### [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

@2023.07.04

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean lemonadeChange(int[] bills)
{
	int[] change = new int[2];
	for (int bill : bills)
	{
		if (bill == 5) change[0]++;
		else if (bill == 10)
		{
			change[0]--;
			if (change[0] < 0) return false;
			change[1]++;
		}
		else
		{
			if (change[1] >= 1 && change[0] >= 1)
			{
				change[1]--;
				change[0]--;
			}
			else if (change[0] >= 3) change[0] -= 3;
			else return false;
		}
	}
	return true;
}
```

```java
/**
 * 贪心
 * 力扣官方题解
 */
public boolean lemonadeChange(int[] bills)
{
	int five = 0, ten = 0;
	for (int bill : bills)
	{
		if (bill == 5)
		{
			five++;
		}
		else if (bill == 10)
		{
			if (five == 0)
			{
				return false;
			}
			five--;
			ten++;
		}
		else
		{
			if (five > 0 && ten > 0)
			{
				five--;
				ten--;
			}
			else if (five >= 3)
			{
				five -= 3;
			}
			else
			{
				return false;
			}
		}
	}
	return true;
}
```

#### [867. 转置矩阵](https://leetcode.cn/problems/transpose-matrix/)

@2023.07.09

```java
/**
 * 模拟
 * Somnia1337
 */
public int[][] transpose(int[][] matrix)
{
	int m = matrix.length, n = matrix[0].length;
	int[][] ret = new int[n][m];
	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			ret[j][i] = matrix[i][j];
		}
	}
	return ret;
}
```

#### [868. 二进制间距](https://leetcode.cn/problems/binary-gap/)

@2023.07.04

1. 字符串 + 一次遍历

```java
/**
 * 字符串 + 一次遍历
 * Somnia1337
 */
public int binaryGap(int n)
{
	String s = Integer.toBinaryString(n);
	int slow = 0, fast = 1, len = s.length();
	int ans = 0;
	while (fast < len)
	{
		while (fast < len && s.charAt(fast) == '0') fast++;
		if (fast == len) return ans;
		else ans = Math.max(ans, fast - slow);
		slow = fast;
		fast++;
	}
	return ans;
}
```

优化：只需记录上一个'1'的位置即可。

```java
/**
 * 字符串 + 一次遍历
 * 陆小6
 */
public int binaryGap(int n)
{
	String binStr = Integer.toBinaryString(n);
	int lastOneIdx = n, max = 0;
	for (int i = 0; i < binStr.length(); i++)
	{
		if (binStr.charAt(i) == '1')
		{
			max = Math.max(max, i - lastOneIdx);
			lastOneIdx = i;
		}
	}
	return max;
}
```

2. 位运算

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int binaryGap(int n)
{
	int last = -1, ans = 0;
	for (int i = 0; n != 0; ++i)
	{
		if ((n & 1) == 1)
		{
			if (last != -1)
			{
				ans = Math.max(ans, i - last);
			}
			last = i;
		}
		n >>= 1;
	}
	return ans;
}
```

#### [883. 三维形体投影面积](https://leetcode.cn/problems/projection-area-of-3d-shapes/)

@2023.07.10

```java
/**
 * 数学
 * Somnia1337
 */
public int projectionArea(int[][] grid)
{
	int len = grid.length;
	int xCnt = 0, yCnt = 0, zCnt = len * len;
	for (int i = 0; i < len; i++)
	{
		int curXMax = 0, curYMax = 0;
		for (int j = 0; j < len; j++)
		{
			curXMax = Math.max(curXMax, grid[i][j]);
			curYMax = Math.max(curYMax, grid[j][i]);
			if (grid[i][j] == 0) zCnt--;
		}
		xCnt += curXMax;
		yCnt += curYMax;
	}
	return xCnt + yCnt + zCnt;
}
```

#### [884. 两句话中的不常见单词](https://leetcode.cn/problems/uncommon-words-from-two-sentences/)

@2023.07.04

```java
/**
 * 哈希表
 * Somnia1337
 */
public String[] uncommonFromSentences(String s1, String s2)
{
	String[] pieces1 = s1.split(" ");
	String[] pieces2 = s2.split(" ");
	HashMap<String, Integer> counts = new HashMap<>();
	for (String piece1 : pieces1)
	{
		if (counts.containsKey(piece1)) counts.put(piece1, 0);
		else counts.put(piece1, 1);
	}
	for (String piece2 : pieces2)
	{
		if (counts.containsKey(piece2)) counts.put(piece2, 0);
		else counts.put(piece2, 1);
	}
	ArrayList<String> list = new ArrayList<>();
	for (String key : counts.keySet())
		if (counts.get(key) == 1) list.add(key);
	return list.toArray(new String[0]);
}
```

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public String[] uncommonFromSentences(String s1, String s2)
{
	Map<String, Integer> freq = new HashMap<String, Integer>();
	insert(s1, freq);
	insert(s2, freq);
	List<String> ans = new ArrayList<String>();
	for (Map.Entry<String, Integer> entry : freq.entrySet())
	{
		if (entry.getValue() == 1)
		{
			ans.add(entry.getKey());
		}
	}
	return ans.toArray(new String[0]);
}

public void insert(String s, Map<String, Integer> freq)
{
	String[] arr = s.split(" ");
	for (String word : arr)
	{
		freq.put(word, freq.getOrDefault(word, 0) + 1);
	}
}
```

#### [888. 公平的糖果交换](https://leetcode.cn/problems/fair-candy-swap/)

@2023.07.04

```java
/**
 * 哈希表
 * Somnia1337
 */
public int[] fairCandySwap(int[] aliceSizes, int[] bobSizes)
{
	int sumA = 0, sumB = 0;
	HashSet<Integer> hashset = new HashSet<>();
	for (int a : aliceSizes) sumA += a;
	for (int b : bobSizes)
	{
		sumB += b;
		hashset.add(b);
	}
	int offset = (sumA + sumB) / 2 - sumA;
	for (int a : aliceSizes) if (hashset.contains(a + offset)) return new int[]{a, a + offset};
	return new int[]{-1, -1};
}
```

#### [892. 三维形体的表面积](https://leetcode.cn/problems/surface-area-of-3d-shapes/)

@2023.09.14

```java
/**
 * 直接计算
 * Somnia1337
 */
public int surfaceArea(int[][] grid) {
	int n = grid.length, ans = 0;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			int h = grid[i][j];
			if (h == 0) continue;
			ans += 2;
			int u = i - 1 >= 0 ? grid[i - 1][j] : 0;
			int d = i + 1 < n ? grid[i + 1][j] : 0;
			int l = j - 1 >= 0 ? grid[i][j - 1] : 0;
			int r = j + 1 < n ? grid[i][j + 1] : 0;
			ans += Math.max(h - u, 0);
			ans += Math.max(h - d, 0);
			ans += Math.max(h - l, 0);
			ans += Math.max(h - r, 0);
		}
	}
	return ans;
}
```

```java
/**
 * 直接计算
 * ?
 */
public int surfaceArea(int[][] grid) {
	int n = grid.length, ans = 0;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			int h = grid[i][j];
			if (h > 0) {
				ans += 4 * h + 2;
				if (i > 0) ans -= Math.min(grid[i - 1][j], h) * 2;
				if (j > 0) ans -= Math.min(grid[i][j - 1], h) * 2;
			}
		}
	}
	return ans;
}
```

#### [896. 单调数列](https://leetcode.cn/problems/monotonic-array/)

@2023.07.05

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isMonotonic(int[] nums)
{
	int len = nums.length;
	int i = 0;
	while (i < len - 1 && nums[i + 1] == nums[i]) i++;
	if (i == len - 1) return true;
	else if (nums[i + 1] < nums[i])
	{
		while (i < len - 1)
		{
			if (nums[i + 1] > nums[i]) return false;
			i++;
		}
		return true;
	}
	else
	{
		while (i < len - 1)
		{
			if (nums[i + 1] < nums[i]) return false;
			i++;
		}
		return true;
	}
}
```

优化：定义两个布尔值，初始化为true，分别在递增、递减时置false，一次遍历后只要有一个为true就说明单调。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public boolean isMonotonic(int[] nums)
{
	boolean inc = true, dec = true;
	int n = nums.length;
	for (int i = 0; i < n - 1; ++i)
	{
		if (nums[i] > nums[i + 1])
		{
			inc = false;
		}
		if (nums[i] < nums[i + 1])
		{
			dec = false;
		}
	}
	return inc || dec;
}
```

#### [905. 按奇偶排序数组](https://leetcode.cn/problems/sort-array-by-parity/)

@2023.07.05

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] sortArrayByParity(int[] nums)
{
	int len = nums.length;
	int left = 0, right = len - 1;
	while (left < right)
	{
		while (left < right && nums[left] % 2 == 0) left++;
		while (right > left && nums[right] % 2 != 0) right--;
		if (left == right) break;
		swap(nums, left, right);
		left++;
		right--;
	}
	return nums;
}

public void swap(int[] nums, int i, int j)
{
	int temp = nums[i];
	nums[i] = nums[j];
	nums[j] = temp;
}
```

#### [908. 最小差值 I](https://leetcode.cn/problems/smallest-range-i/)

@2023.07.06

```java
/**
 * 数学
 * Somnia1337
 */
public int smallestRangeI(int[] nums, int k)
{
	int min = nums[0], max = nums[0];
	for (int num : nums)
	{
		min = Math.min(min, num);
		max = Math.max(max, num);
	}
	return Math.max((max - k) - (min + k), 0);
}
```

#### [914. 卡牌分组](https://leetcode.cn/problems/x-of-a-kind-in-a-deck-of-cards/)

@2023.07.07

1. 最小公约数 + 哈希表

```java
/**
 * 最小公约数 + 哈希表
 * Somnia1337
 */
public boolean hasGroupsSizeX(int[] deck)
{
	HashMap<Integer, Integer> counts = new HashMap<>();
	for (int card : deck)
		counts.merge(card, 1, Integer::sum);
	Set<Integer> keySet = counts.keySet();
	int minCnt = counts.get(deck[0]), secMinCnt = counts.get(deck[0]);
	for (int key : keySet)
	{
		int cur = counts.get(key);
		if (cur < minCnt)
		{
			secMinCnt = minCnt;
			minCnt = cur;
		}
		else if (cur < secMinCnt) secMinCnt = cur;
	}
	int scd = scd(minCnt, secMinCnt);
	if (scd < 2) return false;
	for (int key : keySet)
		if (counts.get(key) % scd != 0) return false;
	return true;
}

public int scd(int a, int b)
{
	for (int i = 2; i <= Math.min(a, b); i++)
	{
		if (a % i == 0 && b % i == 0) return i;
	}
	return 1;
}
```

2. 最大公约数

```java
/**
 * 最大公约数
 * 力扣官方题解
 */
public boolean hasGroupsSizeX(int[] deck)
{
	int[] count = new int[10000];
	for (int c : deck)
	{
		count[c]++;
	}
	int g = -1;
	for (int i = 0; i < 10000; ++i)
	{
		if (count[i] > 0)
		{
			if (g == -1)
			{
				g = count[i];
			}
			else
			{
				g = gcd(g, count[i]);
			}
		}
	}
	return g >= 2;
}

public int gcd(int x, int y)
{
	return x == 0 ? y : gcd(y % x, x);
}
```

#### [917. 仅仅反转字母](https://leetcode.cn/problems/reverse-only-letters/)

@2023.07.05

```java
/**
 * 双指针
 * Somnia1337
 */
public String reverseOnlyLetters(String s)
{
	int len = s.length();
	int left = 0, right = len - 1;
	char[] sChars = s.toCharArray();
	while (left < right)
	{
		while (left < right && !Character.isLetter(sChars[left])) left++;
		while (right > left && !Character.isLetter(sChars[right])) right--;
		if (left == right) break;
		swap(sChars, left, right);
		left++;
		right--;
	}
	return new String(sChars);
}

public void swap(char[] nums, int i, int j)
{
	char temp = nums[i];
	nums[i] = nums[j];
	nums[j] = temp;
}
```

#### [922. 按奇偶排序数组 II](https://leetcode.cn/problems/sort-array-by-parity-ii/)

@2023.07.05

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] sortArrayByParityII(int[] nums)
{
	int len = nums.length;
	int pEven = 0, pOdd = 1;
	while (pEven < len && pOdd < len)
	{
		while (pEven < len && nums[pEven] % 2 == 0) pEven += 2;
		while (pOdd < len && nums[pOdd] % 2 != 0) pOdd += 2;
		if (pEven >= len || pOdd >= len) break;
		swap(nums, pEven, pOdd);
		pEven += 2;
		pOdd += 2;
	}
	return nums;
}

public void swap(int[] nums, int i, int j)
{
	int temp = nums[i];
	nums[i] = nums[j];
	nums[j] = temp;
}
```

#### [925. 长按键入](https://leetcode.cn/problems/long-pressed-name/)

@2023.07.06

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean isLongPressedName(String name, String typed)
{
	int len1 = name.length(), len2 = typed.length();
	int i, j;
	for (i = 0, j = 0; i < len1 && j < len2; i++, j++)
	{
		char c = name.charAt(i);
		int cnt1 = 1, cnt2 = 1;
		while (i < len1 - 1 && name.charAt(i + 1) == c)
		{
			i++;
			cnt1++;
		}
		if (typed.charAt(j) != c) return false;
		while (j < len2 - 1 && typed.charAt(j + 1) == c)
		{
			j++;
			cnt2++;
		}
		if (cnt2 < cnt1) return false;
	}
	return i == len1 && j == len2;
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean isLongPressedName(String name, String typed)
{
	int i = 0, j = 0;
	while (j < typed.length())
	{
		if (i < name.length() && name.charAt(i) == typed.charAt(j))
		{
			i++;
			j++;
		}
		else if (j > 0 && typed.charAt(j) == typed.charAt(j - 1))
		{
			j++;
		}
		else
		{
			return false;
		}
	}
	return i == name.length();
}
```

#### [929. 独特的电子邮件地址](https://leetcode.cn/problems/unique-email-addresses/)

@2023.07.06

```java
/**
 * 哈希表
 * Somnia1337
 */
public int numUniqueEmails(String[] emails)
{
	HashSet<String> uniqueEmails = new HashSet<>();
	for (String email : emails)
	{
		int len = email.length();
		StringBuilder validSegments = new StringBuilder();
		for (int i = 0; i < len; i++)
		{
			char c = email.charAt(i);
			if (c == '@')
			{
				validSegments.append(email.substring(i));
				break;
			}
			else if (c == '.') continue;
			else if (c == '+') while (email.charAt(i + 1) != '@') i++;
			else validSegments.append(c);
		}
		uniqueEmails.add(validSegments.toString());
	}
	return uniqueEmails.size();
}
```

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public int numUniqueEmails(String[] emails)
{
	Set<String> emailSet = new HashSet<String>();
	for (String email : emails)
	{
		int i = email.indexOf('@');
		String local = email.substring(0, i)
							.split("\\+")[0]; //去掉本地名第一个加号之后的部分
		local = local.replace(".", ""); //去掉本地名中所有的句点
		emailSet.add(local + email.substring(i));
	}
	return emailSet.size();
}
```

#### [941. 有效的山脉数组](https://leetcode.cn/problems/valid-mountain-array/)

@2023.07.06

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean validMountainArray(int[] arr)
{
	int len = arr.length;
	if (len < 3) return false;
	int i = 0;
	while (i < len - 1 && arr[i + 1] > arr[i]) i++;
	if (i == 0 || i == len - 1) return false;
	while (i < len - 1 && arr[i + 1] < arr[i]) i++;
	return i == len - 1;
}
```

#### [942. 增减字符串匹配](https://leetcode.cn/problems/di-string-match/)

@2023.07.06

1. 双指针 + 两次遍历

```java
/**
 * 双指针 + 两次遍历
 * Somnia1337
 */
public int[] diStringMatch(String s)
{
	int len = s.length();
	int start = 0;
	for (int i = 0; i < len; i++) if (s.charAt(i) == 'D') start++;
	int left = start - 1, right = start + 1;
	int[] ret = new int[len + 1];
	ret[0] = start;
	for (int i = 1; i <= len; i++)
	{
		if (s.charAt(i - 1) == 'D') ret[i] = left--;
		else ret[i] = right++;
	}
	return ret;
}
```

2. 双指针 + 贪心

```java
/**
 * 双指针 + 贪心
 * 力扣官方题解
 */
public int[] diStringMatch(String s)
{
	int n = s.length(), lo = 0, hi = n;
	int[] perm = new int[n + 1];
	for (int i = 0; i < n; ++i)
	{
		perm[i] = s.charAt(i) == 'I' ? lo++ : hi--;
	}
	perm[n] = lo; //最后剩下一个数，此时lo=hi
	return perm;
}
```

#### [944. 删列造序](https://leetcode.cn/problems/delete-columns-to-make-sorted/)

@2023.07.06

```java
/**
 * 纵向扫描
 * Somnia1337
 */
public int minDeletionSize(String[] strs)
{
	int n = strs[0].length();
	int cnt = 0;
	for (int i = 0; i < n; i++)
	{
		char c = strs[0].charAt(i);
		for (String str : strs)
		{
			if (str.charAt(i) - c < 0)
			{
				cnt++;
				break;
			}
			c = str.charAt(i);
		}
	}
	return cnt;
}
```

#### [953. 验证外星语词典](https://leetcode.cn/problems/verifying-an-alien-dictionary/)

@2023.07.06

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isAlienSorted(String[] words, String order)
{
	int len = words.length;
	String lastWord = words[0];
	for (int i = 1; i < len; i++)
	{
		String curWord = words[i];
		int lastLen = lastWord.length(), curLen = curWord.length();
		for (int j = 0; j < Math.min(lastLen, curLen); j++)
		{
			char lastChar = lastWord.charAt(j), curChar = curWord.charAt(j);
			if (order.indexOf(lastChar) > order.indexOf(curChar)) return false;
			else if (order.indexOf(lastChar) < order.indexOf(curChar)) break;
			else if (j == curLen - 1 && j < lastLen - 1) return false;
		}
		lastWord = curWord;
	}
	return true;
}
```

#### [961. 在长度 2N 的数组中找出重复 N 次的元素](https://leetcode.cn/problems/n-repeated-element-in-size-2n-array/)

@2023.07.06

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public int repeatedNTimes(int[] nums)
{
	int n = nums.length / 2;
	Arrays.sort(nums);
	return nums[n] == nums[n + 1] ? nums[n] : nums[n - 1];
}
```

2. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int repeatedNTimes(int[] nums)
{
	HashSet<Integer> hashset = new HashSet<>();
	for (int num : nums)
		if (!hashset.add(num)) return num;
	return 0;
}
```

#### [976. 三角形的最大周长](https://leetcode.cn/problems/largest-perimeter-triangle/)

@2023.07.06

```java
/**
 * 排序 + 贪心
 * Somnia1337
 */
public int largestPerimeter(int[] nums)
{
	int len = nums.length;
	Arrays.sort(nums);
	for (int i = len - 3; i >= 0; i--)
		if (nums[i] + nums[i + 1] > nums[i + 2]) return nums[i] + nums[i + 1] + nums[i + 2];
	return 0;
}
```

#### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

@2023.07.09

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] sortedSquares(int[] nums) {
	int i = nums.length, l = 0, r = i - 1;
	int[] ans = new int[nums.length];
	while (--i >= 0) {
		if (Math.abs(nums[l]) > Math.abs(nums[r])) ans[i] = nums[l] * nums[l++];
		else ans[i] = nums[r] * nums[r--];
	}
	return ans;
}
```

#### [989. 数组形式的整数加法](https://leetcode.cn/problems/add-to-array-form-of-integer/)

@2023.09.06

力扣第 300 题之世纪攻坚战。

![[Snipaste_230906_202939.png|300]]

最终的成绩也令人欣喜，runtime 前 1.23%。

![[Snipaste_230906_203007.png|400]]

![[Snipaste_230906_203021.png|400]]

```java
/**
 * 模拟
 * Somnia1337
 */
public List<Integer> addToArrayForm(int[] num, int k) {
	int i = num.length - 1, c = 0;
	while (k > 0 && i >= 0) {
		num[i] += k % 10 + c;
		c = num[i] / 10;
		num[i] %= 10;
		k /= 10;
		i--;
	}
	while (i >= 0) {
		num[i] += c;
		c = num[i] / 10;
		num[i] %= 10;
		i--;
	}
	
	List<Integer> ans = new ArrayList<>();
	k += c;
	if (k > 0) {
		Deque<Integer> t = new ArrayDeque<>();
		while (k > 0) {
			t.add(k % 10);
			k /= 10;
		}
		while (!t.isEmpty()) ans.add(t.pollLast());
	}
	for (int d : num) ans.add(d);
	return ans;
}
```

官解好简洁。

```java
/**
 * 模拟
 * 力扣官方题解
 */
public List<Integer> addToArrayForm(int[] num, int k) {
	List<Integer> ans = new ArrayList<>();
	for (int i = num.length - 1; i >= 0; i--) {
		int sum = num[i] + k % 10;
		k /= 10;
		if (sum >= 10) {
			k++;
			sum -= 10;
		}
		ans.add(sum);
	}
	while (k > 0) {
		ans.add(k % 10);
		k /= 10;
	}
	Collections.reverse(ans);
	return ans;
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public List<Integer> addToArrayForm(int[] num, int k) {
	List<Integer> res = new ArrayList<>();
	for (int i = num.length - 1; i >= 0 || k > 0; i--, k /= 10) {
		if (i >= 0) k += num[i];
		res.add(k % 10);
	}
	Collections.reverse(res);
	return res;
}
```

#### [997. 找到小镇的法官](https://leetcode.cn/problems/find-the-town-judge/)

@2023.07.06

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int findJudge(int n, int[][] trust)
{
	int len = trust.length;
	if (len == 0) return n == 1 ? 1 : -1;
	HashSet<Integer> banned = new HashSet<>();
	HashMap<Integer, Integer> trustCnt = new HashMap<>();
	for (int i = 0; i < len; i++)
	{
		banned.add(trust[i][0]);
		trustCnt.remove(trust[i][0]);
		if (!banned.contains(trust[i][1])) trustCnt.merge(trust[i][1], 1, Integer::sum);
	}
	for (int key : trustCnt.keySet()) if (trustCnt.get(key) == n - 1) return key;
	return -1;
}
```

2. 图

```java
/**
 * 图
 * 力扣官方题解
 */
public int findJudge(int n, int[][] trust)
{
	int[] inDegrees = new int[n + 1];
	int[] outDegrees = new int[n + 1];
	for (int[] edge : trust)
	{
		int x = edge[0], y = edge[1];
		++inDegrees[y];
		++outDegrees[x];
	}
	for (int i = 1; i <= n; ++i)
	{
		if (inDegrees[i] == n - 1 && outDegrees[i] == 0)
		{
			return i;
		}
	}
	return -1;
}
```

```java
/**
 * 图
 * newzhu514
 */
public int findJudge(int N, int[][] trust)
{
	int[] trusted = new int[N];
	for (int[] arr : trust)
	{
		trusted[arr[0] - 1]--;
		trusted[arr[1] - 1]++;
	}
	for (int i = 0; i < N; i++)
	{
		if (trusted[i] == N - 1)
		{
			return i + 1;
		}
	}
	return -1;
}
```

#### [999. 可以被一步捕获的棋子数](https://leetcode.cn/problems/available-captures-for-rook/)

@2023.07.10

```java
/**
 * 模拟
 * Somnia1337
 */
public int numRookCaptures(char[][] board)
{
	int x = 0, y = 0;
	int ans = 4;
	for (int i = 0; i < 8; i++)
	{
		for (int j = 0; j < 8; j++)
		{
			if (board[i][j] == 'R')
			{
				x = i;
				y = j;
				break;
			}
		}
	}
	int cur = x - 1;
	while (cur >= 0 && board[cur][y] == '.') cur--;
	if (cur < 0 || board[cur][y] != 'p') ans--;
	cur = x + 1;
	while (cur < 8 && board[cur][y] == '.') cur++;
	if (cur == 8 || board[cur][y] != 'p') ans--;
	cur = y - 1;
	while (cur >= 0 && board[x][cur] == '.') cur--;
	if (cur < 0 || board[x][cur] != 'p') ans--;
	cur = y + 1;
	while (cur < 8 && board[x][cur] == '.') cur++;
	if (cur == 8 || board[x][cur] != 'p') ans--;
	return ans;
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public int numRookCaptures(char[][] board)
{
	int cnt = 0, st = 0, ed = 0;
	int[] dx = {0, 1, 0, -1};
	int[] dy = {1, 0, -1, 0};
	for (int i = 0; i < 8; ++i)
	{
		for (int j = 0; j < 8; ++j)
		{
			if (board[i][j] == 'R')
			{
				st = i;
				ed = j;
				break;
			}
		}
	}
	for (int i = 0; i < 4; ++i)
	{
		for (int step = 0; ; ++step)
		{
			int tx = st + step * dx[i];
			int ty = ed + step * dy[i];
			if (tx < 0 || tx >= 8 || ty < 0 || ty >= 8 || board[tx][ty] == 'B')
			{
				break;
			}
			if (board[tx][ty] == 'p')
			{
				cnt++;
				break;
			}
		}
	}
	return cnt;
}
```

#### [1002. 查找共用字符](https://leetcode.cn/problems/find-common-characters/)

@2023.07.07

```java
/**
 * 字符统计
 * Somnia1337
 */
public List<String> commonChars(String[] words)
{
	int[] minCnt = new int[26];
	for (int i = 0; i < words[0].length(); i++) minCnt[words[0].charAt(i) - 'a']++;
	for (int i = 1; i < words.length; i++)
	{
		int[] curCnt = new int[26];
		for (int j = 0; j < words[i].length(); j++) curCnt[words[i].charAt(j) - 'a']++;
		for (int k = 0; k < 26; k++)
			minCnt[k] = Math.min(minCnt[k], curCnt[k]);
	}
	ArrayList<String> ret = new ArrayList<>();
	for (int k = 0; k < 26; k++)
		while (minCnt[k] > 0)
		{
			ret.add(String.valueOf((char) ('a' + k)));
			minCnt[k]--;
		}
	return ret;
}
```

#### [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

@2023.07.07

```java
/**
 * 排序 + 贪心
 * Somnia1337
 */
public int largestSumAfterKNegations(int[] nums, int k)
{
	Arrays.sort(nums);
	int len = nums.length;
	int negativeCnt = 0;
	int i, sum = 0;
	for (i = 0; i < len && nums[i] < 0; i++) negativeCnt++;
	i = 0;
	while (i < len && negativeCnt > 0 && k > 0)
	{
		sum -= nums[i++];
		negativeCnt--;
		k--;
	}
	if (negativeCnt > 0 || k % 2 == 0)
	{
		while (i < len) sum += nums[i++];
		return sum;
	}
	else
	{
		int min = Integer.MAX_VALUE;
		while (i < len) sum += nums[i++];
		for (i = 0; i < len; i++) min = Math.min(min, Math.abs(nums[i]));
		return sum - 2 * min;
	}
}
```

```java
/**
 * 排序 + 贪心
 * Yipsen
 * @improver Somnia1337
 */
public int largestSumAfterKNegations(int[] nums, int k)
{
	Arrays.sort(nums);
	int sum = 0, min = Integer.MAX_VALUE;
	for (int num : nums)
	{
		if (num < 0 && k > 0)
		{
			sum -= num;
			k--;
		}
		else sum += num;
		min = Math.min(min, Math.abs(num));
	}
	return sum - (k % 2 == 0 ? 0 : 2 * min);
}
```

#### [1009. 十进制整数的反码](https://leetcode.cn/problems/complement-of-base-10-integer/)

@2023.07.07

可以将n与位数相同的全1串$(11\cdots1)_2$异或。

```java
/**
 * 位运算
 * Somnia1337
 */
public int bitwiseComplement(int n)
{
	int len = Integer.toBinaryString(n)
					 .length();
	String s = "1".repeat(len);
	return Integer.parseInt(s, 2) ^ n;
}
```

优化：使用位左移运算符与循环+1构造全1串。

```java
/**
 * 位运算
 * 乀无名指的等待乀
 */
public int bitwiseComplement(int N)
{
	int num = 1;
	while (num < N)
	{
		num = (num << 1) + 1;
	}
	return num ^ N;
}
```

或者使用方法Integer.numberOfLeadingZeros。

```java
/**
 * 位运算
 * nieyuanhong
 */
public int bitwiseComplement(int n)
{
	return n == 0 ? 1 : n ^ ((1 << 32 - Integer.numberOfLeadingZeros(n)) - 1);
}
```

#### [1013. 将数组分成和相等的三个部分](https://leetcode.cn/problems/partition-array-into-three-parts-with-equal-sum/)

@2023.07.07

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean canThreePartsEqualSum(int[] arr)
{
	int sum = 0;
	for (int num : arr) sum += num;
	if (sum % 3 != 0) return false;
	int segSum = sum / 3, len = arr.length;
	int left = 0, right = len - 1;
	int curSum = 0;
	while (left < right - 1)
	{
		curSum += arr[left];
		if (curSum == segSum) break;
		left++;
	}
	curSum = 0;
	while (right > left + 1)
	{
		curSum += arr[right];
		if (curSum == segSum) break;
		right--;
	}
	return left + 1 != right;
}
```

#### [1018. 可被 5 整除的二进制前缀](https://leetcode.cn/problems/binary-prefix-divisible-by-5/)

@2023.07.11

遍历时只需保留对5的余数。

```java
/**
 * 一次遍历
 * 力扣官方题解
 * @improver Somnia1337
 */
public List<Boolean> prefixesDivBy5(int[] nums)
{
	List<Boolean> answer = new ArrayList<Boolean>();
	int rem = 0;
	int length = nums.length;
	for (int num : nums)
	{
		rem = (rem * 2 + num) % 5;
		answer.add(rem == 0);
	}
	return answer;
}
```

#### [1021. 删除最外层的括号](https://leetcode.cn/problems/remove-outermost-parentheses/)

@2023.09.07

```java
/**
 * 栈
 * Somnia1337
 */
public String removeOuterParentheses(String s) {
	Deque<Integer> stk = new ArrayDeque<>();
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < s.length(); i++) {
		int j = 0;
		if (s.charAt(i) == '(') stk.push(i);
		else j = stk.pop();
		if (stk.isEmpty()) ans.append(s, j + 1, i);
	}
	return ans.toString();
}
```

优化：用计数模拟栈，将 `'('` 记为 `1`，`')'` 记为 `-1`，遍历累加，每当和为 `0` 时说明此前为一元组。

```java
/**
 * 栈（模拟）
 * 力扣官方题解
 */
public String removeOuterParentheses(String s) {
	int d = 0;
	StringBuilder ans = new StringBuilder();
	for (char c : s.toCharArray()) {
		if (c == ')') d--;
		if (d > 0) ans.append(c);
		if (c == '(') d++;
	}
	return ans.toString();
}
```

#### [1025. 除数博弈](https://leetcode.cn/problems/divisor-game/)

@2023.07.07

```java
/**
 * 找规律
 * Somnia1337
 */
public boolean divisorGame(int n)
{
	return n % 2 == 0;
}
```

#### [1037. 有效的回旋镖](https://leetcode.cn/problems/valid-boomerang/)

@2023.07.09

这谁蚌得住…

![[Snipaste_ 230709_223221.png|150]]

```java
/**
 * 数学
 * Somnia1337
 */
public boolean isBoomerang(int[][] points)
{
	double x1 = points[0][0], y1 = points[0][1];
	double x2 = points[1][0], y2 = points[1][1];
	double x3 = points[2][0], y3 = points[2][1];
	if (x1 == x2 && x2 == x3 || y1 == y2 && y2 == y3 || x1 == x2 && y1 == y2 || x1 == x3 && y1 == y3 || x2 == x3 && y2 == y3) return false;
	double k1 = x3 - x1 != 0 ? (y3 - y1) / (x3 - x1) : Double.MAX_VALUE;
	double k2 = x2 - x1 != 0 ? (y2 - y1) / (x2 - x1) : Double.MAX_VALUE;
	return k1 != k2;
}
```

$\vec{P_1P_2}$ 与 $\vec{P_2P_3}$ 不共线 <=> $\vec{P_1P_2} \times \vec{P_2P_3} \neq 0$。

```java
/**
 * 数学
 * 力扣官方题解
 */
public boolean isBoomerang(int[][] points)
{
	int[] v1 = {points[1][0] - points[0][0], points[1][1] - points[0][1]};
	int[] v2 = {points[2][0] - points[0][0], points[2][1] - points[0][1]};
	return v1[0] * v2[1] - v1[1] * v2[0] != 0;
}
```

#### [1046. 最后一块石头的重量](https://leetcode.cn/problems/last-stone-weight/)

@2023.07.09

1. 模拟

反复排序效率这么高？

![[Snipaste_ 230709_230613.png]]

```java
/**
 * 模拟
 * Somnia1337
 */
public int lastStoneWeight(int[] stones) {
	int len = stones.length;
	if (len == 1) return stones[0];
	else if (len == 2) return Math.abs(stones[0] - stones[1]);
	while (true) {
		Arrays.sort(stones);
		if (stones[len - 2] == 0) return stones[len - 1];
		stones[len - 1] -= stones[len - 2];
		stones[len - 2] = 0;
	}
}
```

2. 最大堆

```java
/**
 * 最大堆
 * 力扣官方题解
 */
public int lastStoneWeight(int[] stones) {
	Queue<Integer> pq = new PriorityQueue<Integer>((a, b) -> b - a);
	for (int stone : stones) {
		pq.offer(stone);
	}
	while (pq.size() > 1) {
		int a = pq.poll();
		int b = pq.poll();
		if (a > b) pq.offer(a - b);
	}
	return pq.isEmpty() ? 0 : pq.poll();
}
```

#### [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

@2023.07.09

```java
/**
 * 栈
 * Somnia1337
 */
public String removeDuplicates(String s)
{
	char[] chars = s.toCharArray();
	Stack<Character> stack = new Stack<>();
	for (int i = 0; i < chars.length; i++)
	{
		while (i < chars.length && !stack.isEmpty() && stack.peek() == chars[i])
		{
			stack.pop();
			i++;
		}
		if (i == chars.length) break;
		stack.push(chars[i]);
	}
	StringBuilder sb = new StringBuilder();
	int size = stack.size();
	for (int i = 0; i < size; i++)
	{
		sb.append(stack.pop());
	}
	return sb.reverse()
			 .toString();
}
```

优化：虚拟栈。

```java
/**
 * 栈
 * 力扣官方题解
 * @improver Somnia1337
 */
public String removeDuplicates(String s)
{
	StringBuilder stack = new StringBuilder();
	int top = -1;
	for (int i = 0; i < s.length(); ++i)
	{
		char ch = s.charAt(i);
		if (top >= 0 && stack.charAt(top) == ch)
		{
			stack.deleteCharAt(top);
			--top;
		}
		else
		{
			stack.append(ch);
			++top;
		}
	}
	return stack.toString();
}
```

#### [1051. 高度检查器](https://leetcode.cn/problems/height-checker/)

@2023.07.07

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public int heightChecker(int[] heights)
{
	int[] sorted = Arrays.copyOf(heights, heights.length);
	Arrays.sort(sorted);
	int ans = 0;
	for (int i = 0; i < heights.length; i++)
	{
		if (heights[i] != sorted[i])
		{
			ans++;
		}
	}
	return ans;
}
```

2. 统计

```java
/**
 * 统计
 * 力扣官方题解
 */
public int heightChecker(int[] heights)
{
	int m = Arrays.stream(heights)
				  .max()
				  .getAsInt();
	int[] cnt = new int[m + 1];
	for (int h : heights)
	{
		++cnt[h];
	}
	int idx = 0, ans = 0;
	for (int i = 1; i <= m; ++i)
	{
		for (int j = 1; j <= cnt[i]; ++j)
		{
			if (heights[idx] != i)
			{
				++ans;
			}
			++idx;
		}
	}
	return ans;
}
```

#### [1071. 字符串的最大公因子](https://leetcode.cn/problems/greatest-common-divisor-of-strings/)

@2023.07.07

1. 枚举

```java
/**
 * 枚举
 * Somnia1337
 */
public String gcdOfStrings(String str1, String str2)
{
	if (str1.length() > str2.length())
	{
		swap(str1, str2);
	}
	int len1 = str1.length(), len2 = str2.length();
	if (str2.equals(str1.repeat(len2 / len1))) return str1;
	for (int i = len1 / 2; i >= 1; i--)
	{
		if (len1 % i != 0) continue;
		String sub = str1.substring(0, i);
		if (str1.equals(sub.repeat(len1 / i)) && str2.equals(sub.repeat(len2 / i))) return sub;
	}
	return "";
}

public void swap(String s1, String s2)
{
	String temp = s1;
	s1 = s2;
	s2 = temp;
}
```

2. 最大公约数

当且仅当`str1 + str2` = `str2 + str1`时存在最大公因子，判断其存在后，用辗转相除法计算两串长度的最大公约数，取该长度的子串即为最大公因子。

```java
/**
 * 最大公约数
 * 力扣官方题解
 */
public String gcdOfStrings(String str1, String str2)
{
	if (!str1.concat(str2)
			 .equals(str2.concat(str1)))
	{
		return "";
	}
	return str1.substring(0, gcd(str1.length(), str2.length()));
}

public int gcd(int a, int b)
{
	int remainder = a % b;
	while (remainder != 0)
	{
		a = b;
		b = remainder;
		remainder = a % b;
	}
	return b;
}
```

#### [1078. Bigram 分词](https://leetcode.cn/problems/occurrences-after-bigram/)

@2023.07.07

```java
/**
 * 一次遍历
 * Somnia1337
 */
public String[] findOcurrences(String text, String first, String second)
{
	String[] words = text.split(" ");
	int len = words.length;
	String[] ans = new String[len - 2];
	int j = 0;
	for (int i = 0; i < len - 2; i++)
	{
		if (words[i].equals(first) && words[i + 1].equals(second))
		{
			ans[j++] = words[i + 2];
		}
	}
	return Arrays.copyOfRange(ans, 0, j);
}
```

#### [1089. 复写零](https://leetcode.cn/problems/duplicate-zeros/)

@2023.07.11

```java
/**
 * 双指针
 * Somnia1337
 */
public void duplicateZeros(int[] arr)
{
	int len = arr.length;
	int left = 0, right = len - 1;
	while (left < len)
	{
		if (arr[left] == 0)
		{
			while (right >= left + 1)
			{
				arr[right] = arr[right - 1];
				right--;
			}
			left++;
			right = len - 1;
		}
		left++;
	}
}
```

优化：一次遍历找到最末端的元素，然后反向模拟。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public void duplicateZeros(int[] arr)
{
	int n = arr.length;
	int top = 0;
	int i = -1;
	while (top < n)
	{
		i++;
		if (arr[i] != 0)
		{
			top++;
		}
		else
		{
			top += 2;
		}
	}
	int j = n - 1;
	if (top == n + 1)
	{
		arr[j] = 0;
		j--;
		i--;
	}
	while (j >= 0)
	{
		arr[j] = arr[i];
		j--;
		if (arr[i] == 0)
		{
			arr[j] = arr[i];
			j--;
		}
		i--;
	}
}
```

#### [1103. 分糖果 II](https://leetcode.cn/problems/distribute-candies-to-people/)

@2023.07.08

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] distributeCandies(int candies, int num_people)
{
	int[] ret = new int[num_people];
	for (int i = 0; candies > 0; i++)
	{
		ret[i % num_people] += Math.min(candies, i + 1);
		candies -= (i + 1);
	}
	return ret;
}
```

2. 数学

```java
/**
 * 数学
 * 力扣官方题解
 */
public int[] distributeCandies(int candies, int num_people)
{
	int n = num_people;
	// how many people received complete gifts
	int p = (int) (Math.sqrt(2 * candies + 0.25) - 0.5);
	int remaining = (int) (candies - (p + 1) * p * 0.5);
	int rows = p / n, cols = p % n;
	int[] d = new int[n];
	for (int i = 0; i < n; ++i)
	{
		// complete rows
		d[i] = (i + 1) * rows + (int) (rows * (rows - 1) * 0.5) * n;
		// cols in the last row
		if (i < cols)
		{
			d[i] += i + 1 + rows * n;
		}
	}
	// remaining candies        
	d[cols] += remaining;
	return d;
}
```

#### [1108. IP 地址无效化](https://leetcode.cn/problems/defanging-an-ip-address/)

@2023.07.07

1. 构造字符串

```java
/**
 * 构造字符串
 * Somnia1337
 */
public String defangIPaddr(String address)
{
	StringBuilder sb = new StringBuilder();
	for (int i = 0; i < address.length(); i++)
	{
		char c = address.charAt(i);
		if (c != '.') sb.append(address.charAt(i));
		else sb.append("[.]");
	}
	return sb.toString();
}
```

2. 字符串操作

```java
/**
 * 字符串操作
 * 力扣官方题解
 */
public String defangIPaddr(String address)
{
	return address.replace(".", "[.]");
}
```

#### [1122. 数组的相对排序](https://leetcode.cn/problems/relative-sort-array/)

@2023.07.11

1. 模拟 + 双指针

```java
/**
 * 模拟 + 双指针
 * Somnia1337
 */
public int[] relativeSortArray(int[] arr1, int[] arr2)
{
	int len1 = arr1.length, len2 = arr2.length;
	int left = 0, right = 0;
	for (int cur : arr2)
	{
		right = left;
		while (right < len1)
		{
			while (left < len1 && arr1[left] == cur)
			{
				left++;
				right++;
			}
			while (right < len1 && arr1[right] != cur)
			{
				right++;
			}
			if (right == len1) break;
			swap(arr1, left, right);
			left++;
		}
	}
	if (left >= len1 - 1) return arr1;
	int[] leftOver = Arrays.copyOfRange(arr1, left, len1);
	Arrays.sort(leftOver);
	for (int n : leftOver)
	{
		arr1[left++] = n;
	}
	return arr1;
}

public void swap(int[] nums, int i, int j)
{
	int temp = nums[i];
	nums[i] = nums[j];
	nums[j] = temp;
}
```

2. 自定义排序

```java
/**
 * 自定义排序
 * 弹弹霹雳
 */
public int[] relativeSortArray(int[] arr1, int[] arr2)
{
	Map<Integer, Integer> map = new HashMap<>();
	List<Integer> list = new ArrayList<>();
	for (int num : arr1) list.add(num);
	for (int i = 0; i < arr2.length; i++) map.put(arr2[i], i);
	Collections.sort(list, (x, y) -> {
		if (map.containsKey(x) || map.containsKey(y)) return map.getOrDefault(x, 1001) - map.getOrDefault(y, 1001);
		return x - y;
	});
	for (int i = 0; i < arr1.length; i++) arr1[i] = list.get(i);
	return arr1;
}
```

3. 计数排序

```java
/**
 * 计数排序
 * 力扣官方题解
 */
public int[] relativeSortArray(int[] arr1, int[] arr2)
{
	int upper = 0;
	for (int x : arr1)
	{
		upper = Math.max(upper, x);
	}
	int[] frequency = new int[upper + 1];
	for (int x : arr1)
	{
		++frequency[x];
	}
	int[] ans = new int[arr1.length];
	int index = 0;
	for (int x : arr2)
	{
		for (int i = 0; i < frequency[x]; ++i)
		{
			ans[index++] = x;
		}
		frequency[x] = 0;
	}
	for (int x = 0; x <= upper; ++x)
	{
		for (int i = 0; i < frequency[x]; ++i)
		{
			ans[index++] = x;
		}
	}
	return ans;
}
```

#### [1128. 等价多米诺骨牌对的数量](https://leetcode.cn/problems/number-of-equivalent-domino-pairs/)

@2023.07.07

```java
/**
 * 统计
 * Somnia1337
 */
public int numEquivDominoPairs(int[][] dominoes)
{
	int[] counts = new int[100];
	int ans = 0;
	for (int[] card : dominoes)
	{
		int cur = (Math.min(card[0], card[1])) * 10 + Math.max(card[0], card[1]);
		ans += counts[cur]++;
	}
	return ans;
}
```

#### [1137. 第 N 个泰波那契数](https://leetcode.cn/problems/n-th-tribonacci-number/)

@2023.07.07

```java
/**
 * 动态规划
 * Somnia1337
 */
public int tribonacci(int n)
{
	if (n == 0) return 0;
	else if (n <= 2) return 1;
	int[] temp = new int[4];
	temp[0] = 0;
	temp[1] = 1;
	temp[2] = 1;
	temp[3] = 2;
	for (int i = 4; i <= n; i++)
	{
		int cur = temp[1] + temp[2] + temp[3];
		temp[0] = temp[1];
		temp[1] = temp[2];
		temp[2] = temp[3];
		temp[3] = cur;
	}
	return temp[3];
}
```

#### [1154. 一年中的第几天](https://leetcode.cn/problems/day-of-the-year/)

@2023.07.08

```java
/**
 * 直接计算
 * Somnia1337
 */
public int dayOfYear(String date)
{
	String[] y_m_d = date.split("-");
	int[] daysInAMonth = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
	int year = Integer.parseInt(y_m_d[0]), month = Integer.parseInt(y_m_d[1]), day = Integer.parseInt(y_m_d[2]);
	if (month == 1) return day;
	else if (month == 2) return 31 + day;
	else
	{
		daysInAMonth[1] = year % 4 == 0 && (year % 100 != 0 || year % 400 == 0) ? 29 : 28;
		int ans = 0;
		for (int i = 0; i < month - 1; i++)
		{
			ans += daysInAMonth[i];
		}
		return ans + day;
	}
}
```

#### [1160. 拼写单词](https://leetcode.cn/problems/find-words-that-can-be-formed-by-characters/)

@2023.07.08

```java
/**
 * 字符统计
 * Somnia1337
 */
public int countCharacters(String[] words, String chars)
{
	int[] available = new int[26];
	for (int i = 0; i < chars.length(); i++)
	{
		available[chars.charAt(i) - 'a']++;
	}
	int ans = 0;
	for (String word : words)
	{
		int[] count = new int[26];
		int len = word.length();
		for (int i = 0; i < len; i++)
		{
			count[word.charAt(i) - 'a']++;
		}
		ans += len;
		for (int i = 0; i < 26; i++)
		{
			if (count[i] > available[i])
			{
				ans -= len;
				break;
			}
		}
	}
	return ans;
}
```

#### [1185. 一周中的第几天](https://leetcode.cn/problems/day-of-the-week/)

@2023.07.11

```java
/**
 * 直接计算
 * Somnia1337
 */
public String dayOfTheWeek(int day, int month, int year) {
	int[] daysInMonth = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
	int d = day, y = 1971, m = 1;
	while (y < year) d = (d + daysInYear(y++)) % 7;
	while (m < month) {
		d += daysInMonth[m - 1];
		if (m++ == 2 && isLeap(year)) d++;
	}
	String[] days = {"Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday"};
	return days[d % 7];
}

private boolean isLeap(int year) {
	return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
}

private int daysInYear(int year) {
	return isLeap(year) ? 366 : 365;
}
```

#### [1189. “气球” 的最大数量](https://leetcode.cn/problems/maximum-number-of-balloons/)

@2023.07.11

```java
/**
 * 字符统计
 * Somnia1337
 */
public int maxNumberOfBalloons(String text)
{
	int[] counts = new int[26];
	for (char c : text.toCharArray())
	{
		counts[c - 'a']++;
	}
	int ans = 0;
	while (true)
	{
		counts['b' - 'a']--;
		counts[0]--;
		counts['l' - 'a'] -= 2;
		counts['o' - 'a'] -= 2;
		counts['n' - 'a']--;
		if (counts['b' - 'a'] < 0 || counts[0] < 0 || counts['l' - 'a'] < 0 || counts['o' - 'a'] < 0 || counts['n' - 'a'] < 0)
			return ans;
		ans++;
	}
}
```

#### [1200. 最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference/)

@2023.07.11

```java
/**
 * 排序 + 一次遍历
 * Somnia1337
 */
public List<List<Integer>> minimumAbsDifference(int[] arr)
{
	List<List<Integer>> ans = new ArrayList<>();
	Arrays.sort(arr);
	int minDelta = Integer.MAX_VALUE;
	for (int i = 0; i < arr.length - 1; i++)
	{
		int curDelta = arr[i + 1] - arr[i];
		if (curDelta <= minDelta)
		{
			if (curDelta < minDelta)
			{
				ans.clear();
				minDelta = curDelta;
			}
			List<Integer> temp = new ArrayList<>();
			temp.add(arr[i]);
			temp.add(arr[i + 1]);
			ans.add(temp);
		}
	}
	return ans;
}
```

#### [1207. 独一无二的出现次数](https://leetcode.cn/problems/unique-number-of-occurrences/)

@2023.07.11

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean uniqueOccurrences(int[] arr)
{
	Map<Integer, Integer> counts = new HashMap<>();
	Set<Integer> occurrences = new HashSet<>();
	for (int n : arr)
	{
		counts.merge(n, 1, Integer::sum);
	}
	for (int key : counts.keySet())
	{
		if (!occurrences.add(counts.get(key))) return false;
	}
	return true;
}
```

#### [1217. 玩筹码](https://leetcode.cn/problems/minimum-cost-to-move-chips-to-the-same-position/)

@2023.07.12

```java
/**
 * 贪心
 * Somnia1337
 */
public int minCostToMoveChips(int[] position)
{
	int oddCount = 0, evenCount = 0;
	for (int pos : position)
	{
		if (pos % 2 == 1) oddCount++;
		else evenCount++;
	}
	return Math.min(oddCount, evenCount);
}
```

#### [1221. 分割平衡字符串](https://leetcode.cn/problems/split-a-string-in-balanced-strings/)

@2023.07.11

```java
/**
 * 贪心
 * Somnia1337
 */
public int balancedStringSplit(String s)
{
	int len = s.length(), ans = 0;
	int curL = 0, curR = 0;
	for (char c : s.toCharArray())
	{
		if (c == 'L') curL++;
		else curR++;
		if (curL == curR)
		{
			ans++;
			curL = 0;
			curR = 0;
		}
	}
	return ans;
}
```

#### [1232. 缀点成线](https://leetcode.cn/problems/check-if-it-is-a-straight-line/)

@2023.07.11

```java
/**
 * 数学
 * Somnia1337
 */
public boolean checkStraightLine(int[][] coordinates)
{
	double k = coordinates[1][0] - coordinates[0][0] != 0 ?
			((double) coordinates[1][1] - (double) coordinates[0][1]) / ((double) coordinates[1][0] - (double) coordinates[0][0]) : Double.MAX_VALUE;
	for (int i = 2; i < coordinates.length; i++)
	{
		int[] point = coordinates[i];
		double curK = point[0] - coordinates[0][0] != 0 ?
				((double) point[1] - (double) coordinates[0][1]) / ((double) point[0] - (double) coordinates[0][0]) : Double.MAX_VALUE;
		if (curK != k) return false;
	}
	return true;
}
```

#### [1252. 奇数值单元格的数目](https://leetcode.cn/problems/cells-with-odd-values-in-a-matrix/)

@2023.07.12

```java
/**
 * 统计
 * Somnia1337
 */
public int oddCells(int m, int n, int[][] indices)
{
	int[] rowsCount = new int[m]; //记录每行的+1次数
	int[] columnCount = new int[n]; //记录每列的+1次数
	for (int[] point : indices)
	{
		rowsCount[point[0]]++;
		columnCount[point[1]]++;
	}
	int columnEvenCount = 0; //表示+1的次数为偶数的列的个数
	for (int column = 0; column < n; column++)
	{
		if (columnCount[column] % 2 == 0) columnEvenCount++;
	}
	int ans = 0;
	for (int row = 0; row < m; row++)
	{
		//如果当前行的+1次数为偶数，则加上+1的次数为偶数的列的个数，否则加上其补数
		ans += rowsCount[row] % 2 == 1 ? columnEvenCount : n - columnEvenCount;
	}
	return ans;
}
```

#### [1260. 二维网格迁移](https://leetcode.cn/problems/shift-2d-grid/)

@2023.07.14

```java
/**
 * 一次遍历
 * Somnia1337
 */
public List<List<Integer>> shiftGrid(int[][] grid, int k)
{
	List<List<Integer>> ans = new ArrayList<>();
	int rows = grid.length, columns = grid[0].length;
	int mod = Math.floorMod(columns * rows - k, columns * rows);
	int m = mod, n = mod;
	for (int i = 0; i < rows; i++)
	{
		List<Integer> temp = new ArrayList<>();
		for (int j = 0; j < columns; j++)
		{
			temp.add(grid[(m / columns) % rows][n % columns]);
			m++;
			n++;
		}
		ans.add(temp);
	}
	return ans;
}
```

#### [1266. 访问所有点的最小时间](https://leetcode.cn/problems/minimum-time-visiting-all-points/)

@2023.07.12

```java
/**
 * 贪心
 * Somnia1337
 */
public int minTimeToVisitAllPoints(int[][] points)
{
	int ans = 0;
	for (int i = 0; i < points.length - 1; i++)
	{
		ans += Math.max(Math.abs(points[i + 1][0] - points[i][0]), Math.abs(points[i + 1][1] - points[i][1]));
	}
	return ans;
}
```

#### [2670. 找出不同元素数目差数组](https://leetcode.cn/problems/find-the-distinct-difference-array/)

@2024.01.31



```java
/**
 * 前后缀分解
 * Somnia1337
 */
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

#### [1281. 整数的各位积和之差](https://leetcode.cn/problems/subtract-the-product-and-sum-of-digits-of-an-integer/)

@2023.07.11

```java
/**
 * 直接计算
 * Somnia1337
 */
public int subtractProductAndSum(int n)
{
	int sum = 0, product = 1;
	while (n > 0)
	{
		sum += n % 10;
		product *= n % 10;
		n /= 10;
	}
	return product - sum;
}
```

#### [1287. 有序数组中出现次数超过25%的元素](https://leetcode.cn/problems/element-appearing-more-than-25-in-sorted-array/)

@2023.07.11

```java
/**
 * 双指针
 * Somnia1337
 */
public int findSpecialInteger(int[] arr)
{
	int len = arr.length;
	int target = len / 4;
	for (int i = 0; i < len; )
	{
		int j = i;
		while (j < len && arr[j] == arr[i]) j++;
		if (j - i > target) return arr[i];
		i = j;
	}
	return -1;
}
```

#### [1295. 统计位数为偶数的数字](https://leetcode.cn/problems/find-numbers-with-even-number-of-digits/)

@2023.07.11

```java
/**
 * 枚举
 * Somnia1337
 */
public int findNumbers(int[] nums)
{
	int ans = 0;
	for (int num : nums)
	{
		if (num >= 10 && num < 100 || num >= 1000 && num < 10000 || num == 100000)
		{
			ans++;
		}
	}
	return ans;
}
```

#### [1299. 将每个元素替换为右侧最大元素](https://leetcode.cn/problems/replace-elements-with-greatest-element-on-right-side/)

@2023.07.11

1. 贪心

```java
/**
 * 贪心
 * Somnia1337
 */
public int[] replaceElements(int[] arr)
{
	int len = arr.length;
	int curMaxIndex = 0;
	for (int i = 0; i < len - 1; i++)
	{
		if (i < curMaxIndex) arr[i] = arr[curMaxIndex];
		else
		{
			int curMax = arr[i + 1];
			curMaxIndex = i + 1;
			for (int j = i + 1; j < len; j++)
			{
				if (arr[j] > curMax)
				{
					curMax = arr[j];
					curMaxIndex = j;
				}
			}
			arr[i] = arr[curMaxIndex];
		}
	}
	arr[len - 1] = -1;
	return arr;
}
```

2. 倒序遍历

```java
/**
 * 倒序遍历
 * 力扣官方题解
 * @translator ChatGPT
 */
public int[] replaceElements(int[] arr)
{
	int n = arr.length;
	int[] ans = new int[n];
	ans[n - 1] = -1;
	for (int i = n - 2; i >= 0; --i)
	{
		ans[i] = Math.max(ans[i + 1], arr[i + 1]);
	}
	return ans;
}
```

#### [1304. 和为零的 N 个不同整数](https://leetcode.cn/problems/find-n-unique-integers-sum-up-to-zero/)

@2023.07.12

```java
/**
 * 直接计算
 * Somnia1337
 */
public int[] sumZero(int n)
{
	int[] ret = new int[n];
	int mid = n / 2;
	for (int i = 0; i < n; i++)
	{
		ret[i] = i - mid;
		if (i == n / 2 - 1 && n % 2 == 0) mid--;
	}
	return ret;
}
```

#### [1309. 解码字母到整数映射](https://leetcode.cn/problems/decrypt-string-from-alphabet-to-integer-mapping/)

@2023.07.12

```java
/**
 * 倒序遍历
 * Somnia1337
 */
public String freqAlphabets(String s)
{
	StringBuilder ans = new StringBuilder();
	int len = s.length();
	for (int i = len - 1; i >= 0; i--)
	{
		if (s.charAt(i) == '#')
		{
			int n = (s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0');
			ans.append((char) ('a' + n));
			i -= 2;
		}
		else
		{
			ans.append((char) ('a' + s.charAt(i) - '1'));
		}
	}
	return ans.reverse()
			  .toString();
}
```

#### [1313. 解压缩编码列表](https://leetcode.cn/problems/decompress-run-length-encoded-list/)

@2023.07.12

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] decompressRLElist(int[] nums)
{
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < nums.length; i += 2)
	{
		for (int times = nums[i]; times > 0; times--)
		{
			ans.add(nums[i + 1]);
		}
	}
	int[] ret = new int[ans.size()];
	for (int i = 0; i < ans.size(); i++)
	{
		ret[i] = ans.get(i);
	}
	return ret;
}
```

#### [1317. 将整数转换为两个无零整数的和](https://leetcode.cn/problems/convert-integer-to-the-sum-of-two-no-zero-integers/)

@2023.07.13

```java
/**
 * 暴力
 * Somnia1337
 */
public int[] getNoZeroIntegers(int n)
{
	int i = n - 1, j = 1;
	while (i >= j)
	{
		String iString = String.valueOf(i);
		String jString = String.valueOf(j);
		if (iString.indexOf('0') == -1 && jString.indexOf('0') == -1) return new int[]{i, j};
		i--;
		j++;
	}
	return new int[]{};
}
```

#### [1323. 6 和 9 组成的最大数字](https://leetcode.cn/problems/maximum-69-number/)

@2023.07.13

```java
/**
 * 字符串
 * Somnia1337
 */
public int maximum69Number(int num)
{
	return Integer.parseInt(String.valueOf(num)
								  .replaceFirst("6", "9"));
}
```

#### [1331. 数组序号转换](https://leetcode.cn/problems/rank-transform-of-an-array/)

@2023.07.13

```java
public int[] arrayRankTransform(int[] arr)
{
	HashMap<Integer, Integer> positions = new HashMap<>();
	int[] sorted = Arrays.copyOf(arr, arr.length);
	Arrays.sort(sorted);
	int i = 1;
	for (int num : sorted)
	{
		if (!positions.containsKey(num))
		{
			positions.put(num, i++);
		}
	}
	for (i = 0; i < arr.length; i++)
	{
		arr[i] = positions.get(arr[i]);
	}
	return arr;
}
```

#### [1332. 删除回文子序列](https://leetcode.cn/problems/remove-palindromic-subsequences/)

@2023.07.14

```java
/**
 * 脑筋急转弯
 * Somnia1337
 */
public int removePalindromeSub(String s)
{
	int len = s.length();
	for (int i = 0; i < len / 2; i++)
	{
		if (s.charAt(i) != s.charAt(len - i - 1)) return 2;
	}
	return 1;
}
```

#### [1337. 矩阵中战斗力最弱的 K 行](https://leetcode.cn/problems/the-k-weakest-rows-in-a-matrix/)

@2023.07.14

```java
/**
 * 标记数组
 * Somnia1337
 */
public int[] kWeakestRows(int[][] mat, int k)
{
	int rows = mat.length, columns = mat[0].length;
	int[] strengths = new int[rows];
	for (int i = 0; i < rows; i++)
	{
		int j = 0;
		while (j < columns && mat[i][j] == 1) j++;
		strengths[i] = j * 100 + i;
	}
	Arrays.sort(strengths);
	for (int i = 0; i < rows; i++) strengths[i] %= 100;
	return Arrays.copyOfRange(strengths, 0, k);
}
```

待学习\[堆]。

#### [1342. 将数字变成 0 的操作次数](https://leetcode.cn/problems/number-of-steps-to-reduce-a-number-to-zero/)

@2023.07.13

```java
/**
 * 模拟
 * Somnia1337
 */
public int numberOfSteps(int num)
{
	int ans = 0;
	while (num > 0)
	{
		if (num % 2 == 0) num /= 2;
		else num--;
		ans++;
	}
	return ans;
}
```

#### [1346. 检查整数及其两倍数是否存在](https://leetcode.cn/problems/check-if-n-and-its-double-exist/)

@2023.07.13

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean checkIfExist(int[] arr)
{
	HashSet<Integer> memo = new HashSet<>();
	for (int num : arr)
	{
		if (memo.contains(num * 2) || num % 2 == 0 && memo.contains(num / 2)) return true;
		memo.add(num);
	}
	return false;
}
```

2. 排序 + 双指针

```java
/**
 * 排序 + 双指针
 * 力扣官方题解
 * @translator ChatGPT
 */
public boolean checkIfExist(int[] arr)
{
	Arrays.sort(arr);
	for (int i = 0; i < arr.length; i++)
	{
		int target = arr[i] * 2;
		int left = 0;
		int right = arr.length - 1;
		while (left <= right)
		{
			int mid = left + (right - left) / 2;
			if (arr[mid] == target && mid != i)
			{
				return true;
			}
			else if (arr[mid] < target)
			{
				left = mid + 1;
			}
			else
			{
				right = mid - 1;
			}
		}
	}
	return false;
}
```

#### [1365. 有多少小于当前数字的数字](https://leetcode.cn/problems/how-many-numbers-are-smaller-than-the-current-number/)

@2023.07.15

1. 排序 + 哈希表

```java
/**
 * 排序 + 哈希表
 * Somnia1337
 */
public int[] smallerNumbersThanCurrent(int[] nums)
{
	int len = nums.length;
	int[] sorted = Arrays.copyOf(nums, len);
	HashMap<Integer, Integer> level = new HashMap<>();
	Arrays.sort(sorted);
	int lev = 0;
	for (int i = 0; i < len; i++)
	{
		level.put(sorted[i], i);
		while (i < len - 1 && sorted[i + 1] == sorted[i]) i++;
	}
	for (int i = 0; i < len; i++)
	{
		nums[i] = level.get(nums[i]);
	}
	return nums;
}
```

2. 计数排序

```java
/**
 * 计数排序
 * 力扣官方题解
 */
public int[] smallerNumbersThanCurrent(int[] nums)
{
	int[] cnt = new int[101];
	int n = nums.length;
	for (int i = 0; i < n; i++)
	{
		cnt[nums[i]]++;
	}
	for (int i = 1; i <= 100; i++)
	{
		cnt[i] += cnt[i - 1];
	}
	int[] ret = new int[n];
	for (int i = 0; i < n; i++)
	{
		ret[i] = nums[i] == 0 ? 0 : cnt[nums[i] - 1];
	}
	return ret;
}
```

#### [1370. 上升下降字符串](https://leetcode.cn/problems/increasing-decreasing-string/)

@2023.07.15

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public String sortString(String s)
{
	int len = s.length();
	char[] chars = s.toCharArray();
	StringBuilder ans = new StringBuilder();
	int it = 0, count = 0, cycle = 0;
	Arrays.sort(chars);
	while (count < len)
	{
		if (cycle % 2 == 0)
		{
			while (it < len && chars[it] == 'a' - 1) it++;
		}
		else
		{
			while (it > 0 && chars[it] == 'a' - 1) it--;
		}
		char cur = chars[it];
		ans.append(cur);
		chars[it] = 'a' - 1;
		count++;
		if (count == len) break;
		it += cycle % 2 == 0 ? 1 : -1;
		if (cycle % 2 == 0)
		{
			while (it < len && (chars[it] == cur || chars[it] == 'a' - 1)) it++;
		}
		else
		{
			while (it > 0 && (chars[it] == cur || chars[it] == 'a' - 1)) it--;
		}
		if (it == len)
		{
			it = len - 1;
			cycle++;
		}
		else if (it == 0)
		{
			it = 1;
			cycle++;
		}
	}
	return ans.toString();
}
```

2. 桶排序

字符统计，来回遍历，只要还剩就append。

```java
/**
 * 桶排序
 * 力扣官方题解
 */
public String sortString(String s)
{
	int[] num = new int[26];
	for (int i = 0; i < s.length(); i++)
	{
		num[s.charAt(i) - 'a']++;
	}

	StringBuilder ret = new StringBuilder();
	while (ret.length() < s.length())
	{
		for (int i = 0; i < 26; i++)
		{
			if (num[i] > 0)
			{
				ret.append((char) (i + 'a'));
				num[i]--;
			}
		}
		for (int i = 25; i >= 0; i--)
		{
			if (num[i] > 0)
			{
				ret.append((char) (i + 'a'));
				num[i]--;
			}
		}
	}
	return ret.toString();
}
```

#### [1374. 生成每种字符都是奇数个的字符串](https://leetcode.cn/problems/generate-a-string-with-characters-that-have-odd-counts/)

@2023.07.15

```java
/**
 * 分类讨论
 * Somnia1337
 */
public String generateTheString(int n)
{
	if (n % 2 == 0) return "a".repeat(n - 1) + "b";
	else return "a".repeat(n);
}
```

#### [1380. 矩阵中的幸运数](https://leetcode.cn/problems/lucky-numbers-in-a-matrix/)

@2023.07.15

```java
/**
 * 模拟
 * Somnia1337
 */
public List<Integer> luckyNumbers(int[][] matrix)
{
	List<Integer> ans = new ArrayList<>();
	int rows = matrix.length, columns = matrix[0].length;
	for (int[] row : matrix)
	{
		int min = row[0], minIndex = 0;
		for (int j = 1; j < columns; j++)
		{
			if (row[j] < min)
			{
				min = row[j];
				minIndex = j;
			}
		}
		boolean valid = true;
		for (int[] column : matrix)
		{
			if (column[minIndex] > min)
			{
				valid = false;
				break;
			}
		}
		if (valid) ans.add(min);
	}
	return ans;
}
```

#### [1385. 两个数组间的距离值](https://leetcode.cn/problems/find-the-distance-value-between-two-arrays/)

@2023.07.16

```java
/**
 * 排序
 * Somnia1337
 */
public int findTheDistanceValue(int[] arr1, int[] arr2, int d)
{
	int ans = 0, len = arr2.length;
	Arrays.sort(arr1);
	Arrays.sort(arr2);
	int it = 0;
	for (int n : arr1)
	{
		while (it < len && arr2[it] < n - d) it++;
		if (it == len || !(arr2[it] >= n - d && arr2[it] <= n + d)) ans++;
	}
	return ans;
}
```

#### [1389. 按既定顺序创建目标数组](https://leetcode.cn/problems/create-target-array-in-the-given-order/)

@2023.09.08

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] createTargetArray(int[] nums, int[] index) {
	List<Integer> list = new LinkedList<>();
	for (int i = 0; i < nums.length; i++) list.add(index[i], nums[i]);
	int[] ans = new int[nums.length];
	for (int i = 0; i < nums.length; i++) ans[i] = list.get(i);
	return ans;
}
```

#### [1394. 找出数组中的幸运数](https://leetcode.cn/problems/find-lucky-integer-in-an-array/)

@2023.07.15

```java
/**
 * 排序 + 一次遍历
 * Somnia1337
 */
public int findLucky(int[] arr)
{
	Arrays.sort(arr);
	for (int i = arr.length - 1; i >= 0; i--)
	{
		int count = 1;
		while (i > 0 && arr[i - 1] == arr[i])
		{
			count++;
			i--;
		}
		if (count == arr[i]) return arr[i];
	}
	return -1;
}
```

#### [1399. 统计最大组的数目](https://leetcode.cn/problems/count-largest-group/)

@2023.08.27

```java
/**
 * 计数
 * Somnia1337
 */
public int countLargestGroup(int n) {
	int[] group = new int[36]; // 最大为 9999 的数位和 36
	for (int i = 1; i <= n; i++) {
		int sum = digitsSum(i);
		group[sum - 1]++;
	}
	int max = 0, ans = 0;
	for (int g : group) {
		if (g > max) {
			max = g;
			ans = 1;
		}
		else if (g == max) ans++;
	}
	return ans;
}

private int digitsSum(int n) {
	int sum = 0;
	while (n > 0) {
		sum += n % 10;
		n /= 10;
	}
	return sum;
}
```

#### [1403. 非递增顺序的最小子序列](https://leetcode.cn/problems/minimum-subsequence-in-non-increasing-order/)

@2023.07.17

```java
/**
 * 贪心
 * Somnia1337
 */
public List<Integer> minSubsequence(int[] nums)
{
	int sum = 0;
	List<Integer> ans = new ArrayList<>();
	Arrays.sort(nums);
	for (int num : nums) sum += num;
	int newSum = 0;
	for (int i = nums.length - 1; i >= 0; i--)
	{
		ans.add(nums[i]);
		newSum += nums[i];
		if (sum - newSum < sum) break;
	}
	return ans;
}
```

#### [1408. 数组中的字符串匹配](https://leetcode.cn/problems/string-matching-in-an-array/)

@2023.08.27

枚举每对单词。

```java
/**
 * 枚举
 * Somnia1337
 */
public List<String> stringMatching(String[] words) {
	List<String> ans = new ArrayList<>();
	for (String w1 : words) {
		for (String w2 : words) {
			if (w2.length() > w1.length() && w2.contains(w1)) {
				ans.add(w1);
				break;
			}
		}
	}
	return ans;
}
```

优化：拼接所有单词，遍历 `words`，如果第一次出现和最后一次出现的位置不同，说明出现不止一次，即是其他串的子串。

```java
/**
 * 枚举(优化)
 * Benhao
 */
public List<String> stringMatching(String[] words) {
	String s = String.join(" ", words);
	List<String> ans = new ArrayList<>();
	for (String word : words) {
		if (s.indexOf(word) != s.lastIndexOf(word)) ans.add(word);
	}
	return ans;
}
```

#### [1413. 逐步求和得到正数的最小值](https://leetcode.cn/problems/minimum-value-to-get-positive-step-by-step-sum/)

@2023.07.17

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int minStartValue(int[] nums)
{
	int sum = 0, min = Integer.MAX_VALUE;
	for (int num : nums)
	{
		sum += num;
		min = Math.min(min, sum);
	}
	return min > 0 ? 1 : 1 - min;
}
```

#### [1417. 重新格式化字符串](https://leetcode.cn/problems/reformat-the-string/)

@2023.07.17

```java
/**
 * 双指针
 * Somnia1337
 */
public String reformat(String s)
{
	int len = s.length();
	char[] chars = s.toCharArray();
	int letterCount = 0, digitCount = 0;
	for (char ch : chars)
	{
		if (Character.isLetter(ch)) letterCount++;
		else digitCount++;
	}
	if (Math.abs(letterCount - digitCount) > 1) return "";
	int i = 0, j = 1;
	if (letterCount > digitCount)
	{
		while (true)
		{
			while (i < len && Character.isLetter(chars[i])) i += 2;
			while (j < len && Character.isDigit(chars[j])) j += 2;
			if (i >= len || j >= len) break;
			swap(chars, i, j);
		}
	}
	else
	{
		while (true)
		{
			while (i < len && Character.isDigit(chars[i])) i += 2;
			while (j < len && Character.isLetter(chars[j])) j += 2;
			if (i >= len || j >= len) break;
			swap(chars, i, j);
		}
	}
	return new String(chars);
}

public void swap(char[] arr, int i, int j)
{
	char temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```

#### [1422. 分割字符串的最大得分](https://leetcode.cn/problems/maximum-score-after-splitting-a-string/)

@2023.07.17

```java
/**
 * 模拟
 * Somnia1337
 */
public int maxScore(String s)
{
	int len = s.length();
	int zeroCount = 0, oneCount = 0;
	for (int i = 0; i < len; i++) zeroCount += s.charAt(i) == '0' ? 1 : 0;
	oneCount = len - zeroCount;
	int cur = s.charAt(0) == '0' ? oneCount + 1 : oneCount - 1;
	int max = cur;
	for (int i = 1; i < len - 1; i++)
	{
		cur += s.charAt(i) == '0' ? 1 : -1;
		max = Math.max(max, cur);
	}
	return max;
}
```

#### [1431. 拥有最多糖果的孩子](https://leetcode.cn/problems/kids-with-the-greatest-number-of-candies/)

@2023.07.18

```java
/**
 * 贪心
 * Somnia1337
 */
public List<Boolean> kidsWithCandies(int[] candies, int extraCandies)
{
	int len = candies.length;
	int max = candies[0];
	List<Boolean> ans = new ArrayList<>();
	for (int candy : candies)
	{
		max = Math.max(candy, max);
	}
	for (int candy : candies)
	{
		ans.add(candy + extraCandies >= max);
	}
	return ans;
}
```

#### [1436. 旅行终点站](https://leetcode.cn/problems/destination-city/)

@2023.07.18

```java
/**
 * 哈希表
 * Somnia1337
 */
public String destCity(List<List<String>> paths)
{
	int len = paths.size();
	Set<String> banned = new HashSet<>();
	for (List<String> path : paths)
	{
		banned.add(path.get(0));
	}
	for (List<String> path : paths)
	{
		if (!banned.contains(path.get(1))) return path.get(1);
	}
	return "";
}
```

#### [1437. 是否所有 1 都至少相隔 k 个元素](https://leetcode.cn/problems/check-if-all-1s-are-at-least-length-k-places-away/)

@2023.07.18

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean kLengthApart(int[] nums, int k)
{
	if (k == 0) return true;
	int len = nums.length;
	int count = -1;
	for (int num : nums)
	{
		if (num == 1)
		{
			if (count > 0 && count < k + 1) return false;
			else count = 1;
		}
		else if (count > 0) count++;
	}
	return true;
}
```

优化：只记录上个1的位置。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public boolean kLengthApart(int[] nums, int k)
{
	int n = nums.length;
	int prev = -1;
	for (int i = 0; i < n; ++i)
	{
		if (nums[i] == 1)
		{
			if (prev != -1 && i - prev - 1 < k)
			{
				return false;
			}
			prev = i;
		}
	}
	return true;
}
```

#### [1446. 连续字符](https://leetcode.cn/problems/consecutive-characters/)

@2023.07.18

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int maxPower(String s)
{
	int len = s.length();
	int count = 1, max = 1;
	for (int i = 1; i < len; i++)
	{
		if (s.charAt(i) == s.charAt(i - 1)) count++;
		else
		{
			max = Math.max(count, max);
			count = 1;
		}
	}
	return Math.max(count, max);
}
```

#### [1450. 在既定时间做作业的学生人数](https://leetcode.cn/problems/number-of-students-doing-homework-at-a-given-time/)

@2023.08.27

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
	int ans = 0;
	for (int i = 0; i < startTime.length; i++) {
		ans += queryTime >= startTime[i] && queryTime <= endTime[i] ? 1 : 0;
	}
	return ans;
}
```

#### [1455. 检查单词是否为句中其他单词的前缀](https://leetcode.cn/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence/)

@2023.08.27

```java
/**
 * 库函数
 * Aozaki
 */
public int isPrefixOfWord(String sentence, String searchWord) {
	String[] words = sentence.split(" ");
	for (int i = 0; i < words.length; i++) {
		if (words[i].startsWith(searchWord)) return i + 1;
	}
	return -1;
}
```

#### [1460. 通过翻转子数组使两个数组相等](https://leetcode.cn/problems/make-two-arrays-equal-by-reversing-subarrays/)

@2023.08.27

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public boolean canBeEqual(int[] target, int[] arr) {
	Arrays.sort(target);
	Arrays.sort(arr);
	return Arrays.equals(target, arr);
}
```

2. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean canBeEqual(int[] target, int[] arr) {
	Map<Integer, Integer> c1 = new HashMap<>(), c2 = new HashMap<>();
	for (int t : target) c1.merge(t, 1, Integer::sum);
	for (int a : arr) c2.merge(a, 1, Integer::sum);
	return c1.equals(c2);
}
```

#### [1464. 数组中两元素的最大乘积](https://leetcode.cn/problems/maximum-product-of-two-elements-in-an-array/)

@2023.08.27

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int maxProduct(int[] nums) {
	int max = nums[0], sec = 0;
	for (int i = 1; i < nums.length; i++) {
		if (nums[i] > max) {
			sec = max;
			max = nums[i];
		}
		else if (nums[i] > sec) {
			sec = nums[i];
		}
	}
	return (max - 1) * (sec - 1);
}
```

#### [1470. 重新排列数组](https://leetcode.cn/problems/shuffle-the-array/)

@2023.08.29

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] shuffle(int[] nums, int n) {
	int[] ans = new int[n * 2];
	int i1 = 0, i2 = n;
	for (int i = 0; i < n * 2; i += 2) {
		ans[i] = nums[i1++];
		ans[i + 1] = nums[i2++];
	}
	return ans;
}
```

#### [1475. 商品折扣后的最终价格](https://leetcode.cn/problems/final-prices-with-a-special-discount-in-a-shop/)

@2023.08.28

1. 枚举

```java
/**
 * 枚举
 * Somnia1337
 */
public int[] finalPrices(int[] prices) {
	for (int i = 0; i < prices.length; i++) {
		for (int j = i + 1; j < prices.length; j++) {
			if (prices[j] <= prices[i]) {
				prices[i] -= prices[j];
				break;
			}
		}
	}
	return prices;
}
```

2. 单调栈

```java
/**
 * 单调栈
 * 力扣官方题解
 */
public int[] finalPrices(int[] prices) {
	int[] ans = new int[prices.length];
	Deque<Integer> stack = new ArrayDeque<>();
	for (int i = prices.length - 1; i >= 0; i--) {
		while (!stack.isEmpty() && stack.peek() > prices[i]) stack.pop();
		ans[i] = prices[i] - (!stack.isEmpty() ? stack.peek() : 0);
		stack.push(prices[i]);
	}
	return ans;
}
```

#### [1480. 一维数组的动态和](https://leetcode.cn/problems/running-sum-of-1d-array/)

@2023.08.28

```java
/**
 * 原地修改
 * Somnia1337
 */
public int[] runningSum(int[] nums) {
	for (int i = 1; i < nums.length; i++) {
		nums[i] += nums[i - 1];
	}
	return nums;
}
```

#### [1486. 数组异或操作](https://leetcode.cn/problems/xor-operation-in-an-array/)

@2023.08.28

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public int xorOperation(int n, int start) {
	int ans = start;
	for (int i = start + 2; i < start + n * 2; i += 2) {
		ans ^= i;
	}
	return ans;
}
```

2. 数学

```java
/**
 * 数学
 * 力扣官方题解
 */
public int xorOperation(int n, int start) {
	int s = start >> 1;
	return sumXor(s - 1) ^ sumXor(s + n - 1) << 1 | n & start & 1;
}

private int sumXor(int x) {
	if (x % 4 == 0) return x;
	if (x % 4 == 1) return 1;
	if (x % 4 == 2) return x + 1;
	return 0;
}
```

#### [1491. 去掉最低工资和最高工资后的工资平均值](https://leetcode.cn/problems/average-salary-excluding-the-minimum-and-maximum-salary/)

@2023.08.29

```java
/**
 * 一次遍历
 * Somnia1337
 */
public double average(int[] salary) {
	int sum = 0, max = salary[0], min = salary[0];
	for (int wage : salary) {
		max = Math.max(wage, max);
		min = Math.min(wage, min);
		sum += wage;
	}
	return (double) (sum - max - min) / (double) (salary.length - 2);
}
```

#### [1496. 判断路径是否相交](https://leetcode.cn/problems/path-crossing/)

@2023.09.05

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isPathCrossing(String path) {
	int x = 0, y = 0;
	Set<List<Integer>> vis = new HashSet<>();
	vis.add(List.of(0, 0));
	for (Character c : path.toCharArray()) {
		switch (c) {
			case 'N' -> y++;
			case 'S' -> y--;
			case 'E' -> x++;
			default -> x--;
		}
		if (!vis.add(List.of(x, y))) return true;
	}
	return false;
}
```

优化：使用自定义哈希函数，用整形表示每个坐标。

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isPathCrossing(String path) {
	int x = 0, y = 0;
	Set<Integer> vis = new HashSet<>();
	vis.add(0);
	for (Character c : path.toCharArray()) {
		switch (c) {
			case 'N' -> y++;
			case 'S' -> y--;
			case 'E' -> x++;
			default -> x--;
		}
		if (!vis.add(x * 12345 + y)) return true;
	}
	return false;
}
```

#### [1502. 判断能否形成等差数列](https://leetcode.cn/problems/can-make-arithmetic-progression-from-sequence/)

@2023.09.05

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public boolean canMakeArithmeticProgression(int[] arr) {
	int len = arr.length;
	if (len == 2) return true;
	Arrays.sort(arr);
	int d = arr[1] - arr[0];
	for (int i = 1; i < len - 1; i++) {
		if (arr[i + 1] - arr[i] != d) return false;
	}
	return true;
}
```

2. 数学

两次遍历：第一次遍历找到最大值和最小值，确定公差；第二次遍历检查每个对应的位置的元素是否存在。

```java
/**
 * 数学
 * Ray
 */
public boolean canMakeArithmeticProgression(int[] arr) {
	int len = arr.length, min = arr[0], max = arr[0];
	for (int x : arr) {
		min = Math.min(min, x);
		max = Math.max(max, x);
	}
	if ((max - min) % (len - 1) != 0) return false;
	int diff = (max - min) / (len - 1);
	if (diff == 0) return true;
	boolean[] vis = new boolean[len];
	for (int x : arr) {
		if ((x - min) % diff == 0) {
			if (vis[(x - min) / diff]) return false;
			vis[(x - min) / diff] = true;
		}
		else return false;
	}
	return true;
}
```

#### [1640. 能否连接形成数组](https://leetcode.cn/problems/check-array-formation-through-concatenation/)

@2023.12.10

哈希表记录 `pieces` 中每行的 `<首元素, 下标>`，遍历 `arr`，每次需要完全匹配一行。

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean canFormArray(int[] arr, int[][] pieces) {
	Map<Integer, Integer> pos = new HashMap<>();
	for (int i = 0; i < pieces.length; i++) pos.put(pieces[i][0], i);
	int i = 0;
	while (i < arr.length) {
		if (!pos.containsKey(arr[i])) return false;
		for (int n : pieces[pos.get(arr[i])]) {
			if (n != arr[i++]) return false;
		}
	}
	return true;
}
```

#### [1732. 找到最高海拔](https://leetcode.cn/problems/find-the-highest-altitude/)

@2023.09.02

```java
/**
 * 贪心
 * Somnia1337
 */
public int largestAltitude(int[] gain) {
	int cur = 0, ans = 0;
	for (int g : gain) {
		cur += g;
		if (g > 0) ans = Math.max(cur, ans);
	}
	return ans;
}
```

#### [1768. 交替合并字符串](https://leetcode.cn/problems/merge-strings-alternately/)

@2023.08.30

```java
/**
 * 双指针
 * Somnia1337
 */
public String mergeAlternately(String word1, String word2) {
	int l1 = word1.length(), l2 = word2.length(), l = Math.min(l1, l2);
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < l; i++) ans.append(word1.charAt(i)).append(word2.charAt(i));
	if (l1 > l) ans.append(word1, l, l1);
	else if (l2 > l) ans.append(word2, l, l2);
	return ans.toString();
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 */
public String mergeAlternately(String word1, String word2) {
	int l1 = word1.length(), l2 = word2.length(), i1 = 0, i2 = 0;
	StringBuilder ans = new StringBuilder();
	while (i1 < l1 || i2 < l2) {
		if (i1 < l1) {
			ans.append(word1.charAt(i1));
			i1++;
		}
		if (i2 < l2) {
			ans.append(word2.charAt(i2));
			i2++;
		}
	}
	return ans.toString();
}
```

#### [1784. 检查二进制字符串字段](https://leetcode.cn/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)

@2024.01.05

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean checkOnesSegment(String s) {
	char[] chs = s.toCharArray();
	int cnt = 0;
	for (int i = 0; i < chs.length; ) {
		if (chs[i++] == '1') {
			cnt++;
			while (i < chs.length && chs[i] == '1') i++;
		}
	}
	return cnt <= 1;
}
```

最两端的 `"1"` 之间不能有 `"0"`。

```java
/**
 * 库函数
 * Benhao
 */
public boolean checkOnesSegment(String s) {
	return !s.substring(s.indexOf("1"), s.lastIndexOf("1")).contains("0");
}
```

#### [2085. 统计出现过一次的公共字符串](https://leetcode.cn/problems/count-common-words-with-one-occurrence/)

@2024.01.12

```java
/**
 * 哈希表
 * Somnia1337
 */
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

#### [2103. 环和杆](https://leetcode.cn/problems/rings-and-rods/)

@2023.11.02

用 `boolean[10][3]` 表示 10 根杆上分别含有 3 种环的情况。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int countPoints(String rings)
{
	boolean[][] pillars = new boolean[10][3];
	for (int i = 0; i < rings.length(); i += 2)
	{
		int p = rings.charAt(i + 1) - '0';
		if (rings.charAt(i) == 'R') pillars[p][0] = true;
		else if (rings.charAt(i) == 'G') pillars[p][1] = true;
		else pillars[p][2] = true;
	}
	int ans = 0;
	for (boolean[] pillar : pillars)
	{
		if (pillar[0] && pillar[1] && pillar[2]) ans++;
	}
	return ans;
}
```

#### [2215. 找出两数组的不同](https://leetcode.cn/problems/find-the-difference-of-two-arrays/)

@2023.09.02

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
	Set<Integer> s1 = new HashSet<>(), s2 = new HashSet<>();
	List<Integer> l1 = new ArrayList<>(), l2 = new ArrayList<>();
	List<List<Integer>> ans = new ArrayList<>();
	ans.add(l1);
	ans.add(l2);
	for (int num1 : nums1) s1.add(num1);
	for (int num2 : nums2) s2.add(num2);
	for (int num1 : s1) {
		if (!s2.contains(num1)) l1.add(num1);
	}
	for (int num2 : s2) {
		if (!s1.contains(num2)) l2.add(num2);
	}
	return ans;
}
```

#### [2235. 两整数相加](https://leetcode.cn/problems/add-two-integers/)

@2023.12.09

```java
/**
 * 直接计算
 * Somnia1337
 */
public int sum(int num1, int num2) {
	return num1 + num2;
}
```

#### [2485. 找出中枢整数](https://leetcode.cn/problems/find-the-pivot-integer/)

@2024.01.03

设中枢为 $x$，则 ${\large \frac{x (x + 1)}{2}} = {\large \frac{(n - x + 1) (n + n)}{2}}$，化简得 $x^2 = {\large \frac{n (n + 1)}{2}}$，检验 $x$ 是否为整数即可。

```java
/**
 * 数学
 * 小火炉不火
 */
public int pivotInteger(int n) {
	int s = n * (n + 1) >> 1, x = (int) Math.sqrt(s);
	return s == Math.pow(x, 2) ? x : -1;
}
```

#### [2500. 删除每行中的最大值](https://leetcode.cn/problems/delete-greatest-value-in-each-row/)

@2023.12.09

```java
/**
 * 模拟
 * Somnia1337
 */
public int deleteGreatestValue(int[][] grid) {
	int rows = grid.length, cols = grid[0].length, ans = 0;
	for (int k = 0; k < cols; k++) {
		int cur = 0;
		for (int i = 0; i < rows; i++) {
			int max = 0, x = -1, y = -1;
			for (int j = 0; j < cols; j++) {
				if (grid[i][j] > max) {
					max = grid[i][j];
					x = i;
					y = j;
				}
			}
			grid[x][y] = 0;
			cur = Math.max(max, cur);
		}
		ans += cur;
	}
	return ans;
}
```

```java
/**
 * 排序
 * ?
 */
public int deleteGreatestValue(int[][] grid) {
	for (int[] row : grid) Arrays.sort(row);
	int cols = grid[0].length, ans = 0;
	for (int i = 0; i < cols; ++i) {
		int max = grid[0][i];
		for (int[] row : grid) max = Math.max(row[i], max);
		ans += max;
	}
	return ans;
}
```

#### [2511. 最多可以摧毁的敌人城堡数目](https://leetcode.cn/problems/maximum-enemy-forts-that-can-be-captured/)

@2023.09.03

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int captureForts(int[] forts) {
	int len = forts.length, ans = 0;
	for (int i = 0; i < len; i++) {
		if (forts[i] == 1 || forts[i] == -1) {
			int j = i + 1;
			while (j < len && forts[j] == 0) j++;
			if (j < len && forts[j] * forts[i] == -1) ans = Math.max(j - i - 1, ans);
		}
	}
	return ans;
}
```

#### [2520. 统计能整除数字的位数](https://leetcode.cn/problems/count-the-digits-that-divide-a-number/)

@2023.10.26

转换成字符串模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public int countDigits(int num)
{
	char[] digits = String.valueOf(num)
						  .toCharArray();
	int ans = 0;
	for (char d : digits)
	{
		if (d == '0') continue;
		if (num % (d - '0') == 0) ans++;
	}
	return ans;
}
```

#### [2525. 根据规则将箱子分类](https://leetcode.cn/problems/categorize-box-according-to-criteria/)

@2023.10.20

按规则分类。

```java
/**
 * 讨论
 * Somnia1337
 */
public String categorizeBox(int length, int width, int height, int mass)
{
	final int B1 = 10000, B2 = 1000000000;
	boolean isBulky = length >= B1 || width >= B1 || height >= B1 || (long) length * width * height >= B2;
	boolean isHeavy = mass >= 100;
	if (isBulky && isHeavy) return "Both";
	if (isBulky) return "Bulky";
	if (isHeavy) return "Heavy";
	return "Neither";
}
```

#### [2558. 从数量最多的堆取走礼物](https://leetcode.cn/problems/take-gifts-from-the-richest-pile/)

@2023.10.28

大顶堆。

```java
/**
 * 堆
 * Somnia1337
 */
public long pickGifts(int[] gifts, int k)
{
	Queue<Long> pq = new PriorityQueue<>(Collections.reverseOrder());
	for (int gift : gifts) pq.offer((long) gift);
	while (k-- > 0)
	{
		long gift = pq.poll();
		pq.offer((long) Math.sqrt(gift));
	}
	long ans = 0;
	while (!pq.isEmpty()) ans += pq.poll();
	return ans;
}
```

#### [2562. 找出数组的串联值](https://leetcode.cn/problems/find-the-array-concatenation-value/)

@2023.10.12

```java
/**
 * 模拟
 * Somnia1337
 */
public long findTheArrayConcVal(int[] nums)
{
	long ans = 0;
	int left = 0, right = nums.length - 1;
	while (left <= right)
	{
		if (left < right)
		{
			int a = nums[left], b = nums[right];
			int offset = String.valueOf(b)
							   .length();
			long cur = (long) (a * Math.pow(10, offset) + b);
			ans += cur;
		}
		else ans += nums[left];
		left++;
		right--;
	}
	return ans;
}
```

#### [2578. 最小和分割](https://leetcode.cn/problems/split-with-minimum-sum/)

@2023.10.09

```java
/**
 * 贪心
 * Somnia1337
 */
public int splitNum(int num)
{
	char[] chars = String.valueOf(num)
						 .toCharArray();
	Arrays.sort(chars);
	int a = 0, b = 0;
	for (int i = 0; i < chars.length; i += 2) a = a * 10 + chars[i] - '0';
	for (int i = 1; i < chars.length; i += 2) b = b * 10 + chars[i] - '0';
	return a + b;
}
```

#### [2582. 递枕头](https://leetcode.cn/problems/pass-the-pillow/)

@2023.09.26

```java
/**
 * 直接计算
 * Somnia1337
 */
public int passThePillow(int n, int time)
{
	int mod = time % (n - 1);
	int round = time / (n - 1) % 2;
	return round == 0 ? mod + 1 : n - mod;
}
```

#### [2586. 统计范围内的元音字符串数](https://leetcode.cn/problems/count-the-number-of-vowel-strings-in-range/)

@2023.11.07

遍历，检查首尾字符。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int vowelStrings(String[] words, int left, int right)
{
	String v = "aeiou";
	int ans = 0;
	for (int i = left; i <= right; i++)
	{
		char c1 = words[i].charAt(0), c2 = words[i].charAt(words[i].length() - 1);
		if (v.indexOf(c1) >= 0 && v.indexOf(c2) >= 0) ans++;
	}
	return ans;
}
```

#### [2591. 将钱分给最多的儿童](https://leetcode.cn/problems/distribute-money-to-maximum-children/)

@2023.09.22

```java
/**
 * 讨论
 * arignote
 */
public int distMoney(int money, int children)
{
	return money == 8 * children ? children : money > 8 * children - 8 && money != 8 * children - 4 ?
			children - 1 : money < children ? -1 : Math.min(children - 2, (money - children) / 7);
}
```

#### [2605. 从两个数字数组里生成最小数字](https://leetcode.cn/problems/form-smallest-number-from-two-digit-arrays/)

@2023.09.05

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int minNumber(int[] nums1, int[] nums2) {
	int min1 = nums1[0], min2 = nums2[0];
	boolean[] v1 = new boolean[10]; // 记录 nums1 中各数字出现与否
	for (int x1 : nums1) {
		v1[x1] = true;
		min1 = Math.min(x1, min1);
	}
	int same = 10; // 两数组共同的数字, 初始化为不可能的 10
	for (int x2 : nums2) {
		if (v1[x2]) same = Math.min(x2, same);
		min2 = Math.min(x2, min2);
	}
	if (same < 10) return same; // 有共同数字
	return Math.min(min1, min2) * 10 + Math.max(min1, min2);
}
```

#### [2609. 最长平衡子字符串](https://leetcode.cn/problems/find-the-longest-balanced-substring-of-a-binary-string/)

@2023.11.08

一次遍历，分别对 `'0'`、`'1'` 计数，取最小值的最大值。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int findTheLongestBalancedSubstring(String s)
{
	int l = s.length(), it = 0, ans = 0;
	while (it < l)
	{
		int a = 0, b = 0;
		while (it < l && s.charAt(it) == '0' && ++a >= 0) it++;
		while (it < l && s.charAt(it) == '1' && ++b >= 0) it++;
		ans = Math.max(Math.min(a, b) * 2, ans);
	}
	return ans;
}
```

#### [2652. 倍数求和](https://leetcode.cn/problems/sum-multiples/)

@2023.10.17

1. 直接计算

```java
/**
 * 直接计算
 * Somnia1337
 */
public int sumOfMultiples(int n)
{
	int ans = 0;
	for (int i = 1; i <= n; i++)
	{
		if (i % 3 == 0 || i % 5 == 0 || i % 7 == 0) ans += i;
	}
	return ans;
}
```

2. 数学

容斥原理 + 等差数列求和。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public int sumOfMultiples(int n)
{
	return s(n, 3) + s(n, 5) + s(n, 7) - s(n, 15) - s(n, 21) - s(n, 35) + s(n, 105);
}

private int s(int n, int m)
{
	return n / m * (n / m + 1) / 2 * m;
}
```

#### [2656. K 个元素的最大和](https://leetcode.cn/problems/maximum-sum-with-exactly-k-elements/)

@2023.11.15

等差数列求和。

```java
/**
 * 数学
 * Somnia1337
 */
public int maximizeSum(int[] nums, int k)
{
	int max = 0;
	for (int num : nums) max = Math.max(num, max);
	return (max * 2 + k - 1) * k / 2;
}
```

#### [2660. 保龄球游戏的获胜者](https://leetcode.cn/problems/determine-the-winner-of-a-bowling-game/)

@2023.12.27

模拟，在每次得分向前看 2 个。

```java
/**
 * 模拟
 * Somnia1337
 */
public int isWinner(int[] player1, int[] player2) {
	int score1 = count(player1), score2 = count(player2);
	if (score1 > score2) return 1;
	if (score1 < score2) return 2;
	return 0;
}

private int count(int[] arr) {
	int ret = 0;
	for (int i = 0; i < arr.length; i++) {
		if ((i >= 1 && arr[i - 1] == 10) || (i >= 2 && arr[i - 2] == 10)) ret += arr[i] * 2;
		else ret += arr[i];
	}
	return ret;
}
```

#### [2678. 老人的数目](https://leetcode.cn/problems/number-of-senior-citizens/)

@2023.10.23

字符串模拟。

```java
/**
 * 字符串
 * Somnia1337
 */
public int countSeniors(String[] details)
{
	int ans = 0;
	for (String d : details)
	{
		if (Integer.parseInt(d.substring(11, 13)) > 60) ans++;
	}
	return ans;
}
```

#### [2682. 找出转圈游戏输家](https://leetcode.cn/problems/find-the-losers-of-the-circular-game/)

@2024.01.14

模拟传球过程，哈希表记录所有接到过球的人，游戏结束后取出所有没有被哈希表记录的玩家。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] circularGameLosers(int n, int k) {
	Set<Integer> vis = new HashSet<>();
	int cur = 0, i = 1;
	do {
		vis.add(cur);
	} while (!vis.contains(cur = (cur + (i++) * k) % n));
	int[] ans = new int[n - vis.size()];
	for (int x = 0, j = 0; x < n; x++) {
		if (!vis.contains(x)) ans[j++] = x + 1;
	}
	return ans;
}
```

#### [2696. 删除子串后的字符串最小长度](https://leetcode.cn/problems/minimum-string-length-after-removing-substrings/)

@2024.01.10

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

#### [2697. 字典序最小回文串](https://leetcode.cn/problems/lexicographically-smallest-palindrome/)

@2023.12.13

双指针遍历，对称位置取字典序小者。

```java
/**
 * 贪心
 * Somnia1337
 */
public String makeSmallestPalindrome(String s) {
	int l = s.length();
	char[] chs = s.toCharArray();
	for (int i = 0; i < l / 2; i++) {
		if (chs[i] != chs[l - i - 1]) {
			char min = (char) Math.min(chs[i], chs[l - i - 1]);
			chs[i] = chs[l - i - 1] = min;
		}
	}
	return new String(chs);
}
```

#### [2706. 购买两块巧克力](https://leetcode.cn/problems/buy-two-chocolates/)

@2023.12.29

```java
/**
 * 排序
 * Somnia1337
 */
public int buyChoco(int[] prices, int money) {
	Arrays.sort(prices);
	if (prices[0] + prices[1] > money) return money;
	return money - (prices[0] + prices[1]);
}
```

#### [2744. 最大字符串配对数目](https://leetcode.cn/problems/find-maximum-number-of-string-pairs/)

@2024.01.17



```java
/**
 * 字符串
 * Somnia1337
 */
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

#### [2760. 最长奇偶子数组](https://leetcode.cn/problems/longest-even-odd-subarray-with-threshold/)

@2023.11.16

遇到非法元素后截断，取当前长度与之后的子问题解的最大值，用分治的形式较易于实现。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int longestAlternatingSubarray(int[] nums, int threshold)
{
	return solve(nums, 0, nums.length - 1, threshold);
}

private int solve(int[] arr, int l, int r, int max)
{
	if (l > r) return 0;
	if (arr[l] % 2 == 1) return solve(arr, l + 1, r, max);
	for (int i = l; i < r; i++)
	{
		if (arr[i] > max) return Math.max(i - l, solve(arr, i + 1, r, max));
		else if (arr[i] % 2 == arr[i + 1] % 2) return Math.max(i - l + 1, solve(arr, i + 1, r, max));
	}
	return r - l + (arr[r] <= max ? 1 : 0);
}
```

#### [2765. 最长交替子数组](https://leetcode.cn/problems/longest-alternating-subarray/)

@2024.01.23



```java
/**
 * 一次遍历
 * Somnia1337
 */
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

#### [2769. 找出最大的可达成数字](https://leetcode.cn/problems/find-the-maximum-achievable-number/)

@2024.01.02

```java
/**
 * 直接计算
 * Somnia1337
 */
public int theMaximumAchievableX(int num, int t) {
	return num + t * 2;
}
```

#### [2788. 按分隔符拆分字符串](https://leetcode.cn/problems/split-strings-by-separator/)

@2024.01.20



```java
/**
 * 库函数
 * Somnia1337
 */
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

#### [2824. 统计和小于目标的下标对数目](https://leetcode.cn/problems/count-pairs-whose-sum-is-less-than-target/)

@2023.11.24

1. 枚举

```java
/**
 * 枚举
 * Somnia1337
 */
public int countPairs(List<Integer> nums, int target)
{
	int len = nums.size(), ans = 0;
	for (int i = 0; i < len - 1; i++)
	{
		int a = nums.get(i);
		for (int j = i + 1; j < len; j++)
		{
			if (a + nums.get(j) < target) ans++;
		}
	}
	return ans;
}
```

2. 双指针

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int countPairs(List<Integer> nums, int target)
{
	Collections.sort(nums);
	int ans = 0;
	for (int i = 0, j = nums.size() - 1; i < j; i++)
	{
		while (i < j && nums.get(i) + nums.get(j) >= target) j--;
		ans += j - i;
	}
	return ans;
}
```

#### [2828. 判别首字母缩略词](https://leetcode.cn/problems/check-if-a-string-is-an-acronym-of-words/)

@2023.12.20

```java
/**
 * 直接计算
 * Somnia1337
 */
public boolean isAcronym(List<String> words, String s) {
	if (words.size() != s.length()) return false;
	StringBuilder abbr = new StringBuilder();
	for (String word : words) abbr.append(word.charAt(0));
	return abbr.toString().equals(s);
}
```

#### [2833. 距离原点最远的点](https://leetcode.cn/problems/furthest-point-from-origin/)

@2023.09.02

```java
/**
 * 贪心
 * Somnia1337
 */
public int furthestDistanceFromOrigin(String moves) {
	int l = 0, w = 0; // l: 'L'计数, w: '_'计数(wild)
	for (char c : moves.toCharArray()) {
		if (c == 'L') l++;
		else if (c == 'R') l--;
		else w++;
	}
	return Math.abs(l) + w;
}
```

#### [2839. 判断通过操作能否让字符串相等 I](https://leetcode.cn/problems/check-if-strings-can-be-made-equal-with-operations-i/)

@2023.09.02

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

#### [2843. 统计对称整数的数目](https://leetcode.cn/problems/count-symmetric-integers/)

@2023.09.03

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

#### [2848. 与车相交的点](https://leetcode.cn/problems/points-that-intersect-with-cars/)

@2023.09.10

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

#### [2855. 使数组成为递增数组的最少右移次数](https://leetcode.cn/problems/minimum-right-shifts-to-sort-the-array/)

@2023.09.16

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

#### [2859. 计算 K 置位下标对应元素的和](https://leetcode.cn/problems/sum-of-values-at-indices-with-k-set-bits/)

@2023.09.17

直接调用`Integer::bitCount`。

```java
/**
 * 库函数
 * Somnia1337
 */
public int sumIndicesWithKSetBits(List<Integer> nums, int k)
{
	int ans = 0;
	for (int i = 0; i < nums.size(); i++)
	{
		if (Integer.bitCount(i) == k) ans += nums.get(i);
	}
	return ans;
}
```

#### [2864. 最大二进制奇数](https://leetcode.cn/problems/maximum-odd-binary-number/)

@2023.09.24

统计 `'1'` 的个数，优先将 `'1'` 集中在前端。

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

#### [2869. 收集元素的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-collect-elements/)

@2023.09.30

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

#### [2873. 有序三元组中的最大值 I](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-i/)

@2023.10.01

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

#### [2894. 分类求和并作差](https://leetcode.cn/problems/divisible-and-non-divisible-sums-difference/)

@2023.10.08

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

#### [2899. 上一个遍历的整数](https://leetcode.cn/problems/last-visited-integers/)

@2023.10.14

用计数器维护已有数字的数量，遇到新数字时置为 `nums.size() - 1`，查询时递减，减到 0 以下时再次查询添加 -1。

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
		if (word.equals("prev"))
		{
			if (!nums.isEmpty() && cur >= 0) ans.add(nums.get(cur--));
			else ans.add(-1);
		}
		else
		{
			nums.add(Integer.parseInt(word));
			cur = nums.size() - 1;
		}
	}
	return ans;
}
```

#### [2903. 找出满足差值条件的下标 I](https://leetcode.cn/problems/find-indices-with-index-and-value-difference-i/)

@2023.10.15

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

#### [2908. 元素和最小的山形三元组 I](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-i/)

@2023.10.22

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

#### [2913. 子数组不同元素数目的平方和 I](https://leetcode.cn/problems/subarrays-distinct-element-sum-of-squares-i/)

@2023.10.28

枚举每个子数组，哈希表统计。

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

#### [2917. 找出数组中的 K-or 值](https://leetcode.cn/problems/find-the-k-or-of-an-array/)

@2023.10.29

枚举每个位。

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

#### [2923. 找到冠军 I](https://leetcode.cn/problems/find-champion-i/)

@2023.11.05

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

#### [2928. 给小朋友们分糖果 I](https://leetcode.cn/problems/distribute-candies-among-children-i/)

@2023.11.12

1. 数学

分类讨论，组合数计算。

```java
/**
 * 数学
 * 灵茶山艾府
 */
public int distributeCandies(int n, int limit)
{
	return c2(n + 2) - 3 * c2(n - limit + 1) + 3 * c2(n - 2 * limit) - c2(n - 3 * limit - 1);
}

private int c2(int n)
{
	return n > 1 ? n * (n - 1) / 2 : 0;
}
```

2. 枚举

枚举第一个人的糖果，计算后两个人的分类方法。

```java
/**
 * 枚举
 * 不上guardian不改名
 */
public int distributeCandies(int n, int limit)
{
	int ans = 0;
	for (int i = 0; i <= Math.min(n, limit); i++)
	{
		ans += helper(n - i, limit);
	}
	return ans;
}

private int helper(int n, int l)
{
	int a = Math.min(n, l), b = Math.max(n - l, 0);
	return Math.max(a - b + 1, 0);
}
```

#### [2932. 找出强数对的最大异或值 I](https://leetcode.cn/problems/maximum-strong-pair-xor-i/)

@2023.11.12

枚举每一对强数对。

```java
/**
 * 枚举
 * Somnia1337
 */
public int maximumStrongPairXor(int[] nums)
{
	Arrays.sort(nums);
	int ans = 0;
	for (int i = 0; i < nums.length; i++)
	{
		for (int j = i; j < nums.length && nums[j] < nums[i] * 2 + 1; j++)
		{
			ans = Math.max(nums[i] ^ nums[j], ans);
		}
	}
	return ans;
}
```

#### [2937. 使三个字符串相等](https://leetcode.cn/problems/make-three-strings-equal/)

@2023.11.19

一次遍历，求三串的最长公共前缀长度，需要删除的字符数即长度和减去最长公共前缀长 * 3。

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

#### [2942. 查找包含给定字符的单词](https://leetcode.cn/problems/find-words-containing-character/)

@2023.11.26

`indexOf()` 应用。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public List<Integer> findWordsContaining(String[] words, char x)
{
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < words.length; i++)
	{
		if (words[i].indexOf(x) >= 0) ans.add(i);
	}
	return ans;
}
```

#### [2946. 循环移位后的矩阵相似检查](https://leetcode.cn/problems/matrix-similarity-after-cyclic-shifts/)

@2023.11.26

分别检查奇偶行。

```java
/**
 * 模拟
 * Somnia1337
 */
public boolean areSimilar(int[][] mat, int k)
{
	int rows = mat.length, cols = mat[0].length;
	k %= cols;
	for (int i = 0; i < rows; i += 2)
	{
		for (int j = 0; j < cols; j++)
		{
			if (mat[i][j] != mat[i][(j + k) % cols]) return false;
		}
	}
	for (int i = 1; i < rows; i += 2)
	{
		for (int j = 0; j < cols; j++)
		{
			if (mat[i][j] != mat[i][Math.floorMod(j - k, cols)]) return false;
		}
	}
	return true;
}
```

优化：奇偶行其实没有区别，可以合并。

```java
/**
 * 模拟
 * 灵茶山艾府
 */
public boolean areSimilar(int[][] mat, int k)
{
	int cols = mat[0].length;
	k %= cols;
	for (int[] row : mat)
	{
		for (int j = 0; j < cols; j++)
		{
			if (row[j] != row[(j + k) % cols]) return false;
		}
	}
	return true;
}
```

#### [2951. 找出峰值](https://leetcode.cn/problems/find-the-peaks/)

@2023.12.03

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

#### [2956. 找到两个数组中的公共元素](https://leetcode.cn/problems/find-common-elements-between-two-arrays/)

@2023.12.09

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

#### [2960. 统计已测试设备](https://leetcode.cn/problems/count-tested-devices-after-test-operations/)

@2023.12.10

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

#### [2965. 找出缺失和重复的数字](https://leetcode.cn/problems/find-missing-and-repeated-values/)

@2023.12.17

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

#### [2970. 统计移除递增子数组的数目 I](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-i/)

@2023.12.23

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

#### [2974. 最小数字游戏](https://leetcode.cn/problems/minimum-number-game/)

@2023.12.24

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

#### [2980. 检查按位或是否存在尾随零](https://leetcode.cn/problems/check-if-bitwise-or-has-trailing-zeros/)

@2023.12.31

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

#### [2996. 大于等于顺序前缀和的最小缺失整数](https://leetcode.cn/problems/smallest-missing-integer-greater-than-sequential-prefix-sum/)

@2024.01.06

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

#### [3000. 对角线最长的矩形的面积](https://leetcode.cn/problems/maximum-area-of-longest-diagonal-rectangle/)

@2024.01.07

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

#### [3005. 最大频率元素计数](https://leetcode.cn/problems/count-elements-with-maximum-frequency/)

@2024.01.14



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

#### [3010. 将数组分成最小总代价的子数组 I](https://leetcode.cn/problems/divide-an-array-into-subarrays-with-minimum-cost-i/)

@2024.01.20

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

#### [3014. 输入单词需要的最少按键次数 I](https://leetcode.cn/problems/minimum-number-of-pushes-to-type-word-i/)

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

#### [3024. 三角形类型 II](https://leetcode.cn/problems/type-of-triangle-ii/)

@2024.02.04



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

#### [3028. 边界上的蚂蚁](https://leetcode.cn/problems/ant-on-the-boundary/)

@2024.02.04



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

#### [3033. 修改矩阵](https://leetcode.cn/problems/modify-the-matrix/)

@2024.02.11

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

#### [3038. 相同分数的最大操作数目 I](https://leetcode.cn/problems/maximum-number-of-operations-with-the-same-score-i/)

@2024.02.18

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

#### [3042. 统计前后缀下标对 I](https://leetcode.cn/problems/count-prefix-and-suffix-pairs-i/)

@2024.02.18

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

#### [3046. 分割数组](https://leetcode.cn/problems/split-the-array/)

@2024.02.26

统计，不能有出现次数超过 `2` 的元素。

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

#### [LCR 120. 寻找文件副本](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

@2023.12.09

1. 哈希表

```java
/**
 * 哈希表
 * Somnia1337
 */
public int findRepeatDocument(int[] documents) {
	Set<Integer> vis = new HashSet<>();
	for (int doc : documents) {
		if (!vis.add(doc)) return doc;
	}
	return -1;
}
```

2. 图

```java
/**
 * 图
 * ?
 */
public int findRepeatDocument(int[] documents) {
	for (int i = 0; i < documents.length; i++) {
		while (documents[i] != i) {
			if (documents[i] == documents[documents[i]]) return documents[i];
			swap(documents, i, documents[i]);
		}
	}
	return -1;
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [LCR 122. 路径加密](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

@2023.07.22

1. 字符数组

```java
/**
 * 字符数组
 * Somnia1337
 */
public String pathEncryption(String path)
{
	char[] chars = path.toCharArray();
	for (int i = 0; i < chars.length; i++)
	{
		if (chars[i] == '.') chars[i] = ' ';
	}
	return new String(chars);
}
```

2. 库函数

```java
/**
 * 库函数
 * 三进制
 */
public String pathEncryption(String path)
{
	return path.replaceAll(".", " ");
}
```

#### [LCR 125. 图书整理 II](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

@2023.12.09

```java
/**
 * 模拟
 * Somnia1337
 */
class CQueue {
    Deque<Integer> q;
	
    public CQueue() {
        q = new ArrayDeque<>();
    }
	
    public void appendTail(int value) {
        q.addLast(value);
    }
	
    public int deleteHead() {
        return !q.isEmpty() ? q.pollFirst() : -1;
    }
}
```

#### [LCR 126. 斐波那契数](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/)

@2023.12.10

滚动数组，注意取模。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int fib(int n) {
	final int MOD = 1000000007;
	if (n == 0) return 0;
	int a = 0, b = 1;
	for (int i = 1; i < n; i++) {
		int t = a;
		a = b;
		b = (t % MOD + a % MOD) % MOD;
	}
	return b;
}
```

#### [LCR 128. 库存管理 I](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

@2023.12.10

与 [154. 寻找旋转排序数组中的最小值 II](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/) 相同。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int stockManagement(int[] stock) {
	int ans = stock[0];
	for (int s : stock) ans = Math.min(s, ans);
	return ans;
}
```

```java
/**
 * 二分查找
 * Krahets
 */
public int stockManagement(int[] stock) {
	int l = 0, r = stock.length - 1;
	while (l < r) {
		int m = l + r >> 1;
		if (stock[m] > stock[r]) l = m + 1;
		else if (stock[m] < stock[r]) r = m;
		else r--;
	}
	return stock[l];
}
```

#### [LCR 146. 螺旋遍历二维数组](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

@2023.12.09

与 [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/) 相同。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] spiralArray(int[][] matrix) {
	int rows = matrix.length;
	if (rows == 0) return new int[0];
	int cols = matrix[0].length;
	if (cols == 0) return new int[0];
	int[] ans = new int[rows * cols];
	int u = 0, d = rows - 1, l = 0, r = cols - 1, it = 0;
	while (true) {
		for (int i = l; i <= r; i++, it++) ans[it] = matrix[u][i];
		u++;
		if (u > d) break;
		for (int i = u; i <= d; i++, it++) ans[it] = matrix[i][r];
		r--;
		if (r < l) break;
		for (int i = r; i >= l; i--, it++) ans[it] = matrix[d][i];
		d--;
		if (d < u) break;
		for (int i = d; i >= u; i--, it++) ans[it] = matrix[i][l];
		l++;
		if (l > r) break;
	}
	return ans;
}
```

#### [LCR 147. 最小栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)

@2023.12.13

与 [155. 最小栈](https://leetcode.cn/problems/min-stack/) 相同。

```java
/**
 * 栈
 * Somnia1337
 */
class MinStack {
    private Stack<Integer> stk, minStk;
	
    public MinStack() {
        stk = new Stack<>();
        minStk = new Stack<>();
        minStk.push(Integer.MAX_VALUE);
    }
	
    public void push(int val) {
        stk.push(val);
        minStk.push(Math.min(val, minStk.peek()));
    }
	
    public void pop() {
        stk.pop();
        minStk.pop();
    }
	
    public int top() {
        return stk.peek();
    }
	
    public int getMin() {
        return minStk.peek();
    }
}
```

#### [LCR 161. 连续天数的最高销售额](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

@2023.12.09

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maxSales(int[] sales) {
	int cur = 0, max = sales[0], ans = 0;
	for (int sale : sales) {
		max = Math.max(sale, max);
		cur = Math.max(cur + sale, 0);
		ans = Math.max(cur, ans);
	}
	return max > 0 ? ans : max;
}
```

#### [LCR 169. 招式拆解 II](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

@2023.07.02

```java
/**
 * 字符串
 * Somnia1337
 */
public char dismantlingAction(String list)
{
	int len = list.length();
	StringBuilder sb = new StringBuilder(list);
	String reversed = sb.reverse()
						.toString();
	HashSet<Character> chars = new HashSet<>();
	for (int i = 0; i < len; i++)
	{
		char c = list.charAt(i);
		if (chars.add(c) && reversed.indexOf(c) + i == len - 1) return c;
	}
	return ' ';
}
```

#### [LCR 173. 点名](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)

@2023.12.10

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int takeAttendance(int[] records) {
	for (int i = 0; i < records.length; i++) {
		if (records[i] != i) return records[i] - 1;
	}
	return records.length;
}
```

优化：序号有序排列，可用二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int takeAttendance(int[] records) {
	int l = 0, r = records.length;
	while (l < r) {
		int m = l + r >> 1;
		if (records[m] == m) l = m + 1;
		else r = m;
	}
	return l;
}
```

#### [LCR 182. 动态口令](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

@2023.09.06

API：`String::substring`。

```java
/**
 * 库函数
 * Somnia1337
 */
public String dynamicPassword(String password, int target) {
	return password.substring(target) + password.substring(0, target);
}
```

#### [LCR 186. 文物朝代判断](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

@2023.12.10

`boolean[]` 记录每个朝代的存在性。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean checkDynasty(int[] places) {
	boolean[] vis = new boolean[14];
	int v = 0;
	for (int p : places) {
		if (p > 0) {
			if (vis[p]) return false;
			vis[p] = true;
		}
		else v++;
	}
	int cnt = 0;
	for (int i = 1; i < 14 && cnt < 5; i++) {
		if (vis[i]) cnt++;
		else if (cnt > 0 && !vis[i]) {
			if (v-- == 0) return false;
			cnt++;
		}
	}
	return true;
}
```

```java
/**
 * 排序
 * ThyShy
 */
public boolean checkDynasty(int[] nums) {
	Arrays.sort(nums);
	for (int i = 0; i < 4; i++) {
		if (nums[i] > 0 && (nums[4] - nums[i] >= 5 || nums[i] == nums[i + 1])) return false;
	}
	return true;
}
```

#### [LCP 06. 拿硬币](https://leetcode.cn/problems/na-ying-bi/)

@2023.09.20

优先拿2枚，最后再拿1枚。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minCount(int[] coins)
{
	int ans = 0;
	for (int coin : coins) ans += (coin + 1) / 2;
	return ans;
}
```

#### [LCP 50. 宝石补给](https://leetcode.cn/problems/WHnhjV/)

@2023.09.15

直接操作 `gem`，遍历 `operations` 模拟宝石交换，再次遍历 `gem` 找到最值。

```java
/**
 * 模拟
 * Somnia1337
 */
public int giveGem(int[] gem, int[][] operations) {
	for (int[] o : operations) {
		int change = gem[o[0]] / 2;
		gem[o[0]] -= change;
		gem[o[1]] += change;
	}
	int min = gem[0], max = gem[0];
	for (int g : gem) {
		min = Math.min(g, min);
		max = Math.max(g, max);
	}
	return max - min;
}
```

#### [面试题 01.01. 判定字符是否唯一](https://leetcode.cn/problems/is-unique-lcci/)

@2023.12.10

整形位运算记录每个字符的存在性。

```java
/**
 * 位运算
 * Somnia1337
 */
public boolean isUnique(String astr) {
	int mask = 0;
	for (int i = 0; i < astr.length(); i++) {
		int d = (1 << (astr.charAt(i) - 'a'));
		if ((mask & d) == d) return false;
		mask |= d;
	}
	return true;
}
```

#### [面试题 01.06. 字符串压缩](https://leetcode.cn/problems/compress-string-lcci/)

@2023.12.09

```java
/**
 * 一次遍历
 * Somnia1337
 */
public String compressString(String S) {
	int l = S.length();
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < l; ) {
		int j = i + 1;
		while (j < l && S.charAt(j) == S.charAt(i)) j++;
		ans.append(S.charAt(i)).append(j - i);
		i = j;
	}
	return ans.length() < l ? ans.toString() : S;
}
```

#### [面试题 08.06. 汉诺塔问题](https://leetcode.cn/problems/hanota-lcci/)

@2023.12.09

```java
/**
 * 递归
 * omega
 */
public void hanota(List<Integer> A, List<Integer> B, List<Integer> C) {
	move(A.size(), A, B, C);
}

private void move(int n, List<Integer> A, List<Integer> B, List<Integer> C) {
	if (n == 1) C.add(A.remove(0));
	else {
		B.add(A.remove(n - 1));
		move(n - 1, A, B, C);
		C.add(B.remove(B.size() - 1));
	}
}
```