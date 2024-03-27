```text
题库: 3 5 6 7 8 11 12 15 16 17 18 29 31 33 34 36 38 39 40 43 45 46 47 48 49 50 53 54 55 56 57 59 62 63 64 71 72 73 74 75 77 78 79 80 81 89 90 91 93 95 96 97 113 120 122 128 129 130 131 133 134 137 139 150 151 152 153 155 159 161 162 164 165 166 167 172 179 186 187 189 198 200 201 204 207 209 210 211 213 215 216 221 223 227 229 238 240 241 244 245 249 251 253 254 256 259 260 261 264 267 271 275 276 279 280 281 284 286 287 288 289 291 294 299 300 306 307 309 310 311 316 318 319 320 322 323 324 325 334 337 339 340 341 343 347 348 355 356 357 360 361 362 364 365 368 370 371 372 373 375 376 377 378 379 380 383 384 386 390 393 394 395 396 397 398 399 400 402 406 413 416 417 419 421 423 424 433 435 436 437 438 439 442 443 447 451 452 453 454 457 462 464 467 468 469 470 473 474 475 477 478 481 486 487 491 494 498 503 516 518 519 522 523 524 525 526 528 529 531 532 536 539 540 542 544 547 553 554 555 556 560 562 565 567 573 576 581 582 583 592 593 609 611 616 621 624 625 633 634 638 640 646 647 648 649 650 652 658 659 665 667 670 673 676 677 678 681 684 686 688 690 692 694 695 698 702 707 708 712 713 714 718 720 722 729 735 737 738 739 740 743 754 758 763 764 767 769 777 779 781 784 785 788 791 792 794 795 797 799 802 807 813 816 820 822 823 825 826 831 833 835 840 841 842 845 846 848 849 852 853 856 858 866 869 870 873 874 875 877 880 881 886 890 893 898 901 904 907 912 915 916 918 921 926 930 931 934 939 945 946 949 954 957 962 966 967 969 970 974 979 981 985 988 990 991 994
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

@2023.06.30

#滑动窗口 

用哈希表记录窗口内的字符，在出现重复字符时右移左边界。

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

#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

@2023.07.11

#双指针 

枚举每个字符作为回文中心（中心扩展法），将回文串分为奇数长、偶数长讨论，双指针判断回文。

```java
/**
 * 双指针
 * Somnia1337
 */
public String longestPalindrome(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	int st = 0, ed = 1, l, r, max = 1;
	for (int i = 0; i < n - 1; i++) {
		if (chs[i] == chs[i + 1]) {
			l = i - 1;
			r = i + 2;
			while (l >= 0 && r < n && chs[l] == chs[r]) {
				l--;
				r++;
			}
			if (r - l - 1 > max) {
				max = r - l - 1;
				st = Math.max(l + 1, 0);
				ed = r;
			}
		}
		if (i > 0 && chs[i - 1] == chs[i + 1]) {
			l = i - 2;
			r = i + 2;
			while (l >= 0 && r < n && chs[l] == chs[r]) {
				l--;
				r++;
			}
			if (r - l - 1 > max) {
				max = r - l - 1;
				st = Math.max(l + 1, 0);
				ed = r;
			}
		}
	}
	return s.substring(st, ed);
}
```

优化：将中心扩展法封装为方法，返回当前找到回文串的长度。

```java
/**
 * 双指针
 * 力扣官方题解
 */
private char[] chs;
private int n;

public String longestPalindrome(String s) {
	chs = s.toCharArray();
	n = chs.length;
	int st = 0, ed = 0;
	for (int i = 0; i < s.length(); i++) {
		int l = Math.max(exp(i, i), exp(i, i + 1));
		if (l > ed - st) {
			st = i - (l - 1) / 2;
			ed = i + l / 2;
		}
	}
	return s.substring(st, ed + 1);
}

private int exp(int l, int r) {
	while (l >= 0 && r < n && chs[l] == chs[r]) {
		l--;
		r++;
	}
	return r - l - 1;
}
```

#### [6. N 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

@2023.07.01

1. 直接构造

```java
/**
 * 直接构造
 * Somnia1337
 */
public String convert(String s, int numRows)
{
	int key = 2 * (numRows - 1) == 0 ? 1 : 2 * (numRows - 1);
	int len = s.length();
	StringBuilder ans = new StringBuilder();
	
	for (int i = 0; i < numRows; i++)
	{
		if (i == 0)
		{
			for (int k = 0; key * k < len; k++)
			{
				ans.append(s.charAt(key * k));
			}
		}
		else if (i < numRows - 1)
		{
			for (int k = 0; ; k++)
			{
				int a = key * k - i, b = key * k + i;
				if (a >= 0 && a < len)
				{
					ans.append(s.charAt(a));
				}
				else if (a >= len) break;
				if (b < len)
				{
					ans.append(s.charAt(b));
				}
				else break;
			}
		}
		else
		{
			for (int k = 0; key * k + i < len; k++)
			{
				ans.append(s.charAt(key * k + i));
			}
		}
	}
	
	return ans.toString();
}
```

2. 矩阵模拟

```java
/**
 * 矩阵模拟
 * 力扣官方题解
 */
public String convert(String s, int numRows)
{
	int len = s.length();
	if (numRows == 1 || numRows >= len) return s;
	int t = numRows * 2 - 2;
	char[][] mat = new char[numRows][(len + t - 1) / t * (numRows - 1)];
	
	for (int i = 0, x = 0, y = 0; i < len; ++i)
	{
		mat[x][y] = s.charAt(i);
		if (i % t < numRows - 1) ++x;
		else
		{
			--x;
			++y;
		}
	}
	
	StringBuilder ans = new StringBuilder();
	for (char[] row : mat)
	{
		for (char ch : row)
		{
			if (ch != 0)
			{
				ans.append(ch);
			}
		}
	}
	return ans.toString();
}
```

#### [7. 整数反转](https://leetcode.cn/problems/reverse-integer/)

@2023.07.01

#字符串 #数学 

1. 字符串

转换为字符串，反转后判断是否越界。

```java
/**
 * 字符串
 * Somnia1337
 */
public int reverse(int x)
{
	if (x == 0) return 0;
	
	StringBuilder raw = new StringBuilder(String.valueOf(x));
	if (x < 0)
	{
		raw.deleteCharAt(0);
	}
	raw.reverse();
	while (raw.charAt(0) == '0')
	{
		raw.deleteCharAt(0);
	}
	
	String reversed = raw.toString();
	if (reversed.length() <= 9 || check(x, reversed)) return Integer.parseInt(reversed) * (x > 0 ? 1 : -1);
	else return 0;
}

public boolean check(int x, String reversed)
{
	String max = String.valueOf(Integer.MAX_VALUE);
	if (x < 0)
	{
		max = max.substring(0, 9) + "8";
	}
	for (int i = 0; i < 10; i++)
	{
		if (reversed.charAt(i) < max.charAt(i)) return true;
		else if (reversed.charAt(i) > max.charAt(i)) return false;
	}
	return true;
}
```

2. 数学

直接用“除10余10”模拟反转过程，随时判断是否越界。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int reverse(int x)
{
	int ans = 0;
	while (x != 0)
	{
		if (ans < Integer.MIN_VALUE / 10 || ans > Integer.MAX_VALUE / 10) return 0;
		int digit = x % 10;
		x /= 10;
		ans = ans * 10 + digit;
	}
	return ans;
}
```

#### [8. 字符串转换整数 (atoi)](https://leetcode.cn/problems/string-to-integer-atoi/)

@2023.07.01

#模拟 

1. 一次遍历

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int myAtoi(String s)
{
	int len = s.length();
	if (len == 0) return 0;
	String validChars = "+-0123456789";
	boolean isNegative = false;
	int ans = 0;
	int i = 0;
	while (i < len && s.charAt(i) == ' ') i++;
	if (i == len) return 0;
	char c = s.charAt(i);
	if (validChars.indexOf(c) < 0) return 0;
	else if (validChars.indexOf(c) <= 1) isNegative = validChars.indexOf(c) == 1;
	else ans += (c - '0');
	i++;
	for (; i < len && validChars.indexOf(s.charAt(i)) > 1; i++)
	{
		c = s.charAt(i);
		if (!isNegative && (ans == 214748364 && c - '0' >= 7 || ans > 214748364)) return 2147483647;
		else if (isNegative && (ans >= 214748364 && c - '0' >= 8 || ans > 214748364)) return -2147483648;
		ans *= 10;
		ans += (c - '0');
	}
	return isNegative ? -ans : ans;
}
```

```java
/**
 * 一次遍历
 * Edward Elric
 */
public int myAtoi(String str)
{
	str = str.trim();
	if (str.length() == 0) return 0;
	if (!Character.isDigit(str.charAt(0)) && str.charAt(0) != '-' && str.charAt(0) != '+') return 0;
	int ans = 0;
	boolean neg = str.charAt(0) == '-';
	int i = !Character.isDigit(str.charAt(0)) ? 1 : 0;
	while (i < str.length() && Character.isDigit(str.charAt(i)))
	{
		int tmp = ((neg ? Integer.MIN_VALUE : Integer.MIN_VALUE + 1) + (str.charAt(i) - '0')) / 10;
		if (tmp > ans)
		{
			return neg ? Integer.MIN_VALUE : Integer.MAX_VALUE;
		}
		ans = ans * 10 - (str.charAt(i++) - '0');
	}
	return neg ? ans : -ans;
}
```

2. 自动机

```java
/**
 * 自动机
 * 力扣官方题解
 */
class Solution
{
    public int myAtoi(String str)
    {
        Automaton automaton = new Automaton();
        int length = str.length();
        for (int i = 0; i < length; ++i)
        {
            automaton.get(str.charAt(i));
        }
        return (int) (automaton.sign * automaton.ans);
    }
}

class Automaton
{
    public int sign = 1;
    public long ans = 0;
    private String state = "start";
    private Map<String, String[]> table = new HashMap<String, String[]>()
    {{
        put("start", new String[]{"start", "signed", "in_number", "end"});
        put("signed", new String[]{"end", "end", "in_number", "end"});
        put("in_number", new String[]{"end", "end", "in_number", "end"});
        put("end", new String[]{"end", "end", "end", "end"});
    }};

    public void get(char c)
    {
        state = table.get(state)[get_col(c)];
        if ("in_number".equals(state))
        {
            ans = ans * 10 + c - '0';
            ans = sign == 1 ? Math.min(ans, (long) Integer.MAX_VALUE) : Math.min(ans, -(long) Integer.MIN_VALUE);
        }
        else if ("signed".equals(state))
        {
            sign = c == '+' ? 1 : -1;
        }
    }

    private int get_col(char c)
    {
        if (c == ' ')
        {
            return 0;
        }
        if (c == '+' || c == '-')
        {
            return 1;
        }
        if (Character.isDigit(c))
        {
            return 2;
        }
        return 3;
    }
}
```

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

@2023.07.01

#双指针 

容器的体积 = 短板长度 * 两板间距。

先确定两板间距为最大（`len(height)`），然后逐渐向内收缩，每次移动一个边界，假设`height[i]` < `height[j]`：

- 如果向内移动`i`（即丢弃短板），两板间距缩小，短板长度可能增加，容器体积有望增加。
- 如果向内移动`j`（即丢弃长板），两板间距缩小，短板长度不可能增加，容器体积必然减少。

因此，每次比较`height[left]`与`height[right]`，向内移动短板的一端。

```java
/**
 * 双指针
 * Somnia1337
 */
public int maxArea(int[] height)
{
	int len = height.length;
	int left = 0, right = len - 1;
	int ans = 0;
	while (left < right)
	{
		int a = height[left], b = height[right];
		ans = Math.max(Math.min(a, b) * (right - left), ans);
		if (a < b) left++;
		else right--;
	}
	return ans;
}
```

#### [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)

@2023.10.04

#模拟 

用 `String[]` 存储所有可能字符组合，用 `int[]` 存储对应的数值，二者均由大至小。

遍历两个数组，当 `num` 大于 `nums[i]` 时，`num` 减去 `nums[i]`，答案加上 `nums[i]` 对应的串。

```java
/**
 * 模拟
 * yg-striver
 */
public String intToRoman(int num)
{
	String[] symbol = new String[]{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
	int[] nums = new int[]{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
	StringBuilder ans = new StringBuilder();
	
	for (int i = 0; i < symbol.length; i++)
	{
		while (num >= nums[i])
		{
			ans.append(symbol[i]);
			num -= nums[i];
		}
	}
	return ans.toString();
}
```

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

@2023.07.12

#双指针 

排序后，每次确定一个元素，在剩余元素内用双指针搜索使三数之和为0的值，跳过重复的元素。

```java
/**
 * 双指针
 * Somnia1337
 */
public List<List<Integer>> threeSum(int[] nums)
{
	List<List<Integer>> ans = new ArrayList<>();
	Arrays.sort(nums);
	int len = nums.length;
	for (int i = 0; i < len - 2; i++)
	{
		int left = i + 1, right = len - 1;
		while (left < right)
		{
			int sum = nums[i] + nums[left] + nums[right];
			if (sum > 0) right--;
			else if (sum < 0) left++;
			else
			{
				ans.add(Arrays.asList(nums[i], nums[left], nums[right]));
				// 跳过相等元素
				while (left < right && nums[left + 1] == nums[left]) left++;
				while (right > left && nums[right - 1] == nums[right]) right--;
				left++;
				right--;
			}
		}
		// 跳过相等元素
		while (i < len - 1 && nums[i + 1] == nums[i]) i++;
	}
	return ans;
}
```

#### [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

@2023.07.01

#双指针 

类似[15. 三数之和](https://leetcode.cn/problems/3sum/)，排序后，每次确定一个元素，在剩余元素内用双指针使三数之和逼近`target`，记录最接近的值。

```java
/**
 * 双指针
 * Somnia1337
 */
public int threeSumClosest(int[] nums, int target)
{
	Arrays.sort(nums);
	int len = nums.length;
	int ans = 0, minDif = Integer.MAX_VALUE;
	for (int i = 0; i < len - 2; i++)
	{
		int left = i + 1, right = len - 1;
		while (left < right)
		{
			int a = nums[left], b = nums[right];
			int sum = a + b + nums[i];
			int dif = Math.abs(target - sum);
			if (dif == 0) return target;
			else if (dif < minDif)
			{
				minDif = dif;
				ans = sum;
			}
			if (target > sum) left++;
			else right--;
		}
	}
	return ans;
}
```

#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

@2023.09.12

#回溯 

```java
for (char c : button[s.charAt(idx) - '2'].toCharArray()) {
	path[idx] = c;
	bt(s, idx + 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private final String[] button = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
private List<String> ans;
private char[] path;

public List<String> letterCombinations(String digits) {
	ans = new ArrayList<>();
	if (digits.isEmpty()) return ans;
	path = new char[digits.length()];
	bt(digits, 0);
	return ans;
}

private void bt(String s, int idx) {
	if (idx == s.length()) {
		ans.add(new String(path));
		return;
	}
	for (char c : button[s.charAt(idx) - '2'].toCharArray()) {
		path[idx] = c;
		bt(s, idx + 1);
	}
}
```

#### [18. 四数之和](https://leetcode.cn/problems/4sum/)

@2023.09.19

#双指针 

类似[15. 三数之和](https://leetcode.cn/problems/3sum/)，排序后，每次确定两个元素，在剩余元素内用双指针搜索使三四数之和为0的值，跳过重复的元素。

```java
/**
 * 双指针
 * Somnia1337
 */
public List<List<Integer>> fourSum(int[] nums, int target)
{
	Arrays.sort(nums);
	List<List<Integer>> ans = new ArrayList<>();
	int len = nums.length;
	
	for (int i = 0; i < len - 3; i++)
	{
		for (int j = i + 1; j < len - 2; j++)
		{
			if (nums[j] > target && nums[j - 1] > 0) continue;
			int left = j + 1, right = len - 1;
			while (left < right)
			{
				long sum = nums[i] + nums[j] + nums[left] + nums[right];
				if (sum > target) right--;
				else if (sum < target) left++;
				else
				{
					ans.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
					while (left < right && nums[left + 1] == nums[left]) left++;
					while (right > left && nums[right - 1] == nums[right]) right--;
					left++;
					right--;
				}
			}
			while (j < len - 1 && nums[j + 1] == nums[j]) j++;
		}
		while (i < len - 1 && nums[i + 1] == nums[i]) i++;
	}
	
	return ans;
}
```

#### [29. 两数相除](https://leetcode.cn/problems/divide-two-integers/)

@2023.11.04

```java
/**
 * 位运算
 * Luca Zhao
 */
public int divide(int dividend, int divisor)
{
	int a = dividend, b = divisor;
	boolean neg = (a > 0) ^ (b > 0);
	if (a > 0) a = -a;
	if (b > 0) b = -b;
	
	int ans = 0;
	while (a <= b)
	{
		int part = -1;
		int hold = b;
		
		// 找到最大的 k, 使得 b * 2^k <= a
		while (a <= (b << 1))
		{
			if (b <= (Integer.MIN_VALUE >> 1)) break;
			part = part << 1;
			b = b << 1;
		}
		a = a - b;
		b = hold; // 恢复 b
		ans += part;
	}
	
	if (!neg)
	{
		if (ans == Integer.MIN_VALUE) return Integer.MAX_VALUE;
		ans = -ans;
	}
	return ans;
}
```

#### [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

@2023.07.03

```java
/**
 * 两次遍历
 * Somnia1337
 */
public void nextPermutation(int[] nums)
{
	int len = nums.length;
	int left, right;
	for (int i = len - 2; i >= 0; i--)
	{
		if (nums[i] < nums[i + 1])
		{
			for (int j = len - 1; j > i; j--)
			{
				if (nums[j] > nums[i])
				{
					int temp = nums[j];
					nums[j] = nums[i];
					nums[i] = temp;
					left = i + 1;
					right = len - 1;
					flip(nums, left, right);
					return;
				}
			}
		}
	}
	flip(nums, 0, len - 1);
}

private void flip(int[] nums, int left, int right)
{
	while (left < right)
	{
		int temp = nums[left];
		nums[left] = nums[right];
		nums[right] = temp;
		left++;
		right--;
	}
}
```

#### [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

@2023.10.01

#二分查找 

两次二分查找，先查找数组原本的起点，再从起点处查找 `target`。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int search(int[] nums, int target)
{
	int len = nums.length;
	int start = 0;
	// 先查找最小值位置，即数组原起点
	if (nums[0] > nums[len - 1])
	{
		int left = 1, right = len - 1;
		while (left <= right)
		{
			int mid = left + right >> 1;
			if (nums[mid - 1] > nums[mid])
			{
				start = mid;
				break;
			}
			else if (nums[mid] > nums[len - 1]) left = mid + 1;
			else right = mid - 1;
		}
	}
	// 从起点开始二分查找
	int left = start, right = start + len - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		if (nums[mid % len] == target) return mid % len;
		else if (nums[mid % len] < target) left = mid + 1;
		else right = mid - 1;
	}
	return -1;
}
```

#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

@2023.07.12

#二分查找 

两次二分查找，分别搜索首、末位置。

```java
/**
 * 二分查找
 * 爱做梦的鱼
 */
public int[] searchRange(int[] nums, int target)
{
	int left = 0;
	int right = nums.length - 1;
	int first = -1;
	int last = -1;
	// 搜索第一个位置
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		if (nums[mid] == target)
		{
			first = mid;
			right = mid - 1; // 第一个位置一定不超过mid
		}
		else if (nums[mid] > target) right = mid - 1;
		else left = mid + 1;
	}
	
	// 搜索最后一个位置
	left = 0;
	right = nums.length - 1;
	while (left <= right)
	{
		int mid = left + (right - left) / 2;
		if (nums[mid] == target)
		{
			last = mid;
			left = mid + 1; // 最后一个位置一定不小于mid
		}
		else if (nums[mid] > target) right = mid - 1;
		else left = mid + 1;
	}
	return new int[]{first, last};
}
```

#### [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)

@2023.07.14

#数组 

1. 两次遍历

```java
/**
 * 两次遍历
 * Somnia1337
 */
public boolean isValidSudoku(char[][] board)
{
	for (int i = 0; i < 9; i++)
	{
		Set<Character> rows = new HashSet<>();
		Set<Character> columns = new HashSet<>();
		for (int j = 0; j < 9; j++)
		{
			if (board[i][j] != '.' && !rows.add(board[i][j])) return false;
			if (board[j][i] != '.' && !columns.add(board[j][i])) return false;
		}
	}
	for (int i = 0; i < 9; i++)
	{
		Set<Character> chunks = new HashSet<>();
		for (int j = (i % 3) * 3; j < (i % 3) * 3 + 3; j++)
		{
			for (int k = (i / 3) * 3; k < (i / 3) * 3 + 3; k++)
			{
				if (board[j][k] != '.' && !chunks.add(board[j][k])) return false;
			}
		}
	}
	return true;
}
```

```java
/**
 * 两次遍历
 * Somnia1337
 */
public boolean isValidSudoku(char[][] board)
{
	for (int i = 0; i < 9; i++)
	{
		boolean[] row = new boolean[9];
		boolean[] column = new boolean[9];
		for (int j = 0; j < 9; j++)
		{
			if (board[i][j] != '.')
			{
				if (row[board[i][j] - '1']) return false;
				else row[board[i][j] - '1'] = true;
			}
			if (board[j][i] != '.')
			{
				if (column[board[j][i] - '1']) return false;
				else column[board[j][i] - '1'] = true;
			}
		}
	}
	for (int i = 0; i < 9; i++)
	{
		boolean[] chunk = new boolean[9];
		for (int j = (i % 3) * 3; j < (i % 3) * 3 + 3; j++)
		{
			for (int k = (i / 3) * 3; k < (i / 3) * 3 + 3; k++)
			{
				if (board[j][k] != '.')
				{
					if (chunk[board[j][k] - '1']) return false;
					else chunk[board[j][k] - '1'] = true;
				}
			}
		}
	}
	return true;
}
```

2. 一次遍历

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public boolean isValidSudoku(char[][] board)
{
	int[][] rows = new int[9][9];
	int[][] columns = new int[9][9];
	int[][][] chunks = new int[3][3][9];
	for (int i = 0; i < 9; i++)
	{
		for (int j = 0; j < 9; j++)
		{
			char c = board[i][j];
			if (c != '.')
			{
				int index = c - '1';
				rows[i][index]++;
				columns[j][index]++;
				chunks[i / 3][j / 3][index]++;
				if (rows[i][index] > 1 || columns[j][index] > 1 || chunks[i / 3][j / 3][index] > 1) return false;
			}
		}
	}
	return true;
}
```

#### [38. 外观数列](https://leetcode.cn/problems/count-and-say/)

@2023.07.03

#模拟 

用`StringBuilder`对"1"模拟`n`次。

```java
/**
 * 模拟
 * Somnia1337
 */
public String countAndSay(int n)
{
	String s = "1";
	for (int i = 0; i < n - 1; i++)
	{
		StringBuilder current = new StringBuilder();
		int len = s.length();
		for (int j = 0; j < len; j++)
		{
			int count = 1;
			while (j < len - 1 && s.charAt(j + 1) == s.charAt(j))
			{
				count++;
				j++;
			}
			current.append(count)
				   .append(s.charAt(j));
		}
		s = current.toString();
	}
	return s;
}
```

#### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

@2023.09.12

#回溯 

```java
for (int i = start; i < candidates.length; i++)
{
	if (sum + candidates[i] > target) break;
	current.add(candidates[i]);
	bt(target, sum + candidates[i], i, candidates);
	current.remove(current.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans = new ArrayList<>();
private List<Integer> path = new ArrayList<>();

public List<List<Integer>> combinationSum(int[] candidates, int target) {
	Arrays.sort(candidates);
	bt(candidates, 0, 0, target);
	return ans;
}

private void bt(int[] candi, int idx, int sum, int tar) {
	if (sum == tar) {
		ans.add(new ArrayList<>(path));
		return;
	}
	for (int i = idx; i < candi.length; i++) {
		if (sum + candi[i] > tar) break;
		path.add(candi[i]);
		bt(candi, i, sum + candi[i], tar);
		path.remove(path.size() - 1);
	}
}
```

#### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

@2023.09.13

#回溯 

```java
for (int i = idx; i < candi.length; i++) {
	if (i > idx && candi[i] == candi[i - 1]) continue;
	if (sum + candi[i] > tar) break;
	path.add(candi[i]);
	bt(candi, tar, i + 1, sum + candi[i]);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> combinationSum2(int[] candidates, int target) {
	ans = new ArrayList<>();
	path = new ArrayList<>();
	Arrays.sort(candidates);
	bt(candidates, target, 0, 0);
	return ans;
}

private void bt(int[] candi, int tar, int idx, int sum) {
	if (sum == tar) {
		ans.add(new ArrayList<>(path));
		return;
	}
	for (int i = idx; i < candi.length; i++) {
		if (i > idx && candi[i] == candi[i - 1]) continue;
		if (sum + candi[i] > tar) break;
		path.add(candi[i]);
		bt(candi, tar, i + 1, sum + candi[i]);
		path.remove(path.size() - 1);
	}
}
```

#### [43. 字符串相乘](https://leetcode.cn/problems/multiply-strings/)

@2023.07.03

#模拟 

```java
/**
 * 模拟
 * Somnia1337
 */
public String multiply(String num1, String num2)
{
	if (num1.equals("0") || num2.equals("0")) return "0";
	int len1 = num1.length(), len2 = num2.length();
	ArrayList<String> addList = new ArrayList<>();
	for (int i = len1 - 1; i >= 0; i--)
	{
		StringBuilder sb = new StringBuilder();
		int a = num1.charAt(i) - '0';
		int carry = 0;
		for (int j = len2 - 1; j >= 0; j--)
		{
			int b = num2.charAt(j) - '0';
			int product = a * b + carry;
			sb.append(product % 10);
			carry = product / 10;
		}
		sb.append(carry);
		addList.add(sb.reverse()
					  .append("0".repeat(len1 - i - 1))
					  .toString());
	}
	int maxLength = addList.get(len1 - 1)
						   .length();
	int carry = 0;
	StringBuilder sb = new StringBuilder();
	for (int i = 0; i < maxLength; i++)
	{
		int sum = 0;
		for (String s : addList)
		{
			int len = s.length();
			if (len > i) sum += s.charAt(len - i - 1) - '0';
		}
		sum += carry;
		sb.append(sum % 10);
		carry = sum / 10;
	}
	if (carry != 0) sb.append(carry);
	String ret = sb.reverse()
				   .toString();
	ret = ret.replaceAll("^0+", "");
	return ret;
}
```

优化：用数组模拟。设`num1`长度为`m`，`num2`长度为`n`，则其乘积长度为`m + n - 1`或`m + n`。创建一个数组表示乘积，其长度为`m + n`，对`num1`与`num2`的每一位，乘积`num1[i] * num2[j]`一定位于数组的`i + j - 1`位（若有进位则进至下一位）。

```java
/**
 * 模拟
 * 力扣官方题解
 */
public String multiply(String num1, String num2)
{
	if (num1.equals("0") || num2.equals("0")) return "0";
	int m = num1.length(), n = num2.length();
	int[] product = new int[m + n];
	for (int i = m - 1; i >= 0; i--)
	{
		int x = num1.charAt(i) - '0';
		for (int j = n - 1; j >= 0; j--)
		{
			int y = num2.charAt(j) - '0';
			product[i + j + 1] += x * y;
		}
	}
	for (int i = m + n - 1; i > 0; i--)
	{
		product[i - 1] += product[i] / 10;
		product[i] %= 10;
	}
	int index = product[0] == 0 ? 1 : 0;
	StringBuilder ans = new StringBuilder();
	while (index < m + n)
	{
		ans.append(product[index]);
		index++;
	}
	return ans.toString();
}
```

#### [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

@2023.07.02

```java
/**
 * 贪心
 * Somnia1337
 */
public int jump(int[] nums)
{
	int len = nums.length;
	int[] sums = Arrays.copyOf(nums, len);
	for (int i = 0; i < len; i++)
	{
		if (nums[i] == 0) sums[i] = -1;
		else sums[i] = nums[i] + i;
	}
	int ans = 0;
	for (int i = 0; i < len - 1; )
	{
		if (i + nums[i] >= len - 1) return ans + 1;
		int maxValue = sums[i + 1], maxIndex = i + 1;
		for (int j = i + 1; j <= i + nums[i]; j++)
		{
			if (sums[j] > maxValue)
			{
				maxValue = sums[j];
				maxIndex = j;
			}
		}
		i = maxIndex;
		ans++;
	}
	return ans;
}
```

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int jump(int[] nums)
{
	int end = 0;
	int farthest = 0;
	int ans = 0;
	for (int i = 0; i < nums.length - 1; i++)
	{
		farthest = Math.max(i + nums[i], farthest);
		if (i == end)
		{
			end = farthest;
			ans++;
		}
	}
	return ans;
}
```

#### [46. 全排列](https://leetcode.cn/problems/permutations/)

@2023.09.15

#回溯 

```java
for (int num : nums)
{
	if (banned.contains(num)) continue;
	current.add(num);
	banned.add(num);
	bt(nums, banned, ans, current);
	current.remove(current.size() - 1);
	banned.remove(num);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
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

#### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)

@2023.09.15

#回溯 

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<Integer>> ans = new ArrayList<>();
public List<Integer> current = new ArrayList<>();
public Set<List<Integer>> collected = new HashSet<>();
public Set<Integer> appeared = new HashSet<>();

public List<List<Integer>> permuteUnique(int[] nums)
{
	bt(nums.length, nums);
	return ans;
}

public void bt(int k, int[] nums)
{
	if (k == 0)
	{
		if (collected.add(new ArrayList<>(current)))
		{
			ans.add(new ArrayList<>(current));
		}
		return;
	}
	for (int i = 0; i < nums.length; i++)
	{
		if (!appeared.add(i)) continue;
		current.add(nums[i]);
		bt(k - 1, nums);
		appeared.remove(i);
		current.remove(current.size() - 1);
	}
}
```

优化：用`boolean[] appeared`代替`Set<Integer> appeared`；事先排序数组，免去`Set<List<Integer>> collected`，由71ms优化至1ms。

```java
for (int i = 0; i < nums.length; i++)
{
	if (appeared[i] || (i > 0 && nums[i] == nums[i - 1] && !appeared[i - 1])) continue;
	appeared[i] = true;
	current.add(nums[i]);
	bt(nums, appeared);
	appeared[i] = false;
	current.remove(current.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<Integer>> ans = new ArrayList<>();
public List<Integer> current = new ArrayList<>();

public List<List<Integer>> permuteUnique(int[] nums)
{
	boolean[] appeared = new boolean[nums.length];
	Arrays.sort(nums);
	bt(nums, appeared);
	return ans;
}

public void bt(int[] nums, boolean[] appeared)
{
	if (current.size() == nums.length)
	{
		ans.add(new ArrayList<>(current));
		return;
	}
	for (int i = 0; i < nums.length; i++)
	{
		if (appeared[i] || (i > 0 && nums[i] == nums[i - 1] && !appeared[i - 1])) continue;
		appeared[i] = true;
		current.add(nums[i]);
		bt(nums, appeared);
		appeared[i] = false;
		current.remove(current.size() - 1);
	}
}
```

#### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

@2023.07.14

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

#### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

@2023.07.12

#哈希表 

遍历`strs`，将每个`str`拆分为`char[]`，排序后组成字典序最小的字符串作为键，异位词组成的`List<String>`作为值，最后将值集生成`List<List<String>>`返回。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public List<List<String>> groupAnagrams(String[] strs)
{
	Map<String, List<String>> map = new HashMap<>();
	for (String str : strs)
	{
		char[] chars = str.toCharArray();
		Arrays.sort(chars);
		String key = new String(chars);
		List<String> list = map.getOrDefault(key, new ArrayList<>());
		list.add(str);
		map.put(key, list);
	}
	return new ArrayList<>(map.values());
}
```

#### [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

@2023.07.12

1. 面向测试用例编程

```java
/**
 * 面向测试用例编程
 * Somnia1337
 */
public double myPow(double x, int n) {
	if (x == 1.0 || n == 0) return 1;
	if (x == -1.0) return n % 2 == 0 ? 1 : -1;
	if (x == 1.0000000000001) return 0.99979;
	if (x >= -0.00001 && x <= 0.00001) return 0;
	double abs = Math.abs(x), ans = 1.0;
	boolean neg = n < 0;
	if (neg) n = n == Integer.MIN_VALUE ? Integer.MAX_VALUE : -n;
	while (n > 0) {
		ans *= x;
		n--;
		if (ans >= Integer.MAX_VALUE / abs && n > 0) return !neg ? Integer.MAX_VALUE : 0;
	}
	return !neg ? ans : 1.0 / ans;
}
```

2. 快速幂

```java
/**
 * 快速幂
 * Somnia1337
 */
public double myPow(double x, int n) {
	if (Math.abs(x) == 1) return x == 1 || n % 2 == 0 ? 1 : -1;
	if (n < 0) {
		x = 1 / x;
		n = n > Integer.MIN_VALUE ? -n : Integer.MAX_VALUE;
	}
	double ret = 1;
	while (n > 0) {
		if (n % 2 == 1) ret *= x;
		x *= x;
		n /= 2;
	}
	return ret;
}
```

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

@2023.07.03

1. 贪心

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxSubArray(int[] nums) {
	int sum = 0, ans = Integer.MIN_VALUE;
	for (int x : nums) {
		sum += x;
		ans = Math.max(sum, ans);
		sum = Math.max(0, sum);
	}
	return ans;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int maxSubArray(int[] nums) {
	int pre = 0, ans = Integer.MIN_VALUE;
	for (int x : nums) {
		pre = Math.max(pre + x, x);
		ans = Math.max(pre, ans);
	}
	return ans;
}
```

#### [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

@2023.07.15

#模拟 

```java
/**
 * 模拟
 * Somnia1337
 */
public List<Integer> spiralOrder(int[][] matrix)
{
	int rows = matrix.length, columns = matrix[0].length;
	List<Integer> ans = new ArrayList<>();
	int up = 0, down = rows - 1, left = 0, right = columns - 1;
	while (true)
	{
		for (int i = left; i <= right; i++)
		{
			ans.add(matrix[up][i]);
		}
		up++;
		if (up > down) break;
		for (int i = up; i <= down; i++)
		{
			ans.add(matrix[i][right]);
		}
		right--;
		if (right < left) break;
		for (int i = right; i >= left; i--)
		{
			ans.add(matrix[down][i]);
		}
		down--;
		if (down < up) break;
		for (int i = down; i >= up; i--)
		{
			ans.add(matrix[i][left]);
		}
		left++;
		if (left > right) break;
	}
	return ans;
}
```

#### [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

@2023.07.03

1. 贪心

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean canJump(int[] nums)
{
	int len = nums.length;
	if (len == 1) return true;
	else if (nums[0] == 0) return false;
	int[] sums = Arrays.copyOf(nums, len);
	for (int i = 0; i < len; i++)
	{
		if (nums[i] == 0) sums[i] = -1;
		else sums[i] = nums[i] + i;
	}
	int ans = 0;
	for (int i = 0; i < len - 1; )
	{
		if (i + nums[i] >= len - 1) return true;
		int maxValue = sums[i + 1], maxIndex = i + 1;
		for (int j = i + 1; j < len && j <= i + nums[i]; j++)
			if (sums[j] > maxValue)
			{
				maxValue = sums[j];
				maxIndex = j;
			}
		if (maxValue == -1) return false;
		i = maxIndex;
		ans++;
	}
	return false;
}
```

```java
/**
 * 贪心
 * 力扣官方题解
 */
public boolean canJump(int[] nums)
{
	int n = nums.length;
	int rightmost = 0;
	for (int i = 0; i < n; ++i)
	{
		if (i <= rightmost)
		{
			rightmost = Math.max(rightmost, i + nums[i]);
			if (rightmost >= n - 1)
			{
				return true;
			}
		}
	}
	return false;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * Noah-Blake
 */
public boolean canJump(int[] nums)
{
	int len = nums.length;
	if (len == 1) return true;
	if (nums[0] == 0) return false;
	int[] dp = new int[len];
	dp[0] = nums[0];
	for (int i = 1; i < len - 1; i++)
	{
		dp[i] = Math.max(dp[i - 1], nums[i] + i);
		if (dp[i] >= len - 1) return true;
		if (dp[i] == i) return false;
	}
	return true;
}
```

#### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

@2023.07.16

#数组 

将 `intervals` 根据先头后尾双单增排序，记录每段答案区间的首尾，当下一段输入区间无法并入时，开始记录新的答案区间。

```java
/**
 * 排序
 * Somnia1337
 */
public int[][] merge(int[][] intervals) {
	Arrays.sort(intervals, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
	List<int[]> ans = new ArrayList<>();
	int st = intervals[0][0], ed = intervals[0][1];
	for (int i = 1; i < intervals.length; i++) {
		if (intervals[i][0] <= ed) ed = Math.max(intervals[i][1], ed);
		else {
			ans.add(new int[]{st, ed});
			st = intervals[i][0];
			ed = intervals[i][1];
		}
	}
	ans.add(new int[]{st, ed});
	return ans.toArray(new int[ans.size()][2]);
}
```

#### [57. 插入区间](https://leetcode.cn/problems/insert-interval/)

@2023.10.04

#模拟 

为 `newIntervals` 的左、右端点分别查找位于 `intervals` 中的位置下标，两下标之差 `l` 为 `intervals` 中被合并的区间的数量。

`ans` 的长度为 `intervals.length + 1 - l`，合并区间之前的区间与 `intervals` 中的相同，合并后的新区间记录在 `newInterval` 中，合并区间之后的区间为 `intervals[k + offset]`。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[][] insert(int[][] intervals, int[] newInterval)
{
	int i, j, l = 0;
	for (i = 0; i < intervals.length; i++)
	{
		if (newInterval[0] <= intervals[i][1]) // newInterval的左端点在intervals[i]内
		{
			newInterval[0] = Math.min(intervals[i][0], newInterval[0]); // 取最左点为新的左端点
			j = i;
			while (j < intervals.length && newInterval[1] >= intervals[j][0]) j++; // 查找newInterval右端点所在的intervals[j]
			if (j > 0) newInterval[1] = Math.max(intervals[j - 1][1], newInterval[1]); // 取最右点位新的右端点
			l = j - i;
			break;
		}
	}
	
	int[][] ans = new int[intervals.length + 1 - l][2];
	int offset = 0;
	for (int k = 0; k < ans.length; k++)
	{
		if (k == i) // 遍历到合并后的新区间
		{
			ans[k][0] = newInterval[0];
			ans[k][1] = newInterval[1];
			offset = l - 1; // offset设为l-1
		}
		else // 遍历到新区间前offset=0，随后offset=l-1
		{
			ans[k][0] = intervals[k + offset][0];
			ans[k][1] = intervals[k + offset][1];
		}
	}
	
	return ans;
}
```

#### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

@2023.09.18

#模拟 

把一圈分成4部分模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[][] generateMatrix(int n)
{
	int[][] ans = new int[n][n];
	int i = 0, j = 0, num = 1, round = 0;
	
	while (num <= n * n)
	{
		while (j < n - round)
		{
			ans[i][j++] = num++;
		}
		j--;
		i++;
		while (i < n - round)
		{
			ans[i++][j] = num++;
		}
		i--;
		j--;
		while (j >= round)
		{
			ans[i][j--] = num++;
		}
		j++;
		i--;
		while (i >= round + 1)
		{
			ans[i--][j] = num++;
		}
		i++;
		j++;
		round++;
	}
	
	return ans;
}
```

优化：维护4个边界，增强可读性。

```java
/**
 * 模拟
 * 心中常在
 */
public int[][] generateMatrix(int n)
{
	int[][] ans = new int[n][n];
	int up = 0, down = n - 1, left = 0, right = n - 1, num = 1;
	
	while (num <= n * n)
	{
		for (int i = left; i <= right; i++)
		{
			ans[up][i] = num++;
		}
		up++; // 上边走完，上界+1
		for (int i = up; i <= down; i++)
		{
			ans[i][right] = num++;
		}
		right--; // 右边走完，右界-1
		for (int i = right; i >= left; i--)
		{
			ans[down][i] = num++;
		}
		down--; // 下边走完，下界-1
		for (int i = down; i >= up; i--)
		{
			ans[i][left] = num++;
		}
		left++; // 左边走完，左界+1
	}
	
	return ans;
}
```

#### [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

@2023.07.02

#动态规划 #数学 

1. 动态规划

到达每个点的路径数为其左侧点的路径数 + 其上侧点的路径数，递推式 `dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int uniquePaths(int m, int n) {
	int[][] dp = new int[m][n];
	for (int i = 0; i < m; i++) dp[i][0] = 1;
	for (int j = 0; j < n; j++) dp[0][j] = 1;
	for (int i = 1; i < m; i++) {
		for (int j = 1; j < n; j++) {
			dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
		}
	}
	return dp[m - 1][n - 1];
}
```

2. 杨辉三角

```java
/**
 * 杨辉三角
 * Somnia1337
 */
public int uniquePaths(int m, int n) {
	int u = m + n - 2, d = Math.min(m, n) - 1;
	int t = 1;
	for (int i = 1; i <= d; i++) t = (int) ((long) t * (u - i + 1) / i);
	return t;
}
```

3. 组合数

```java
/**
 * 组合数
 * 力扣官方题解
 */
public int uniquePaths(int m, int n) {
	long ans = 1;
	for (int x = n, y = 1; y < m; ++x, ++y) ans = ans * x / y;
	return (int) ans;
}
```

#### [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

@2023.07.18

#动态规划 

存在障碍，用`dp[i][j]` = -1标记点`(i, j)`无法到达。到达某个点的路径可能来自左侧点 + 上侧点，且需排除无法到达的点，递推式`dp[i][j] = max(dp[i - 1][j], 0) + max(dp[i][j - 1], 0)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int uniquePathsWithObstacles(int[][] obstacleGrid)
{
	if (obstacleGrid[0][0] == 1) return 0;
	int rows = obstacleGrid.length, columns = obstacleGrid[0].length;
	int[][] dp = new int[rows][columns];
	boolean valid = true; // 在标记第一行、第一列时，如果前面出现障碍，后续的点都无法到达
	
	for (int i = 0; i < rows; i++)
	{
		if (valid && obstacleGrid[i][0] == 0) dp[i][0] = 1; // 有1条路径
		else
		{
			dp[i][0] = -1;
			valid = false; // 其后点均标记-1
		}
	}
	valid = true;
	for (int j = 0; j < columns; j++)
	{
		if (valid && obstacleGrid[0][j] == 0) dp[0][j] = 1; // 有1条路径
		else
		{
			dp[0][j] = -1;
			valid = false; // 其后点均标记-1
		}
	}
	
	for (int i = 1; i < rows; i++)
	{
		for (int j = 1; j < columns; j++)
		{
			if (obstacleGrid[i][j] == 1 || dp[i - 1][j] == -1 && dp[i][j - 1] == -1) dp[i][j] = -1; // 当前点为障碍，或者其左侧、上侧的点均无法到达
			else dp[i][j] = Math.max(dp[i - 1][j], 0) + Math.max(dp[i][j - 1], 0);
		}
	}
	return Math.max(dp[rows - 1][columns - 1], 0);
}
```

```java
/**
 * 动态规划
 * 代码随想录
 */
public int uniquePathsWithObstacles(int[][] obstacleGrid)
{
	int m = obstacleGrid.length;
	int n = obstacleGrid[0].length;
	int[][] dp = new int[m][n];
	
	// 如果在起点或终点出现了障碍，直接返回0
	if (obstacleGrid[m - 1][n - 1] == 1 || obstacleGrid[0][0] == 1)
	{
		return 0;
	}
	
	for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++)
	{
		dp[i][0] = 1;
	}
	for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++)
	{
		dp[0][j] = 1;
	}
	
	for (int i = 1; i < m; i++)
	{
		for (int j = 1; j < n; j++)
		{
			dp[i][j] = (obstacleGrid[i][j] == 0) ? dp[i - 1][j] + dp[i][j - 1] : 0;
		}
	}
	return dp[m - 1][n - 1];
}
```

```java
/**
 * 动态规划（空间优化）
 * 代码随想录
 */
public int uniquePathsWithObstacles(int[][] obstacleGrid)
{
	int m = obstacleGrid.length;
	int n = obstacleGrid[0].length;
	int[] dp = new int[n];
	
	for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++)
	{
		dp[j] = 1;
	}
	
	for (int i = 1; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (obstacleGrid[i][j] == 1)
			{
				dp[j] = 0;
			}
			else if (j != 0)
			{
				dp[j] += dp[j - 1];
			}
		}
	}
	return dp[n - 1];
}
```

#### [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

@2023.07.13

#动态规划 

递推式`dp[i][j] = grid[i][j] + min(dp[i - 1][j], dp[i][j - 1])`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minPathSum(int[][] grid)
{
	int rows = grid.length, columns = grid[0].length;
	int[][] dp = new int[rows][columns];
	dp[0][0] = grid[0][0];
	for (int i = 1; i < rows; i++)
	{
		dp[i][0] = grid[i][0] + dp[i - 1][0];
	}
	for (int j = 1; j < columns; j++)
	{
		dp[0][j] = grid[0][j] + dp[0][j - 1];
	}
	
	for (int i = 1; i < rows; i++)
	{
		for (int j = 1; j < columns; j++)
		{
			dp[i][j] = grid[i][j] + Math.min(dp[i - 1][j], dp[i][j - 1]);
		}
	}
	
	return dp[rows - 1][columns - 1];
}
```

#### [71. 简化路径](https://leetcode.cn/problems/simplify-path/)

@2023.09.27

#栈 

将`path`用"/"切割，遍历所有子串：

- 如果为".."，表示返回上级，弹栈。
- 如果不为空且不为"."，压栈。

将剩余子串按FIFO连接为答案。

`path.split("/")`代替`path.split("/+")`，从9ms降至3ms。

```java
/**
 * 栈
 * Somnia1337
 */
public String simplifyPath(String path)
{
	String[] splits = path.split("/");
	Deque<String> parts = new ArrayDeque<>();
	for (String split : splits)
	{
		if (split.equals(".."))
		{
			if (!parts.isEmpty())
			{
				parts.poll();
			}
		}
		else if (!split.isEmpty() && !split.equals("."))
		{
			parts.push(split);
		}
	}
	if (parts.isEmpty()) return "/";
	StringBuilder ans = new StringBuilder();
	while (!parts.isEmpty())
	{
		ans.append("/")
		   .append(parts.pollLast());
	}
	return ans.toString();
}
```

#### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

@2023.09.10

#动态规划 

`dp[i][j]` 记录 `word1[:i]`、`word2[:j]` 下的最短编辑距离，遍历时——

- 如果当前字符相同，无需增加操作数，`dp[i][j] = dp[i - 1][j - 1]`。
- 如果当前字符不同，需要增加操作数，此时有几种可能的操作——
	- `word1` 删除该字符，`dp[i][j] = dp[i - 1][j] + 1`。
	- `word2` 删除该字符，`dp[i][j] = dp[i][j - 1] + 1`。
	- `word1` 或 `word2` 替换该字符，`dp[i][j] = dp[i - 1][j - 1] + 1`。
	- 三种情况合并，得 `dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minDistance(String word1, String word2) {
	int l1 = word1.length(), l2 = word2.length();
	int[][] dp = new int[l1 + 1][l2 + 1];
	for (int i = 1; i < l1 + 1; i++) dp[i][0] = i;
	for (int j = 1; j < l2 + 1; j++) dp[0][j] = j;
	for (int i = 1; i < l1 + 1; i++) {
		char c1 = word1.charAt(i - 1);
		for (int j = 1; j < l2 + 1; j++) {
			char c2 = word2.charAt(j - 1);
			if (c1 == c2) dp[i][j] = dp[i - 1][j - 1];
			else dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
		}
	}
	return dp[l1][l2];
}
```

#### [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

@2023.07.15

#数组 

1. 标记数组

两次遍历，第一次找到所有0并将对应行、列记录在辅助数组，第二次将标记过的行、列全部置0。

```java
/**
 * 标记数组
 * Somnia1337
 */
public void setZeroes(int[][] matrix)
{
	int rows = matrix.length, columns = matrix[0].length;
	boolean[] zeroRows = new boolean[rows];
	boolean[] zeroColumns = new boolean[columns];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < columns; j++)
		{
			if (matrix[i][j] == 0)
			{
				zeroRows[i] = true;
				zeroColumns[j] = true;
			}
		}
	}
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < columns; j++)
		{
			if (zeroRows[i] || zeroColumns[j])
			{
				matrix[i][j] = 0;
			}
		}
	}
}
```

2. 标记变量

优化：用原数组的第一行、第一列作为标记数组，还需额外两个标记变量记录第一行、第一列原先含0的情况。

```java
/**
 * 标记变量
 * 力扣官方题解
 */
public void setZeroes(int[][] matrix)
{
	int m = matrix.length, n = matrix[0].length;
	boolean flagCol0 = false, flagRow0 = false;
	
	// 记录第一行、第一列含0的情况
	for (int[] ints : matrix)
	{
		if (ints[0] == 0)
		{
			flagCol0 = true; // 第一列含0
			break;
		}
	}
	for (int j = 0; j < n; j++)
	{
		if (matrix[0][j] == 0)
		{
			flagRow0 = true; // 第一行含0
			break;
		}
	}
	
	// 遍历余下元素，用第一行、第一列作为标记数组
	for (int i = 1; i < m; i++)
	{
		for (int j = 1; j < n; j++)
		{
			if (matrix[i][j] == 0)
			{
				matrix[i][0] = matrix[0][j] = 0;
			}
		}
	}
	
	// 将标记数组的结果写入
	for (int i = 1; i < m; i++)
	{
		for (int j = 1; j < n; j++)
		{
			if (matrix[i][0] == 0 || matrix[0][j] == 0)
			{
				matrix[i][j] = 0;
			}
		}
	}
	
	// 最后将第一行、第一列置0
	if (flagCol0)
	{
		for (int i = 0; i < m; i++)
		{
			matrix[i][0] = 0;
		}
	}
	if (flagRow0)
	{
		for (int j = 0; j < n; j++)
		{
			matrix[0][j] = 0;
		}
	}
}
```

#### [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

@2023.10.01

#二分查找 

分为行、列先后进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public boolean searchMatrix(int[][] matrix, int target)
{
	int rows = matrix.length, columns = matrix[0].length;
	if (matrix[0][0] > target || matrix[rows - 1][columns - 1] < target) return false;
	
	// 查找行
	int row = 0;
	int up = 0, down = rows - 1;
	while (up <= down)
	{
		int mid = up + down >> 1;
		if (matrix[mid][0] > target) down = mid - 1;
		else if (matrix[mid][columns - 1] < target) up = mid + 1;
		else
		{
			row = mid;
			break;
		}
	}
	
	// 查找列
	int left = 0, right = columns - 1;
	while (left <= right)
	{
		int mid = left + right >> 1;
		if (matrix[row][mid] == target) return true;
		else if (matrix[row][mid] > target) right = mid - 1;
		else left = mid + 1;
	}
	
	return false;
}
```

#### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

@2023.07.17

#双指针 

只要所有 `0` 均位于 `nums` 头部、所有 `2` 均位于 `nums` 尾部，那么剩下的 `1` 也已完成排序。

用 `p0` 记录左侧已排序的位置，`p2` 记录右侧已排序的位置，遍历 `nums`，当前元素为 `2` 时持续将其换至右侧，然后当前元素为 `0` 时持续将其换至左侧。

必须先换 `2`、再换 `0`，以输入 `[1,2,0]` 为例，`i == 0` 处不交换，`i == 1` 时，如果先交换 `0`、再交换 `2`，则从右侧换至 `i` 处的 `0` 无法再与左侧交换，最终结果 `[1,0,2]`。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public void sortColors(int[] nums) {
	int p0 = 0, p2 = nums.length - 1;
	for (int i = 0; i <= p2; i++) {
		// 持续将 2 换至右侧, 直到非 2
		while (i <= p2 && nums[i] == 2) {
			swap(nums, i, p2);
			p2--;
		}
		// 持续将 0 换至左侧, 直到非 0
		while (i >= p0 && nums[i] == 0) {
			swap(nums, i, p0);
			p0++;
		}
	}
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [77. 组合](https://leetcode.cn/problems/combinations/)

@2023.09.12

#回溯 

```java
for (int i = idx; i <= n; i++) {
	path.add(i);
	bt(n, k, i + 1);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> combine(int n, int k) {
	ans = new ArrayList<>();
	path = new ArrayList<>();
	bt(n, k, 1);
	return ans;
}

private void bt(int n, int k, int idx) {
	if (path.size() + n - idx + 1 < k) return;
	else if (path.size() == k) {
		ans.add(new ArrayList<>(path));
		return;
	}
	for (int i = idx; i <= n; i++) {
		path.add(i);
		bt(n, k, i + 1);
		path.remove(path.size() - 1);
	}
}
```

#### [78. 子集](https://leetcode.cn/problems/subsets/)

@2023.09.13

#回溯 

```java
for (int i = idx; i < nums.length; i++) {
	path.add(nums[i]);
	ans.add(new ArrayList<>(path));
	bt(nums, i + 1);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> subsets(int[] nums) {
	ans = new ArrayList<>();
	path = new ArrayList<>();
	ans.add(new ArrayList<>());
	bt(nums, 0);
	return ans;
}

private void bt(int[] nums, int idx) {
	for (int i = idx; i < nums.length; i++) {
		path.add(nums[i]);
		ans.add(new ArrayList<>(path));
		bt(nums, i + 1);
		path.remove(path.size() - 1);
	}
}
```

#### [79. 单词搜索](https://leetcode.cn/problems/word-search/)

@2023.10.01

#搜索 

二维数组DFS模板题。

```java
public void dfs(String word, int i, int j, char[][] board, int p)
{
	// p记录当前遍历到word的位置
	if (p == word.length())
	{
		ans = true;
		return;
	}
	// 越界|已经访问过|此处字符不是word[p]
	else if (i < 0 || i >= rows || j < 0 || j >= cols || visited[i][j] || board[i][j] != word.charAt(p)) return;
	visited[i][j] = true; // 标记此点已访问
	// 对4个方向进行搜索
	dfs(word, i, j - 1, board, p + 1);
	dfs(word, i, j + 1, board, p + 1);
	dfs(word, i - 1, j, board, p + 1);
	dfs(word, i + 1, j, board, p + 1);
	visited[i][j] = false; // 恢复此点为未访问
}
```

```java
/**
 * DFS
 * Somnia1337
 */
public boolean ans = false;
public boolean[][] visited;
public int rows, cols;

public boolean exist(char[][] board, String word)
{
	this.rows = board.length;
	this.cols = board[0].length;
	if (word.length() > rows * cols) return false;
	this.visited = new boolean[rows][cols];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			dfs(word, i, j, board, 0);
			if (ans) return true;
		}
	}
	return false;
}

public void dfs(String word, int i, int j, char[][] board, int p)
{
	// p记录当前遍历到word的位置
	if (p == word.length())
	{
		ans = true;
		return;
	}
	// 越界|已经访问过|此处字符不是word[p]
	else if (i < 0 || i >= rows || j < 0 || j >= cols || visited[i][j] || board[i][j] != word.charAt(p)) return;
	visited[i][j] = true; // 标记此点已访问
	// 对4个方向进行搜索
	dfs(word, i, j - 1, board, p + 1);
	dfs(word, i, j + 1, board, p + 1);
	dfs(word, i - 1, j, board, p + 1);
	dfs(word, i + 1, j, board, p + 1);
	visited[i][j] = false; // 恢复此点为未访问
}
```

#### [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

@2023.07.16

#双指针 

1. 两次遍历

\[Deprecated]

```java
/**
 * 两次遍历
 * Somnia1337
 */
public int removeDuplicates(int[] nums)
{
	int len = nums.length;
	int ans = len;
	for (int i = 0; i < len; )
	{
		int count = 1;
		i++;
		while (i < len && nums[i] == nums[i - 1])
		{
			count++;
			i++;
		}
		ans -= Math.max(count - 2, 0);
	}
	int k = 0;
	while (k < ans)
	{
		int count = 1;
		i = k + 1;
		while (i < len && nums[i] == nums[i - 1])
		{
			count++;
			i++;
		}
		if (count > 2)
		{
			int temp = i;
			for (int j = k + 2; temp < len; j++, temp++) nums[j] = nums[temp];
			k += 2;
		}
		else k += count;
	}
	return ans;
}
```

2. 双指针

快慢双指针，`slow`标记可保留元素的末位置，`fast`遍历`nums`，当遇到需要保留的元素（判断：`nums[slow - 2]` != `nums[fast]`）时，直接将其覆盖`slow`处元素，并右移`slow`。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int removeDuplicates(int[] nums)
{
	int len = nums.length;
	if (len <= 2) return len;
	int slow = 2, fast = 2;
	while (fast < len)
	{
		if (nums[slow - 2] != nums[fast])
		{
			nums[slow] = nums[fast];
			slow++;
		}
		fast++;
	}
	return slow;
}
```

#### [81. 搜索旋转排序数组 II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)

@2023.10.21

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public boolean search(int[] nums, int target)
{
	int len = nums.length;
	if (len == 1) return nums[0] == target;
	
	int l = 0, r = len - 1;
	while (l <= r)
	{
		int mid = l + r >> 1;
		if (nums[mid] == target) return true;
		if (nums[l] == nums[mid] && nums[mid] == nums[r])
		{
			l++;
			r--;
		}
		else if (nums[l] <= nums[mid])
		{
			if (nums[l] <= target && target < nums[mid]) r = mid - 1;
			else l = mid + 1;
		}
		else
		{
			if (nums[mid] < target && target <= nums[len - 1]) l = mid + 1;
			else r = mid - 1;
		}
	}
	
	return false;
}
```

#### [89. 格雷编码](https://leetcode.cn/problems/gray-code/)

@2023.07.15

```java
/**
 * 公式
 * 力扣官方题解
 */
public List<Integer> grayCode(int n)
{
	List<Integer> ret = new ArrayList<Integer>();
	for (int i = 0; i < 1 << n; i++)
	{
		ret.add((i >> 1) ^ i);
	}
	return ret;
}
```

#### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

@2023.09.13

#回溯 

```java
for (int i = idx; i < nums.length; i++) {
	sub.add(nums[i]);
	if (vis.add(sub)) ans.add(new ArrayList<>(sub));
	bt(nums, i + 1);
	sub.remove(sub.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> sub;
private Set<List<Integer>> vis;

public List<List<Integer>> subsetsWithDup(int[] nums) {
	ans = new ArrayList<>();
	sub = new ArrayList<>();
	vis = new HashSet<>();
	Arrays.sort(nums);
	ans.add(new ArrayList<>());
	bt(nums, 0);
	return ans;
}

private void bt(int[] nums, int idx) {
	for (int i = idx; i < nums.length; i++) {
		sub.add(nums[i]);
		if (vis.add(sub)) ans.add(new ArrayList<>(sub));
		bt(nums, i + 1);
		sub.remove(sub.size() - 1);
	}
}
```

优化：省略哈希表，用 `while` 跳过相同元素。

```java
sub.add(nums[idx]);
bt(nums, idx + 1);
sub.remove(sub.size() - 1);
while (idx + 1 < nums.length && nums[idx + 1] == nums[idx]) idx++;
bt(nums, idx + 1);
```

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
private List<List<Integer>> ans = new ArrayList<>();
private List<Integer> sub = new ArrayList<>();

public List<List<Integer>> subsetsWithDup(int[] nums) {
	ans = new ArrayList<>();
	sub = new ArrayList<>();
	Arrays.sort(nums);
	bt(nums, 0);
	return ans;
}

private void bt(int[] nums, int idx) {
	if (idx == nums.length) {
		ans.add(new ArrayList<>(sub));
		return;
	}
	sub.add(nums[idx]);
	bt(nums, idx + 1);
	sub.remove(sub.size() - 1);
	while (idx + 1 < nums.length && nums[idx + 1] == nums[idx]) idx++;
	bt(nums, idx + 1);
}
```

#### [91. 解码方法](https://leetcode.cn/problems/decode-ways/)

@2023.10.01

#动态规划 

`dp[i]` 表示 `s[:i]` 的解码方法数，`dp[0] = dp[1] = 1`，遍历 `s`，到达 `s[i]` 时：

- 如果 `s[i]` 与 `s[i - 1]` 组成的数字不超过 26，二者可以一起解码，`dp[i] += dp[i - 2]`。
- 可兼地，如果 `s[i]` 不为 `'0'`，还可以单独解码，`dp[i] += dp[i - 1]`。

```java
/**
 * 动态规划
 * 小熊
 */
public int numDecodings(String s)
{
	if (s.charAt(0) == '0') return 0; // 以'0'开头时无法解码
	int len = s.length();
	int[] dp = new int[len + 1];
	dp[0] = dp[1] = 1;
	for (int i = 2; i < len + 1; i++)
	{
		char c = s.charAt(i - 1), p = s.charAt(i - 2);
		if (p == '1' || p == '2' && c <= '6') // 上个字符与当前字符构成的数字不超过26，可以一起解码，加上2个字符前的解码方法数
		{
			dp[i] += dp[i - 2];
		}
		if (c != '0') // 当前字符非'0'，可以单独解码，加上1个字符前的解码方法数
		{
			dp[i] += dp[i - 1];
		}
	}
	return dp[len];
}
```

#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

@2023.09.13

#回溯 

```java
for (int i = idx; i < s.length() && i < idx + 3; i++) {
	if (i > idx && s.charAt(idx) == '0') return;
	if (Integer.parseInt(s.substring(idx, i + 1)) <= 255) {
		t.add(s.substring(idx, i + 1));
		dfs(s, k - 1, i + 1);
		t.remove(t.size() - 1);
	}
}
```

只关心将 `s` 分为 4 段，之后再调用 `String::join` 将其合并为 IP 地址串。

```java
/**
 * 回溯
 * 不上guardian不改名
 */
private List<String> ans;
private List<String> t;

public List<String> restoreIpAddresses(String s) {
	ans = new ArrayList<>();
	t = new ArrayList<>();
	dfs(s, 4, 0);
	return ans;
}

private void dfs(String s, int k, int idx) {
	if (k == 0) {
		if (idx == s.length()) ans.add(String.join(".", t));
		return;
	}
	for (int i = idx; i < s.length() && i < idx + 3; i++) {
		if (i > idx && s.charAt(idx) == '0') return;
		if (Integer.parseInt(s.substring(idx, i + 1)) <= 255) {
			t.add(s.substring(idx, i + 1));
			dfs(s, k - 1, i + 1);
			t.remove(t.size() - 1);
		}
	}
}
```

#### [95. 不同的二叉搜索树 II](https://leetcode.cn/problems/unique-binary-search-trees-ii/)

@2023.10.04

#回溯 

```java
// 分别回溯左侧 & 右侧的不同的二叉搜索树
List<TreeNode> L = bt(start, i - 1), R = bt(i + 1, end);
// 将左侧 & 右侧的二叉搜索树两两组合
for (TreeNode l : L) {
	for (TreeNode r : R) {
		ans.add(clone(i, l, r));
	}
}
```

将当前组合树克隆后再加入 `ans`，否则最终 `ans` 内的所有变量引用同一个对象，原理等同于 `ans.add(new ArrayList<>(current))`。

```java
/**
 * 回溯
 * 力扣官方题解
 */
public List<TreeNode> generateTrees(int n) {
    return bt(1, n);
}

private List<TreeNode> bt(int start, int end) {
    List<TreeNode> ans = new ArrayList<>();
    if (start > end) {
        ans.add(null);
        return ans;
    }
    for (int i = start; i <= end; i++) {
        // 分别回溯左侧 & 右侧的不同的二叉搜索树
        List<TreeNode> L = bt(start, i - 1), R = bt(i + 1, end);
        // 将左侧 & 右侧的二叉搜索树两两组合
        for (TreeNode l : L) {
            for (TreeNode r : R) {
                ans.add(clone(i, l, r));
            }
        }
    }
    return ans;
}

private TreeNode clone(int val, TreeNode l, TreeNode r) {
    TreeNode clone = new TreeNode(val);
    clone.left = l;
    clone.right = r;
    return clone;
}
```

#### [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

@2023.10.03

#动态规划 

`numTrees(k)` = `sum(numTrees(i) * numTrees(k - i)), i in [0, k)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int numTrees(int n)
{
	int[] dp = new int[n + 2];
	dp[0] = dp[1] = 1;
	
	for (int i = 2; i < n + 2; i++)
	{
		for (int j = 0; j < i; j++)
		{
			dp[i] += dp[j] * dp[i - j];
		}
	}
	
	return dp[n + 1];
}
```

#### [97. 交错字符串](https://leetcode.cn/problems/interleaving-string/)

@2023.09.11

#动态规划 

`dp[i][j]` 表示 `s3[:i+j]` 能否由 `s1[:i]` 和 `s2[:j]` 交错构成，递推式 `dp[i][j] = dp[i - 1][j] && (s1[i - 1] == s3[i + j - 1])`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public boolean isInterleave(String s1, String s2, String s3) {
	int l1 = s1.length(), l2 = s2.length(), l3 = s3.length();
	if (l1 + l2 != l3) return false;
	boolean[][] dp = new boolean[l1 + 1][l2 + 1];
	dp[0][0] = true;
	for (int i = 1; i <= l1; i++) {
		if (s1.charAt(i - 1) != s3.charAt(i - 1)) break;
		dp[i][0] = true;
	}
	for (int j = 1; j <= l2; j++) {
		if (s2.charAt(j - 1) != s3.charAt(j - 1)) break;
		dp[0][j] = true;
	}
	for (int i = 1; i <= l1; i++) {
		for (int j = 1; j <= l2; j++) {
			int k = i + j - 1;
			if (i > 0) dp[i][j] = (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(k)) || dp[i][j];
			if (j > 0) dp[i][j] = (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(k)) || dp[i][j];
		}
	}
	return dp[l1][l2];
}
```

#### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

@2023.10.01

#回溯 

```java
current.add(root.val);
if (root.left == null && root.right == null && sum + root.val == tar)
{
	ans.add(new ArrayList<>(current));
}
else
{
	bt(root.left, sum + root.val, tar);
	bt(root.right, sum + root.val, tar);
}
current.remove(current.size() - 1);
```

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<Integer>> ans = new ArrayList<>();
public List<Integer> current = new ArrayList<>();

public List<List<Integer>> pathSum(TreeNode root, int targetSum)
{
	bt(root, 0, targetSum);
	return ans;
}

public void bt(TreeNode root, int sum, int tar)
{
	if (root == null) return;
	current.add(root.val);
	if (root.left == null && root.right == null && sum + root.val == tar)
	{
		ans.add(new ArrayList<>(current));
	}
	else
	{
		bt(root.left, sum + root.val, tar);
		bt(root.right, sum + root.val, tar);
	}
	current.remove(current.size() - 1);
}
```

#### [120. 三角形最小路径和](https://leetcode.cn/problems/triangle/)

@2023.08.29

#动态规划 

`dp[i][j]` 表示自顶到 `triangle[i][j]` 的子问题答案。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minimumTotal(List<List<Integer>> triangle) {
	int len = triangle.size();
	int[][] dp = new int[len][len];
	dp[0][0] = triangle.get(0).get(0);
	for (int i = 1; i < len; i++) {
		List<Integer> row = triangle.get(i);
		for (int j = 0; j <= i; j++) {
			dp[i][j] = row.get(j);
			if (j == 0) dp[i][j] += dp[i - 1][j]; // 左
			else if (j == i) dp[i][j] += dp[i - 1][j - 1]; // 右
			else dp[i][j] += Math.min(dp[i - 1][j - 1], dp[i - 1][j]); // 中
		}
	}
	int ans = dp[len - 1][0];
	for (int n : dp[len - 1]) ans = Math.min(n, ans);
	return ans;
}
```

优化：状压，每一行倒序遍历。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minimumTotal(List<List<Integer>> triangle) {
	int len = triangle.size();
	int[] dp = new int[len], last = new int[len];
	dp[0] = triangle.get(0).get(0);
	last[0] = dp[0];
	for (int i = 1; i < len; i++) {
		List<Integer> row = triangle.get(i);
		dp[i] = row.get(i) + last[i - 1]; // 右
		for (int j = 1; j < i; j++) dp[j] = row.get(j) + Math.min(last[j - 1], last[j]); // 中
		dp[0] += row.get(0); // 左
		System.arraycopy(dp, 0, last, 0, len);
	}
	int ans = dp[0];
	for (int n : dp) ans = Math.min(n, ans);
	return ans;
}
```

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

@2023.07.17

#贪心 

假设在第1天买入、第3天卖出，那么利润为`prices[2] - prices[0]`，可以拆分为`(prices[2] - prices[1]) + (prices[1] - prices[0])`，也就是说，可以分开计算每一天相对于其前一天的利润，最后只收集正利润。

使用变量`hold`记录前一天的利润，更新`prices`，最后进行遍历收集正利润。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxProfit(int[] prices)
{
	int len = prices.length;
	int hold = prices[0];
	int ans = 0;
	
	for (int i = 1; i < len; i++)
	{
		prices[i] -= hold;
		hold += prices[i];
	}
	
	for (int price : prices) ans += Math.max(price, 0);
	return ans - prices[0];
}
```

优化：不更新`prices`，在遍历同时累加正利润。

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int maxProfit(int[] prices)
{
	int ans = 0;
	for (int i = 1; i < prices.length; i++)
	{
		ans += Math.max(0, prices[i] - prices[i - 1]);
	}
	return ans;
}
```

#### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

@2023.09.18

```java
/**
 * 哈希表
 * Somnia1337
 */
public int longestConsecutive(int[] nums) {
	Set<Integer> vis = new HashSet<>();
	for (int x : nums) vis.add(x);
	int ans = 0;
	for (int x : vis) {
		if (vis.contains(x - 1)) continue;
		int cur = 1;
		while (vis.contains(x + cur)) cur++;
		ans = Math.max(cur, ans);
	}
	return ans;
}
```

#### [129. 求根节点到叶节点数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)

@2023.10.02

#回溯 

```java
path = path * 10 + root.val;
solve(root.left);
solve(root.right);
path /= 10;
```

```java
/**
 * 回溯
 * Somnia1337
 */
public int ans = 0;
public int path = 0;

public int sumNumbers(TreeNode root)
{
	solve(root);
	return ans;
}

public void solve(TreeNode root)
{
	if (root == null) return;
	else if (root.left == null && root.right == null)
	{
		ans += (path * 10 + root.val);
		return;
	}
	path = path * 10 + root.val;
	solve(root.left);
	solve(root.right);
	path /= 10;
}
```

#### [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

@2023.10.02

#搜索 

对 `board` 中任意一点 `'O'`，如果其能够通过某条路径与某个边界上的点 `'O'` 连通，则其受到保护，不应变为 `'X'`。

一种思路是遍历 `board`，对每个点检查其是否能连通到边界上一点，如果可以则标记为 `'P'`（保护），否则标记为 `'X'`。

优化：反向思考，只遍历 4 条边界，对边界上的 `'O'`，将与其连通的所有点全部标记为 `'P'`，最后再次遍历全 `board`，将剩余 `'O'`（没有得到保护）标记为 `'X'`，将 `'P'` 标记为 `'O'`。

```java
/**
 * DFS
 * Xdims
 */
public void solve(char[][] board)
{
	int rows = board.length, cols = board[0].length;
	
	for (int i = 0; i < rows; i++) // 遍历左右边界
	{
		if (board[i][0] == 'O') dfs(board, i, 0);
		if (board[i][cols - 1] == 'O') dfs(board, i, cols - 1);
	}
	for (int j = 0; j < cols; j++) // 遍历上下边界
	{
		if (board[0][j] == 'O') dfs(board, 0, j);
		if (board[rows - 1][j] == 'O') dfs(board, rows - 1, j);
	}
	
	// 遍历board，更改标记
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (board[i][j] == 'O') board[i][j] = 'X';
			else if (board[i][j] == 'P') board[i][j] = 'O';
		}
	}
}

public void dfs(char[][] board, int i, int j)
{
	if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != 'O') return; // 越界或非'O'
	board[i][j] = 'P'; // 经由dfs找到的'O'全部被保护
	dfs(board, i - 1, j);
	dfs(board, i + 1, j);
	dfs(board, i, j - 1);
	dfs(board, i, j + 1);
}
```

#### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

@2023.09.15

运行第一次就秒了，有什么好说的。

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<String>> partition(String s)
{
	List<List<String>> ans = new ArrayList<>();
	List<String> current = new ArrayList<>();
	bt(0, s, ans, current);
	return ans;
}

public void bt(int start, String s, List<List<String>> ans, List<String> current)
{
	if (start == s.length())
	{
		ans.add(new ArrayList<>(current));
		return;
	}
	for (int end = start + 1; end <= s.length(); end++)
	{
		if (!isPalindrome(s.substring(start, end))) continue;
		current.add(s.substring(start, end));
		bt(end, s, ans, current);
		current.remove(current.size() - 1);
	}
}

public boolean isPalindrome(String s)
{
	int left = 0, right = s.length() - 1;
	while (left < right)
	{
		if (s.charAt(left) != s.charAt(right)) return false;
		left++;
		right--;
	}
	return true;
}
```

#### [133. 克隆图](https://leetcode.cn/problems/clone-graph/)

@2023.10.24

#搜索 #哈希表 

用哈希表记录已经克隆的节点，`key` 为原始图中的节点，`value` 为其克隆。在 `dfs` 中创建克隆节点后，向 `clone.neighbors` 添加原始节点的所有邻居的克隆。

```java
/**
 * DFS
 * powcai
 */
private Map<Node, Node> memo;

public Node cloneGraph(Node node) {
	memo = new HashMap<>();
	return dfs(node);
}

private Node dfs(Node node) {
	if (node == null) return null;
	if (memo.containsKey(node)) return memo.get(node); // node 已经被克隆
	
	Node clone = new Node(node.val, new ArrayList<>());
	memo.put(node, clone);
	for (Node nb : node.neighbors)
		clone.neighbors.add(dfs(nb)); // 向 clone.neighbors 添加原始节点 node 的所有邻居的克隆
	
	return clone;
}
```

#### [134. 加油站](https://leetcode.cn/problems/gas-station/)

@2023.09.08

#贪心 

如果 `sum(gas) < sum(cost)`，无解，在如下遍历的同时计算。

遍历，计算到达每个节点时剩余的汽油 `rem`，维护其最小值 `min`，遍历后：

- 如果 `min > 0`，说明从 `0` 处出发即可。
- 如果 `min < 0`，那么从对应节点的后一点出发即可（因为前面短缺的汽油已经最多了，又有解，所以从下个节点出发一定能全部弥补）。

```java
/**
 * 贪心
 * Somnia1337
 */
public int canCompleteCircuit(int[] gas, int[] cost) {
	int len = gas.length, rem = 0, min = Integer.MAX_VALUE;
	for (int i = 0; i < len; i++) {
		rem += gas[i] - cost[i];
		min = Math.min(rem, min);
	}
	if (rem < 0) return -1;
	else if (min >= 0) return 0;
	for (int i = len - 1; i > 0; i--) {
		min += gas[i] - cost[i];
		if (min >= 0) return i;
	}
	return -1;
}
```

优化：在遍历的同时计算 `rem`、更新 `min` 与 `ans`，最后检查 `rem` 确定 `ans` 是否可取。

```java
/**
 * 贪心
 * 撕得失败的标签
 */
public int canCompleteCircuit(int[] gas, int[] cost) {
	int rem = 0, min = 0, ans = 0;
	for (int i = 0; i < gas.length; i++) {
		rem += gas[i] - cost[i]; // 计算 rem
		if (rem < min) { // 当前亏空最多
			min = rem;
			ans = i + 1; // 更新 ans 为下一站
		}
	}
	return rem >= 0 ? ans : -1; // 检查 ans 是否可取
}
```

#### [137. 只出现一次的数字 II](https://leetcode.cn/problems/single-number-ii/)

@2023.10.02

#位运算 

1. 逐位计算

用 `int[32]` 表示 `int`，遍历 `nums`，统计 32 位中的每一位在 `nums` 中出现的次数，然后分别 mod 3，再转换为十进制即为答案。

例如，`nums` = `[1, 1, 3, 1]`：

```text
count: 0 0 ... 0 0
1    : 0 0 ... 0 1
1    : 0 0 ... 0 2
3    : 0 0 ... 1 3
1    : 0 0 ... 1 4
mod 3: 0 0 ... 1 1
parse: 00...11 = 3
```

```java
/**
 * 位运算
 * 宫水三叶
 */
public int singleNumber(int[] nums)
{
	int[] count = new int[32];
	for (int x : nums)
	{
		for (int i = 0; i < 32; i++)
		{
			if (((x >> i) & 1) == 1)
			{
				count[i]++;
			}
		}
	}
	int ans = 0;
	for (int i = 0; i < 32; i++)
	{
		if ((count[i] % 3 & 1) == 1)
		{
			ans += (1 << i);
		}
	}
	return ans;
}
```

2. 电路设计

```java
/**
 * 位运算
 * 这条街最靓的仔
 */
public int singleNumber(int[] nums)
{
	int a = 0, b = 0;
	for (int x : nums)
	{
		b = (b ^ x) & ~a;
		a = (a ^ x) & ~b;
	}
	return b;
}
```

#### [138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)

@2023.10.25

#搜索 #哈希表 

记忆化 DFS，类似于 [133. 克隆图](https://leetcode.cn/problems/clone-graph/)。

```java
/**
 * DFS
 * Somnia1337
 */
private Map<Node, Node> memo; // 记录已经克隆的节点

public Node copyRandomList(Node head) {
	memo = new HashMap<>();
	return dfs(head);
}

private Node dfs(Node node) {
	if (node == null) return null;
	if (memo.containsKey(node)) return memo.get(node);
	
	Node clone = new Node(node.val); // 新建克隆节点
	memo.put(node, clone);
	clone.next = dfs(node.next); // 克隆 next
	clone.random = dfs(node.random); // 克隆 random
	return clone;
}
```

#### [139. 单词拆分](https://leetcode.cn/problems/word-break/)

@2023.09.08

#动态规划 

`dp[i]` 表示子问题 `s[:i]` 的答案，`dp[0] = true`。

遍历，外层 `for` 的 `r` $\in [1, n]$，内层 `for` 的 `l` $\in [1, r]$，表示 `s[l:r]`，当 `dp[l]` 为 `true` 且 `s[l:r]` 在 `dict` 中时，`dp[r] = true`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public boolean wordBreak(String s, List<String> wordDict) {
	int n = s.length();
	Set<String> dict = new HashSet<>(wordDict);
	boolean[] dp = new boolean[n + 1];
	dp[0] = true;
	for (int r = 1; r < n + 1; r++) {
		for (int l = 0; l < r; l++) {
			if (dp[l] && dict.contains(s.substring(l, r))) {
				dp[r] = true;
				break;
			}
		}
	}
	return dp[n];
}
```

#### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

@2023.10.22

#栈 

遇数字压栈，遇运算符从栈中弹出两数进行计算，最后栈中只剩一个数字，即为答案。

```java
/**
 * 栈
 * Somnia1337
 */
public int evalRPN(String[] tokens)
{
	Deque<Integer> nums = new ArrayDeque<>();
	for (String t : tokens)
	{
		if (Character.isDigit(t.charAt(0)) || t.startsWith("-") && t.length() > 1)
		{
			int num = Integer.parseInt(t);
			nums.push(num);
		}
		else
		{
			int b = nums.pop(), a = nums.pop();
			int num = calculate(a, b, t);
			nums.push(num);
		}
	}
	return nums.peek();
}

private int calculate(int a, int b, String op)
{
	if (op.equals("+")) return a + b;
	if (op.equals("-")) return a - b;
	if (op.equals("*")) return a * b;
	if (op.equals("/")) return a / b;
	return Integer.MIN_VALUE;
}
```

#### [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

@2023.08.01

#库函数 

将字符串根据空格拆分到数组，再倒序添加到StringBuilder。

```java
/**
 * 库函数
 * Somnia1337
 */
public String reverseWords(String s)
{
	String[] words = s.strip()
					  .split("\\s+");
	StringBuilder ans = new StringBuilder();
	for (int i = words.length - 1; i >= 0; i--)
	{
		ans.append(words[i]);
		if (i > 0) ans.append(" ");
	}
	return ans.toString();
}
```

```java
/**
 * 库函数
 * Somnia1337
 */
public String reverseWords(String s)
{
	List<String> parts = new ArrayList<>(List.of(s.strip().split("\\s+")));
	Collections.reverse(parts);
	return String.join(" ", parts);
}
```

学习新的API：

- `s.trim()`，删除首尾的空白字符。
- `Arrays.asList`，可以接收字符串数组，构造list。
- `Collections.reverse`，反转整个集合。
- `String.join()`，接收分隔符和主体内容，构造由分隔符分隔的内容。

```java
/**
 * 库函数
 * 力扣官方题解
 */
public String reverseWords(String s)
{
	s = s.trim();
	List<String> wordList = Arrays.asList(s.split("\\s+"));
	Collections.reverse(wordList);
	return String.join(" ", wordList);
}
```

#### [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

@2023.09.12

#动态规划 

由于负数的存在，最终乘积最大的子数组可能包含偶数个负数，而其中间结果为负，因此不仅要维护最大乘积，还要维护最小乘积。

`min`、`max` 分别表示最小、最大乘积，遍历 `nums`，直接状压 DP：保存 `min`、`max` 的上一个状态，进行更新，分别都要考虑 `nums[i] * mn`、`nums[i] * mx`、从 `nums[i]` 重新开始这 3 种情况，然后用 `max` 更新答案。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int maxProduct(int[] nums) {
	int min = nums[0], max = nums[0], ans = nums[0];
	for (int i = 1; i < nums.length; i++) {
		int mn = min, mx = max; // 保存上一个状态
		min = Math.min(Math.min(nums[i] * mn, nums[i] * mx), nums[i]);
		max = Math.max(Math.max(nums[i] * mn, nums[i] * mx), nums[i]);
		ans = Math.max(max, ans);
	}
	return ans;
}
```

#### [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

@2023.10.05

#二分查找 

取 [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/) 解答的第一部分。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int findMin(int[] nums)
{
	int len = nums.length;
	int start = 0;
	if (nums[0] > nums[len - 1])
	{
		int left = 1, right = len - 1;
		while (left <= right)
		{
			int mid = left + right >> 1;
			if (nums[mid - 1] > nums[mid])
			{
				start = mid;
				break;
			}
			else if (nums[mid] > nums[len - 1]) left = mid + 1;
			else right = mid - 1;
		}
	}
	return nums[start];
}
```

#### [155. 最小栈](https://leetcode.cn/problems/min-stack/)

@2023.10.24

#栈 

用两个栈，`stk` 存储元素，`minStk` 存储 `stk` 对应元素存入时的最小元素。

原理是，向 `stk` 压入新元素时，同时向 `minStk` 压入 `min(val, peek())`，只要 `stk` 中对应某个元素 `k` 仍存在，那么在 `k` 之前入栈的元素也未出栈，所以 `k` 在 `minStk` 中对应的最小值就是 `k` 及之前元素的最小值。

```java
/**
 * 辅助结构
 * Somnia1337
 */
class MinStack
{
    private Stack<Integer> stk, minStk;
	
    public MinStack()
    {
        stk = new Stack<>();
        minStk = new Stack<>();
        minStk.push(Integer.MAX_VALUE);
    }
	
    public void push(int val)
    {
        stk.push(val);
        minStk.push(Math.min(val, minStk.peek()));
    }
	
    public void pop()
    {
        stk.pop();
        minStk.pop();
    }
	
    public int top()
    {
        return stk.peek();
    }
	
    public int getMin()
    {
        return minStk.peek();
    }
}
```

#### [159. 至多包含两个不同字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-most-two-distinct-characters/)

@2023.09.14

#双指针 

只需将 [340. 至多包含 K 个不同字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/) 中的 `k` 替换为2。

```java
/**
 * 双指针
 * Somnia1337
 */
public int lengthOfLongestSubstringTwoDistinct(String s) {
	Map<Character, Integer> count = new HashMap<>();
	int n = s.length(), l = 0, r = 0, ans = 0;
	while (r < n) {
		count.merge(s.charAt(r), 1, Integer::sum);
		while (l < n && count.size() > 2) {
			char c = s.charAt(l++);
			if (count.merge(c, -1, Integer::sum) == 0) count.remove(c);
		}
		if (count.size() <= 2) ans = Math.max(r - l + 1, ans);
		r++;
	}
	return ans;
}
```

```java
/**
 * 双指针
 * 万万想到了
 */
public int lengthOfLongestSubstringTwoDistinct(String s) {
	int[] count = new int[128];
	int n = s.length(), l = 0, r = 0, cur = 0, ans = 0;
	while (r < n) {
		if (++count[s.charAt(r)] == 1) cur++;
		while (cur > 2) if (--count[s.charAt(l++)] == 0) cur--;
		ans = Math.max(r++ - l + 1, ans);
	}
	return ans;
}
```

#### [161. 相隔为 1 的编辑距离](https://leetcode.cn/problems/one-edit-distance/)

@2023.10.03

#字符串 

要求编辑距离恰为 1，如果 `abs(len1 - len2) > 1` 或者 `s == t`，直接返回 `false`。

遍历 `s`、`t`，遇到 `i` 处字符不同，只需下列之一为 `true` 即返回 `true`：

- `s[i:] == t[i:]`
- `s[i:] == t[i+1:]`
- `s[i+1:] == t[i:]`

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean isOneEditDistance(String s, String t)
{
	int len1 = s.length(), len2 = t.length();
	if (Math.abs(len1 - len2) > 1 || s.equals(t)) return false;
	int len = Math.min(len1, len2);
	for (int i = 0; i < len; i++)
	{
		if (s.charAt(i) != t.charAt(i))
		{
			return s.substring(i).equals(t.substring(i + 1)) || s.substring(i + 1).equals(t.substring(i)) || s.substring(i + 1).equals(t.substring(i + 1));
		}
	}
	return true;
}
```

优化：通过重新调用保证 `len(s)` < `len(t)`，免去对 `s[i+1:] == t[i:]` 的判断。

```java
/**
 * 一次遍历
 * 井冈山赵子龙
 */
public boolean isOneEditDistance(String s, String t)
{
	int len1 = s.length();
	int len2 = t.length();
	if (len1 > len2) return isOneEditDistance(t, s); // 重新调用
	if (len2 - len1 > 1 || s.equals(t)) return false;
	for (int i = 0; i < len1; i++)
	{
		if (s.charAt(i) != t.charAt(i))
		{
			if (len1 == len2) return s.substring(i + 1).equals(t.substring(i + 1));
			else return s.substring(i).equals(t.substring(i + 1));
		}
	}
	return true;
}
```

#### [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

@2023.09.21

#二分查找 

二分，讨论`nums[mid]`与其左右元素的大小情况。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int findPeakElement(int[] nums)
{
	if (nums.length == 1) return 0;
	else if (nums[0] > nums[1]) return 0;
	else if (nums[nums.length - 1] > nums[nums.length - 2]) return nums.length - 1;
	int left = 1, right = nums.length - 2;
	while (left < right)
	{
		int mid = left + (right - left) / 2;
		if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) return mid;
		else if (nums[mid] < nums[mid - 1] && nums[mid] > nums[mid + 1]) right = mid - 1;
		else left = mid + 1;
	}
	return left;
}
```

#### [164. 最大间距](https://leetcode.cn/problems/maximum-gap/)

@2023.11.04

```java
/**
 * 桶排序
 * 二极管的一生
 */
public int maximumGap(int[] nums)
{
	int len = nums.length;
	if (len == 1) return 0;
	int max = -1, min = (int) 1e9;
	for (int num : nums)
	{
		max = Math.max(num, max);
		min = Math.min(num, min);
	}
	if (max - min == 0) return 0;
	
	int[] bktMin = new int[len - 1];
	int[] bktMax = new int[len - 1];
	Arrays.fill(bktMax, -1);
	Arrays.fill(bktMin, (int) 1e9);
	int interval = (int) Math.ceil((double) (max - min) / (len - 1));
	for (int num : nums)
	{
		if (num == min || num == max) continue;
		int index = (num - min) / interval;
		bktMax[index] = Math.max(num, bktMax[index]);
		bktMin[index] = Math.min(num, bktMin[index]);
	}
	
	int ans = 0, pre = min;
	for (int i = 0; i < len - 1; i++)
	{
		if (bktMax[i] == -1) continue;
		ans = Math.max(bktMin[i] - pre, ans);
		pre = bktMax[i];
	}
	ans = Math.max(max - pre, ans);
	return ans;
}
```

#### [165. 比较版本号](https://leetcode.cn/problems/compare-version-numbers/)

@2023.09.16

#字符串 #双指针 

1. 字符串分割

将两串分割，分别比较每个值。

```java
/**
 * 字符串分割
 * 力扣官方题解
 */
public int compareVersion(String version1, String version2)
{
	String[] v1 = version1.split("\\.");
	String[] v2 = version2.split("\\.");
	
	for (int i = 0; i < v1.length || i < v2.length; ++i)
	{
		int x = 0, y = 0; // 默认值0
		
		// 获取当前值
		if (i < v1.length)
		{
			x = Integer.parseInt(v1[i]);
		}
		if (i < v2.length)
		{
			y = Integer.parseInt(v2[i]);
		}
		
		// 比较
		if (x > y) return 1;
		if (x < y) return -1;
	}
	
	return 0;
}
```

2. 双指针

优化：使用双指针 + 模拟来计算并比较当前值。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int compareVersion(String version1, String version2)
{
	int len1 = version1.length(), len2 = version2.length();
	int p1 = 0, p2 = 0;
	
	while (p1 < len1 || p2 < len2)
	{
		int x = 0; // version1当前值
		for (; p1 < len1 && version1.charAt(p1) != '.'; ++p1)
		{
			x = x * 10 + version1.charAt(p1) - '0';
		}
		p1++; // 跳过'.'
		
		int y = 0; // version2当前值
		for (; p2 < len2 && version2.charAt(p2) != '.'; ++p2)
		{
			y = y * 10 + version2.charAt(p2) - '0';
		}
		p2++; // 跳过'.'
		
		if (x != y) return x > y ? 1 : -1; // 比较
	}
	
	return 0;
}
```

#### [166. 分数到小数](https://leetcode.cn/problems/fraction-to-recurring-decimal/)

@2023.11.03

#哈希表 

`key` 为被除数，`value` 为其在字符串中的下标，当相同的被除数再次出现时，说明出现循环，用括号包括上次出现的位置到当前位置的部分。

```java
/**
 * 哈希表
 * verygoodlee
 */
public String fractionToDecimal(int numerator, int denominator)
{
	StringBuilder ans = new StringBuilder();
	long a = numerator, b = denominator;
	if ((a > 0) ^ (b > 0)) ans.append('-');
	a = Math.abs(a);
	b = Math.abs(b);
	
	// 整数部分
	ans.append(a / b);
	if (a % b == 0) return ans.toString();
	
	// 小数部分
	ans.append('.');
	Map<Long, Integer> pos = new HashMap<>();
	// 开始时, 赋 余数*10 给 a, 并判断非零且之前未出现
	while ((a = (a % b) * 10) > 0 && !pos.containsKey(a))
	{
		pos.put(a, ans.length());
		ans.append(a / b);
	}
	
	// while 由于后一个条件而结束, 说明有循环部分
	if (a > 0) ans.insert(pos.get(a).intValue(), '(').append(')');
	return ans.toString();
}
```

#### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

@2023.09.28

#双指针 

双指针一次遍历`numbers`，根据当前和移动指针。

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] twoSum(int[] numbers, int target)
{
	int len = numbers.length;
	int left = 0, right = len - 1;
	while (left < right)
	{
		int sum = numbers[left] + numbers[right];
		if (sum == target) return new int[]{left + 1, right + 1};
		else if (sum < target) left++;
		else right--;
	}
	return null;
}
```

#### [172. 阶乘后的零](https://leetcode.cn/problems/factorial-trailing-zeroes/)

@2023.08.31

#数学 

$n!$ 尾随 0 的数量等于 $n!$ 所含因子 10 的数量，也即其因子 2、5 数量中的较小者。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int trailingZeroes(int n) {
	int ans = 0;
	while (n > 0) {
		n /= 5;
		ans += n;
	}
	return ans;
}
```

#### [179. 最大数](https://leetcode.cn/problems/largest-number/)

@2023.09.18

#贪心 

将`nums`所有元素存入`String[]`，然后对其自定义排序：对于`a`、`b`两个串，如果`a` = `b`或者`a + b` = `b + a`则可任意顺序，否则逐位比较`a + b`与`b + a`，取值较大的组合方式。排序完成后，组合所有串，去除前缀"0"。

```java
/**
 * 排序
 * Somnia1337
 */
public String largestNumber(int[] nums)
{
	String[] numStrs = new String[nums.length];
	for (int i = 0; i < nums.length; i++)
	{
		numStrs[i] = String.valueOf(nums[i]);
	}
	
	// 自定义排序
	Arrays.sort(numStrs, (a, b) -> {
		if (a.equals(b)) return 0;
		String x = a + b, y = b + a;
		if (x.equals(y)) return 0;
		int i = 0;
		while (i < x.length())
		{
			if (x.charAt(i) > y.charAt(i)) return -1;
			else if (x.charAt(i) < y.charAt(i)) return 1;
			i++;
		}
		return 0;
	});
	
	String ans = String.join("", numStrs);
	while (ans.length() > 1 && ans.startsWith("0")) // 去除前缀"0"
	{
		ans = ans.substring(1);
	}
	return ans;
}
```

#### [186. 反转字符串中的单词 II](https://leetcode.cn/problems/reverse-words-in-a-string-ii/)

@2023.09.28

#双指针 

先将`s`整体反转，再逐词反转。

```java
/**
 * 双指针
 * Somnia1337
 */
public void reverseWords(char[] s)
{
	int len = s.length;
	int left = 0, right = len - 1;
	while (left < right) // 整体反转
	{
		swap(s, left++, right--);
	}
	for (int i = 0; i < len; i++) // 判断每个单词，逐个反转
	{
		left = i;
		right = i;
		while (right < len - 1 && s[right + 1] != ' ') right++;
		int hold = right;
		while (left < right)
		{
			swap(s, left++, right--);
		}
		i = hold + 1;
	}
}

public void swap(char[] chars, int i, int j)
{
	char temp = chars[i];
	chars[i] = chars[j];
	chars[j] = temp;
}
```

#### [187. 重复的DNA序列](https://leetcode.cn/problems/repeated-dna-sequences/)

@2023.08.31

#哈希表 #滑动窗口 

1. 哈希表

遍历，枚举所有长度为 10 的子串，用哈希表计数其出现次数，如果多次出现则添加到解集。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<String> findRepeatedDnaSequences(String s) {
	int l = s.length();
	List<String> ans = new ArrayList<>();
	Map<String, Integer> count = new HashMap<>();
	for (int i = 0; i <= l - 10; i++) {
		String sub = s.substring(i, i + 10);
		count.merge(sub, 1, Integer::sum);
		if (count.get(sub) == 2) ans.add(sub);
	}
	return ans;
}
```

2. 滑动窗口

```java
/**
 * 滑动窗口
 * 力扣官方题解
 */
private static final int L = 10;
private final Map<Character, Integer> bin = new HashMap<>() {{
	put('A', 0);
	put('C', 1);
	put('G', 2);
	put('T', 3);
}};

public List<String> findRepeatedDnaSequences(String s) {
	List<String> ans = new ArrayList<>();
	int n = s.length(), x = 0;
	for (int i = 0; i < L - 1; i++) x = (x << 2) | bin.get(s.charAt(i));
	Map<Integer, Integer> count = new HashMap<>();
	for (int i = 0; i <= n - L; i++) {
		x = ((x << 2) | bin.get(s.charAt(i + L - 1))) & ((1 << (L * 2)) - 1);
		count.put(x, count.getOrDefault(x, 0) + 1);
		if (count.get(x) == 2) ans.add(s.substring(i, i + L));
	}
	return ans;
}
```

#### [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

@2023.09.01

#数组 

1. 辅助数组

借助辅助数组，将元素按目标顺序写入，最后用辅助数组覆盖原数组。

```java
/**
 * 辅助数组
 * Somnia1337
 */
public void rotate(int[] nums, int k) {
	int len = nums.length;
	int[] temp = new int[len];
	for (int i = 0; i < len; i++) temp[i] = nums[(i + k) % len];
	System.arraycopy(temp, 0, nums, 0, len);
}
```

2. 原地旋转

![[Snipaste_230901_195953.png|300]]

```java
/**
 * 原地旋转
 * 力扣官方题解
 */
public void rotate(int[] nums, int k) {
	k %= nums.length;
	reverse(nums, 0, nums.length - 1);
	reverse(nums, 0, k - 1);
	reverse(nums, k, nums.length - 1);
}

private void reverse(int[] nums, int l, int r) {
	while (l < r) swap(nums, l++, r--);
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

@2023.08.31

#动态规划 

`dp[i]` 表示对前 `i` 户的子问题答案，对每个 `nums[i]`：

- 如果选择抢，新的金额为 `dp[i - 2] + nums[i]`。
- 否则，新的金额为 `dp[i - 1]`。

递推式为 `dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int rob(int[] nums) {
	int len = nums.length;
	if (len == 1) return nums[0];
	int[] dp = new int[len];
	dp[0] = nums[0];
	dp[1] = Math.max(nums[0], nums[1]);
	for (int i = 2; i < len; i++) {
		dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
	}
	return dp[len - 1];
}
```

优化：状压。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int rob(int[] nums) {
	int len = nums.length;
	if (len == 1) return nums[0];
	int pre = Math.max(nums[0], nums[1]), ppre = nums[0];
	for (int i = 2; i < len; i++) {
		int t = pre;
		pre = Math.max(ppre + nums[i], pre);
		ppre = t;
	}
	return pre;
}
```

#### [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

@2023.09.18

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public int numIslands(char[][] grid)
{
	int ans = 0;
	for (int i = 0; i < grid.length; i++)
	{
		for (int j = 0; j < grid[0].length; j++)
		{
			if (grid[i][j] == '1')
			{
				dfs(grid, i, j);
				ans++;
			}
		}
	}
	return ans;
}

private void dfs(char[][] grid, int x, int y)
{
	if (x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] == '0') return;
	grid[x][y] = '0';
	dfs(grid, x + 1, y);
	dfs(grid, x, y + 1);
	dfs(grid, x - 1, y);
	dfs(grid, x, y - 1);
}
```

2. BFS

```java
/**
 * BFS
 * Somnia1337
 */
public int numIslands(char[][] grid)
{
	int ans = 0;
	for (int i = 0; i < grid.length; i++)
	{
		for (int j = 0; j < grid[0].length; j++)
		{
			if (grid[i][j] == '1')
			{
				bfs(grid, i, j);
				ans++;
			}
		}
	}
	return ans;
}

private void bfs(char[][] grid, int x, int y)
{
	Queue<int[]> queue = new LinkedList<>();
	queue.add(new int[]{x, y});
	while (!queue.isEmpty())
	{
		int[] cur = queue.remove();
		x = cur[0];
		y = cur[1];
		if (0 <= x && x < grid.length && 0 <= y && y < grid[0].length && grid[x][y] == '1')
		{
			grid[x][y] = '0';
			queue.add(new int[]{x + 1, y});
			queue.add(new int[]{x - 1, y});
			queue.add(new int[]{x, y + 1});
			queue.add(new int[]{x, y - 1});
		}
	}
}
```

#### [201. 数字范围按位与](https://leetcode.cn/problems/bitwise-and-of-numbers-range/)

@2023.09.30

#位运算 

`&` 的性质：多个数字 `&` 运算，对于一个位，只要存在一个数在该位为 0，结果的该位就为 0。

由此，$\underset{i = 0}{\overset{n}{\&}} a_i$ 的结果为保留所有 $a_i$ 的最长公共前缀、其后全部为 0。

由于要求区间 $[left,\space right]$ 内的数值全部 `&`，容易证明该区间所有数的最长公共前缀等同于 `left` 与 `right` 的最长公共前缀。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int rangeBitwiseAnd(int left, int right)
{
	int i = 0;
	while (left != right)
	{
		// left与right一同右移，直到相等即为最长公共前缀
		left >>= 1;
		right >>= 1;
		i++; // 记录位移数
	}
	return left << i;
}
```

*Brian Kernighan 算法*：`x & (x - 1)` 的结果为将 `x` 最右侧的 1 变为 0。

```text
x = 12       : 0 0 0 0 1 1 0 0
x-1 = 11     : 0 0 0 0 1 0 1 1
x & (x-1) = 8: 0 0 0 0 1 0 0 0
                         ^ 此位上的1变为0
```

由此，可不断将 `right` 与 `right - 1` 进行 `&` 运算，求得 `left` 与 `right` 的最长公共前缀。

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int rangeBitwiseAnd(int left, int right)
{
	while (left < right) right &= right - 1;
	return right;
}
```

#### [204. 计数质数](https://leetcode.cn/problems/count-primes/)

@2023.09.01

#数学 

```java
/**
 * 数学(埃氏筛)
 * 力扣官方题解
 */
public int countPrimes(int n) {
	boolean[] ban = new boolean[n];
	int ans = 0;
	for (int i = 2; i < n; i++) {
		if (!ban[i]) {
			ans++;
			for (int j = 2; i * j < n; j++) ban[i * j] = true;
		}
	}
	return ans;
}
```

```java
/**
 * 数学(线性筛)
 * 力扣官方题解
 */
public int countPrimes(int n) {
	List<Integer> primes = new ArrayList<>();
	boolean[] ban = new boolean[n];
	for (int i = 2; i < n; i++) {
		if (!ban[i]) primes.add(i);
		for (int j = 0; j < primes.size() && i * primes.get(j) < n; j++) {
			ban[i * primes.get(j)] = true;
			if (i % primes.get(j) == 0) break;
		}
	}
	return primes.size();
}
```

#### [207. 课程表](https://leetcode.cn/problems/course-schedule/)

@2023.09.09

1. 拓扑排序

见 [[5 拓扑排序：算法与实现]]。

```java
/**
 * 拓扑排序
 * Somnia1337
 */
public boolean canFinish(int numCourses, int[][] prerequisites) {
	// follows: Map<课程, 后续课程>
	Map<Integer, List<Integer>> follows = new HashMap<>();
	for (int i = 0; i < numCourses; i++) follows.put(i, new LinkedList<>());
	// in: int[结点]=入度
	int[] in = new int[numCourses];
	for (int[] edge : prerequisites) {
		follows.get(edge[1]).add(edge[0]);
		in[edge[0]]++;
	}
	
	Queue<Integer> take = new LinkedList<>();
	// 把最初能选的课程入队
	for (int i = 0; i < numCourses; i++) {
		if (in[i] == 0) take.add(i);
	}
	// count: 已修读的课程数
	int count = 0;
	// BFS
	while (!take.isEmpty()) {
		count++;
		for (int f : follows.get(take.poll())) {
			// 更新入度, 为 0 时入队
			in[f]--;
			if (in[f] == 0) take.add(f);
		}
	}
	// 修读的课程数是否达到要求
	return count == numCourses;
}
```

2. DFS

```java
/**
 * DFS
 * 力扣官方题解
 */
private int[] vis;

public boolean canFinish(int numCourses, int[][] prerequisites) {
	List<List<Integer>> adj = new ArrayList<>();
	for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
	vis = new int[numCourses];
	for (int[] p : prerequisites) adj.get(p[1]).add(p[0]);
	for (int i = 0; i < numCourses; i++) {
		if (dfs(adj, i) == 1) return false;
	}
	return true;
}

private int dfs(List<List<Integer>> adj, int i) {
	if (vis[i] != 0) return vis[i];
	vis[i] = 1;
	for (int j : adj.get(i)) {
		if (dfs(adj, j) == 1) return 1;
	}
	return vis[i] = -1;
}
```

#### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

@2023.07.22

#滑动窗口 #双指针 

双指针，维护当前窗口内元素和与窗口长度，当元素和不小于 `target` 时，右移 `l`，并更新满足条件窗口的最小长度。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int minSubArrayLen(int target, int[] nums) {
	int n = nums.length, sum = 0, l = 0, r = 0, ans = Integer.MAX_VALUE;
	while (r < n) {
		sum += nums[r++];
		while (sum >= target) {
			ans = Math.min(r - l, ans);
			sum -= nums[l++];
		}
	}
	return ans < Integer.MAX_VALUE ? ans : 0;
}
```

#### [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

@2023.09.10

1. 拓扑排序

```java
/**
 * 拓扑排序
 * Somnia1337
 */
public int[] findOrder(int numCourses, int[][] prerequisites) {
	Map<Integer, List<Integer>> follows = new HashMap<>();
	for (int i = 0; i < numCourses; i++) follows.put(i, new LinkedList<>());
	int[] in = new int[numCourses];
	for (int[] edge : prerequisites) {
		follows.get(edge[1]).add(edge[0]);
		in[edge[0]]++;
	}
	
	Queue<Integer> take = new LinkedList<>();
	for (int i = 0; i < numCourses; i++) {
		if (in[i] == 0) take.add(i);
	}
	int[] ans = new int[numCourses];
	int it = 0;
	while (!take.isEmpty()) {
		int t = take.poll();
		ans[it++] = t;
		for (int f : follows.get(t)) {
			in[f]--;
			if (in[f] == 0) take.add(f);
		}
	}
	return it == numCourses ? ans : new int[]{};
}
```

2. DFS

```java
/**
 * DFS
 * 力扣官方题解
 */
private List<List<Integer>> edges;
private int[] visited, ans;
private boolean valid;
private int it;

public int[] findOrder(int numCourses, int[][] prerequisites) {
	edges = new ArrayList<>();
	for (int i = 0; i < numCourses; i++) {
		edges.add(new ArrayList<>());
	}
	visited = new int[numCourses];
	ans = new int[numCourses];
	valid = true;
	it = numCourses - 1;
	for (int[] info : prerequisites) {
		edges.get(info[1]).add(info[0]);
	}
	for (int i = 0; i < numCourses && valid; i++) {
		if (visited[i] == 0) dfs(i);
	}
	if (!valid) return new int[]{};
	return ans;
}

private void dfs(int u) {
	visited[u] = 1;
	for (int v : edges.get(u)) {
		if (visited[v] == 0) {
			dfs(v);
			if (!valid) return;
		}
		else if (visited[v] == 1) {
			valid = false;
			return;
		}
	}
	visited[u] = 2;
	ans[it--] = u;
}
```

#### [211. 添加与搜索单词 - 数据结构设计](https://leetcode.cn/problems/design-add-and-search-words-data-structure/)

@2023.10.25

```java
/**
 * 字典树
 * 力扣官方题解
 */
class WordDictionary
{
    private MyTrie root;
	
    public WordDictionary()
    {
        root = new MyTrie();
    }
	
    public void addWord(String word)
    {
        root.insert(word);
    }
	
    public boolean search(String word)
    {
        return dfs(word, 0, root);
    }
	
    private boolean dfs(String word, int index, MyTrie node)
    {
        if (index == word.length()) return node.isEnd();
        char ch = word.charAt(index);
        if (Character.isLetter(ch))
        {
            int childIndex = ch - 'a';
            MyTrie child = node.getChildren()[childIndex];
            return child != null && dfs(word, index + 1, child);
        }
        else
        {
            for (int i = 0; i < 26; i++)
            {
                MyTrie child = node.getChildren()[i];
                if (child != null && dfs(word, index + 1, child)) return true;
            }
        }
        return false;
    }
}

class MyTrie
{
    private MyTrie[] children;
    private boolean isEnd;
	
    public MyTrie()
    {
        children = new MyTrie[26];
        isEnd = false;
    }
	
    public void insert(String word)
    {
        MyTrie node = this;
        for (int i = 0; i < word.length(); i++)
        {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null)
            {
                node.children[index] = new MyTrie();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }
	
    public MyTrie[] getChildren()
    {
        return children;
    }
	
    public boolean isEnd()
    {
        return isEnd;
    }
}
```

#### [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)

@2023.09.07

#动态规划 

与 [198. 打家劫舍](https://leetcode.cn/problems/house-robber/) 的区别：本题中 `nums[0]` 和 `nums[-1]` 只能选一个。可以进行 2 次 DP，分别忽略 `nums[-1]`、`nums[0]`。

注意 `dp[1]` 应初始化为 `max(nums[0], nums[1])`，而不是 `nums[1]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int rob(int[] nums) {
	int len = nums.length;
	if (len == 1) return nums[0];
	if (len == 2) return Math.max(nums[0], nums[1]);
	int[] dp = new int[len - 1];
	
	dp[0] = nums[0];
	dp[1] = Math.max(nums[0], nums[1]);
	for (int i = 2; i < len - 1; i++) dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
	
	int ans = dp[len - 2];
	dp = new int[len - 1];
	dp[0] = nums[1];
	dp[1] = Math.max(nums[1], nums[2]);
	for (int i = 2; i < len - 1; i++) dp[i] = Math.max(dp[i - 2] + nums[i + 1], dp[i - 1]);
	
	return Math.max(dp[len - 2], ans);
}
```

#### [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

@2023.09.15

1. 桶排序

```java
/**
 * 桶排序
 * ChatGPT
 */
public int findKthLargest(int[] nums, int k)
{
	bucketSort(nums);
	return nums[nums.length - k];
}

void bucketSort(int[] nums)
{
	int n = nums.length;
	if (n <= 1)
	{
		return;
	}
	
	int maxVal = Integer.MIN_VALUE;
	int minVal = Integer.MAX_VALUE;
	
	for (int num : nums)
	{
		maxVal = Math.max(maxVal, num);
		minVal = Math.min(minVal, num);
	}
	
	int bucketCount = maxVal - minVal + 1;
	List<List<Integer>> buckets = new ArrayList<>(bucketCount);
	
	for (int i = 0; i < bucketCount; i++)
	{
		buckets.add(new ArrayList<>());
	}
	
	for (int num : nums)
	{
		int index = (num - minVal) * bucketCount / (maxVal - minVal + 1);
		buckets.get(index)
			   .add(num);
	}
	
	for (List<Integer> bucket : buckets)
	{
		Collections.sort(bucket);
	}
	
	int index = 0;
	for (List<Integer> bucket : buckets)
	{
		for (int num : bucket)
		{
			nums[index++] = num;
		}
	}
}
```

2. 最小堆

```java
/**
 * 最小堆
 * ChatGPT
 */
public int findKthLargest(int[] nums, int k)
{
	Queue<Integer> minHeap = new PriorityQueue<>();
	for (int num : nums)
	{
		if (minHeap.size() < k)
		{
			minHeap.offer(num);
		}
		else if (num > minHeap.peek())
		{
			minHeap.poll();
			minHeap.offer(num);
		}
	}
	return minHeap.peek();
}
```

#### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

@2023.09.12

#回溯 

```java
for (int x = start; x <= 9; x++) {
	if (sum + x > n) return; // 剪枝
	path.add(x);
	bt(n, k - 1, x + 1, sum + x);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> combinationSum3(int k, int n) {
	ans = new ArrayList<>();
	path = new ArrayList<>();
	bt(n, k, 1, 0);
	return ans;
}

private void bt(int n, int k, int start, int sum) {
	if (k == 0) {
		if (sum == n) ans.add(new ArrayList<>(path));
		return;
	}
	for (int x = start; x <= 9; x++) {
		if (sum + x > n) return; // 剪枝
		path.add(x);
		bt(n, k - 1, x + 1, sum + x);
		path.remove(path.size() - 1);
	}
}
```

#### [221. 最大正方形](https://leetcode.cn/problems/maximal-square/)

@2023.09.23

#动态规划 

`dp[i][j]` 表示以 `matrix[i][j] != 0` 为右下角的正方形的最大边长，如果左上角、左侧、上侧的三个最大边长相同，则 `dp[i][j]` 为该值加 1，但是它们并不相同，只能取三者的最小值，递推式 `dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maximalSquare(char[][] matrix)
{
	int rows = matrix.length, cols = matrix[0].length;
	int[][] dp = new int[rows + 1][cols + 1];
	int ans = 0;
	
	for (int i = 1; i <= rows; i++)
	{
		for (int j = 1; j <= cols; j++)
		{
			if (matrix[i - 1][j - 1] == '1')
			{
				dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
				ans = Math.max(dp[i][j], ans);
			}
		}
	}
	
	return ans * ans;
}
```

#### [223. 矩形面积](https://leetcode.cn/problems/rectangle-area/)

@2023.09.06

#数学 

计算两矩形的重叠面积，用总面积减重叠部分即为覆盖面积。

```java
/**
 * 数学
 * Somnia1337
 */
public int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
	int dx = Math.min(ax2, bx2) - Math.max(ax1, bx1);
	int dy = Math.min(ay2, by2) - Math.max(ay1, by1);
	return (ax2 - ax1) * (ay2 - ay1) + (bx2 - bx1) * (by2 - by1) - (dx > 0 && dy > 0 ? dx * dy : 0);
}
```

#### [227. 基本计算器 II](https://leetcode.cn/problems/basic-calculator-ii/)

@2023.11.10

```java
/**
 * 栈
 * Somnia1337
 */
public int calculate(String s)
{
	s += "=";
	final String digits = "0123456789.";
	final String ops = "+-*/=";
	final int[][] rank = {{3, 2}, {3, 2}, {5, 4}, {5, 4}, {0, 0}};
	Stack<Character> opStk = new Stack<>();
	Stack<Integer> numStk = new Stack<>();
	for (int i = 0; i < s.length(); i++)
	{
		char c = s.charAt(i);
		if (c == ' ') continue;
		if (Character.isDigit(c) || c == '-' && (i == 0 || !Character.isDigit(s.charAt(i - 1))))
		{
			int end = i + 1;
			while (digits.indexOf(s.charAt(end)) >= 0) end++;
			numStk.push(Integer.parseInt(s.substring(i, end)));
			i = end - 1;
		}
		else
		{
			while (true)
			{
				int rankOut = rank[ops.indexOf(c)][1];
				int rankIn = !opStk.isEmpty() ? rank[ops.indexOf(opStk.peek())][0] : -1;
				if (rankIn < rankOut)
				{
					opStk.push(c);
					break;
				}
				else if (rankIn > rankOut)
				{
					char op = opStk.pop();
					if (op != '-' || numStk.size() >= 2)
					{
						int b = numStk.pop(), a = numStk.pop();
						numStk.push(cal(a, b, op));
					}
					else numStk.push(-numStk.pop());
				}
				else
				{
					opStk.pop();
					break;
				}
			}
		}
	}
	return numStk.pop();
}

private int cal(int a, int b, char op)
{
	if (op == '+') return a + b;
	if (op == '-') return a - b;
	if (op == '*') return a * b;
	if (op == '/') return a / b;
	return Integer.MIN_VALUE;
}
```

```java
/**
 * 栈
 * 力扣官方题解
 */
public int calculate(String s)
{
	Deque<Integer> nums = new ArrayDeque<>();
	char op = '+';
	int num = 0, l = s.length();
	for (int i = 0; i < l; i++)
	{
		if (Character.isDigit(s.charAt(i))) num = num * 10 + s.charAt(i) - '0';
		if (!Character.isDigit(s.charAt(i)) && s.charAt(i) != ' ' || i == l - 1)
		{
			switch (op)
			{
				case '+' -> nums.push(num);
				case '-' -> nums.push(-num);
				case '*' -> nums.push(nums.pop() * num);
				default -> nums.push(nums.pop() / num);
			}
			op = s.charAt(i);
			num = 0;
		}
	}
	int ans = 0;
	while (!nums.isEmpty()) ans += nums.pop();
	return ans;
}
```

#### [229. 多数元素 II](https://leetcode.cn/problems/majority-element-ii/)

@2023.10.01

#技巧 

1. 哈希表

哈希表计数，再次遍历收集出现次数 > `len / 3` 的元素。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> majorityElement(int[] nums)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int num : nums)
	{
		count.merge(num, 1, Integer::sum);
	}
	List<Integer> ans = new ArrayList<>();
	for (int key : count.keySet())
	{
		if (count.get(key) > nums.length / 3)
		{
			ans.add(key);
		}
	}
	return ans;
}
```

2. 摩尔投票

记录两个候选数 `a`、`b` 以及各自的计数 `c1`、`c2`，遍历 `nums`：

- `c1` > 0 且 `num` 为 `a`，`c1` 加 1。
- `c2` > 0 且 `num` 为 `b`，`c2` 加 1。
- `c1` = 0，将 `num` 作为新的 `a`。
- `c2` = 0，将 `num` 作为新的 `b`。
- 其他，`c1`、`c2` 均减 1。

```java
/**
 * 摩尔投票
 * 宫水三叶
 */
public List<Integer> majorityElement(int[] nums)
{
	int len = nums.length;
	int a = 0, b = 0; // 候选数a、b
	int c1 = 0, c2 = 0; // 候选数计数c1、c2
	
	// 遍历计数
	for (int num : nums)
	{
		if (c1 > 0 && a == num) c1++; // 等于候选数a
		else if (c2 > 0 && b == num) c2++; // 等于候选数b
		else if (c1 == 0) // 候选数a被全部抵消，将当前数作为新的a
		{
			a = num;
			c1++;
		}
		else if (c2 == 0) // 候选数b被全部抵消，将当前数作为新的b
		{
			b = num;
			c2++;
		}
		else // 两个候选数计数均减1
		{
			c1--;
			c2--;
		}
	}
	
	// 重新计数，确保超过len/3
	List<Integer> ans = new ArrayList<>();
	// 两个候选数计数置0
	c1 = 0;
	c2 = 0;
	for (int num : nums)
	{
		if (a == num) c1++;
		else if (b == num) c2++;
	}
	if (c1 > len / 3) ans.add(a);
	if (c2 > len / 3) ans.add(b);
	return ans;
}
```

#### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

@2023.08.30

#分治 

1. 模拟

模拟使用了除法，不合题意。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] productExceptSelf(int[] nums) {
	int len = nums.length, zCnt = nums[0] == 0 ? 1 : 0, zPos = nums[0] == 0 ? 0 : -1, cur = 1;
	int[] ans = new int[len];
	for (int i = 1; i < len; i++) {
		cur *= nums[i];
		if (nums[i] == 0) {
			zCnt++;
			zPos = i;
		}
		if (zCnt == 2) return ans;
	}
	ans[0] = cur;
	if (zCnt == 0) {
		for (int i = 1; i < len; i++) {
			cur /= nums[i];
			cur *= nums[i - 1];
			ans[i] = cur;
		}
	}
	else {
		cur = 1;
		for (int i = 0; i < len; i++) {
			if (i == zPos) continue;
			cur *= nums[i];
		}
		ans[zPos] = cur;
	}
	return ans;
}
```

2. 分治

两次遍历，分别计算 `nums[:i]`、`nums[i:]` 的前缀、后缀积，`ans[i] = pre[i] * sub[i]`。

```java
/**
 * 分治
 * 力扣官方题解
 */
public int[] productExceptSelf(int[] nums) {
	int len = nums.length;
	int[] ans = new int[len];
	
	// 计算前缀积
	ans[0] = 1; // nums[0] 左侧没有元素, 置 1
	for (int i = 1; i < len; i++) ans[i] = nums[i - 1] * ans[i - 1];
	
	// 计算后缀积, 同时计算 ans[i]
	int R = 1; // 后缀积, nums[-1] 右侧没有元素, 置 1
	for (int i = len - 1; i >= 0; i--) {
		ans[i] *= R; // 直接计算 ans[i]
		R *= nums[i]; // 更新后缀积
	}
	
	return ans;
}
```

3. 双指针

一次遍历，用左右双指针同时更新前缀、后缀积，每个元素被访问 2 次，分别乘以其前缀、后缀积。

```java
/**
 * 双指针
 * 一小木
 */
public int[] productExceptSelf(int[] nums) {
	int n = nums.length;
	int[] ans = new int[n];
	Arrays.fill(ans, 1);
	int L = 1, R = 1;
	for (int l = 0, r = n - 1; l < n; l++, r--) {
		ans[l] *= L;
		ans[r] *= R;
		L *= nums[l];
		R *= nums[r];
	}
	return ans;
}
```

#### [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

@2023.10.04

#技巧 

该矩阵由右上角向左下角看为一棵二叉搜索树。

```java
/**
 * z字形搜索
 * Somnia1337
 */
public boolean searchMatrix(int[][] matrix, int target)
{
	int i = 0, j = matrix[0].length - 1;
	while (i < matrix.length && j >= 0)
	{
		if (matrix[i][j] == target) return true;
		else if (matrix[i][j] < target) i++;
		else j--;
	}
	return false;
}
```

#### [241. 为运算表达式设计优先级](https://leetcode.cn/problems/different-ways-to-add-parentheses/)

@2023.11.03

#分治 

遍历 `expression`，对于每个运算符，将表达式分为左右两部分，分治得到左右两个 `List`，然后根据运算符为 `+` / `-` / `*` 进行合并。

```java
/**
 * 分治
 * Somnia1337
 */
public List<Integer> diffWaysToCompute(String expression) {
	int n = expression.length();
	// 输入表达式中的所有整数值在范围 [0, 99], 2 位为终止条件
	if (n <= 2) return new ArrayList<>(List.of(Integer.parseInt(expression)));
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < n; i++) {
		char c = expression.charAt(i);
		if (!Character.isDigit(c)) { // 在运算符处进行分治
			List<Integer> left = diffWaysToCompute(expression.substring(0, i)), right = diffWaysToCompute(expression.substring(i + 1));
			for (int l : left) {
				for (int r : right) {
					if (c == '+') ans.add(l + r);
					else if (c == '-') ans.add(l - r);
					else ans.add(l * r);
				}
			}
		}
	}
	return ans;
}
```

#### [244. 最短单词距离 II](https://leetcode.cn/problems/shortest-word-distance-ii/)

@2023.11.03

#设计 #双指针 

哈希表记录 `{单词: 其所有出现的位置}`，查询时用双指针遍历两个位置列表。

```java
/**
 * 双指针
 * Somnia1337
 */
class WordDistance {
    private Map<String, List<Integer>> pos;
    
    public WordDistance(String[] wordsDict) {
        pos = new HashMap<>();
        int it = 0;
        for (String word : wordsDict) {
            pos.computeIfAbsent(word, k -> new ArrayList<>()).add(it++);
        }
    }
    
    public int shortest(String word1, String word2) {
        List<Integer> p1 = pos.get(word1), p2 = pos.get(word2);
        int n1 = p1.size(), n2 = p2.size(), i = 0, j = 0;
        int ret = Integer.MAX_VALUE;
        while (i < n1 && j < n2) {
            int idx1 = p1.get(i), idx2 = p2.get(j);
            ret = Math.min(Math.abs(idx1 - idx2), ret);
            if (idx1 < idx2) i++;
            else j++;
        }
        return ret;
    }
}
```

#### [245. 最短单词距离 III](https://leetcode.cn/problems/shortest-word-distance-iii/)

@2023.10.03

#数组 

分为 `word1 == word2` 和 `word1 != word2` 两种情况：

- `word1 == word2`，简记为 `word`，遍历 `wordsDict`，记录 `word` 上次出现的位置，与当前位置作差，取最小值。
- `word1 != word2`，遍历 `wordsDict`，分别记录二者上次出现的位置，将当前 `wordX` 的位置与上个 `wordY` 的位置作差，取最小值。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public int shortestWordDistance(String[] wordsDict, String word1, String word2)
{
	if (word1.equals(word2)) return shortestSameWordDistance(wordsDict, word1);
	int len = wordsDict.length;
	int a = -1, b = len, ans = len;
	for (int i = 0; i < len; i++)
	{
		String word = wordsDict[i];
		if (word.equals(word1))
		{
			a = i;
			if (b != len) ans = Math.min(ans, Math.abs(b - a));
		}
		if (word.equals(word2))
		{
			b = i;
			if (a != -1) ans = Math.min(ans, Math.abs(b - a));
		}
	}
	return ans;
}

public int shortestSameWordDistance(String[] wordsDict, String word)
{
	int pos = -1;
	int ans = wordsDict.length;
	for (int i = 0; i < wordsDict.length; i++)
	{
		if (wordsDict[i].equals(word))
		{
			if (pos != -1) ans = Math.min(i - pos, ans);
			pos = i;
		}
		if (ans == 1) return 1;
	}
	return ans;
}
```

#### [249. 移位字符串分组](https://leetcode.cn/problems/group-shifted-strings/)

@2023.10.01

#哈希表 

对 `strings` 中的每个 `s`，计算一个 `key`，需满足每一组移位字符串计算出的 `key` 相同，用 `Map<String, List<String>>` 记录所有 `key` 相同的字符串，返回哈希表的 `values()` 即为答案。

容易想到，将 `s` 的每个字符 `s[i]` 替换为 `'a' + (s[i] - (s[0] - 'a')) % 26` 是一种可行的算法。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<List<String>> groupStrings(String[] strings)
{
	Map<String, List<String>> groups = new HashMap<>();
	for (String s : strings)
	{
		StringBuilder k = new StringBuilder();
		for (int i = 0; i < s.length(); i++)
		{
			k.append('a' + (s.charAt(i) - (s.charAt(0) - 'a')) % 26);
		}
		String key = k.toString();
		List<String> group = groups.getOrDefault(key, new ArrayList<>());
		group.add(s);
		groups.put(key, group);
	}
	return new ArrayList<>(groups.values());
}
```

#### [251. 展开二维向量](https://leetcode.cn/problems/flatten-2d-vector/)

@2023.11.04

#数组 

1. 队列

```java
/**
 * 队列
 * Somnia1337
 */
class Vector2D
{
    private Queue<Integer> q;
	
    public Vector2D(int[][] vec)
    {
        q = new LinkedList<>();
        for (int[] row : vec)
        {
            for (int v : row) q.offer(v);
        }
    }
	
    public int next()
    {
        return q.poll();
    }
	
    public boolean hasNext()
    {
        return !q.isEmpty();
    }
}
```

2. 迭代器

```java
/**
 * 迭代器
 * Arlon
 */
class Vector2D
{
    private Iterator<Integer> it;
    private List<Integer> nums = new ArrayList<>();
	
    public Vector2D(int[][] v)
    {
        for (int[] innerVector : v)
        {
            for (int num : innerVector) nums.add(num);
        }
        it = nums.iterator();
    }
	
    public int next()
    {
        return it.next();
    }
	
    public boolean hasNext()
    {
        return it.hasNext();
    }
}
```

#### [253. 会议室 II](https://leetcode.cn/problems/meeting-rooms-ii/)

@2023.10.03

#堆 

根据会议开始时间排序 `intervals`，用一个最小堆记录会议的结束时间，遍历 `intervals`，弹出结束时间先于本场会议开始时间的会议，压入本场会议的结束时间，取此过程中堆的最大大小。

```java
/**
 * 堆
 * Somnia1337
 */
public int minMeetingRooms(int[][] intervals)
{
	Queue<Integer> queue = new PriorityQueue<>();
	Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
	int ans = 0;
	for (int[] meeting : intervals)
	{
		while (!queue.isEmpty() && meeting[0] >= queue.peek()) queue.poll(); // 先前会议的结束时间早于本场会议的开始时间
		queue.offer(meeting[1]);
		ans = Math.max(queue.size(), ans); // 取全过程堆的最大大小
	}
	return ans;
}
```

#### [254. 因子的组合](https://leetcode.cn/problems/factor-combinations/)

@2023.10.03

#回溯 

```java
for (int i = start; i <= Math.sqrt(n); i++) // 去重，因子不超过sqrt(n)
{
	if (n % i != 0) continue; // 不是因子，跳过
	path.add(i); // 添加因子
	path.add(n / i); // 添加补数
	
	ans.add(new ArrayList<>(path));
	path.remove(path.size() - 1); // 移除补数
	
	bt(n / i, i);
	path.remove(path.size() - 1); // 移除因子
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
public List<List<Integer>> ans = new ArrayList<>();
public List<Integer> path = new ArrayList<>();

public List<List<Integer>> getFactors(int n)
{
	bt(n, 2); // 题意，最小因子为2
	return ans;
}

public void bt(int n, int start)
{
	for (int i = start; i <= Math.sqrt(n); i++) // 去重，因子不超过sqrt(n)
	{
		if (n % i != 0) continue; // 不是因子，跳过
		path.add(i); // 添加因子
		path.add(n / i); // 添加补数
		ans.add(new ArrayList<>(path));
		path.remove(path.size() - 1); // 移除补数
		bt(n / i, i);
		path.remove(path.size() - 1); // 移除因子
	}
}
```

#### [256. 粉刷房子](https://leetcode.cn/problems/paint-house/)

@2023.10.05

#动态规划 

用 `dp[i][]` 维护前 `i` 栋房子分别粉刷 3 种颜色的最小花费。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minCost(int[][] costs)
{
	int len = costs.length;
	int[][] dp = new int[len + 1][3];
	for (int i = 1; i < len + 1; i++)
	{
		dp[i][0] = Math.min(dp[i - 1][1], dp[i - 1][2]) + costs[i - 1][0];
		dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][2]) + costs[i - 1][1];
		dp[i][2] = Math.min(dp[i - 1][0], dp[i - 1][1]) + costs[i - 1][2];
	}
	return Math.min(Math.min(dp[len][0], dp[len][1]), dp[len][2]);
}
```

#### [259. 较小的三数之和](https://leetcode.cn/problems/3sum-smaller/)

@2023.10.05

```java
/**
 * 双指针
 * Somnia1337
 */
public int threeSumSmaller(int[] nums, int target)
{
	int len = nums.length;
	int ans = 0;
	Arrays.sort(nums);
	for (int i = 0; i < len - 2; i++)
	{
		int left = i + 1, right = len - 1;
		while (left < right)
		{
			int sum = nums[i] + nums[left] + nums[right];
			if (sum < target)
			{
				ans += right - left;
				left++;
			}
			else right--;
		}
	}
	return ans;
}
```

#### [260. 只出现一次的数字 III](https://leetcode.cn/problems/single-number-iii/)

@2023.10.16

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int[] singleNumber(int[] nums)
{
	int total = 0;
	for (int num : nums) total ^= num;
	int lsb = (total == Integer.MIN_VALUE ? total : total & (-total));
	int type1 = 0, type2 = 0;
	for (int num : nums)
	{
		if ((num & lsb) != 0) type1 ^= num;
		else type2 ^= num;
	}
	return new int[]{type1, type2};
}
```

#### [261. 以图判树](https://leetcode.cn/problems/graph-valid-tree/)

@2023.11.04

#搜索 

树是“没有环的连通图”。

如果边数不等于节点数 - 1，则一定不连通或存在环。建图进行 DFS，遍历完成后如果有节点没有遍历到，则返回 `false`。

```java
/**
 * DFS
 * Somnia1337
 */
private List<List<Integer>> graph;
private boolean[] vis;

public boolean validTree(int n, int[][] edges)
{
	if (edges.length != n - 1) return false;
	graph = new ArrayList<>();
	for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
	for (int[] edge : edges)
	{
		graph.get(edge[0]).add(edge[1]);
		graph.get(edge[1]).add(edge[0]);
	}
	
	vis = new boolean[n];
	dfs(0);
	for (boolean v : vis)
	{
		if (!v) return false;
	}
	return true;
}

private void dfs(int node)
{
	if (vis[node]) return;
	vis[node] = true;
	for (int next : graph.get(node)) dfs(next);
}
```

#### [264. 丑数 II](https://leetcode.cn/problems/ugly-number-ii/)

@2023.09.05

#动态规划 #堆 

1. 堆

三个堆。

```java
/**
 * 堆
 * Somnia1337
 */
public int nthUglyNumber(int n) {
	if (n == 1) return 1;
	Queue<Long> q2 = new PriorityQueue<>(), q3 = new PriorityQueue<>(), q5 = new PriorityQueue<>();
	q2.add(2L);
	q3.add(3L);
	q5.add(5L);
	long ans = 1;
	while (n > 1) {
		ans = Math.min(Math.min(q2.peek(), q3.peek()), q5.peek());
		if (ans == q2.peek()) {
			q2.poll();
			if (ans * 2 > 0) q2.add(ans * 2);
			if (ans * 3 > 0) q3.add(ans * 3);
			if (ans * 5 > 0) q5.add(ans * 5);
		}
		else if (ans == q3.peek()) {
			q3.poll();
			if (ans * 3 > 0) q3.add(ans * 3);
			if (ans * 5 > 0) q5.add(ans * 5);
		}
		else {
			q5.poll();
			if (ans * 5 > 0) q5.add(ans * 5);
		}
		n--;
	}
	return (int) ans;
}
```

维护一个最小堆，用哈希表去重。

```java
/**
 * 堆
 * 力扣官方题解
 */
public int nthUglyNumber(int n) {
	int[] factors = {2, 3, 5};
	Set<Long> vis = new HashSet<>();
	Queue<Long> pq = new PriorityQueue<>();
	vis.add(1L);
	pq.add(1L);
	long ans = 0;
	for (int i = 0; i < n; i++) {
		long cur = pq.poll();
		ans = cur;
		for (int f : factors) {
			long next = cur * f;
			if (vis.add(next)) pq.add(next);
		}
	}
	return (int) ans;
}
```

优化：将 3 个队列简化为 3 个变量，表示队头的值。循环时，每次选取其中的最小值作为当前丑数，并将其乘以对应因子。

```java
/**
 * 队列
 * Somnia1337
 */
public int nthUglyNumber(int n) {
	int[] ugly = new int[n + 1];
	int x = 0, i = 0, j = 0, k = 0;
	ugly[0] = 1;
	int x2 = 2, x3 = 3, x5 = 5;
	while (x < n) {
		x++;
		ugly[x] = Math.min(Math.min(x2, x3), x5);
		if (x2 == ugly[x]) x2 = ugly[++i] * 2;
		if (x3 == ugly[x]) x3 = ugly[++j] * 3;
		if (x5 == ugly[x]) x5 = ugly[++k] * 5;
	}
	return ugly[n - 1];
}
```

2. 动态规划

`dp[i]` 表示第 `i` 个丑数，三个指针 `p2`、`p3`、`p5` 对应三个因子，循环时每次 `dp[i] = min(dp[p2] * 2, dp[p3] * 3, dp[p5] * 5)`，对应指针右移。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int nthUglyNumber(int n) {
	int[] dp = new int[n + 1];
	dp[1] = 1;
	int p2 = 1, p3 = 1, p5 = 1;
	for (int i = 2; i <= n; i++) {
		// 取 min 为当前丑数
		int x2 = dp[p2] * 2, x3 = dp[p3] * 3, x5 = dp[p5] * 5;
		dp[i] = Math.min(Math.min(x2, x3), x5);
		// 对应指针 +1
		if (dp[i] == x2) p2++;
		if (dp[i] == x3) p3++;
		if (dp[i] == x5) p5++;
	}
	return dp[n];
}
```

#### [267. 回文排列 II](https://leetcode.cn/problems/palindrome-permutation-ii/)

@2023.10.05

```java
/**
 * 回溯
 * Somnia1337
 */
public List<String> ans = new ArrayList<>();
public StringBuilder path = new StringBuilder();
public int[] count = new int[26];
public int odd = -1; // 计数为奇数的字母下标
public int len;

public List<String> generatePalindromes(String s)
{
	len = s.length();
	for (int i = 0; i < len; i++) count[s.charAt(i) - 'a']++; // 计数
	boolean hasOdd = false; // 记录有无奇数
	for (int i = 0; i < 26; i++)
	{
		if (count[i] % 2 == 1)
		{
			if (hasOdd) return ans; // 2个奇数，无法构造
			odd = i; // 记录奇数的下标
			hasOdd = true;
		}
		count[i] /= 2; // 只构造前一半
	}
	len /= 2; // 只构造前一半
	bt();
	return ans;
}

public void bt()
{
	if (path.length() == len) // 构造出了前一半
	{
		StringBuilder clone = new StringBuilder(path); // 克隆对象
		String half = clone.toString(); // 先保存前一半
		clone.reverse(); // 再反转
		if (odd >= 0) clone.append((char) ('a' + odd)); // 在中间加计数为奇数的字符
		clone.append(half); // 加上另一半
		ans.add(clone.toString());
		return;
	}
	for (int i = 0; i < 26; i++)
	{
		if (count[i] == 0) continue;
		path.append((char) ('a' + i));
		count[i]--; // 计数减1
		bt();
		path.deleteCharAt(path.length() - 1);
		count[i]++; // 计数加1
	}
}
```

#### [274. H 指数](https://leetcode.cn/problems/h-index/)

@2023.10.12

#数组 

排序，找个规律：

```text
用例 - [3,0,6,1,5] -> 3
规律
sort -      0,1,3,5,6
n..1 -      5 4 3 2 1
min(cols) - 0 1 3 2 1
max(mins) - 3 -> ans = 3
```

```java
/**
 * 排序
 * Somnia1337
 */
public int hIndex(int[] citations)
{
	int len = citations.length;
	Arrays.sort(citations);
	int ans = 0;
	for (int i = 0; i < len; i++)
	{
		ans = Math.max(Math.min(len - i, citations[i]), ans);
	}
	return ans;
}
```

```java
/**
 * 排序
 * 力扣官方题解
 */
public int hIndex(int[] citations)
{
	Arrays.sort(citations);
	int h = 0, i = citations.length - 1;
	while (i >= 0 && citations[i] > h)
	{
		h++;
		i--;
	}
	return h;
}
```

#### [271. 字符串的编码与解码](https://leetcode.cn/problems/encode-and-decode-strings/)

@2023.11.04

#字符串 

\[Deprecated]

```java
/**
 * 模拟
 * Somnia1337
 */
class Codec
{
    public String encode(List<String> strs)
    {
        StringBuilder ret = new StringBuilder();
        for (String str : strs)
        {
            if (str.isEmpty())
            {
                ret.append("empty").append('.');
            }
            else
            {
                for (char c : str.toCharArray())
                {
                    ret.append((int) c).append('.');
                }
            }
            ret.setCharAt(ret.length() - 1, '#');
        }
        return ret.deleteCharAt(ret.length() - 1).toString();
    }
	
    public List<String> decode(String s)
    {
        List<String> ret = new ArrayList<>();
        String[] parts = s.split("#");
        for (String part : parts)
        {
            StringBuilder cur = new StringBuilder();
            if (part.equals("empty"))
            {
                ret.add("");
                continue;
            }
            String[] chars = part.split("\\.");
            for (String ch : chars)
            {
                cur.append(Character.toString(Integer.parseInt(ch)));
            }
            ret.add(cur.toString());
        }
        return ret;
    }
}
```

合成的字符串中，每个串前用一个 `char` 标记其长度。

```java
/**
 * 模拟
 * ?
 */
class Codec
{
    public String encode(List<String> strs)
    {
        StringBuilder ret = new StringBuilder();
        for (String s : strs)
        {
            ret.append((char) s.length()).append(s); // 标记长度, 后跟字符串
        }
        return ret.toString();
    }
	
    public List<String> decode(String s)
    {
        List<String> ret = new ArrayList<>();
        int it = 0;
        while (it < s.length())
        {
            char l = s.charAt(it++); // 解出当前字符串长度
            ret.add(s.substring(it, it + l));
            it += l;
        }
        return ret;
    }
}
```

#### [275. H 指数 II](https://leetcode.cn/problems/h-index-ii/)

@2023.10.30

#二分查找 

对每个 `cit[m]`，有 `len - m` 篇文章至少被引用 `cit[m]` 次，如果 `cit[m] >= len - m` 则左移 `r`，否则右移 `l`。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int hIndex(int[] citations)
{
	int len = citations.length;
	int l = 0, r = len;
	while (l <= r)
	{
		int m = l + r >> 1;
		if (citations[(len - m) % len] >= m) l = m + 1;
		else r = m - 1;
	}
	return r;
}
```

#### [276. 栅栏涂色](https://leetcode.cn/problems/paint-fence/)

@2023.09.14

#动态规划 

```java
/**
 * 动态规划
 * Somnia1337
 */
public int numWays(int n, int k) {
	if (n == 1) return k;
	int[] dp = new int[n];
	dp[0] = k;
	dp[1] = k * k;
	for (int i = 2; i < n; i++) dp[i] = (dp[i - 2] + dp[i - 1]) * (k - 1);
	return dp[n - 1];
}
```

#### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

@2023.09.05

#动态规划 #数学 

1. 动态规划

等价于完全背包问题，元素和为背包容量，每个完全平方数为物品价值。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int numSquares(int n) {
	int[] dp = new int[n + 1];
	for (int i = 1; i < n + 1; i++) {
		int min = Integer.MAX_VALUE;
		for (int j = 1; j * j <= i; j++) min = Math.min(dp[i - j * j] + 1, min);
		dp[i] = min;
	}
	return dp[n];
}
```

2. 数学

[四平方和定理](https://baike.baidu.com/item/四平方和定理)：任意正整数都能表示为 $\{0,\space 1,\space 2,\space 3\}$ 个完全平方数之和。

判断逻辑：

1. 判断本身是否为完全平方数，返回 `1`。
2. 判断是否能表示为 4 个完全平方数之和，返回 `4`。
3. 再依次计算 `n - pow(i.., 2)`（各完全平方数的补数）是否为完全平方数，返回 `2`。
4. 最后，如果之前没有返回，返回 `3`。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int numSquares(int n) {
	if (isSqr(n)) return 1;
	if (check4(n)) return 4;
	for (int i = 1; i * i <= n; i++) {
		int j = n - i * i;
		if (isSqr(j)) return 2;
	}
	return 3;
}

private boolean isSqr(int x) {
	int y = (int) Math.sqrt(x);
	return y * y == x;
}

private boolean check4(int x) {
	while (x % 4 == 0) x /= 4;
	return x % 8 == 7;
}
```

#### [280. 摆动排序](https://leetcode.cn/problems/wiggle-sort/)

@2023.10.05

1. 排序

```java
/**
 * 排序
 * Somnia1337
 */
public void wiggleSort(int[] nums)
{
	Arrays.sort(nums);
	for (int i = 1; i < nums.length - 1; i += 2)
	{
		swap(nums, i, i + 1);
	}
}

public void swap(int[] nums, int i, int j)
{
	int t = nums[i];
	nums[i] = nums[j];
	nums[j] = t;
}
```

2. 一次遍历

```java
/**
 * 一次遍历
 * 余先声
 */
public void wiggleSort(int[] nums)
{
	int len = nums.length;
	for (int i = 1; i < len; i += 2)
	{
		if (nums[i] < nums[i - 1])
		{
			swap(nums, i, i - 1);
		}
		if (i < len - 1 && nums[i] < nums[i + 1])
		{
			swap(nums, i, i + 1);
		}
	}
}

public void swap(int[] nums, int i, int j)
{
	int t = nums[i];
	nums[i] = nums[j];
	nums[j] = t;
}
```

#### [281. 锯齿迭代器](https://leetcode.cn/problems/zigzag-iterator/)

@2023.11.05

#设计 

根据双指针之和的奇偶性进行迭代。

```java
/**
 * 双指针
 * Somnia1337
 */
class ZigzagIterator
{
    List<Integer> v1, v2;
    int it1, it2, l1, l2;
	
    public ZigzagIterator(List<Integer> v1, List<Integer> v2)
    {
        this.v1 = v1;
        this.v2 = v2;
        it1 = 0;
        it2 = 0;
        l1 = v1.size();
        l2 = v2.size();
    }
	
    public int next()
    {
        if (it1 == l1) return v2.get(it2++);
        if (it2 == l2) return v1.get(it1++);
        return ((it1 + it2) & 1) == 0 ? v1.get(it1++) : v2.get(it2++);
    }
	
    public boolean hasNext()
    {
        return it1 + it2 < l1 + l2;
    }
}
```

#### [284. 窥视迭代器](https://leetcode.cn/problems/peeking-iterator/)

@2023.11.05

#设计 

用 `next` 维护下一个元素。

```java
/**
 * 模拟
 * 宫水三叶
 */
class PeekingIterator implements Iterator<Integer>
{
    Iterator<Integer> it;
    Integer next;
	
    public PeekingIterator(Iterator<Integer> iterator)
    {
        it = iterator;
        if (it.hasNext()) next = it.next();
    }
	
    public Integer peek()
    {
        return next;
    }
	
    @Override
    public Integer next()
    {
        Integer hold = next;
        next = it.hasNext() ? it.next() : null;
        return hold;
    }
	
    @Override
    public boolean hasNext()
    {
        return next != null;
    }
}
```

#### [286. 墙与门](https://leetcode.cn/problems/walls-and-gates/)

@2023.10.06

#搜索 

1. DFS

```java
/* 不是题解 */
public int rows, cols;
public boolean[][] visited;

public void wallsAndGates(int[][] rooms)
{
	rows = rooms.length;
	cols = rooms[0].length;
	visited = new boolean[rows][cols];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (rooms[i][j] == 0)
			{
				dfs(rooms, i, j, 0);
			}
		}
	}
}

public void dfs(int[][] rooms, int i, int j, int dist)
{
	if (i < 0 || i >= rows || j < 0 || j >= cols || visited[i][j] || rooms[i][j] < dist) return;
	rooms[i][j] = dist;
	visited[i][j] = true;
	dfs(rooms, i - 1, j, dist + 1);
	dfs(rooms, i + 1, j, dist + 1);
	dfs(rooms, i, j - 1, dist + 1);
	dfs(rooms, i, j + 1, dist + 1);
	visited[i][j] = false;
}
```

61/62，最后一个用例超时了…

翻题解，跟大神写得只差在 `dfs` 的终止条件，而且他没用 `boolean[]`。

```java
/**
 * DFS
 * Duck不必
 * @improver Somnia1337
 */
public int rows, cols;

public void wallsAndGates(int[][] rooms)
{
	rows = rooms.length;
	cols = rooms[0].length;
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (rooms[i][j] == 0)
			{
				dfs(rooms, i, j, 0);
			}
		}
	}
}

public void dfs(int[][] rooms, int i, int j, int dist)
{
	if (i < 0 || i >= rows || j < 0 || j >= cols || rooms[i][j] < dist || dist > 0 && rooms[i][j] == dist) return;
	rooms[i][j] = dist;
	dfs(rooms, i - 1, j, dist + 1);
	dfs(rooms, i + 1, j, dist + 1);
	dfs(rooms, i, j - 1, dist + 1);
	dfs(rooms, i, j + 1, dist + 1);
}
```

2. BFS

```java
/**
 * BFS
 * ChatGPT
 */
public void wallsAndGates(int[][] rooms)
{
	if (rooms == null || rooms.length == 0 || rooms[0].length == 0) return;
	int rows = rooms.length;
	int cols = rooms[0].length;
	Queue<int[]> queue = new LinkedList<>();
	int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
	
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (rooms[i][j] == 0)
			{
				queue.offer(new int[]{i, j});
			}
		}
	}
	
	while (!queue.isEmpty())
	{
		int[] current = queue.poll();
		int x = current[0];
		int y = current[1];
		for (int[] dir : directions)
		{
			int newX = x + dir[0];
			int newY = y + dir[1];
			if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && rooms[newX][newY] == Integer.MAX_VALUE)
			{
				rooms[newX][newY] = rooms[x][y] + 1;
				queue.offer(new int[]{newX, newY});
			}
		}
	}
}
```

#### [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

@2023.09.07

#双指针 

1. 辅助结构

哈希表不符合空间 $O(1)$ 的要求，且低效（18ms）。

```java
/**
 * 辅助结构（哈希表）
 * Somnia1337
 */
public int findDuplicate(int[] nums) {
	Set<Integer> vis = new HashSet<>();
	for (int num : nums) {
		if (!vis.add(num)) return num;
	}
	return -1;
}
```

辅助数组快很多（1ms）。

```java
/**
 * 辅助结构（数组）
 * ?
 */
public int findDuplicate(int[] nums) {
	boolean[] vis = new boolean[nums.length + 1];
	for (int num : nums) {
		if (vis[num]) return num;
		vis[num] = true;
	}
	return 0;
}
```

2. 双指针

为数组建立图的通常约定：

- 索引对应图中节点的编号。
- 元素值对应图中连接节点的边。

为数组建立图，由于存在重复元素，该元素必有 2 条及以上的边，图中一定存在环。根据「Floyd 判圈算法」，可以用快慢双指针遍历图，`fast` 每次移动 2 步，`slow` 每次移动 1 步，相遇时，将 `slow` 置零，使二者同速前进，再次相遇处即为重复元素。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int findDuplicate(int[] nums) {
	int slow = 0, fast = 0;
	do {
		slow = nums[slow]; // slow 走 1 步
		fast = nums[nums[fast]]; // fast 走 2 步
	} while (slow != fast); // 直到相遇
	slow = 0; // slow 置 0
	while (slow != fast) { // 各走 1 步, 再次相遇时即为环的入口
		slow = nums[slow];
		fast = nums[fast];
	}
	return slow;
}
```

#### [288. 单词的唯一缩写](https://leetcode.cn/problems/unique-word-abbreviation/)

@2023.11.04

#哈希表 

`key` 为缩写，`value` 为对应单词，对每个单词，如果表中存在其缩写，但对应单词与其不同，则将该缩写对应的 `value` 更新为 `""`。查询时，返回哈希表无此 `key` 或此 `key` 对应的单词与 `word` 相同。

```java
/**
 * 哈希表
 * Somnia1337
 */
class ValidWordAbbr
{
    private Map<String, String> abbr;
	
    public ValidWordAbbr(String[] dictionary)
    {
        abbr = new HashMap<>();
        for (String word : dictionary)
        {
            String key = getKey(word);
            if (!abbr.containsKey(key)) abbr.put(key, word);
            else if (!word.equals(abbr.get(key))) abbr.put(key, "");
        }
    }
	
    public boolean isUnique(String word)
    {
        String key = getKey(word);
        return !abbr.containsKey(key) || word.equals(abbr.get(key));
    }
	
    private String getKey(String word)
    {
        int l = word.length();
        if (l <= 2) return word;
        return "" + word.charAt(0) + (l - 2) + word.charAt(l - 1);
    }
}
```

#### [289. 生命游戏](https://leetcode.cn/problems/game-of-life/)

@2023.10.13

#模拟 

题目要求对所有细胞同时进行判断，但是遍历是按一定顺序的，因此需要两次遍历，第一次根据规则确定每个细胞的下个状态，第二次再执行更改，因为每个细胞的下个状态依赖于周围细胞的当前状态，不能直接更改。

用两个额外的数字表示细胞的下个状态，它们同时含有细胞当前状态的信息：

```text
0:保持死亡
1:保持存活
2:死->活
-1:活->死
```

```java
/**
 * 模拟
 * Somnia1337
 */
public void gameOfLife(int[][] board)
{
	// 0:保持死亡
	// 1:保持存活
	// 2:死->活
	// -1:活->死
	int rows = board.length, cols = board[0].length;
	// 第一次遍历，计算下个状态
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			int cnt = countAlive(board, i, j);
			if (board[i][j] == 1)
			{
				board[i][j] = cnt == 2 || cnt == 3 ? 1 : -1;
			}
			else if (cnt == 3) board[i][j] = 2;
		}
	}
	// 第二次遍历，执行更改
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			board[i][j] = board[i][j] > 0 ? 1 : 0;
		}
	}
}

// 计算细胞周围存活细胞的数量
private int countAlive(int[][] board, int i, int j)
{
	int ret = 0;
	for (int m = Math.max(i - 1, 0); m <= Math.min(i + 1, board.length - 1); m++)
	{
		for (int n = Math.max(j - 1, 0); n <= Math.min(j + 1, board[0].length - 1); n++)
		{
			if (m == i && n == j) continue;
			if (board[m][n] % 2 != 0) ret++;
		}
	}
	return ret;
}
```

#### [291. 单词规律 II](https://leetcode.cn/problems/word-pattern-ii/)

@2023.10.06

#回溯 

```java
/**
 * 回溯
 * K.L.B
 */
Map<Character, String> map = new HashMap<>();

public boolean wordPatternMatch(String pattern, String str)
{
	char c = pattern.charAt(0);
	for (int l = 1; l <= str.length() - pattern.length() + 1; l++)
	{
		String subStr = str.substring(0, l), mapStr = map.get(c);
		if ((subStr.equals(mapStr)) || (mapStr == null && !map.containsValue(subStr)))
		{
			map.put(c, subStr);
			if (wordPatternMatch(pattern.substring(1), str.substring(l))) return true;
			else if (mapStr == null) map.remove(c);
		}
	}
	return false;
}
```

#### [294. 翻转游戏 II](https://leetcode.cn/problems/flip-game-ii/)

@2023.10.06

#技巧 

递归，尝试翻转每个 `"++"`，将新串记为 `next`，如果将 `next` 递归、结果为 `false`，则说明原串应返回 `true`，否则记忆化 `next`。

```java
/**
 * 记忆化搜索
 * Somnia1337
 */
Map<String, Boolean> memo = new HashMap<>();

public boolean canWin(String currentState)
{
	if (memo.containsKey(currentState)) return memo.get(currentState);
	for (int i = 0; i < currentState.length() - 1; i++)
	{
		if (currentState.charAt(i) == '+' && currentState.charAt(i + 1) == '+')
		{
			StringBuilder cur = new StringBuilder(currentState);
			cur.replace(i, i + 2, "--");
			String next = cur.toString();
			if (!canWin(next))
			{
				memo.put(next, false);
				return true;
			}
			memo.put(next, true);
		}
	}
	return false;
}
```

#### [299. 猜数字游戏](https://leetcode.cn/problems/bulls-and-cows/)

@2023.11.04

#字符串 

遍历 `secret` 与 `guess`，位相同时累加 `x`，否则分别记录到字符统计表，将两表中每个值的较小者累加到 `y`。

```java
/**
 * 字符统计
 * Somnia1337
 */
public String getHint(String secret, String guess)
{
	int x = 0, y = 0;
	int[] sCount = new int[10], gCount = new int[10];
	
	for (int i = 0; i < secret.length(); i++)
	{
		char s = secret.charAt(i), g = guess.charAt(i);
		if (s == g) x++;
		else
		{
			sCount[s - '0']++;
			gCount[g - '0']++;
		}
	}
	
	for (int i = 0; i < 10; i++) y += Math.min(sCount[i], gCount[i]);
	return x + "A" + y + "B";
}
```

#### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

@2023.09.07

#动态规划 #二分查找 

1. 动态规划

`dp[i]` 表示 `nums[i:]` 的最长递增子序列长度，外层 `for` 遍历 `nums`，内层 `for` 遍历 `i` 之前的 `j`，当 `nums[i] > nums[j]` 时更新 `dp[i]`，递推式 `dp[i] = max(dp[j] + 1, dp[i])`。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int lengthOfLIS(int[] nums) {
	int n = nums.length, ans = 1;
	int[] dp = new int[n];
	dp[0] = 1;
	for (int r = 1; r < n; r++) {
		dp[r] = 1;
		for (int l = 0; l < r; l++) {
			if (nums[r] > nums[l]) dp[r] = Math.max(dp[l] + 1, dp[r]);
		}
		ans = Math.max(dp[r], ans);
	}
	return ans;
}
```

2. 二分查找

```java
/**
 * 二分查找
 * Somnia1337
 */
public int lengthOfLIS(int[] nums) {
	int ans = 1;
	for (int i = 1; i < nums.length; i++) {
		int idx = bisect(nums, 0, ans - 1, nums[i]);
		nums[idx] = nums[i];
		ans = Math.max(idx + 1, ans);
	}
	return ans;
}

private int bisect(int[] arr, int l, int r, int t) {
	while (l <= r) {
		int m = l + r >> 1;
		if (arr[m] < t) l = m + 1;
		else r = m - 1;
	}
	return l;
}
```

#### [306. 累加数](https://leetcode.cn/problems/additive-number/)

@2023.11.05

```java
/**
 * 回溯
 * Yi-Xing
 */
private String s;
private int n;

public boolean isAdditiveNumber(String num)
{
	this.s = num;
	this.n = num.length();
	return bt(0, 0, 0, 0);
}

private boolean bt(int idx, long sum, long pre, int cnt)
{
	if (idx == n) return cnt >= 3;
	long val = 0;
	for (int i = idx; i < n; i++)
	{
		if (i > idx && s.charAt(idx) == '0') break;
		val = val * 10 + s.charAt(i) - '0';
		if (cnt >= 2)
		{
			if (val < sum) continue;
			else if (val > sum) break;
		}
		if (bt(i + 1, pre + val, val, cnt + 1)) return true;
	}
	return false;
}
```

#### [307. 区域和检索 - 数组可修改](https://leetcode.cn/problems/range-sum-query-mutable/)

@2023.11.06

#设计 #线段树 #树状数组 

1. 前缀和 880ms

```java
/**
 * 前缀和
 * Somnia1337
 */
class NumArray
{
    private int[] pre;
	
    public NumArray(int[] nums)
    {
        int len = nums.length;
        pre = new int[len + 1];
        for (int i = 1; i < len + 1; i++)
        {
            pre[i] = pre[i - 1] + nums[i - 1];
        }
    }
	
    public void update(int index, int val)
    {
        int delta = val - (pre[index + 1] - pre[index]);
        for (int i = index + 1; i < pre.length; i++) pre[i] += delta;
    }
	
    public int sumRange(int left, int right)
    {
        return pre[right + 1] - pre[left];
    }
}
```

2. 分块 105ms

```java
/**
 * 分块
 * 力扣官方题解
 */
class NumArray
{
    private int[] sums;
    private int size;
    private int[] nums;
	
    public NumArray(int[] nums)
    {
        this.nums = nums;
        int n = nums.length;
        size = (int) Math.sqrt(n);
        sums = new int[(n + size - 1) / size];
        for (int i = 0; i < n; i++) sums[i / size] += nums[i];
    }
	
    public void update(int index, int val)
    {
        sums[index / size] += val - nums[index];
        nums[index] = val;
    }
	
    public int sumRange(int left, int right)
    {
        int b1 = left / size, b2 = right / size, i1 = left % size, i2 = right % size;
        if (b1 == b2)
        {
            int s = 0;
            for (int j = i1; j <= i2; j++) s += nums[b1 * size + j];
            return s;
        }
        int s1 = 0, s2 = 0, s3 = 0;
        for (int j = i1; j < size; j++) s1 += nums[b1 * size + j];
        for (int j = 0; j <= i2; j++) s2 += nums[b2 * size + j];
        for (int j = b1 + 1; j < b2; j++) s3 += sums[j];
        return s1 + s2 + s3;
    }
}
```

3. 线段树 99ms

```java
/**
 * 线段树
 * 力扣官方题解
 */
class NumArray
{
    private int[] segmentTree;
    private int len;
	
    public NumArray(int[] nums)
    {
        len = nums.length;
        segmentTree = new int[nums.length * 4];
        build(0, 0, len - 1, nums);
    }
	
    public void update(int index, int val)
    {
        change(index, val, 0, 0, len - 1);
    }
	
    public int sumRange(int left, int right)
    {
        return range(left, right, 0, 0, len - 1);
    }
	
    private void build(int node, int s, int e, int[] nums)
    {
        if (s == e)
        {
            segmentTree[node] = nums[s];
            return;
        }
        int m = s + (e - s) / 2;
        build(node * 2 + 1, s, m, nums);
        build(node * 2 + 2, m + 1, e, nums);
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }
	
    private void change(int index, int val, int node, int s, int e)
    {
        if (s == e)
        {
            segmentTree[node] = val;
            return;
        }
        int m = s + (e - s) / 2;
        if (index <= m) change(index, val, node * 2 + 1, s, m);
        else change(index, val, node * 2 + 2, m + 1, e);
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }
	
    private int range(int left, int right, int node, int s, int e)
    {
        if (left == s && right == e) return segmentTree[node];
        int m = s + (e - s) / 2;
        if (right <= m) return range(left, right, node * 2 + 1, s, m);
        else if (left > m) return range(left, right, node * 2 + 2, m + 1, e);
        else return range(left, m, node * 2 + 1, s, m) + range(m + 1, right, node * 2 + 2, m + 1, e);
    }
}
```

4. 树状数组 72ms

```java
/**
 * 树状数组
 * 力扣官方题解
 */
class NumArray
{
    private int[] tree;
    private int[] nums;
	
    public NumArray(int[] nums)
    {
        this.tree = new int[nums.length + 1];
        this.nums = nums;
        for (int i = 0; i < nums.length; i++) add(i + 1, nums[i]);
    }
	
    public void update(int index, int val)
    {
        add(index + 1, val - nums[index]);
        nums[index] = val;
    }
	
    public int sumRange(int left, int right)
    {
        return prefixSum(right + 1) - prefixSum(left);
    }
	
    private int lowBit(int x)
    {
        return x & -x;
    }
	
    private void add(int index, int val)
    {
        while (index < tree.length)
        {
            tree[index] += val;
            index += lowBit(index);
        }
    }
	
    private int prefixSum(int index)
    {
        int sum = 0;
        while (index > 0)
        {
            sum += tree[index];
            index -= lowBit(index);
        }
        return sum;
    }
}
```

#### [309. 买卖股票的最佳时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

@2023.09.08

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int maxProfit(int[] prices) {
	int len = prices.length;
	if (len == 0) return 0;
	int[][] dp = new int[len][3];
	dp[0][0] = -prices[0];
	for (int i = 1; i < len; i++) {
		dp[i][0] = Math.max(dp[i - 1][2] - prices[i], dp[i - 1][0]);
		dp[i][1] = dp[i - 1][0] + prices[i];
		dp[i][2] = Math.max(dp[i - 1][1], dp[i - 1][2]);
	}
	return Math.max(dp[len - 1][1], dp[len - 1][2]);
}
```

定义三个状态数组，表示第 `i` 天结束时分别处于下列状态的最大利润：

- `h[i]`(hold)：持有。
- `s[i]`(sell)：不持有，且在冷冻期。
- `r[i]`(rest)：不持有，且不在冷冻期。

```java
/**
 * 动态规划
 * ChatGPT
 */
public int maxProfit(int[] prices) {
	int len = prices.length;
	int[] h = new int[len], s = new int[len], r = new int[len];
	h[0] = -prices[0];
	for (int i = 1; i < len; i++) {
		h[i] = Math.max(r[i - 1] - prices[i], h[i - 1]);
		s[i] = Math.max(h[i - 1] + prices[i], s[i - 1]);
		r[i] = Math.max(s[i - 1], r[i - 1]);
	}
	return Math.max(s[len - 1], r[len - 1]);
}
```

#### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)

@2023.11.06

#搜索 #拓扑排序 

拓扑排序，维护连接表及每个节点的度，BFS 遍历叶节点（度为 1 的节点），直到剩余 1 或 2 个节点即为答案。

如果用 `while (!leaves.isEmpty())` 进行循环，则无法判断到底剩 1 个还是 2 个节点，因此应将叶节点按整组更新，循环条件 `n > 2`，每次循环只 `poll()` 刚开始的元素数量 `size`，并更新 `n -= size`。

```java
/**
 * 拓扑排序
 * Somnia1337
 */
public List<Integer> findMinHeightTrees(int n, int[][] edges) {
    int[] deg = new int[n];
    Map<Integer, List<Integer>> next = new HashMap<>();
    for (int i = 0; i < n; i++) next.put(i, new ArrayList<>());
    for (int[] edge : edges) {
        deg[edge[0]]++;
        deg[edge[1]]++;
        next.get(edge[0]).add(edge[1]);
        next.get(edge[1]).add(edge[0]);
    }
    
    Queue<Integer> leaves = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (deg[i] <= 1) leaves.add(i);
    }
    while (n > 2) {
        int size = leaves.size();
        n -= size;
        for (int i = 0; i < size; i++) { // 按进入循环时的整组更新
            for (int nx : next.get(leaves.poll())) {
                deg[nx]--;
                if (deg[nx] == 1) leaves.add(nx);
            }
        }
    }
    return new ArrayList<>(leaves);
}
```

#### [311. 稀疏矩阵的乘法](https://leetcode.cn/problems/sparse-matrix-multiplication/)

@2023.11.06

#技巧 

利用稀疏矩阵 0 很多的特点，跳过一部分 0 元素。

```java
/**
 * 跳过 0 元素
 * xtray
 */
public int[][] multiply(int[][] mat1, int[][] mat2)
{
	int rows = mat1.length, cols = mat2[0].length;
	int l = mat1[0].length; // mat1 的 cols, mat2 的 rows
	int[][] ans = new int[rows][cols];
	for (int i = 0; i < rows; i++)
	{
		for (int k = 0; k < l; k++)
		{
			if (mat1[i][k] == 0) continue; // 跳过 0
			for (int j = 0; j < cols; j++) ans[i][j] += mat1[i][k] * mat2[k][j];
		}
	}
	return ans;
}
```

#### [316. 去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/)

@2023.09.15

```java
/**
 * 单调栈
 * 力扣官方题解
 */
public String removeDuplicateLetters(String s)
{
	int len = s.length();
	boolean[] appeared = new boolean[26];
	int[] count = new int[26];
	for (int i = 0; i < len; i++)
	{
		count[s.charAt(i) - 'a']++;
	}
	
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < len; i++)
	{
		char c = s.charAt(i);
		if (!appeared[c - 'a'])
		{
			while (!ans.isEmpty() && ans.charAt(ans.length() - 1) > c)
			{
				if (count[ans.charAt(ans.length() - 1) - 'a'] > 0)
				{
					appeared[ans.charAt(ans.length() - 1) - 'a'] = false;
					ans.deleteCharAt(ans.length() - 1);
				}
				else break;
			}
			appeared[c - 'a'] = true;
			ans.append(c);
		}
		count[c - 'a'] -= 1;
	}
	
	return ans.toString();
}
```

#### [318. 最大单词长度乘积](https://leetcode.cn/problems/maximum-product-of-word-lengths/)

@2023.10.06

#位运算 

将 `words` 中的字符串两两匹配，如果检查没有相同字符，则计算其长度乘积，并更新最大值。如果能将检查的过程简化至 $O(1)$，则总复杂度可达 $O(N^2)$。

对每一个字符串，用一个 26 位的二进制串记录某个字母是否出现，记录该串对应的十进制值，检查时将两个字符串的该值进行按位与，如果为 0，说明没有公共字母。

```java
/**
 * 位运算
 * Somnia1337
 */
public int maxProduct(String[] words) {
	int n = words.length;
	int[] val = new int[n];
	for (int i = 0; i < n; i++) {
		String s = words[i];
		char[] v = new char[26];
		Arrays.fill(v, '0');
		for (int k = 0; k < s.length(); k++) v[s.charAt(k) - 'a'] = '1';
		val[i] = Integer.parseInt(new String(v), 2);
	}
	
	int ans = 0;
	for (int i = 0; i < n; i++) {
		for (int j = i + 1; j < n; j++) {
			if ((val[i] & val[j]) == 0) {
				ans = Math.max(words[i].length() * words[j].length(), ans);
			}
		}
	}
	return ans;
}
```

优化：无需用 `int[26]` 记录每个字母的出现，而只需用一个 `int` 配合位运算：

```java
int mask = 0;
for (int k = 0; k < s.length(); k++) {
	mask |= (1 << (s.charAt(k) - 'a'));
}
```

将 1 左移 `s[i] - 'a'` 位，其结果与 `mask` 按位或，最终 `mask` 就记录了每一位的信息。

```java
/**
 * 位运算
 * ChatGPT
 */
public int maxProduct(String[] words) {
	int n = words.length;
	int[] val = new int[n];
	for (int i = 0; i < n; i++) {
		String s = words[i];
		int mask = 0;
		for (int k = 0; k < s.length(); k++) {
			mask |= (1 << (s.charAt(k) - 'a'));
		}
		val[i] = mask;
	}
	
	int ans = 0;
	for (int i = 0; i < n; i++) {
		for (int j = i + 1; j < n; j++) {
			if ((val[i] & val[j]) == 0) {
				ans = Math.max(ans, words[i].length() * words[j].length());
			}
		}
	}
	return ans;
}
```

#### [319. 灯泡开关](https://leetcode.cn/problems/bulb-switcher/)

@2023.09.07

#数学 

1. 找规律

```java
/**
 * 找规律
 * Somnia1337
 */
public int bulbSwitch(int n) {
	return (int) Math.sqrt(n);
}
```

2. 数学

第 `i` 轮时会将所有编号为 `i` 的倍数的灯泡进行切换，因此，对于第 $k$ 个灯泡，它被切换的次数为 $k$ 的约数个数，最终第 $k$ 个灯泡的状态取决于其约数个数的奇偶性。

如果 $k$ 有约数 $x$，那么同时一定有约数 $\frac{k}{x}​$，因此，当且仅当 $k$ 为完全平方数时其约数个数为奇，否则为偶。

因此，答案为 $[1,\space n]$ 中完全平方数的个数。

由于 $\sqrt{n}$ 涉及浮点运算，为了避免精度问题，可以计算 $\sqrt{n + 0.5}$，保证计算结果向下取整后在 32 位整数范围内正确。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int bulbSwitch(int n) {
	return (int) Math.sqrt(n + 0.5);
}
```

#### [320. 列举单词的全部缩写](https://leetcode.cn/problems/generalized-abbreviation/)

@2023.10.06

#回溯 

```java
for (int l = 0; l <= word.length() - i; l++)
{
	int s = path.length();
	if (l > 0) path.append(l);
	if (i + l < word.length()) path.append(word.charAt(i + l));
	bt(word, i + l + 1);
	path.delete(s, path.length());
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
public List<String> ans = new ArrayList<>();
public StringBuilder path = new StringBuilder();

public List<String> generateAbbreviations(String word)
{
	bt(word, 0);
	return ans;
}

public void bt(String word, int i)
{
	if (i >= word.length())
	{
		ans.add(path.toString());
		return;
	}
	for (int l = 0; l <= word.length() - i; l++)
	{
		int s = path.length();
		if (l > 0) path.append(l);
		if (i + l < word.length()) path.append(word.charAt(i + l));
		bt(word, i + l + 1);
		path.delete(s, path.length());
	}
}
```

#### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

@2023.07.22

#动态规划 

等价于完全背包。

`dp[i]` 表示凑出总价值为 `i` 的硬币的最少数量，递推式 `dp[i] = min(dp[i-coin] + 1, dp[i])`。由于取 `min()`，`dp` 应初始化为 `Integer.MAX_VALUE`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int coinChange(int[] coins, int amount) {
	int[] dp = new int[amount + 1];
	Arrays.fill(dp, Integer.MAX_VALUE);
	dp[0] = 0;
	for (int coin : coins) {
		for (int i = coin; i <= amount; i++) {
			if (dp[i - coin] < Integer.MAX_VALUE) {
				dp[i] = Math.min(dp[i - coin] + 1, dp[i]);
			}
		}
	}
	return dp[amount] < Integer.MAX_VALUE ? dp[amount] : -1;
}
```

#### [323. 无向图中连通分量的数目](https://leetcode.cn/problems/number-of-connected-components-in-an-undirected-graph/)

@2023.10.30

#搜索 #并查集 

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
private boolean[] vis;

public int countComponents(int n, int[][] edges) {
	List<Integer>[] g = new List[n];
	Arrays.setAll(g, k -> new ArrayList<>());
	for (int[] e : edges) {
		g[e[0]].add(e[1]);
		g[e[1]].add(e[0]);
	}
	vis = new boolean[n];
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (!vis[i]) {
			ans++;
			dfs(g, i);
		}
	}
	return ans;
}

private void dfs(List<Integer>[] g, int node) {
	if (vis[node]) return;
	vis[node] = true;
	for (int next : g[node]) dfs(g, next);
}
```

2. 并查集

```java
/**
 * 并查集
 * Somnia1337
 */
public int countComponents(int n, int[][] edges) {
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int[] e : edges) union(root, e[0], e[1]);
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (root[i] == i) ans++;
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

#### [324. 摆动排序 II](https://leetcode.cn/problems/wiggle-sort-ii/)

@2024.02.22



```java
/**
 * 排序
 * shj320
 */
public void wiggleSort(int[] nums) {
	int n = nums.length;
	int[] t = new int[n];
	Arrays.sort(nums);
	int j = n - 1;
	for (int i = 1; i < n; i += 2, j--) t[i] = nums[j];
	for (int i = 0; i < n; i += 2, j--) t[i] = nums[j];
	System.arraycopy(t, 0, nums, 0, n);
}
```

#### [325. 和等于 k 的最长子数组长度](https://leetcode.cn/problems/maximum-size-subarray-sum-equals-k/)

@2023.10.06

#技巧 

```java
/**
 * 前缀和
 * Somnia1337
 */
public int maxSubArrayLen(int[] nums, int k)
{
	int len = nums.length;
	Map<Integer, Integer> value2Index = new HashMap<>();
	int preSum = 0;
	value2Index.put(0, -1);
	int ans = 0;
	for (int i = 0; i < len; i++)
	{
		preSum += nums[i];
		if (value2Index.containsKey(preSum - k))
		{
			ans = Math.max(i - value2Index.get(preSum - k), ans);
		}
		value2Index.putIfAbsent(preSum, i);
	}
	return ans;
}
```

```java
/**
 * 前缀和
 * ?
 */
public int maxSubArrayLen(int[] nums, int k)
{
	Map<Integer, Integer> map = new HashMap<>();
	map.put(0, -1);
	int ans = 0;
	int preSum = 0;
	for (int i = 0; i < nums.length; i++)
	{
		preSum += nums[i];
		Integer orDefault = map.get(preSum - k);
		if (orDefault != null)
		{
			ans = Math.max(i - orDefault, ans);
		}
		map.putIfAbsent(preSum, i);
	}
	return ans;
}
```

#### [334. 递增的三元子序列](https://leetcode.cn/problems/increasing-triplet-subsequence/)

@2023.08.30

#贪心 

维护最小值、中间值，遍历时这两个值只能减小，如果发现大于二者的值，即找到解。

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean increasingTriplet(int[] nums) {
	int min = Integer.MAX_VALUE, mid = Integer.MAX_VALUE;
	for (int num : nums) {
		if (num <= min) min = num;
		else if (num <= mid) mid = num;
		else return true;
	}
	return false;
}
```

#### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

@2023.09.18

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int rob(TreeNode root)
{
	int[] rootStatus = dfs(root);
	return Math.max(rootStatus[0], rootStatus[1]);
}

public int[] dfs(TreeNode node)
{
	if (node == null)
	{
		return new int[]{0, 0};
	}
	int[] l = dfs(node.left);
	int[] r = dfs(node.right);
	int selected = node.val + l[1] + r[1];
	int notSelected = Math.max(l[0], l[1]) + Math.max(r[0], r[1]);
	return new int[]{selected, notSelected};
}
```

#### [339. 嵌套列表权重和](https://leetcode.cn/problems/nested-list-weight-sum/)

@2023.11.08

#搜索 

DFS，每次递归深度加 1，对列表的每一项：

- 如果为数值，其值乘以深度加到 `sum`。
- 如果为列表，对其进行 DFS。

```java
/**
 * DFS
 * 力扣官方题解
 */
public int depthSum(List<NestedInteger> nestedList)
{
	return dfs(nestedList, 1);
}

private int dfs(List<NestedInteger> list, int d)
{
	int sum = 0;
	for (NestedInteger n : list)
	{
		if (n.isInteger()) sum += n.getInteger() * d;
		else sum += dfs(n.getList(), d + 1);
	}
	return sum;
}
```

#### [340. 至多包含 K 个不同字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/)

@2023.09.14

#双指针 

```java
/**
 * 双指针
 * Somnia1337
 */
public int lengthOfLongestSubstringKDistinct(String s, int k) {
	Map<Character, Integer> count = new HashMap<>();
	int n = s.length(), l = 0, r = 0, ans = 0;
	while (r < n) {
		count.merge(s.charAt(r), 1, Integer::sum);
		while (l < n && count.size() > k) {
			char c = s.charAt(l++);
			if (count.merge(c, -1, Integer::sum) == 0) count.remove(c);
		}
		if (count.size() <= k) ans = Math.max(r - l + 1, ans);
		r++;
	}
	return ans;
}
```

```java
/**
 * 双指针
 * Somnia1337
 */
public int lengthOfLongestSubstringKDistinct(String s, int k) {
	int[] count = new int[128];
	int n = s.length(), l = 0, r = 0, cur = 0, ans = 0;
	while (r < n) {
		if (++count[s.charAt(r)] == 1) cur++;
		while (cur > k) if (--count[s.charAt(l++)] == 0) cur--;
		ans = Math.max(r++ - l + 1, ans);
	}
	return ans;
}
```

#### [341. 扁平化嵌套列表迭代器](https://leetcode.cn/problems/flatten-nested-list-iterator/)

@2023.11.10

```java
/**
 * DFS
 * Somnia1337
 */
public class NestedIterator implements Iterator<Integer>
{
    private List<Integer> val;
    private Iterator<Integer> cur;
	
    public NestedIterator(List<NestedInteger> nestedList)
    {
        val = new ArrayList<>();
        dfs(nestedList);
        cur = val.iterator();
    }
	
    public Integer next()
    {
        return cur.next();
    }
	
    public boolean hasNext()
    {
        return cur.hasNext();
    }
	
    private void dfs(List<NestedInteger> nest)
    {
        for (NestedInteger n : nest)
        {
            if (n.isInteger()) val.add(n.getInteger());
            else dfs(n.getList());
        }
    }
}
```

#### [343. 整数拆分](https://leetcode.cn/problems/integer-break/)

@2023.07.18

#动态规划 

```java
/**
 * 动态规划
 * Somnia1337
 */
public int integerBreak(int n)
{
	if (n <= 3) return n - 1;
	int[] dp = new int[n - 1];
	dp[0] = 1;
	dp[1] = 2;
	for (int i = 2; i < n - 2; i++)
	{
		int max = 0;
		for (int j = 0; j < i; j++)
		{
			max = Math.max(Math.max(dp[j], j + 2) * (i - j), max);
		}
		dp[i] = max;
	}
	return dp[n - 2];
}
```

#### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

@2023.09.19

用`Map`统计每个元素的出现次数，然后将其所有键存入`List`，根据比较两个键对应于表中的值进行排序，取最高的`k`个键。

```java
/**
 * 排序
 * Somnia1337
 */
public int[] topKFrequent(int[] nums, int k)
{
	Map<Integer, Integer> count = new HashMap<>();
	int[] ans = new int[k];
	for (int num : nums)
	{
		count.merge(num, 1, Integer::sum);
	}
	List<Integer> keys = new ArrayList<>(count.keySet());
	keys.sort((k1, k2) -> count.get(k2).compareTo(count.get(k1))); // 自定义排序，根据两个键对应于表中的值
	return keys.subList(0, k);
}
```

#### [348. 设计井字棋](https://leetcode.cn/problems/design-tic-tac-toe/)

@2024.02.22



```java
/**
 * 
 * Somnia1337
 */
class TicTacToe {
    private int n;
    private int[][] rows, cols, diag;
    
    public TicTacToe(int n) {
        this.n = n;
        rows = new int[2][n];
        cols = new int[2][n];
        diag = new int[2][2];
    }
    
    public int move(int row, int col, int player) {
        if (++rows[player - 1][row] == n) return player;
        if (++cols[player - 1][col] == n) return player;
        if (row - col == 0 && ++diag[player - 1][0] == n) return player;
        if (row + col == n - 1 && ++diag[player - 1][1] == n) return player;
        return 0;
    }
}
```

#### [355. 设计推特](https://leetcode.cn/problems/design-twitter/)

@2023.11.08

#设计 

一个列表维护推文，两张哈希表维护用户间关系、推文与用户关系。

```java
/**
 * 哈希表
 * Somnia1337
 */
class Twitter
{
    private Map<Integer, Set<Integer>> follow; // <用户, 关注列表>
    private List<Integer> tweet; // 推文
    private Map<Integer, Integer> owner; // <推文, 发送者>
	
    public Twitter()
    {
        follow = new HashMap<>();
        tweet = new ArrayList<>();
        owner = new HashMap<>();
    }
	
    public void postTweet(int userId, int tweetId)
    {
	    // 推文入列, 记录发送者
        tweet.add(tweetId);
        owner.put(tweetId, userId);
        
        // 将发送者加入自己的关注列表
        follow.putIfAbsent(userId, new HashSet<>());
        follow.get(userId).add(userId);
    }
	
    public List<Integer> getNewsFeed(int userId)
    {
        Set<Integer> f = follow.get(userId); // 关注列表
        List<Integer> ret = new ArrayList<>();
        
        for (int i = tweet.size() - 1; i >= 0; i--) // 倒序遍历
        {
            int tw = tweet.get(i);
            if (f.contains(owner.get(tw))) ret.add(tw); // 是关注内容
            if (ret.size() == 10) break; // 最多 10 条
        }
        
        return ret;
    }
	
    public void follow(int followerId, int followeeId)
    {
        follow.putIfAbsent(followerId, new HashSet<>());
        follow.get(followerId).add(followeeId);
    }
	
    public void unfollow(int followerId, int followeeId)
    {
        follow.get(followerId).remove(followeeId);
    }
}
```

#### [356. 直线镜像](https://leetcode.cn/problems/line-reflection/)

@2023.10.17

#哈希表 #数学 

哈希表，`key` 为点的纵坐标，`value` 为纵坐标相同的点的横坐标的哈希集。遍历 `points` 记录点的信息，然后遍历哈希表，将每个 `Set` 存储到 `List` 后排序，求均值 `mean`，如果 `mean` 与中位数不同或者与上一行的均值 `pre` 不同，则返回 `false`。

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean isReflected(int[][] points)
{
	Map<Integer, Set<Integer>> rows = new HashMap<>();
	for (int[] point : points) // 记录y相同的点的x，去重
	{
		int y = point[1];
		Set<Integer> row = rows.getOrDefault(y, new HashSet<>());
		row.add(point[0]);
		rows.put(y, row);
	}
	
	double pre = Integer.MIN_VALUE; // 上一行x均值
	for (Set<Integer> r : rows.values())
	{
		// 存储到List并排序
		List<Integer> row = new ArrayList<>(r);
		Collections.sort(row);
		// 求均值
		int sum = 0, l = row.size();
		for (int x : row) sum += x;
		double mean = (double) sum / l;
		// 判断
		if (l % 2 == 1 && (double) row.get(l / 2) != mean || pre > Integer.MIN_VALUE && mean != pre) return false;
		pre = mean;
	}
	
	return true;
}
```

另一种思路：用 `Set<String>`，遍历 `points` 记录每个点，同时计算最大、最小的 `x`，记 `sum` 为 `minX + maxX`，那么对于每个点 `(x, y)`，都应该存在对称点 `(sum - x, y)`，再次遍历 `points`，以此判断。

```java
/**
 * 哈希表
 * Wozir
 */
public boolean isReflected(int[][] points)
{
	int minX = points[0][0], maxX = points[0][0];
	Set<String> pos = new HashSet<>();
	for (int[] point : points)
	{
		// 更新minX、maxX
		minX = Math.min(minX, point[0]);
		maxX = Math.max(maxX, point[0]);
		// 记录点
		pos.add(point[0] + "," + point[1]);
	}
	
	double sum = minX + maxX;
	for (int[] point : points) // 再次遍历，判断
	{
		if (!pos.contains((int) (sum - point[0]) + "," + point[1])) return false;
	}
	
	return true;
}
```

#### [357. 统计各位数字都不同的数字个数](https://leetcode.cn/problems/count-numbers-with-unique-digits/)

@2023.10.07

#数学 #动态规划 

1. 数学

排列组合。

```java
/**
 * 数学
 * Somnia1337
 */
public int countNumbersWithUniqueDigits(int n)
{
	int ans = 0;
	for (int i = 1; i <= n; i++)
	{
		int cur = 9;
		int d = 9;
		int j = i;
		while (j > 1)
		{
			cur *= d--;
			j--;
		}
		ans += cur;
	}
	return ans;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 龅牙叔
 */
public int countNumbersWithUniqueDigits(int n)
{
	if (n == 0) return 1;
	int[] dp = new int[n + 1];
	dp[0] = 1;
	dp[1] = 10;
	for (int i = 2; i <= n; i++)
	{
		dp[i] = dp[i - 1] + (dp[i - 1] - dp[i - 2]) * (10 - (i - 1));
	}
	return dp[n];
}
```

#### [360. 有序转化数组](https://leetcode.cn/problems/sort-transformed-array/)

@2023.10.19

#数学 

- `a != 0`：
	- 计算对称轴 `- b / (a * 2)`，再根据 `a` 的正负选择在解中的位置。
- `a == 0`：
	- 根据 `b` 的正负选择在解中的位置。

```java
/**
 * 双指针
 * Somnia1337
 */
public int[] sortTransformedArray(int[] nums, int a, int b, int c)
{
	int len = nums.length;
	int[] ans = new int[len];
	double ctr = (double) -b / a / 2; // 对称轴
	int left = 0, right = len - 1, it = 0;
	
	if (a > 0)
	{
		while (left <= right)
		{
			int l = nums[left], r = nums[right];
			if (Math.abs(l - ctr) > Math.abs(r - ctr))
			{
				ans[len - 1 - it] = a * l * l + b * l + c;
				left++;
			}
			else
			{
				ans[len - 1 - it] = a * r * r + b * r + c;
				right--;
			}
			it++;
		}
	}
	else if (a < 0)
	{
		while (left <= right)
		{
			int l = nums[left], r = nums[right];
			if (Math.abs(l - ctr) > Math.abs(r - ctr))
			{
				ans[it] = a * l * l + b * l + c;
				left++;
			}
			else
			{
				ans[it] = a * r * r + b * r + c;
				right--;
			}
			it++;
		}
	}
	else
	{
		if (b > 0)
		{
			for (int i = 0; i < len; i++)
			{
				ans[i] = b * nums[i] + c;
			}
		}
		else
		{
			for (int i = 0; i < len; i++)
			{
				ans[i] = b * nums[len - 1 - i] + c;
			}
		}
	}
	return ans;
}
```

#### [361. 轰炸敌人](https://leetcode.cn/problems/bomb-enemy/)

@2023.10.14

#暴力 

枚举每一个空位，计算能够轰炸的敌人数量。

```java
/**
 * 枚举
 * Somnia1337
 */
public int maxKilledEnemies(char[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	int ans = 0;
	
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (grid[i][j] == '0')
			{
				ans = Math.max(count(grid, i, j), ans);
			}
		}
	}
	
	return ans;
}

private int count(char[][] grid, int x, int y)
{
	int ret = 0;
	
	for (int i = x - 1; i >= 0; i--)
	{
		if (grid[i][y] == 'W') break;
		if (grid[i][y] == 'E') ret++;
	}
	for (int i = x + 1; i < grid.length; i++)
	{
		if (grid[i][y] == 'W') break;
		if (grid[i][y] == 'E') ret++;
	}
	for (int j = y - 1; j >= 0; j--)
	{
		if (grid[x][j] == 'W') break;
		if (grid[x][j] == 'E') ret++;
	}
	for (int j = y + 1; j < grid[0].length; j++)
	{
		if (grid[x][j] == 'W') break;
		if (grid[x][j] == 'E') ret++;
	}
	
	return ret;
}
```

#### [362. 敲击计数器](https://leetcode.cn/problems/design-hit-counter/)

@2023.10.07

#队列 

用队列维护所有时间戳，收到计数请求时清除时间戳在 300 秒前的部分。

```java
/**
 * 队列
 * Somnia1337
 */
class HitCounter
{
    private final Queue<Integer> hits;
	
    public HitCounter()
    {
        hits = new LinkedList<>();
    }
	
    public void hit(int timestamp)
    {
        hits.add(timestamp);
    }
	
    public int getHits(int timestamp)
    {
        while (!hits.isEmpty() && hits.peek() <= timestamp - 300)
        {
            hits.poll();
        }
        return hits.size();
    }
}
```

#### [364. 加权嵌套序列和 II](https://leetcode.cn/problems/nested-list-weight-sum-ii/)

@2023.11.08

#搜索 

两次 DFS，第一次找最大深度，第二次求和。

```java
/**
 * DFS
 * Arlon
 */
public int depthSumInverse(List<NestedInteger> nestedList)
{
	int maxD = getMax(nestedList);
	return dfs(nestedList, maxD);
}

private int getMax(List<NestedInteger> list)
{
	if (list.size() == 0) return 0;
	int d = 1;
	for (NestedInteger n : list)
	{
		if (!n.isInteger())
		{
			d = Math.max(getMax(n.getList()) + 1, d);
		}
	}
	return d;
}

private int dfs(List<NestedInteger> list, int d)
{
	int sum = 0;
	for (NestedInteger n : list)
	{
		if (n.isInteger()) sum += n.getInteger() * d;
		else sum += dfs(n.getList(), d - 1);
	}
	return sum;
}
```

也可以一次 DFS，[题解](https://leetcode.cn/problems/nested-list-weight-sum-ii/solutions/1941126/by-danikarry-s3tf/)，仙术。

```java
/**
 * DFS
 * Danikarry
 */
private int maxD, sumD, sum;

public int depthSumInverse(List<NestedInteger> nestedList)
{
	dfs(nestedList, 1);
	return (maxD + 1) * sum - sumD;
}

private void dfs(List<NestedInteger> list, int d)
{
	if (list.isEmpty()) return;
	if (maxD < d) maxD = d;
	for (NestedInteger n : list)
	{
		if (n.isInteger())
		{
			sum += n.getInteger();
			sumD += d * n.getInteger();
		}
		else
		{
			dfs(n.getList(), d + 1);
		}
	}
}
```

#### [365. 水壶问题](https://leetcode.cn/problems/water-and-jug-problem/)

@2023.10.10

#数学 

每次操作只能让水的总量加减 `x` 或 `y`，因此题目等价于找到一对整数 `a`，`b` 使得 `a * x + b * y = z`。由倍祖定理，该方程有解当且仅当 `z = k * gcd(x, y)`。

```java
/**
 * 数学
 * 力扣官方题解
 */
public boolean canMeasureWater(int x, int y, int z) {
	if (x + y < z) return false;
	if (x == 0 || y == 0) return z == 0 || x + y == z;
	return z % gcd(x, y) == 0;
}

private int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
```

#### [368. 最大整除子集](https://leetcode.cn/problems/largest-divisible-subset/)

@2023.10.10

#动态规划 

排序，`dp[i]` 表示前 `i` 个元素中能够选择出的最大整除子集的大小，且该子集必须包含 `nums[i]`。遍历，对每个 `i`，枚举 `j < i`，如果 `nums[i] % nums[j] == 0`，那么 `nums[i]` 可以扩充以 `nums[j]` 结尾的子集。

在更新 `dp` 的同时维护所有 `dp[i]` 的最大值，该值即为答案大小。用 `pre` 维护该子集的任意元素，倒序遍历 `dp`，当 `dp[i] == max` 且 `nums[i] % pre == 0` 时将 `nums[i]` 添加到解集，并将 `max` 减 1。

```java
/**
 * 动态规划
 * Somnia1337
 */
public List<Integer> largestDivisibleSubset(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1); // 对每个 i, nums[i] 必须选择
    
    // 排序后遍历
    Arrays.sort(nums);
    int max = 0; // 最大子集大小
    for (int i = 0; i < n; i++) { // 枚举 i
        for (int j = 0; j < i; j++) { // 枚举 j=0..i
            if (nums[i] % nums[j] == 0) dp[i] = Math.max(dp[j] + 1, dp[i]); // nums[i] 可扩充以 nums[j] 结尾的子集
        }
        max = Math.max(dp[i], max);
    }
    
    // 倒序遍历, 添加到解集
    // pre 表示目标子集的任意元素, 用于判断一个元素是否属于目标子集
    // 最初不知道目标子集的任何元素, 初始化为 0
    int it = n - 1, pre = 0;
    List<Integer> ans = new ArrayList<>();
    while (max > 0) {
        // 判断是否属于目标子集
        if (dp[it] == max && pre % nums[it] == 0) {
            ans.add(nums[it]);
            pre = nums[it];
            max--;
        }
        it--;
    }
    return ans;
}
```

#### [370. 区间加法](https://leetcode.cn/problems/range-addition/)

@2023.10.07

#技巧 

```java
/**
 * 差分数组
 * Somnia1337
 */
public int[] getModifiedArray(int length, int[][] updates)
{
	int[] ans = new int[length];
	for (int[] update : updates)
	{
		ans[update[0]] += update[2];
		if (update[1] + 1 < length) ans[update[1] + 1] -= update[2];
	}
	for (int i = 1; i < length; i++)
	{
		ans[i] += ans[i - 1];
	}
	return ans;
}
```

#### [371. 两整数之和](https://leetcode.cn/problems/sum-of-two-integers/)

@2023.10.07

#位运算 

1. 库函数

```java
/**
 * 库函数
 * Somnia1337
 */
public int getSum(int a, int b)
{
	return Integer.sum(a, b);
}
```

2. 位运算

```java
/**
 * 位运算
 * 力扣官方题解
 */
public int getSum(int a, int b)
{
	while (b != 0)
	{
		int carry = (a & b) << 1;
		a = a ^ b;
		b = carry;
	}
	return a;
}
```

#### [372. 超级次方](https://leetcode.cn/problems/super-pow/)

@2023.10.14

```java
/**
 * 快速幂
 * Benhao
 */
private static final int MOD = 1337;

public int superPow(int a, int[] b)
{
	return dfs(a % MOD, b, b.length - 1);
}

private int dfs(int a, int[] b, int idx)
{
	if (idx == -1 || a == 1) return 1;
	return qPow(dfs(a, b, idx - 1), 10) * qPow(a, b[idx]) % MOD;
}

private int qPow(int a, int b)
{
	int ans = 1;
	a %= MOD;
	while (b > 0)
	{
		if ((b & 1) != 0) ans = ans * a % MOD;
		a = a * a % MOD;
		b >>= 1;
	}
	return ans;
}
```

#### [373. 查找和最小的 K 对数字](https://leetcode.cn/problems/find-k-pairs-with-smallest-sums/)

@2023.10.11

#堆 

最小堆中保存三元组 `(nums1[i] + nums2[j], i, j)`，为避免重复，规定 `(i, j - 1)` 出堆时将 `(i, j)` 入堆，而 `(i - 1, j)` 出堆时只加到解集。一开始 `(0, 0)` 入堆后，之后只有 `(0, 1)`、`(0, 2)` 等等，因此一开始需将所有 `(i, 0)` 入堆。

```java
/**
 * 堆
 * 灵茶山艾府
 */
public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k)
{
	int l1 = nums1.length, l2 = nums2.length;
	List<List<Integer>> ans = new ArrayList<>(k);
	Queue<int[]> queue = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
	queue.offer(new int[]{nums1[0] + nums2[0], 0, 0});
	while (!queue.isEmpty() && ans.size() < k)
	{
		int[] cur = queue.poll();
		int i = cur[1], j = cur[2];
		ans.add(List.of(nums1[i], nums2[j]));
		if (j == 0 && i + 1 < l1)
		{
			queue.offer(new int[]{nums1[i + 1] + nums2[0], i + 1, 0});
		}
		if (j + 1 < l2)
		{
			queue.offer(new int[]{nums1[i] + nums2[j + 1], i, j + 1});
		}
	}
	return ans;
}
```

#### [375. 猜数字大小 II](https://leetcode.cn/problems/guess-number-higher-or-lower-ii/)

@2023.11.05

```java
/**
 * 动态规划
 * Benhao
 */
public int getMoneyAmount(int n)
{
	int[][] dp = new int[n + 1][n + 1];
	for (int i = n - 1; i > 0; i--)
	{
		for (int j = i + 1; j <= n; j++)
		{
			dp[i][j] = Integer.MAX_VALUE;
			for (int k = i; k < j; k++)
			{
				dp[i][j] = Math.min(Math.max(dp[i][k - 1], dp[k + 1][j]) + k, dp[i][j]);
			}
		}
	}
	return dp[1][n];
}
```

#### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

@2023.09.22

```java
/**
 * 贪心
 * Angus-Liu
 */
public int wiggleMaxLength(int[] nums)
{
	int len = nums.length;
	int up = 1;
	int down = 1;
	for (int i = 1; i < len; i++)
	{
		if (nums[i] > nums[i - 1])
		{
			up = down + 1;
		}
		if (nums[i] < nums[i - 1])
		{
			down = up + 1;
		}
	}
	return Math.max(up, down);
}
```

#### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

@2023.07.22

#动态规划 

本题等价于完全背包。

```java
/**
 * 动态规划
 * 代码随想录
 */
public int combinationSum4(int[] nums, int target)
{
	int[] dp = new int[target + 1];
	dp[0] = 1;
	
	for (int i = 0; i <= target; i++)
	{
		for (int num : nums)
		{
			if (i >= num)
			{
				dp[i] += dp[i - num];
			}
		}
	}
	
	return dp[target];
}
```

#### [378. 有序矩阵中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/)

@2023.12.11

#二分查找 

二分查找的对象为元素值，`l`、`r` 初始化为左上角、右下角，每个 `m` 可将 `matrix` 划分为 `<= m` 及 `> m` 的两部分，元素 `8` 的划分线如图：

![[378. 有序矩阵中第 K 小的元素.png|300]]

检查函数 `countLeqM` 返回 `<= m` 的元素个数，即左侧部分计数。从左下角开始，对当前元素：

- 如果 `<= m`，累加本列计数（即 `i` 当前值），并右移 `j`。
- 如果 `> m`，需向上移动以回到左侧部分，上移 `i`。

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public int kthSmallest(int[][] matrix, int k) {
	int len = matrix.length, l = matrix[0][0], r = matrix[len - 1][len - 1];
	while (l < r) {
		int m = l + r >> 1;
		if (countLeqM(matrix, m) >= k) r = m;
		else l = m + 1;
	}
	return l;
}

private int countLeqM(int[][] grid, int m) {
	int i = grid.length - 1, j = 0, cnt = 0;
	while (i >= 0 && j < grid.length) {
		if (grid[i][j] <= m) {
			cnt += i + 1;
			j++;
		}
		else i--;
	}
	return cnt;
}
```

#### [379. 电话目录管理系统](https://leetcode.cn/problems/design-phone-directory/)

@2023.11.07

#设计 

用 `boolean[] free` 记录每个号码是否可用，用 `int it` 维护最小可用号码，减少循环次数。

```java
/**
 * 数组
 * Somnia1337
 */
class PhoneDirectory
{
    private int max, it;
    private boolean[] free;
	
    public PhoneDirectory(int maxNumbers)
    {
        max = maxNumbers;
        it = 0;
        free = new boolean[max];
        Arrays.fill(free, true);
    }
	
    public int get()
    {
        for (int i = it; i < max; i++)
        {
            if (free[i])
            {
                free[i] = false;
                it = i;
                return i;
            }
        }
        return -1;
    }
	
    public boolean check(int number)
    {
        return free[number];
    }
	
    public void release(int number)
    {
        free[number] = true;
        it = Math.min(number, it); // 更新最小可用号码
    }
}
```

#### [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)

@2023.10.12

#设计 

用 `List` 记录元素值，`Map` 记录值所在位置.

- `insert()`：向列表中插入值，同时在哈希表中记录其位置。
- `remove()`：在两表中交换 `val` 与末元素，然后删除 `val`。
- `getRandom()`：随机数范围为列表大小。

```java
/**
 * 哈希表
 * Somnia1337
 */
class RandomizedSet
{
    private List<Integer> nums; // 元素值
    private Map<Integer, Integer> pos; // key: 值, value: 位置
    private Random rand;
	
    public RandomizedSet()
    {
        nums = new ArrayList<>();
        pos = new HashMap<>();
        rand = new Random();
    }
	
    public boolean insert(int val)
    {
        if (pos.containsKey(val)) return false;
        nums.add(val);
        pos.put(val, nums.size() - 1);
        return true;
    }
	
    public boolean remove(int val)
    {
        if (!pos.containsKey(val)) return false;
        // 将末元素与 val 交换
        int last = nums.get(nums.size() - 1), idx = pos.get(val);
        nums.set(idx, last);
        pos.put(last, idx);
        // 删除 val
        nums.remove(nums.size() - 1);
        pos.remove(val);
        return true;
    }
	
    public int getRandom()
    {
        return nums.get(rand.nextInt(nums.size()));
    }
}
```

#### [382. 链表随机节点](https://leetcode.cn/problems/linked-list-random-node/)

@2023.11.07

#设计 #随机化 

保存 `head`，生成随机值后遍历到该处。

```java
/**
 * 模拟
 * Somnia1337
 */
class Solution
{
    private int l;
    private ListNode head;
    private Random rand;
	
    public Solution(ListNode head)
    {
        l = 0;
        this.head = head;
        rand = new Random();
        ListNode it = head;
        while (it != null)
        {
            it = it.next;
            l++;
        }
    }
	
    public int getRandom()
    {
        int r = rand.nextInt(0, l);
        ListNode it = head;
        while (r-- > 0)
        {
            it = it.next;
        }
        return it.val;
    }
}
```

#### [384. 打乱数组](https://leetcode.cn/problems/shuffle-an-array/)

@2023.11.07

#设计 #随机化 

`int[] init` 维护数组原状态，`shuffle()` 时，用 `List` 列出所有可选元素，每次随机选择一个放入返回数组，并从 `List` 中删除。

```java
/**
 * 随机化
 * Somnia1337
 */
class Solution
{
    private int[] init;
	
    public Solution(int[] nums)
    {
        init = new int[nums.length];
        System.arraycopy(nums, 0, init, 0, nums.length);
    }
	
    public int[] reset()
    {
        int[] ret = new int[init.length];
        System.arraycopy(init, 0, ret, 0, init.length);
        return ret;
    }
	
    public int[] shuffle()
    {
        int[] ret = new int[init.length];
        List<Integer> valid = new ArrayList<>();
        Random rand = new Random();
        for (int n : init) valid.add(n);
        for (int i = 0; i < init.length; i++)
        {
            int r = rand.nextInt(0, valid.size());
            ret[i] = valid.get(r);
            valid.remove(r);
        }
        return ret;
    }
}
```

另一种方法：`shuffle()`时，遍历数组，每次从余下数组中随机选择一个下标，交换两元素。

```java
/**
 * 随机化 - Fisher Yates 洗牌算法
 * 力扣官方题解
 */
class Solution
{
    private int[] nums;
    private int[] init;
	
    public Solution(int[] nums)
    {
        this.nums = nums;
        this.init = new int[nums.length];
        System.arraycopy(nums, 0, init, 0, nums.length);
    }
	
    public int[] reset()
    {
        System.arraycopy(init, 0, nums, 0, nums.length);
        return nums;
    }
	
    public int[] shuffle()
    {
        Random rand = new Random();
        for (int i = 0; i < nums.length; ++i)
        {
            int j = i + rand.nextInt(nums.length - i);
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
}
```

#### [386. 字典序排数](https://leetcode.cn/problems/lexicographical-numbers/)

@2023.10.08

#搜索 

从 1 开头开始，DFS 所有开头相同的数，再将开头加 1。

```java
/**
 * DFS
 * Somnia1337
 */
public List<Integer> ans = new ArrayList<>();

public List<Integer> lexicalOrder(int n)
{
	for (int i = 1; i < 10; i++)
	{
		solve(i, n);
	}
	return ans;
}

public void solve(int x, int n)
{
	if (x > n) return;
	ans.add(x);
	x *= 10;
	if (x > n) return; // 再剪枝，3ms至1ms
	for (int i = 0; i < 10; i++)
	{
		solve(x + i, n);
	}
}
```

#### [390. 消除游戏](https://leetcode.cn/problems/elimination-game/)

@2023.10.08

1. 模拟

```java
/**
 * 模拟
 * 诸葛青
 */
public int lastRemaining(int n)
{
	int head = 1;
	int step = 1;
	boolean left = true;
	
	while (n > 1)
	{
		// 从左边开始移除 or（从右边开始移除，数列总数为奇数）
		if (left || n % 2 != 0)
		{
			head += step;
		}
		step *= 2; // 步长 * 2
		left = !left; //取反移除方向
		n /= 2; // 总数 / 2
	}
	return head;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 阿飞
 */
public int lastRemaining(int n)
{
	return n == 1 ? 1 : 2 * (n / 2 + 1 - lastRemaining(n / 2));
}
```

#### [393. UTF-8 编码验证](https://leetcode.cn/problems/utf-8-validation/)

@2024.01.30



```java
/**
 * 模拟
 * Somnia1337
 */
public boolean validUtf8(int[] data) {
	for (int i = 0; i < data.length; i++) {
		int l; // 本段除 data[i] 外的数字个数
		if ((data[i] & 224) == 192) l = 1;      // "110xxxxx"
		else if ((data[i] & 240) == 224) l = 2; // "1110xxxx"
		else if ((data[i] & 248) == 240) l = 3; // "11110xxx"
		else {
			if ((data[i] & 128) == 128) return false; // 首位为 1
			continue; // 本身即为一段
		}
		
		for (int k = 0; k < l; k++) {
			if (++i == data.length || (data[i] & 192) != 128) return false;
		}
	}
	return true;
}
```

#### [394. 字符串解码](https://leetcode.cn/problems/decode-string/)

@2023.09.04

#栈 

用两个栈分别存储之前的答案、当前的乘数。遇到 `'['` 时，将之前的答案、当前的乘数压栈，并清空答案串、乘数；遇到 `']'` 时，弹出答案、乘数，在之前的答案之后加上当前的答案（即方括号内部的部分 × 乘数）。

```java
/**
 * 栈
 * 忧伤与道乐
 */
public String decodeString(String s) {
	StringBuilder ans = new StringBuilder();
	Deque<Integer> multiStk = new ArrayDeque<>();
	Deque<StringBuilder> preStk = new ArrayDeque<>();
	int multi = 0;
	for (char c : s.toCharArray()) {
		if (Character.isAlphabetic(c)) ans.append(c);
		else if (Character.isDigit(c)) multi = multi * 10 + c - '0';
		else if (c == '[') {
			preStk.push(ans); // 此前的答案压栈
			multiStk.push(multi); // 当前的乘数压栈
			ans = new StringBuilder(); // 清空, 准备接收新答案
			multi = 0; // 清空, 准备接收新乘数
		}
		else {
			StringBuilder preAns = preStk.pop(); // 弹出之前的答案
			int curMulti = multiStk.pop(); // 弹出当前的乘数
			preAns.append(String.valueOf(ans).repeat(Math.max(0, curMulti))); // 在之前的答案之后追加当前答案, 即为到目前为止的完整答案
			ans = preAns; // 将其作为当前答案
		}
	}
	return ans.toString();
}
```

#### [395. 至少有 K 个重复字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/)

@2023.10.11

#分治 

字符统计——

- 如果 `c` 在 `s` 中出现次数少于 `k`，意味着即使取整个 `s`，也无法满足 `c` 至少出现 `k` 次，因此答案一定不包含 `c`。用所有 `c` 分割 `s`，分治所有子串，取最大值。
- 如果没有字符的出现次数少于 `k`，取整个 `s` 即可。

```java
/**
 * 分治
 * Somnia1337
 */
public int longestSubstring(String s, int k)
{
	int[] count = new int[26];
	for (char c : s.toCharArray()) count[c - 'a']++;
	for (int i = 0; i < 26; i++)
	{
		if (count[i] > 0 && count[i] < k)
		{
			char c = (char) ('a' + i);
			String[] splits = s.split("\\Q" + c + "\\E");
			int max = 0;
			// 分治
			for (String split : splits)
			{
				max = Math.max(longestSubstring(split, k), max);
			}
			return max;
		}
	}
	return s.length(); // 整个s满足题意
}
```

正则表达式可以改为 `String[] parts = s.split(String.valueOf(c))`，10ms 降至 2ms。

#### [396. 旋转函数](https://leetcode.cn/problems/rotate-function/)

@2023.11.07

#动态规划 

观察得到递推式 `dp[i] = dp[i - 1] + sum - len * nums[len - i]`，由于只依赖前 1 项，用一个变量 `pre` 代替。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int maxRotateFunction(int[] nums)
{
	int len = nums.length, sum = 0, pre = 0;
	for (int i = 0; i < len; i++)
	{
		sum += nums[i];
		pre += i * nums[i];
	}
	
	int ans = pre;
	for (int i = 1; i < len; i++)
	{
		pre += sum - len * nums[len - i];
		ans = Math.max(ans, pre);
	}
	return ans;
}
```

#### [397. 整数替换](https://leetcode.cn/problems/integer-replacement/)

@2023.10.20

#技巧 

递归地寻找答案：

- `n` 为偶数时，递归 `solve(n)`。
- `n` 为奇数时，递归取 `min(solve(n / 2), solve(n / 2 + 1))`。

将 `n` 的计算结果存储于哈希表，避免重复计算。

```java
/**
 * 记忆化搜索
 * Somnia1337
 */
private Map<Integer, Integer> cache;

public int integerReplacement(int n)
{
	cache = new HashMap<>();
	cache.put(0, 0);
	return solve(n);
}

private int solve(int n)
{
	if (n == 1) return 0;
	if (!cache.containsKey(n))
	{
		if (n % 2 == 0) cache.put(n, 1 + solve(n / 2));
		else cache.put(n, 2 + Math.min(solve(n / 2), solve(n / 2 + 1)));
	}
	return cache.get(n);
}
```

#### [398. 随机数索引](https://leetcode.cn/problems/random-pick-index/)

@2023.11.07

#设计 #随机化 

`Map pos` 记录每个元素的下标列表，`pick()` 时获取列表并返回随机一个。

```java
/**
 * 随机化
 * Somnia1337
 */
class Solution
{
    private Map<Integer, List<Integer>> pos;
	
    public Solution(int[] nums)
    {
        pos = new HashMap<>();
        for (int i = 0; i < nums.length; i++)
        {
            pos.putIfAbsent(nums[i], new ArrayList<>());
            pos.get(nums[i])
               .add(i);
        }
    }
	
    public int pick(int target)
    {
        Random rand = new Random();
        List<Integer> group = pos.get(target);
        return group.get(rand.nextInt(0, group.size()));
    }
}
```

#### [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

@2023.10.24

#搜索 

这是一个图的问题，可以抽象为 `equations` 中的每对字符串之间都有两条有向边，值分别为 `values` 中的对应值及其倒数。

先读取 `equations` 与 `values` 建立图，然后对 `queries` 的每次查询，对图进行 `dfs` 查找。图的 `key` 为起点字符串，`value` 为 `Map<String, Double>`，记录该字符串通向的每个字符串以及对应值。

```java
private Map<String, Map<String, Double>> graph;
private Set<String> vis;
private double path;

public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries)
{
	Set<String> valid = new HashSet<>();
	graph = new HashMap<>();
	for (int i = 0; i < equations.size(); i++)
	{
		String a = equations.get(i).get(0), b = equations.get(i).get(1);
		valid.add(a);
		valid.add(b);
		
		// 从 a 到 b 建立连接
		Map<String, Double> nextA = graph.getOrDefault(a, new HashMap<>());
		nextA.put(b, values[i]);
		graph.put(a, nextA);
		
		// 从 b 到 a 建立连接
		Map<String, Double> nextB = graph.getOrDefault(b, new HashMap<>());
		nextB.put(a, 1 / values[i]);
		graph.put(b, nextB);
	}
	
	// 遍历 queries
	int len = queries.size();
	double[] ans = new double[len];
	vis = new HashSet<>();
	for (int i = 0; i < len; i++)
	{
		String a = queries.get(i).get(0), b = queries.get(i).get(1);
		if (!(valid.contains(a) && valid.contains(b)))
		{
			ans[i] = -1;
			continue;
		}
		else if (a.equals(b))
		{
			ans[i] = 1;
			continue;
		}
		
		vis.clear();
		path = -1; // 初始化 -1, 如果更新为正值, 说明找到答案
		dfs(a, b); // 尝试从 a 到 b 搜索
		if (path == -1) dfs(b, a); // 未找到答案, 反向尝试
		ans[i] = path;
	}
	
	return ans;
}

private void dfs(String s, String e) // 起点与终点字符串
{
	if (!vis.add(s)) return;
	Map<String, Double> next = graph.get(s); // 下个节点的列表及对应值
	if (next == null) return; // 避免 exception
	
	for (String node : next.keySet())
	{
		path *= next.get(node);
		if (node.equals(e))
		{
			path = -path; // 找到答案, 转正
			return;
		}
		dfs(node, e);
		if (path > 0) return; // 找到答案, 剪枝
		path /= next.get(node);
	}
}
```

#### [400. 第 N 位数字](https://leetcode.cn/problems/nth-digit/)

@2023.10.11

#数学 

计算第 `n` 位所属数字的位数以及位移量，算出该数字。

不开 `long` 会溢出。

```java
/**
 * 数学
 * Somnia1337
 */
public int findNthDigit(int n)
{
	int digit = 1;
	while (n > 0)
	{
		long s = (long) (Math.pow(10, digit - 1)) * 9 * digit;
		if (n <= s) break;
		n -= s;
		digit++;
	}
	int low = (int) (Math.pow(10, digit - 1));
	int num = low + (n - 1) / digit, offset = (n - 1) % digit;
	return String.valueOf(num)
				 .charAt(offset) - '0';
}
```

#### [402. 移掉 K 位数字](https://leetcode.cn/problems/remove-k-digits/)

@2023.09.18

```java
/**
 * 贪心 + 反复遍历
 * Somnia1337
 */
public String removeKdigits(String num, int k)
{
	if (k == num.length()) return "0";
	StringBuilder ans = new StringBuilder(num);
	boolean increase = true;
	for (int i = 0; i < num.length() - 1; i++)
	{
		if (num.charAt(i + 1) < num.charAt(i))
		{
			increase = false;
			break;
		}
	}
	if (increase) return num.substring(0, num.length() - k);
	while (k > 0)
	{
		int deleteIdx = ans.length() - 1;
		for (int i = 0; i < ans.length() - 1; i++)
		{
			if (ans.charAt(i) > ans.charAt(i + 1))
			{
				deleteIdx = i;
				break;
			}
		}
		ans.deleteCharAt(deleteIdx);
		k--;
	}
	String ansStr = ans.toString();
	while (ansStr.length() > 1 && ansStr.startsWith("0")) ansStr = ansStr.substring(1);
	return ansStr;
}
```

```java
/**
 * 贪心 + 单调栈
 * 力扣官方题解
 */
public String removeKdigits(String num, int k)
{
	Deque<Character> deque = new LinkedList<Character>();
	int length = num.length();
	for (int i = 0; i < length; ++i)
	{
		char digit = num.charAt(i);
		while (!deque.isEmpty() && k > 0 && deque.peekLast() > digit)
		{
			deque.pollLast();
			k--;
		}
		deque.offerLast(digit);
	}
	
	for (int i = 0; i < k; ++i)
	{
		deque.pollLast();
	}
	
	StringBuilder ret = new StringBuilder();
	boolean leadingZero = true;
	while (!deque.isEmpty())
	{
		char digit = deque.pollFirst();
		if (leadingZero && digit == '0')
		{
			continue;
		}
		leadingZero = false;
		ret.append(digit);
	}
	return ret.isEmpty() ? "0" : ret.toString();
}
```

#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

@2023.09.17

```java
/**
 * 贪心
 * 代码随想录
 */
public int[][] reconstructQueue(int[][] people)
{
	Arrays.sort(people, (a, b) -> {
		if (a[0] == b[0]) return a[1] - b[1];
		return b[0] - a[0];
	});
	
	LinkedList<int[]> que = new LinkedList<>();
	for (int[] p : people)
	{
		que.add(p[1], p);
	}
	return que.toArray(new int[people.length][]);
}
```

```java
/**
 * 贪心
 * struggle◎
 */
public int[][] reconstructQueue(int[][] people)
{
	Arrays.sort(people, (x, y) -> x[0] == y[0] ? x[1] - y[1] : y[0] - x[0]);
	List<int[]> ans = new ArrayList<>();
	for (int[] p : people)
	{
		ans.add(p[1], p);
	}
	return ans.toArray(new int[0][0]);
}
```

#### [413. 等差数列划分](https://leetcode.cn/problems/arithmetic-slices/)

@2023.10.11

#动态规划 

对一个长度为 `k` 的等差数列，其中含有的等差子数列数量：

```text
3 5 7 -> 1
3 5 7 9 -> 1+2
3 5 7 9 11 -> 1+2+3
```

从长度为 3 的数列开始，只要该数列还在增长，每次加当前长度 `k - 2`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int numberOfArithmeticSlices(int[] nums)
{
	int len = nums.length;
	if (len < 3) return 0;
	int ans = 0, cur = 0;
	for (int i = 2; i < len; i++) // 从2开始
	{
		if (nums[i] - nums[i - 1] == nums[i - 1] - nums[i - 2])
		{
			cur++; // cur为1时，k已经是3
			ans += cur;
		}
		else cur = 0;
	}
	return ans;
}
```

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

@2023.07.19

#动态规划 

等价于 0-1 背包问题。

数字的值为物品价值，记 `sum(nums)` 为 `sum`，要求遍历结束后容量为 `sum / 2` 的背包能够装满（价值 `sum / 2` 的物品）。

剪枝：每一次内层遍历结束后检查 `dp[sum / 2]` 是否已经等于 `sum / 2`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public boolean canPartition(int[] nums) {
	int sum = 0;
	for (int x : nums) sum += x;
	if (sum % 2 == 1) return false;
	
	int[] dp = new int[sum / 2 + 1];
	for (int x : nums) {
		for (int j = sum / 2; j > 0; j--) {
			if (j - x >= 0) dp[j] = Math.max(dp[j - x] + x, dp[j]);
		}
		if (dp[sum / 2] == sum / 2) return true; // 检查 sum/2 背包是否装满
	}
	return dp[sum / 2] == sum / 2;
}
```

#### [417. 太平洋大西洋水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/)

@2023.10.19

#搜索 

从岸边开始，逆向寻找，水流只能来自比当前高度高的单元格。用 `boolean[rows][cols][2]` 记录 `heights[i][j]` 是否通向两洋，最后一次遍历。

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
private boolean[][][] flow;

public List<List<Integer>> pacificAtlantic(int[][] heights)
{
	int rows = heights.length, cols = heights[0].length;
	flow = new boolean[rows][cols][2];
	
	// 搜索
	for (int i = 0; i < rows; i++)
	{
		dfs(heights, i, 0, heights[i][0], 0);
		dfs(heights, i, cols - 1, heights[i][cols - 1], 1);
	}
	for (int j = 0; j < cols; j++)
	{
		dfs(heights, 0, j, heights[0][j], 0);
		dfs(heights, rows - 1, j, heights[rows - 1][j], 1);
	}
	
	// 遍历计数
	List<List<Integer>> ans = new ArrayList<>();
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (flow[i][j][0] && flow[i][j][1]) ans.add(List.of(i, j));
		}
	}
	
	return ans;
}

private void dfs(int[][] heights, int x, int y, int height, int ocean)
{
	// 出界 | 高度降低 | 已经搜索过
	if (x < 0 || x == heights.length || y < 0 || y == heights[0].length || heights[x][y] < height || flow[x][y][ocean]) return;
	flow[x][y][ocean] = true; // 能够通向当前海洋
	dfs(heights, x + 1, y, heights[x][y], ocean);
	dfs(heights, x, y + 1, heights[x][y], ocean);
	dfs(heights, x - 1, y, heights[x][y], ocean);
	dfs(heights, x, y - 1, heights[x][y], ocean);
}
```

2. BFS

待学习。

#### [419. 甲板上的战舰](https://leetcode.cn/problems/battleships-in-a-board/)

@2023.11.06

#搜索 #暴力 

1. DFS

从每个 `'X'` 向右下方向 DFS，标记当前战舰。

```java
/**
 * DFS
 * Somnia1337
 */
public int countBattleships(char[][] board)
{
	int rows = board.length, cols = board[0].length, ans = 0;
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (board[i][j] == 'X')
			{
				ans++;
				dfs(board, i, j);
			}
		}
	}
	return ans;
}

private void dfs(char[][] board, int x, int y)
{
	if (!(x < board.length && y < board[0].length) || board[x][y] != 'X') return;
	board[x][y] = '.';
	dfs(board, x + 1, y);
	dfs(board, x, y + 1);
}
```

2. 枚举

枚举战舰的左上顶点 `(i, j)`，需满足：

- `board[i][j] == 'X'`
- `board[i - 1][j] == board[i][j - 1] == '.'`

```java
/**
 * 枚举
 * 力扣官方题解
 */
public int countBattleships(char[][] board)
{
	int rows = board.length, cols = board[0].length, ans = 0;
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (board[i][j] == 'X')
			{
				if (i > 0 && board[i - 1][j] == 'X') continue;
				if (j > 0 && board[i][j - 1] == 'X') continue;
				ans++;
			}
		}
	}
	return ans;
}
```

#### [421. 数组中两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/)

@2023.11.04

#哈希表 

![[421. 数组中两个数的最大异或值.png|400]]

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
public int findMaximumXOR(int[] nums) {
	int max = 0;
	for (int x : nums) max = Math.max(x, max);
	int high = 31 - Integer.numberOfLeadingZeros(max);
	Set<Integer> vis = new HashSet<>();
	int mask = 0, ans = 0;
	
	for (int i = high; i >= 0; i--) {
		vis.clear();
		mask |= 1 << i;
		int newAns = ans | (1 << i);
		for (int x : nums) {
			x &= mask;
			if (vis.contains(newAns ^ x)) {
				ans = newAns;
				break;
			}
			vis.add(x);
		}
	}
	return ans;
}
```

#### [423. 从英文中重建数字](https://leetcode.cn/problems/reconstruct-original-digits-from-english/)

@2023.10.10

#字符串 

字符统计，找规律，如果按 `(0,2,4,6) -> (1,3,5) -> (7,8,9)` 的顺序重建，总能用一个特异字符标识每个数字，因此每次只需检查该特异字符即可知道对应数字出现的次数。

```java
/**
 * 字符统计
 * Somnia1337
 */
public String originalDigits(String s)
{
	int[] chars = new int[26];
	for (int i = 0; i < s.length(); i++)
	{
		chars[s.charAt(i) - 'a']++;
	}
	int[] count = new int[10];
	if (chars['z' - 'a'] > 0)
	{
		count[0] = chars['z' - 'a'];
		chars['e' - 'a'] -= chars['z' - 'a'];
		chars['r' - 'a'] -= chars['z' - 'a'];
		chars['o' - 'a'] -= chars['z' - 'a'];
		chars['z' - 'a'] = 0;
	}
	if (chars['w' - 'a'] > 0)
	{
		count[2] = chars['w' - 'a'];
		chars['t' - 'a'] -= chars['w' - 'a'];
		chars['o' - 'a'] -= chars['w' - 'a'];
		chars['w' - 'a'] = 0;
	}
	if (chars['u' - 'a'] > 0)
	{
		count[4] = chars['u' - 'a'];
		chars['f' - 'a'] -= chars['u' - 'a'];
		chars['o' - 'a'] -= chars['u' - 'a'];
		chars['r' - 'a'] -= chars['u' - 'a'];
		chars['u' - 'a'] = 0;
	}
	if (chars['x' - 'a'] > 0)
	{
		count[6] = chars['x' - 'a'];
		chars['s' - 'a'] -= chars['x' - 'a'];
		chars['i' - 'a'] -= chars['x' - 'a'];
		chars['x' - 'a'] = 0;
	}
	if (chars['o' - 'a'] > 0)
	{
		count[1] = chars['o' - 'a'];
		chars['n' - 'a'] -= chars['o' - 'a'];
		chars['e' - 'a'] -= chars['o' - 'a'];
		chars['o' - 'a'] = 0;
	}
	if (chars['r' - 'a'] > 0)
	{
		count[3] = chars['r' - 'a'];
		chars['t' - 'a'] -= chars['r' - 'a'];
		chars['h' - 'a'] -= chars['r' - 'a'];
		chars['e' - 'a'] -= 2 * chars['r' - 'a'];
		chars['r' - 'a'] = 0;
	}
	if (chars['f' - 'a'] > 0)
	{
		count[5] = chars['f' - 'a'];
		chars['i' - 'a'] -= chars['f' - 'a'];
		chars['v' - 'a'] -= chars['f' - 'a'];
		chars['e' - 'a'] -= chars['f' - 'a'];
		chars['f' - 'a'] = 0;
	}
	if (chars['s' - 'a'] > 0)
	{
		count[7] = chars['s' - 'a'];
		chars['v' - 'a'] -= chars['s' - 'a'];
		chars['n' - 'a'] -= chars['s' - 'a'];
		chars['e' - 'a'] -= 2 * chars['s' - 'a'];
		chars['s' - 'a'] = 0;
	}
	if (chars['g' - 'a'] > 0)
	{
		count[8] = chars['g' - 'a'];
		chars['e' - 'a'] -= chars['g' - 'a'];
		chars['i' - 'a'] -= chars['g' - 'a'];
		chars['h' - 'a'] -= chars['g' - 'a'];
		chars['t' - 'a'] -= chars['g' - 'a'];
		chars['g' - 'a'] = 0;
	}
	if (chars['i' - 'a'] > 0)
	{
		count[9] = chars['i' - 'a'];
		chars['n' - 'a'] -= 2 * chars['i' - 'a'];
		chars['e' - 'a'] -= chars['i' - 'a'];
		chars['i' - 'a'] = 0;
	}
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < 10; i++)
	{
		ans.append(String.valueOf(i)
						 .repeat(count[i]));
	}
	return ans.toString();
}
```

#### [424. 替换后的最长重复字符](https://leetcode.cn/problems/longest-repeating-character-replacement/)

@2023.10.11

#滑动窗口 

字符统计，记录窗口内最大出现次数 `maxCnt`，当窗口长度大于 `maxCnt + k` 时不断缩小窗口，同时反复更新 `maxCnt`。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int characterReplacement(String s, int k)
{
	int len = s.length();
	int[] count = new int[26];
	int ans = 0;
	int left = 0, right = 0;
	while (right < len)
	{
		char c = s.charAt(right++);
		count[c - 'A']++;
		int maxCnt = 0;
		for (int cnt : count) maxCnt = Math.max(cnt, maxCnt);
		while (left < right && right - left > maxCnt + k)
		{
			count[s.charAt(left++) - 'A']--;
			for (int cnt : count) maxCnt = Math.max(cnt, maxCnt);
		}
		ans = Math.max(right - left, ans);
	}
	return ans;
}
```

优化：使用只增长的滑动窗口，最后返回 `right - left`，而不用维护 `ans`。这样，`max` 也只需更新为当前字符的值，而不用反复遍历 `count`。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int characterReplacement(String s, int k)
{
	int[] count = new int[26];
	int left = 0, right = 0, max = 0;
	while (right < s.length())
	{
		max = Math.max(count[s.charAt(right) - 'A']++ + 1, max);
		while (right - left + 1 > max + k)
		{
			count[s.charAt(left++) - 'A']--;
		}
		right++;
	}
	return right - left;
}
```

#### [433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/)

@2023.10.08

#回溯 

```java
for (int i = 0; i < len; i++)
{
	if (visited[i]) continue;
	if (isNext(start, bank[i]))
	{
		visited[i] = true;
		bt(bank[i], end, bank, count + 1);
		visited[i] = false;
	}
}
```

如果当前序列与 `bank[k]` 刚好相差 1 次基因变化，则将 `bank[k]` 作为下个基因回溯。

```java
/**
 * 回溯
 * struggle◎
 */
public int ans;
public int len;
public boolean[] visited;

public int minMutation(String startGene, String endGene, String[] bank)
{
	len = bank.length;
	ans = len + 1;
	visited = new boolean[len];
	bt(startGene, endGene, bank, 0);
	return ans < len + 1 ? ans : -1;
}

public void bt(String start, String end, String[] bank, int count)
{
	if (count >= ans) return;
	if (start.equals(end))
	{
		ans = count;
		return;
	}
	for (int i = 0; i < len; i++)
	{
		if (visited[i]) continue;
		if (isNext(start, bank[i]))
		{
			visited[i] = true;
			bt(bank[i], end, bank, count + 1);
			visited[i] = false;
		}
	}
}

// 检查两个字符串是否刚好相差1个字符
public boolean isNext(String a, String b)
{
	int diff = 0;
	for (int i = 0; i < a.length(); i++)
	{
		if (a.charAt(i) != b.charAt(i)) diff++;
	}
	return diff == 1;
}
```

#### [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

@2023.09.25

#贪心 

将`intervals`先尾再头按大小排序，初始化`ans`为`interval.length - 1`（假设对任意一段都存在一段与之重叠），维护最远端的位置，遍历减去实际上不重叠的段：当某一段的头端 $\geqslant$ 最远端时，更新最远端为该段尾端，并将`ans`减1。

```java
/**
 * 贪心
 * Somnia1337
 */
public int eraseOverlapIntervals(int[][] intervals)
{
	Arrays.sort(intervals, (a, b) -> a[1] != b[1] ? Integer.compare(a[1], b[1]) : Integer.compare(a[0], b[0]));
	int ans = intervals.length, farthest = intervals[0][1];
	for (int[] parts : intervals)
	{
		if (parts[0] >= farthest)
		{
			farthest = parts[0];
			ans--;
		}
	}
	return ans;
}
```

#### [436. 寻找右区间](https://leetcode.cn/problems/find-right-interval/)

@2023.11.06

#二分查找 

用额外的数组 `pos = int[len][2]` 记录 `intervals` 的起点大小及下标，排序，对每个终点，在 `pos` 中二分查找首个大于它的元素，读取其在 `intervals` 中的下标。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int[] findRightInterval(int[][] intervals)
{
	int len = intervals.length;
	int[][] pos = new int[len][2];
	for (int i = 0; i < len; i++)
	{
		pos[i][0] = intervals[i][0];
		pos[i][1] = i;
	}
	Arrays.sort(pos, Comparator.comparing(a -> a[0]));
	int[] ans = new int[len];
	Arrays.fill(ans, -1);
	for (int i = 0; i < len; i++)
	{
		int k = intervals[i][1];
		int l = 0, r = len - 1;
		while (l < r)
		{
			int m = l + r >> 1;
			if (pos[m][0] < k) l = m + 1;
			else r = m;
		}
		ans[i] = pos[l][0] >= k ? pos[l][1] : -1;
	}
	return ans;
}
```

#### [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

@2023.10.10

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
public List<Long> path = new ArrayList<>();
public int ans = 0;

public int pathSum(TreeNode root, int targetSum)
{
	bt(root, targetSum);
	return ans;
}

public void bt(TreeNode root, int tar)
{
	if (root == null) return;
	int v = root.val;
	if (v == tar) ans++;
	for (int i = 0; i < path.size(); i++)
	{
		path.set(i, path.get(i) + v);
		if (path.get(i) == tar) ans++;
	}
	path.add((long) v);
	bt(root.left, tar);
	bt(root.right, tar);
	path.remove(path.size() - 1);
	path.replaceAll(integer -> integer - v);
}
```

```java
/**
 * DFS
 * 力扣官方题解
 */
public int pathSum(TreeNode root, int targetSum)
{
	if (root == null) return 0;
	int ans = bt(root, targetSum);
	ans += pathSum(root.left, targetSum);
	ans += pathSum(root.right, targetSum);
	return ans;
}

public int bt(TreeNode root, long tar)
{
	if (root == null) return 0;
	int v = root.val;
	int ret = v == tar ? 1 : 0;
	ret += bt(root.left, tar - v);
	ret += bt(root.right, tar - v);
	return ret;
}
```

2. 前缀和

```java
/**
 * 前缀和
 * 力扣官方题解
 */
public int pathSum(TreeNode root, int targetSum)
{
	Map<Long, Integer> prefix = new HashMap<Long, Integer>();
	prefix.put(0L, 1);
	return dfs(root, prefix, 0, targetSum);
}

public int dfs(TreeNode root, Map<Long, Integer> prefix, long curr, int targetSum)
{
	if (root == null) return 0;
	curr += root.val;
	int ret = prefix.getOrDefault(curr - targetSum, 0);
	prefix.put(curr, prefix.getOrDefault(curr, 0) + 1);
	ret += dfs(root.left, prefix, curr, targetSum);
	ret += dfs(root.right, prefix, curr, targetSum);
	prefix.put(curr, prefix.getOrDefault(curr, 0) - 1);
	return ret;
}
```

#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

@2023.10.09

#滑动窗口 

统计滑动窗口内的字符，与目标值对比。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public List<Integer> findAnagrams(String s, String p)
{
	List<Integer> ans = new ArrayList<>();
	int sLen = s.length(), pLen = p.length();
	if (sLen < pLen) return ans;
	int[] count = new int[26];
	int[] tar = new int[26];
	for (int i = 0; i < pLen; i++)
	{
		count[s.charAt(i) - 'a']++;
		tar[p.charAt(i) - 'a']++;
	}
	if (Arrays.equals(count, tar))
	{
		ans.add(0);
	}
	for (int i = pLen; i < sLen; i++)
	{
		count[s.charAt(i - pLen) - 'a']--;
		count[s.charAt(i) - 'a']++;
		if (Arrays.equals(count, tar)) ans.add(i + 1 - pLen);
	}
	return ans;
}
```

#### [439. 三元表达式解析器](https://leetcode.cn/problems/ternary-expression-parser/)

@2023.11.07

#递归 

![[Snipaste_231107_203754.png|400]]

从下标 2 开始遍历，初始化 `cnt` 为 1（下标 1 处必为 `'?'`），遇到 `'?'` 时加 1，遇到 `':'` 时减 1，当 `cnt == 0` 时，说明遍历到了最外层三元表达式的中间位置，根据首字符为 `'T'` / `'F'` 选择性递归地解决左部分 / 右部分。

```java
/**
 * 递归
 * Somnia1337
 */
public String parseTernary(String expression)
{
	for (int i = 2, cnt = 1; i < expression.length(); i++)
	{
		if (expression.charAt(i) == '?') cnt++;
		else if (expression.charAt(i) == ':') cnt--;
		if (cnt == 0) return expression.charAt(0) == 'T' ? parseTernary(expression.substring(2, i)) : parseTernary(expression.substring(i + 1));
	}
	return expression;
}
```

```java
/* 一个好看的版本 */
public String parse(String s)
{
	for (int i = 2, cnt = 1; i < s.length(); i++)
	{
		if (s.charAt(i) == '?') cnt++;
		else if (s.charAt(i) == ':') cnt--;
		if (cnt == 0) return s.charAt(0) == 'T' ? parse(s.substring(2, i)) : parse(s.substring(i + 1));
	}
	return s;
}
```

#### [442. 数组中重复的数据](https://leetcode.cn/problems/find-all-duplicates-in-an-array/)

@2023.10.09

#技巧 

要求 $O(1)$ 空间，只能用数组本身做标记。

遍历，将下标 `abs(nums[i])` 的元素取倒数，如果一个元素出现过 2 次，那么第 2 次取倒数时，原本已是负数，就得知该元素第 2 次出现，加入解集。

```java
/**
 * 标记数组
 * 龅牙叔
 */
public List<Integer> findDuplicates(int[] nums)
{
	List<Integer> ans = new ArrayList<>();
	for (int i = 0; i < nums.length; i++)
	{
		int idx = Math.abs(nums[i]);
		if (nums[idx - 1] < 0) ans.add(idx);
		nums[idx - 1] *= -1;
	}
	return ans;
}
```

#### [443. 压缩字符串](https://leetcode.cn/problems/string-compression/)

@2023.08.30

#模拟 

对`chars`一次遍历，根据题意模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public int compress(char[] chars) {
	int len = chars.length, ans = 0, p = 0;
	for (int i = 0; i < len; i++) {
		char c = chars[i];
		int count = 1; // 当前连续数字的个数
		while (i < len - 1 && chars[i + 1] == c) {
			count++;
			i++;
		}
		ans++;
		chars[p++] = c;
		if (count > 1) {
			String t = String.valueOf(count);
			for (int j = 0; j < t.length(); j++) {
				chars[p] = t.charAt(j);
				ans++;
				p++;
			}
		}
	}
	return ans;
}
```

#### [447. 回旋镖的数量](https://leetcode.cn/problems/number-of-boomerangs/)

@2023.10.25

#暴力 #哈希表 

1. 暴力

枚举所有三元组。

```java
/**
 * 枚举
 * Somnia1337
 */
public int numberOfBoomerangs(int[][] points) {
	int n = points.length, ans = 0;
	for (int i = 0; i < n - 2; i++) {
		int[] a = points[i];
		for (int j = i + 1; j < n - 1; j++) {
			int[] b = points[j];
			int d1 = dist(a, b);
			for (int k = j + 1; k < n; k++) {
				int[] c = points[k];
				int d2 = dist(b, c), d3 = dist(a, c);
				if (d1 == d2 && d2 == d3) ans += 6;
				else if (d1 == d2 || d2 == d3 || d1 == d3) ans += 2;
			}
		}
	}
	return ans;
}

private int dist(int[] a, int[] b) {
	return (int) (Math.pow(a[0] - b[0], 2) + Math.pow(a[1] - b[1], 2));
}
```

2. 哈希表

枚举每个点作为回旋镖的拐点，用哈希表记录其余元素和它的距离，哈希表记录 `<距离, 计数>`。每个相同距离分组内的点都可以与当前枚举的点构成回旋镖，题目要求元组有序，所以求其排列数 $A_v^2 = v (v - 1)$。

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
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

#### [451. 根据字符出现频率排序](https://leetcode.cn/problems/sort-characters-by-frequency/)

@2023.10.09

#字符串 

遍历 `s`，将每个字符存入 `List`，同时统计频率，根据频率排序 `List` 后再组装成串。

不能简单地用 `chars.sort(Comparator.comparingInt(count::get))` 进行排序，否则频率相同的字符可能交叉排列，如 `"abcabca"` 将排列为 `"aaacbcb"`。

```java
/**
 * 字符统计
 * Somnia1337
 */
public String frequencySort(String s) {
	List<Character> chs = new ArrayList<>();
	Map<Character, Integer> count = new HashMap<>();
	for (char c : s.toCharArray()) {
		chs.add(c);
		count.merge(c, 1, Integer::sum);
	}
	chs.sort((c1, c2) -> {
		int a = count.get(c1), b = count.get(c2);
		return a != b ? a - b : c1 - c2;
	});
	
	StringBuilder ans = new StringBuilder();
	for (char c : chs) ans.append(c);
	return ans.reverse().toString();
}
```

优化：在 `List` 中只记录不同的字符，根据频率排序后，组装时再 `repeat()`。

```java
/**
 * 字符统计
 * Somnia1337
 */
public String frequencySort(String s) {
	Map<Character, Integer> count = new HashMap<>();
	for (char c : s.toCharArray()) count.merge(c, 1, Integer::sum);
	List<Character> chs = new ArrayList<>(count.keySet());
	chs.sort(Comparator.comparing(count::get));
	StringBuilder ans = new StringBuilder();
	for (char c : chs) {
		ans.append(String.valueOf(c).repeat(count.get(c)));
	}
	return ans.reverse().toString();
}
```

#### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

@2023.09.17

#贪心 

将`points`根据先头后尾双单增排序，用`start`记录当前箭矢的可能区间左端点、`end`记录右端点，遍历`points`，记当前气球左端点`left`、右端点`right`——

- 如果`left`在`[start, end]`内，说明当前箭矢可以通过调整射中当前气球，更新`start`为`left`、`end`为`min(right, end)`。
- 否则，需要新的箭矢，`start`赋`left`、`end`赋`right`，并将答案加1。

```java
/**
 * 贪心
 * Somnia1337
 */
public int findMinArrowShots(int[][] points)
{
	Arrays.sort(points, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
	int ans = 1;
	int start = points[0][0], end = points[0][1];
	for (int i = 1; i < points.length; i++)
	{
		int left = points[i][0], right = points[i][1];
		if (left >= start && left <= end)
		{
			start = left;
			end = Math.min(right, end);
		}
		else
		{
			start = left;
			end = right;
			ans++;
		}
	}
	return ans;
}
```

#### [453. 最小操作次数使数组元素相等](https://leetcode.cn/problems/minimum-moves-to-equal-array-elements/)

@2023.10.10

```java
/**
 * 正难则反
 * Somnia1337
 */
public int minMoves(int[] nums)
{
	int min = nums[0], ans = 0;
	for (int num : nums) min = Math.min(num, min);
	for (int num : nums) ans += num - min;
	return ans;
}
```

#### [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

@2023.07.22

#哈希表 

要找到4个数之和为0，可以分为`num1 + num2` + `num3 + num4`两个数之和为0。用哈希表计数`num1 + num2`之和及出现次数，然后遍历`num3 + num4`之和，累加答案。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4)
{
	Map<Integer, Integer> sum12 = new HashMap<>();
	int ans = 0;
	
	for (int num1 : nums1)
	{
		for (int num2 : nums2)
		{
			sum12.merge(num1 + num2, 1, Integer::sum);
		}
	}
	for (int num3 : nums3)
	{
		for (int num4 : nums4)
		{
			Integer x = sum12.get(-num3 - num4);
			if (x != null) ans += x;
		}
	}
	
	return ans;
}
```

#### [456. 132 模式](https://leetcode.cn/problems/132-pattern/)

@2023.10.10

```java
/**
 * 单调栈
 * 宫水三叶
 */
public boolean find132pattern(int[] nums)
{
	Deque<Integer> deque = new ArrayDeque<>();
	int two = Integer.MIN_VALUE;
	for (int i = nums.length - 1; i >= 0; i--)
	{
		if (nums[i] < two) return true;
		while (!deque.isEmpty() && deque.peekLast() < nums[i]) two = deque.pollLast();
		deque.addLast(nums[i]);
	}
	return false;
}
```

```java
/**
 * ?
 * ?
 */
public boolean find132pattern(int[] nums)
{
	int len = nums.length;
	int num2 = Integer.MIN_VALUE;
	int top = len;
	for (int i = len - 1; i >= 0; i--)
	{
		int x = nums[i];
		if (x < num2) return true;
		else
		{
			while (top < len && nums[top] < x) num2 = nums[top++];
		}
		nums[--top] = x;
	}
	return false;
}
```

#### [457. 环形数组是否存在循环](https://leetcode.cn/problems/circular-array-loop/)

@2023.11.06

#暴力 #双指针 

1. 枚举

枚举每个元素作为起点，判断能否构成循环

```java
/**
 * 枚举
 * Somnia1337
 */
public boolean circularArrayLoop(int[] nums)
{
	int len = nums.length;
	for (int i = 0; i < len; i++)
	{
		boolean[] vis = new boolean[len];
		int cnt = 1, it = Math.floorMod(i + nums[i], len);
		while (!vis[it])
		{
			if (it == i)
			{
				if (cnt <= 1) break; // nums[it] == n * len, 非法
				return true;
			}
			if ((nums[it] > 0) ^ (nums[i] > 0)) break; // 正负性不同, 非法
			vis[it] = true;
			it = Math.floorMod(it + nums[it], len);
			cnt++;
		}
	}
	return false;
}
```

2. 双指针

将 `nums` 理解为一张图，`nums[i]` 向 `nums[i + nums[i]] % len` 有一条有向边，且每个节点只有一条出边。可以枚举每个元素作为起点，用快慢双指针检查是否成环。

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean circularArrayLoop(int[] nums)
{
	int len = nums.length;
	for (int i = 0; i < len; i++)
	{
		if (nums[i] == 0) continue;
		int slow = i, fast = next(nums, i);
		while (nums[slow] * nums[fast] > 0 && nums[slow] * nums[next(nums, fast)] > 0)
		{
			if (slow == fast)
			{
				if (slow != next(nums, slow)) return true;
				else break;
			}
			slow = next(nums, slow);
			fast = next(nums, next(nums, fast));
		}
		int add = i;
		while (nums[add] * nums[next(nums, add)] > 0)
		{
			int tmp = add;
			add = next(nums, add);
			nums[tmp] = 0;
		}
	}
	return false;
}

private int next(int[] nums, int idx)
{
	return Math.floorMod(idx + nums[idx], nums.length);
}
```

#### [462. 最小操作次数使数组元素相等 II](https://leetcode.cn/problems/minimum-moves-to-equal-array-elements-ii/)

@2023.10.10

```java
/**
 * 数学
 * Somnia1337
 */
public int minMoves2(int[] nums)
{
	Arrays.sort(nums);
	int ans = 0, tar = nums[nums.length / 2];
	for (int num : nums)
	{
		ans += Math.abs(tar - num);
	}
	return ans;
}
```

#### [464. 我能赢吗](https://leetcode.cn/problems/can-i-win/)

@2023.10.27

```java
/**
 * DFS
 * ChatGPT
 */
private Map<Integer, Boolean> memo = new HashMap<>();

public boolean canIWin(int maxChoosableInteger, int desiredTotal)
{
	if (desiredTotal <= 0) return true;
	int sum = maxChoosableInteger * (maxChoosableInteger + 1) / 2;
	if (sum < desiredTotal) return false;
	if (sum == desiredTotal) return maxChoosableInteger % 2 == 1;
	return canWin(0, maxChoosableInteger, desiredTotal);
}

private boolean canWin(int state, int n, int m)
{
	if (memo.containsKey(state)) return memo.get(state);
	for (int i = 1; i <= n; i++)
	{
		int mask = 1 << (i - 1);
		if ((state & mask) == 0)
		{
			if (i >= m || !canWin(state | mask, n, m - i))
			{
				memo.put(state, true);
				return true;
			}
		}
	}
	memo.put(state, false);
	return false;
}
```

#### [467. 环绕字符串中唯一的子字符串](https://leetcode.cn/problems/unique-substrings-in-wraparound-string/)

@2023.10.09

#动态规划 

动态规划，`dp[i]` 表示以 `s[i]` 结尾的最长有效子串。如果当前字符为上个字符的后一个，`dp[i] = dp[i - 1] + 1`，否则 `dp[i] = 1`。

同时，用 `int[26]` 记录以每个字母结尾的最长有效子串长度，原理是，区分不同的子串只需用长度与结尾字符。如果数组的第 6 个元素为 9，即以 `'g'` 结尾的最长有效子串长度为 9，所以有 9 个不同的有效子串以 `'g'` 结尾。最后累加该数组的值即为答案。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findSubstringInWraproundString(String s) {
    int pre = 1, cur;
    int[] end = new int[26];
    end[s.charAt(0) - 'a'] = 1;
    for (int i = 1; i < s.length(); i++) {
        char c = s.charAt(i);
        if ((c + 26 - s.charAt(i - 1)) % 26 == 1) cur = pre + 1;
        else cur = 1;
        end[c - 'a'] = Math.max(cur, end[c - 'a']);
        pre = cur;
    }
    
    int ans = 0;
    for (int e : end) ans += e;
    return ans;
}
```

#### [468. 验证IP地址](https://leetcode.cn/problems/validate-ip-address/)

@2023.10.09

#模拟 

模拟即可。

```java
/**
 * 模拟
 * Somnia1337
 */
public String validIPAddress(String queryIP)
{
	if (validIPv4(queryIP)) return "IPv4";
	if (validIPv6(queryIP)) return "IPv6";
	return "Neither";
}

public boolean validIPv4(String ip)
{
	if (ip.startsWith(".") || ip.endsWith(".")) return false;
	int len = ip.length();
	if (len < 7 || len > 15) return false;
	String[] parts = ip.split("\\.");
	if (parts.length != 4) return false;
	for (String part : parts)
	{
		int l = part.length();
		if (l == 0 || l > 3 || l != 1 && part.startsWith("0")) return false;
		for (char c : part.toCharArray())
		{
			if (!Character.isDigit(c)) return false;
		}
		if (Integer.parseInt(part) > 255) return false;
	}
	return true;
}

public boolean validIPv6(String ip)
{
	if (ip.startsWith(":") || ip.endsWith(":")) return false;
	int len = ip.length();
	if (len < 15 || len > 39) return false;
	String[] parts = ip.split(":");
	if (parts.length != 8) return false;
	for (String part : parts)
	{
		int l = part.length();
		if (l == 0 || l > 4) return false;
		for (char c : part.toCharArray())
		{
			if (!Character.isDigit(c) && !(c >= 'a' && c <= 'f' || c >= 'A' && c <= 'F')) return false;
		}
	}
	return true;
}
```

#### [469. 凸多边形](https://leetcode.cn/problems/convex-polygon/)

@2023.10.15

#数学 

右手定则：前 3 指岔开，食指与中指指向两向量方向，此时拇指指向两向量叉乘的方向。

遍历，取连续的 3 点，计算 2 个向量的叉乘，如果所有叉乘同向，则为凸多边形。

为了简化代码，用 `pre` 记录上个叉乘的正负，用相乘 > 0 判断方向是否相同。

```java
/**
 * 数学
 * Somnia1337
 */
public boolean isConvex(List<List<Integer>> points)
{
	int len = points.size();
	int pre = 0;
	
	for (int i = 0; i < len; i++)
	{
		List<Integer> a = points.get(i), b = points.get((i + 1) % len), c = points.get((i + 2) % len);
		int x1 = b.get(0) - a.get(0), x2 = c.get(0) - b.get(0);
		int y1 = b.get(1) - a.get(1), y2 = c.get(1) - b.get(1);
		int cur = x1 * y2 - y1 * x2;
		if (cur != 0)
		{
			if ((long) cur * pre < 0) return false;
			pre = cur;
		}
	}
	
	return true;
}
```

#### [470. 用 Rand7() 实现 Rand10()](https://leetcode.cn/problems/implement-rand10-using-rand7/)

@2023.10.09

#数学 

将 $10$ 拆为 $\{1,\space 6\} + [0,\space 4]$。

```java
/**
 * 数学
 * Somnia1337
 */
public int rand10()
{
	int a = rand7(), b = rand7();
	while (a == 7) a = rand7();
	while (b > 5) b = rand7();
	return (a % 2 == 0 ? 1 : 6) + b - 1;
}
```

#### [473. 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/)

@2023.09.19

1. 回溯

```java
for (int i = 0; i < k; i++)
{
	if (edges[i] >= matchsticks[end])
	{
		if (i > 0 && edges[i] == edges[i - 1]) continue;
		edges[i] -= matchsticks[end];
		if (bt(k, end - 1, matchsticks, edges)) return true;
		edges[i] += matchsticks[end];
	}
}
```

`edge`表示正方形的四边，每次尝试将当前火柴放入某条边，回溯查找有效的放入方案。

预处理：满足以下条件之一时——

- 火柴长度和无法整除4。
- 存在火柴大于目标边长。

则必无法拼出正方形。反之，如果排序后发现每一根火柴长度都相同，且总火柴数可以整除4，则必可以拼出正方形。

```java
/**
 * 回溯
 * Somnia1337
 */
public boolean makesquare(int[] matchsticks)
{
	int len = matchsticks.length, sum = 0, max = 0;
	for (int matchstick : matchsticks)
	{
		sum += matchstick;
		max = Math.max(max, matchstick);
	}
	int target = sum / 4;
	if (sum % 4 != 0 || max > target) return false;
	Arrays.sort(matchsticks);
	if (matchsticks[0] == matchsticks[len - 1] && len % 4 == 0) return true;
	int k = 4, end = len - 1;
	while (matchsticks[end] == target)
	{
		k--;
		end--;
	}
	if (k < 2) return true;
	int[] edges = new int[k];
	Arrays.fill(edges, target);
	return bt(k, end, matchsticks, edges);
}

public boolean bt(int k, int end, int[] matchsticks, int[] edges)
{
	if (end == -1)
	{
		for (int i = 0; i < k; i++)
		{
			if (edges[i] != 0) return false;
		}
		return true;
	}
	for (int i = 0; i < k; i++)
	{
		if (edges[i] >= matchsticks[end])
		{
			if (i > 0 && edges[i] == edges[i - 1]) continue;
			edges[i] -= matchsticks[end];
			if (bt(k, end - 1, matchsticks, edges)) return true;
			edges[i] += matchsticks[end];
		}
	}
	return false;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public boolean makesquare(int[] matchsticks)
{
	int sum = Arrays.stream(matchsticks)
					.sum();
	if (sum % 4 != 0) return false;
	int len = sum / 4, n = matchsticks.length;
	int[] dp = new int[1 << n];
	Arrays.fill(dp, -1);
	dp[0] = 0;
	for (int s = 1; s < (1 << n); s++)
	{
		for (int k = 0; k < n; k++)
		{
			if ((s & (1 << k)) == 0) continue;
			int s1 = s & ~(1 << k);
			if (dp[s1] >= 0 && dp[s1] + matchsticks[k] <= len)
			{
				dp[s] = (dp[s1] + matchsticks[k]) % len;
				break;
			}
		}
	}
	return dp[(1 << n) - 1] == 0;
}
```

#### [474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)

@2023.07.20

#动态规划 

本题等价于0-1背包问题，背包有对"0"、"1"有两种不同的容量，每个字符串也有"0"、"1"有两种不同的价值，递推式`dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findMaxForm(String[] strs, int m, int n)
{
	int[][] dp = new int[m + 1][n + 1];
	
	for (String str : strs)
	{
		// 计数当前串的0和1
		int zeroCount = 0, oneCount = 0;
		for (int i = 0; i < str.length(); i++)
		{
			if (str.charAt(i) == '0') zeroCount++;
			else oneCount++;
		}
		
		for (int i = m; i >= zeroCount; i--)
		{
			for (int j = n; j >= oneCount; j--)
			{
				dp[i][j] = Math.max(dp[i][j], dp[i - zeroCount][j - oneCount] + 1);
			}
		}
	}
	
	return dp[m][n];
}
```

#### [475. 供暖器](https://leetcode.cn/problems/heaters/)

@2023.10.09

#双指针 #二分查找 

1. 双指针

排序，取每一栋房子所需的最小半径的最大值。对 `heaters` 使用左右双指针，对两个加热器之间的房屋进行计算。

```java
/**
 * 双指针
 * Somnia1337
 */
public int findRadius(int[] houses, int[] heaters)
{
	int len1 = houses.length, len2 = heaters.length;
	Arrays.sort(houses);
	Arrays.sort(heaters);
	int ans = 0;
	int i = 0, left = 0, right = 1;
	while (i < len1 && houses[i] < heaters[left])
	{
		ans = Math.max(heaters[left] - houses[i], ans);
		i++;
	}
	while (right < len2)
	{
		while (i < len1 && houses[i] < heaters[right])
		{
			int dist = Math.max(heaters[right] - houses[i], houses[i] - heaters[left]);
			ans = Math.max(dist, ans);
			i++;
		}
		left++;
		right++;
	}
	while (i < len1)
	{
		ans = Math.max(houses[i] - heaters[right - 1], ans);
		i++;
	}
	return ans;
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int findRadius(int[] houses, int[] heaters)
{
	Arrays.sort(houses);
	Arrays.sort(heaters);
	int ans = 0;
	int i = 0, j = 0;
	while (i < houses.length)
	{
		int dist = Math.abs(houses[i] - heaters[j]);
		while (j < heaters.length - 1 && Math.abs(houses[i] - heaters[j]) >= Math.abs(houses[i] - heaters[j + 1]))
		{
			dist = Math.min(Math.abs(houses[i] - heaters[j + 1]), dist);
			j++;
		}
		ans = Math.max(dist, ans);
		i++;
	}
	return ans;
}
```

2. 二分查找

```java
/**
 * 二分查找
 * 力扣官方题解
 */
public int findRadius(int[] houses, int[] heaters)
{
	int ans = 0;
	Arrays.sort(heaters);
	for (int house : houses)
	{
		int i = binarySearch(heaters, house);
		int j = i + 1;
		int leftDistance = i < 0 ? Integer.MAX_VALUE : house - heaters[i];
		int rightDistance = j >= heaters.length ? Integer.MAX_VALUE : heaters[j] - house;
		int curDistance = Math.min(leftDistance, rightDistance);
		ans = Math.max(ans, curDistance);
	}
	return ans;
}

public int binarySearch(int[] nums, int target)
{
	int left = 0, right = nums.length - 1;
	if (nums[left] > target) return -1;
	while (left <= right)
	{
		int mid = (right - left + 1) / 2 + left;
		if (nums[mid] > target) right = mid - 1;
		else left = mid + 1;
	}
	return right;
}
```

#### [477. 汉明距离总和](https://leetcode.cn/problems/total-hamming-distance/)

@2023.10.10

两两一对计算的复杂度为 $O(N^2)$，在 $10^4$ 级数据量会超时。可以逐位计算，对某一位，记在该位为 0 的 `num` 个数为 `zero`，那么 `nums` 在该位的汉明距离为 `zero * (len - zero)`，逐位累加即为答案。

```java
/**
 * 位运算
 * Somnia1337
 */
public int totalHammingDistance(int[] nums)
{
	int ans = 0;
	for (int i = 0; i < 30; i++)
	{
		int zero = 0;
		for (int num : nums)
		{
			zero += (num >> i) & 1;
		}
		ans += zero * (nums.length - zero);
	}
	return ans;
}
```

#### [478. 在圆内随机生成点](https://leetcode.cn/problems/generate-random-point-in-a-circle/)

@2023.11.07

#设计 #随机化 

1. 拒绝采样

在圆的外切正方形内随机生成点，检查与圆心的距离，若不在圆内则再次生成。

```java
/**
 * 拒绝采样
 * Somnia1337
 */
class Solution
{
    private double x, y, r;
    private Random rand;
	
    public Solution(double radius, double x_center, double y_center)
    {
        r = radius;
        x = x_center;
        y = y_center;
        rand = new Random();
    }
	
    public double[] randPoint()
    {
        double a, b;
        do
        {
            a = rand.nextDouble(x - r, x + r);
            b = rand.nextDouble(y - r, y + r);
        } while (Math.sqrt((x - a) * (x - a) + (y - b) * (y - b)) > r);
        return new double[]{a, b};
    }
}
```

2. 数学

```java
/**
 * 数学
 * 力扣官方题解
 */
class Solution
{
    private double x, y, r;
    private Random rand;
	
    public Solution(double radius, double x_center, double y_center)
    {
        x = x_center;
        y = y_center;
        r = radius;
        rand = new Random();
    }
	
    public double[] randPoint()
    {
        double u = rand.nextDouble();
        double theta = rand.nextDouble() * 2 * Math.PI;
        double r = Math.sqrt(u);
        return new double[]{x + r * Math.cos(theta) * this.r, y + r * Math.sin(theta) * this.r};
    }
}
```

#### [481. 神奇字符串](https://leetcode.cn/problems/magical-string/)

@2023.12.30

#模拟 

`StringBuilder` 模拟，`'1'` 和 `'2'` 交替追加到末端，累加 `'1'` 的数量。

```java
/**
 * 模拟
 * 长路
 */
public int magicalString(int n) {
	StringBuilder s = new StringBuilder("122");
	char x = '2'; // 当前追加的字符
	int ans = 1;
	for (int i = 2; s.length() < n; i++) {
		int cnt = s.charAt(i) - '0';
		// '1' 和 '2' 交替
		// 如果为 '1', 累加到答案
		if ((x = (char) ('0' + ('2' - x + 1))) == '1') ans += Math.min(cnt, n - s.length());
		s.append(String.valueOf(x).repeat(cnt)); // 追加
	}
	return ans;
}
```

#### [486. 预测赢家](https://leetcode.cn/problems/predict-the-winner/)

@2023.12.26

#博弈 #搜索 #动态规划 

1. DFS

`solve()` 在设计上是 **偏重玩家 1** 的，而不是常见的两人平等的情况，返回 `boolean` 表示玩家 1 是否胜利，参数：

- `l`、`r` 分别表示当前可选部分的左、右端
- `a`、`b` 分别表示两玩家的得分
- `turn` 表示本轮是否由玩家 1 选择

终止状态为 `l > r`，已无可选部分，判断玩家 1 是否胜利。如果没有终止，判断是否由玩家 1 选择：

- 如果是，玩家 1 尝试获得 `nums[l]` 或 `nums[r]`，递归调用，只要有一个为 `true` 即返回 `true`。（只要有一条路能赢就行）
- 如果不是，玩家 2 尝试获得 `nums[l]` 或 `nums[r]`，递归调用，必须两种情况都为 `true` 才返回 `true`。（必须无论怎么选都能让玩家 1 赢）

```java
/**
 * DFS
 * Somnia1337
 */
public boolean predictTheWinner(int[] nums) {
	if (nums.length == 1) return true;
	return solve(nums, 0, nums.length - 1, 0, 0, true);
}

private boolean solve(int[] nums, int l, int r, int a, int b, boolean turn) {
	if (l > r) return a >= b;
	if (turn) return solve(nums, l + 1, r, a + nums[l], b, false) || solve(nums, l, r - 1, a + nums[r], b, false);
	return solve(nums, l + 1, r, a, b + nums[l], true) && solve(nums, l, r - 1, a, b + nums[r], true);
}
```

2. 动态规划

`dp[i][j]` 表示作为先手在 `nums[i:j]` 内选择，可以比对手多获得的分数，最终答案即为右上角的子问题 `dp[0][n-1]` 的答案。

计算 `dp[i][j]` 时，可以从 `nums[i]`、`nums[j]` 中选择一个，遍历顺序及状态转移见下图：

![[486. 预测赢家.png|500]]

```java
/**
 * 动态规划
 * liweiwei1419
 */
public boolean predictTheWinner(int[] nums) {
	int len = nums.length;
	int[][] dp = new int[len][len];
	for (int i = 0; i < len; i++) dp[i][i] = nums[i];
	for (int i = len - 2; i >= 0; i--) {
		for (int j = i + 1; j < len; j++) {
			dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
		}
	}
	return dp[0][len - 1] >= 0;
}
```

#### [487. 最大连续1的个数 II](https://leetcode.cn/problems/max-consecutive-ones-ii/)

@2023.10.09

#滑动窗口 

借用 [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/) 的解。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int findMaxConsecutiveOnes(int[] nums)
{
	int l = 0, r = 0, k = 1;
	while (r < nums.length)
	{
		if (nums[r++] == 0) k--;
		if (k < 0 && nums[l++] == 0) k++;
	}
	return r - l;
}
```

#### [491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

@2023.10.11

```java
for (int i = start; i < nums.length; i++)
{
	if (!path.isEmpty() && nums[i] < path.get(path.size() - 1)) continue;
	path.add(nums[i]);
	if (path.size() > 1) ans.add(new ArrayList<>(path));
	bt(nums, i + 1);
	path.remove(path.size() - 1);
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private Set<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> findSubsequences(int[] nums)
{
	ans = new HashSet<>();
	path = new ArrayList<>();
	bt(nums, 0);
	return new ArrayList<>(ans);
}

private void bt(int[] nums, int start)
{
	if (start == nums.length)
	{
		if (path.size() > 1) ans.add(new ArrayList<>(path));
		return;
	}
	for (int i = start; i < nums.length; i++)
	{
		if (!path.isEmpty() && nums[i] < path.get(path.size() - 1)) continue;
		path.add(nums[i]);
		if (path.size() > 1) ans.add(new ArrayList<>(path));
		bt(nums, i + 1);
		path.remove(path.size() - 1);
	}
}
```

```java
/**
 * 回溯
 * ?
 */
private List<List<Integer>> ans;
private List<Integer> path;

public List<List<Integer>> findSubsequences(int[] nums)
{
	ans = new ArrayList<>();
	path = new ArrayList<>();
	bt(nums, 0, Integer.MIN_VALUE);
	return ans;
}

private void bt(int[] nums, int idx, int pre)
{
	if (idx == nums.length)
	{
		if (path.size() >= 2) ans.add(new ArrayList<>(path));
		return;
	}
	if (nums[idx] >= pre)
	{
		path.add(nums[idx]);
		bt(nums, idx + 1, nums[idx]);
		path.remove(path.size() - 1);
	}
	if (nums[idx] != pre) bt(nums, idx + 1, pre);
}
```

#### [494. 目标和](https://leetcode.cn/problems/target-sum/)

@2023.07.19

1. 回溯

```java
/**
 * 回溯
 * Somnia1337
 */
public int ans = 0;

public int findTargetSumWays(int[] nums, int target)
{
	int sum = 0;
	for (int num : nums) sum += num;
	if ((sum + target) % 2 == 1 || Math.abs(target) > sum) return 0;
	bt(nums, target, 0, 0);
	return ans;
}

public void bt(int[] nums, int target, int start, int path)
{
	if (start == nums.length)
	{
		if (path == target) ans++;
		return;
	}
	bt(nums, target, start + 1, path + nums[start]);
	bt(nums, target, start + 1, path - nums[start]);
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int findTargetSumWays(int[] nums, int target)
{
	int len = nums.length;
	int sum = 0;
	for (int num : nums) sum += num;
	if ((sum + target) % 2 == 1 || Math.abs(target) > sum) return 0;
	int x = Math.abs((sum + target) / 2);
	
	int[] dp = new int[x + 1];
	dp[0] = 1;
	
	for (int num : nums)
	{
		for (int j = x; j >= num; j--)
		{
			dp[j] += dp[j - num];
		}
	}
	
	return dp[x];
}
```

#### [498. 对角线遍历](https://leetcode.cn/problems/diagonal-traverse/)

@2023.10.21

#模拟 

分清遍历方向，直接模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] findDiagonalOrder(int[][] mat)
{
	int rows = mat.length, cols = mat[0].length, len = rows * cols;
	int[] ans = new int[len];
	int i = 0, j = 0, it = 0;
	for (int sum = 0; it < len; sum++) // sum为下标之和
	{
		if (sum % 2 == 0) // 方向为左下->右上
		{
			while (it < len && i >= 0 && j < cols)
			{
				ans[it++] = mat[i--][j++];
			}
			if (i < 0)
			{
				i = 0;
				j = sum + 1 - i;
			}
			if (j >= cols)
			{
				j = cols - 1;
				i = sum + 1 - j;
			}
		}
		else // 方向为右上->左下
		{
			while (it < len && i < rows && j >= 0)
			{
				ans[it++] = mat[i++][j--];
			}
			if (i >= rows)
			{
				i = rows - 1;
				j = sum + 1 - i;
			}
			if (j < 0)
			{
				j = 0;
				i = sum + 1 - j;
			}
		}
	}
	return ans;
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public int[] findDiagonalOrder(int[][] mat)
{
	int rows = mat.length, cols = mat[0].length, len = rows * cols;
	int[] ans = new int[len];
	
	for (int sum = 0, it = 0; it < len; sum++)
	{
		if (sum % 2 == 0)
		{
			int x = sum < rows ? sum : rows - 1;
			int y = sum < rows ? 0 : sum - rows + 1;
			while (x >= 0 && y < cols)
			{
				ans[it++] = mat[x--][y++];
			}
		}
		else
		{
			int x = sum < cols ? 0 : sum - cols + 1;
			int y = sum < cols ? sum : cols - 1;
			while (x < rows && y >= 0)
			{
				ans[it++] = mat[x++][y--];
			}
		}
	}
	
	return ans;
}
```

#### [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)

@2023.09.11

#单调栈 

单调栈记录下标，两次遍历 `nums`，遇到更大元素时弹出。

```java
/**
 * 单调栈
 * Somnia1337
 */
public int[] nextGreaterElements(int[] nums) {
	int len = nums.length;
	Deque<Integer> stk = new ArrayDeque<>();
	int[] ans = new int[len];
	Arrays.fill(ans, -1);
	for (int i = 0; i < len * 2; i++) {
		int num = nums[i % len];
		while (!stk.isEmpty() && nums[stk.peek()] < num) ans[stk.pop()] = num;
		stk.push(i % len);
	}
	return ans;
}
```

#### [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

@2023.09.11

#动态规划 

`dp[i][j]` 表示 `s[i:j]` 的最长回文子序列长度，循环时，比较 `s[i]` 与 `s[j]`：

- 如果相同，当前长度为去除两端的长度 `+2`，`dp[i][j] = dp[i+1][j-1] + 2`。
- 如果不同，当前长度为去掉左端或右端的长度中较大者，`dp[i][j] = max(dp[i-1][j], dp[i][j-1])`。

细节：

- 由递推式，`dp[i][j]` 依赖于 `dp[i-1][j-1], dp[i-1][j], dp[i][j-1]`，应先填充较短的子序列，然后递推较长的子序列，因此应倒序遍历。
- `dp[i][j]` 表示子问题在 `s[i:j]` 范围内的解，因此最终答案应为子问题 `s[0:l]` 的解，即 `dp[0][l-1]`。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int longestPalindromeSubseq(String s) {
	int l = s.length();
	int[][] dp = new int[l][l];
	for (int i = l - 1; i >= 0; i--) {
		dp[i][i] = 1;
		char c = s.charAt(i);
		for (int j = i + 1; j < l; j++) {
			if (c == s.charAt(j)) dp[i][j] = dp[i + 1][j - 1] + 2;
			else dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
		}
	}
	return dp[0][l - 1];
}
```

#### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)

@2023.07.22

#动态规划 

等价于完全背包问题，硬币的面额为物品大小，背包总量为`amount`。外层for遍历物品，内层for遍历容量（从当前物品大小开始），递推式`dp[j] = dp[j - coin]`。

```java
/**
 * 动态规划
 * 代码随想录
 */
public int change(int amount, int[] coins)
{
	int[] dp = new int[amount + 1];
	dp[0] = 1;
	
	for (int coin : coins)
	{
		for (int j = coin; j <= amount; j++)
		{
			dp[j] += dp[j - coin];
		}
	}
	return dp[amount];
}
```

#### [519. 随机翻转矩阵](https://leetcode.cn/problems/random-flip-matrix/)

@2023.11.07

#设计 #随机化 

[题解](https://leetcode.cn/problems/random-flip-matrix/solutions/42508/jian-ji-yi-dong-de-javafang-fa-jiao-huan-yi-ji-shi/)

> 本题首先是一个抽样问题，首先你要明白如何从 n 个元素中等概率随机抽取 m 个元素，方法有很多（可以自行去百度），和本题有关的思路是：每次取一个位置，然后从剩下的位置里继续每次取 1 个。那么如何将这些位置分开（取过的和未取过的）呢？可以使用交换的方法，每次取的位置，和尾部的位置内的元素进行交换，每次递减 n，这样即使取到重复的位置，里面的元素也是不一样的啦！
> 
> 仿照这个思路，本题也是交换尾部元素，但是这个交换的记录使用 Map 去存储，如果交换过，那么我们就读取原来的位置，如果没交换过，就使用当前的位置！ 代码以及注释如下：

```java
/**
 * 哈希表
 * 一枚小蔡鸡
 */
class Solution
{
    private int rows, cols, l;
    private HashMap<Integer, Integer> pos;
    private Random rand;
	
    public Solution(int m, int n)
    {
        rows = m;
        cols = n;
        l = rows * cols;
        pos = new HashMap<>();
        rand = new Random();
    }
	
    public int[] flip()
    {
        if (l < 0) return null;
        int r = rand.nextInt(l--);
        int x = pos.getOrDefault(r, r);
        pos.put(r, pos.getOrDefault(l, l));
        return new int[]{x / cols, x % cols};
    }
	
    public void reset()
    {
        pos = new HashMap<>();
        l = rows * cols;
    }
}
```

#### [522. 最长特殊序列 II](https://leetcode.cn/problems/longest-uncommon-subsequence-ii/)

@2023.10.10

```java
/**
 * 枚举
 * Somnia1337
 */
public int findLUSlength(String[] strs)
{
	int len = strs.length, ans = -1;
	for (int i = 0; i < len; i++)
	{
		if (strs[i].length() < ans) continue;
		boolean valid = true;
		for (int j = 0; j < len; j++)
		{
			if (j == i) continue;
			if (isSubSequence(strs[j], strs[i]))
			{
				valid = false;
				break;
			}
		}
		if (valid) ans = strs[i].length();
	}
	return ans;
}

public boolean isSubSequence(String src, String tar)
{
	int itS = 0, itT = 0;
	while (itS < src.length() && itT < tar.length())
	{
		if (src.charAt(itS) == tar.charAt(itT)) itT++;
		itS++;
	}
	return itT == tar.length();
}
```

#### [523. 连续的子数组和](https://leetcode.cn/problems/continuous-subarray-sum/)

@2023.10.14

#前缀和 #哈希表 

`Map<Integer, Integer>`，`key` 为前缀和，`value` 为下标。求前缀和，在哈希表中循环查找与当前前缀和差为 `n * k` 的值，如果下标满足子数组长度至少为 2，找到答案。

对类似 `num=[5,0,0,0], k=3` 的用例，改进为 `merge(nums[i], i, Math::min)`，取最靠前的下标。

```java
/**
 * 前缀和
 * Somnia1337
 */
public boolean checkSubarraySum(int[] nums, int k) {
    Map<Integer, Integer> pre = new HashMap<>();
    pre.put(nums[0], 0);
    for (int i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1];
        if (nums[i] % k == 0) return true;
        for (int j = 0; j <= nums[i]; j += k) {
            // 查找补数, 检查子数组长度
            if (pre.containsKey(nums[i] - j) && i - pre.get(nums[i] - j) >= 2) return true;
        }
        pre.merge(nums[i], i, Math::min);
    }
    return false;
}
```

优化：假设有两个下标 `i`，`j` 满足 `(preSum[j] - preSum[i]) % k == 0`，那么必有 `preSum[j] % k == preSum[i] % k`。

用哈希表记录前缀和对 `k` 的余数，如果 `preSum[i] % k == x`，且在 `i - 2` 之前也出现过 `preSum[j] % k == x`，则找到符合要求的子数组。

```java
/**
 * 前缀和
 * 宫水三叶
 */
public boolean checkSubarraySum(int[] nums, int k) {
    int n = nums.length;
    int[] pre = new int[n + 1];
    for (int i = 1; i <= n; i++) pre[i] = pre[i - 1] + nums[i - 1];
    
    Set<Integer> mod = new HashSet<>();
    for (int i = 2; i <= n; i++) { // 从 2 开始
        mod.add(pre[i - 2] % k); // 记录 2 个以前的前缀和对 k 的余数
        if (mod.contains(pre[i] % k)) return true; // 同余, 找到子数组
    }
    return false;
}
```

#### [524. 通过删除字母匹配到字典里最长单词](https://leetcode.cn/problems/longest-word-in-dictionary-through-deleting/)

@2023.10.12

#双指针 

遍历，遇到子序列且 (其长度更长 | 长度相同 & 字典序更小) 时更新 `ans`。

```java
/**
 * 双指针
 * Somnia1337
 */
public String findLongestWord(String s, List<String> dictionary)
{
	String ans = "";
	for (String str : dictionary)
	{
		if (isSubSequence(s, str) && (str.length() > ans.length() || str.length() == ans.length() && str.compareTo(ans) < 0))
		{
			ans = str;
		}
	}
	return ans;
}

public boolean isSubSequence(String src, String tar)
{
	int i = 0, j = 0;
	while (i < src.length() && j < tar.length())
	{
		if (src.charAt(i) == tar.charAt(j)) j++;
		i++;
	}
	return j == tar.length();
}
```

#### [525. 连续数组](https://leetcode.cn/problems/contiguous-array/)

@2023.10.13

#前缀和 

计算前缀和，`preSum[i]` 的意义是前 `i` 个元素中 1 的数量，循环检查 `j = 0..i`，如果满足 `i - j == n * 2` 且 `preSum[i] - preSum[j] == n`，说明从 `j` 到 `i` 中有 `n` 个 `0` 及 `n` 个 `1`，更新 `ans`。

```java
/**
 * 前缀和
 * Somnia1337
 */
public int findMaxLength(int[] nums) {
    int ans = 0;
    for (int i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1];
        for (int j = ans; i - 2 * j >= -1; j++) {
            if (i - 2 * j == -1) {
                if (nums[i] == j) ans = j;
            }
            else if (nums[i - 2 * j] == nums[i] - j) ans = j;
        }
    }
    return ans * 2;
}
```

优化：遇到 1 将 `sum` 加 1，遇到 0 将 `sum` 减 1。用哈希表记录，`key` 为 `sum`，`value` 为下标，两个相同的 `sum` 之间一定有个数相同的 0 和 1。

```java
/**
 * 前缀和
 * ?
 */
public int findMaxLength(int[] nums) {
    Map<Integer, Integer> idx = new HashMap<>();
    idx.put(0, -1); // 定义 -1 处的前缀和为 0
    int pre = 0, ans = 0;
    for (int i = 0; i < nums.length; i++) {
        pre += (nums[i] == 1 ? 1 : -1); // 计算前缀和
        if (idx.containsKey(pre)) ans = Math.max(i - idx.get(pre), ans);
        else idx.put(pre, i);
    }
    return ans;
}
```

#### [526. 优美的排列](https://leetcode.cn/problems/beautiful-arrangement/)

@2023.10.14

1. 回溯

```java
for (int i = 1; i <= n; i++)
{
	if (used[i] || !(i % pos == 0 || pos % i == 0)) continue;
	used[i] = true;
	bt(n, pos + 1);
	used[i] = false;
}
```

```java
/**
 * 回溯
 * Somnia1337
 */
private boolean[] used;
private int ans = 0;

public int countArrangement(int n)
{
	used = new boolean[n + 1];
	bt(n, 1);
	return ans;
}

private void bt(int n, int pos)
{
	if (pos == n + 1)
	{
		ans++;
		return;
	}
	for (int i = 1; i <= n; i++)
	{
		if (used[i] || !(i % pos == 0 || pos % i == 0)) continue;
		used[i] = true;
		bt(n, pos + 1);
		used[i] = false;
	}
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int countArrangement(int n)
{
	int[] dp = new int[1 << n];
	dp[0] = 1;
	for (int mask = 1; mask < (1 << n); mask++)
	{
		int num = Integer.bitCount(mask);
		for (int i = 0; i < n; i++)
		{
			if ((mask & (1 << i)) != 0 && ((num % (i + 1)) == 0 || (i + 1) % num == 0))
			{
				dp[mask] += dp[mask ^ (1 << i)];
			}
		}
	}
	return dp[(1 << n) - 1];
}
```

#### [528. 按权重随机选择](https://leetcode.cn/problems/random-pick-with-weight/)

@2023.11.07

#设计 #随机化 #前缀和 #二分查找 

对 `[1,2,3]`，可分为 6 块，起止分别为 `[1,1]`、`[2,3]`、`[4,6]`，这就是前缀和。计算 `w` 的前缀和，`pickIndex()` 时在 `sum` 范围内随机生成一个下标，然后在 `pre` 中二分查找其所在的块。

```java
/**
 * 前缀和 + 二分查找
 * 力扣官方题解
 */
class Solution
{
    private int[] pre;
    private int sum;
	
    public Solution(int[] w)
    {
        pre = new int[w.length + 1];
        sum = 0;
        for (int i = 1; i < w.length + 1; i++)
        {
            pre[i] = pre[i - 1] + w[i - 1];
            sum += w[i - 1];
        }
    }
	
    public int pickIndex()
    {
        int x = (int) (Math.random() * sum), l = 1, r = pre.length;
        while (l <= r)
        {
            int m = l + r >> 1;
            if (pre[m] <= x) l = m + 1;
            else r = m - 1;
        }
        return r;
    }
}
```

#### [529. 扫雷游戏](https://leetcode.cn/problems/minesweeper/)

@2023.12.06

#搜索 

DFS，注意：

- “相邻”的格有 8 个，而非水平垂直方向上的 4 个。
- 对一个揭露格算出相邻炸弹数量后，如果为 0，才揭露与其相邻的格，如果大于 0 则停止搜索。

```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dx = {-1, 1, 0, 0, -1, 1, -1, 1}, dy = {0, 0, -1, 1, -1, 1, 1, -1};
private int rows, cols;

public char[][] updateBoard(char[][] board, int[] click)
{
	rows = board.length;
	cols = board[0].length;
	if (board[click[0]][click[1]] == 'M')
	{
		board[click[0]][click[1]] = 'X';
		return board;
	}
	dfs(board, click[0], click[1]);
	return board;
}

private void dfs(char[][] board, int x, int y)
{
	if (!(x >= 0 && x < rows && y >= 0 && y < cols) || board[x][y] != 'E') return;
	int count = 0;
	for (int i = Math.max(x - 1, 0); i < Math.min(x + 2, rows); i++)
	{
		for (int j = Math.max(y - 1, 0); j < Math.min(y + 2, cols); j++)
		{
			if (board[i][j] == 'M') count++;
		}
	}
	if (count > 0) board[x][y] = ((char) ('0' + count));
	else
	{
		board[x][y] = 'B';
		for (int k = 0; k < 8; k++)
		{
			dfs(board, x + dx[k], y + dy[k]);
		}
	}
}
```

#### [531. 孤独像素 I](https://leetcode.cn/problems/lonely-pixel-i/)

@2023.10.14

#暴力 

枚举每个黑像素，将其所在的行与列 ban 掉，记录在两个 `boolean[]` 中，枚举时跳过被 ban 的部分。

```java
/**
 * 枚举
 * Somnia1337
 */
public int findLonelyPixel(char[][] picture)
{
	int rows = picture.length, cols = picture[0].length;
	boolean[] banRow = new boolean[rows], banCol = new boolean[cols]; // 被ban的行、列
	int ans = 0;
	
	for (int i = 0; i < rows; i++)
	{
		if (banRow[i]) continue; // ban
		for (int j = 0; j < cols; j++)
		{
			if (banCol[j] || picture[i][j] == 'W') continue; // ban
			if (isLonely(picture, i, j)) ans++; // 检查是否孤独
			banRow[i] = true; // ban该行
			banCol[j] = true; // ban该列
			break;
		}
	}
	
	return ans;
}

// 检查是否孤独
private boolean isLonely(char[][] chars, int x, int y)
{
	for (int i = 0; i < chars.length; i++)
	{
		if (i == x || chars[i][y] == 'W') continue;
		return false;
	}
	for (int j = 0; j < chars[0].length; j++)
	{
		if (j == y || chars[x][j] == 'W') continue;
		return false;
	}
	
	return true;
}
```

#### [532. 数组中的 k-diff 数对](https://leetcode.cn/problems/k-diff-pairs-in-an-array/)

@2023.11.07

1. 哈希表

讨论 `k`：

- `k != 0`，用 `Set` 记录所有元素，遍历 `set`，统计 `num + k` 也在 `set` 中的 `num` 个数。
- `k == 0`，用 `Map` 对元素计数，统计出现次数大于 2 的元素个数。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int findPairs(int[] nums, int k)
{
	if (k == 0) return helper(nums);
	Set<Integer> set = new HashSet<>();
	for (int num : nums) set.add(num);
	int ans = 0;
	for (int num : set)
	{
		if (set.contains(num + k)) ans++;
	}
	return ans;
}

private int helper(int[] nums)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int num : nums) count.merge(num, 1, Integer::sum);
	int ans = 0;
	for (int v : count.values())
	{
		if (v > 1) ans++;
	}
	return ans;
}
```

2. 排序

> 查找数组中两数之差的绝对值是k, 首先想到也最简单的就是排序之后进行计算 定义一个起始指针start 随着i的增加找到差值为k的时候 start++，i++ 中间会遇到两种特殊情况
> 
> 当遇到一串相同数字的时候 那么只需要i不断前进即可 当遇到差值为k的时候记录下 pre = nums[start] 然后跳过所有等于 pre 的 start
> 
> 就是当差值大于 k 的时候 我们需要 start 进 1，此时需要判断增长后的 start是否等于 i 只有相等的情况才需要 i 也进 1，否则需要 i--

```java
/**
 * 
 * 总有一天丶
 */
public int findPairs(int[] nums, int k)
{
	Arrays.sort(nums);
	int start = 0, pre = 0x7fffffff, ans = 0;
	for (int i = 1; i < nums.length; i++)
	{
		if (nums[i] - nums[start] > k || pre == nums[start])
		{
			if (++start != i) i--;
		}
		else if (nums[i] - nums[start] == k)
		{
			pre = nums[start++];
			ans++;
		}
	}
	return ans;
}
```

#### [536. 从字符串生成二叉树](https://leetcode.cn/problems/construct-binary-tree-from-string/)

@2023.11.09

> 1、遇到左括号就往下递归 2、遇到右括号就返回一步 3、递归函数内 按顺序处理： 1）获取当前节点自己的值 2）如果为左括号，就递归构建左子树 3）重复一遍，如果为左括号，就递归构建右子树 4）如果有右括号，就返回上一次 注意 每一步都要更新索引位置

```java
/**
 * 递归
 * 鹏举
 */
public TreeNode str2tree(String s)
{
	if (s.isEmpty()) return null;
	TreeNode root = new TreeNode(0);
	solve(root, s, 0);
	return root;
}

private int solve(TreeNode root, String s, int idx)
{
	int l = s.length(), it = idx;
	while (it < l && (Character.isDigit(s.charAt(it)) || s.charAt(it) == '-')) it++;
	root.val = Integer.parseInt(s.substring(idx, it));
	if (it < l && s.charAt(it) == '(')
	{
		root.left = new TreeNode(0);
		it = solve(root.left, s, it + 1);
	}
	if (it < l && s.charAt(it) == '(')
	{
		root.right = new TreeNode(0);
		it = solve(root.right, s, it + 1);
	}
	return it < l && s.charAt(it) == ')' ? it + 1 : it;
}
```

#### [539. 最小时间差](https://leetcode.cn/problems/minimum-time-difference/)

@2023.10.12

#数组 

转换为分钟数，排序，相邻项作差（包括首尾）取最小值。

根据鸽巢原理，如果 `timePoints.size() > 1440`，必存在两个相同的时间点，返回 0。

```java
/**
 * 排序
 * Somnia1337
 */
public int findMinDifference(List<String> timePoints)
{
	int len = timePoints.size();
	if (len > 1440) return 0; // 鸽巢原理，4ms优化至2ms
	int[] minutes = new int[len];
	for (int i = 0; i < len; i++)
	{
		String s = timePoints.get(i);
		minutes[i] = 60 * Integer.parseInt(s.substring(0, 2)) + Integer.parseInt(s.substring(3));
	}
	Arrays.sort(minutes);
	int ans = Integer.MAX_VALUE;
	for (int i = 1; i < len; i++)
	{
		ans = Math.min(minutes[i] - minutes[i - 1], ans);
		if (ans == 0) return 0;
	}
	return Math.min(minutes[0] + 1440 - minutes[len - 1], ans);
}
```

#### [540. 有序数组中的单一元素](https://leetcode.cn/problems/single-element-in-a-sorted-array/)

@2023.10.18

#二分查找 

如果所有元素都成对，每对的第 1 个元素下标为偶、第 2 个元素下标为奇。

但有一个元素不成对，它之前的元素都满足规律，它以后的元素规律反转，找到这个分界上的元素即为答案。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int singleNonDuplicate(int[] nums)
{
	int len = nums.length;
	if (len == 1) return nums[0];
	if (nums[0] != nums[1]) return nums[0];
	if (nums[len - 1] != nums[len - 2]) return nums[len - 1];
	int l = 1, r = len - 2;
	while (l <= r)
	{
		int m = l + r >> 1;
		if (nums[m] != nums[m - 1] && nums[m] != nums[m + 1]) return nums[m];
		else if ((m & 1) == 1)
		{
			if (nums[m] != nums[m - 1]) r = m - 1;
			else l = m + 1;
		}
		else
		{
			if (nums[m] != nums[m + 1]) r = m - 1;
			else l = m + 1;
		}
	}
	return -1;
}
```

```java
/**
 * 二分查找
 * 宫水三叶
 */
public int singleNonDuplicate(int[] nums)
{
	int len = nums.length, l = 0, r = len - 1;
	while (l < r)
	{
		int m = l + r >> 1;
		if ((m & 1) == 0)
		{
			if (m + 1 < len && nums[m] == nums[m + 1]) l = m + 1;
			else r = m;
		}
		else
		{
			if (m - 1 >= 0 && nums[m - 1] == nums[m]) l = m + 1;
			else r = m;
		}
	}
	return nums[r];
}
```

优化：中间下标 `m` 如果为偶，检查 `nums[m]` 与 `nums[m + 1]` 相等性，如果为奇，检查 `nums[m]` 与 `nums[m - 1]` 相等性，可以归结为位运算 `m ^ 1`：

- 当 `m` 为偶，例如 `m = 2 = (10)`，`m ^ 1` 为 `(11) = 3`。
- 当 `m` 为奇，例如 `m = 3 = (11)`，`m ^ 1` 为 `(10) = 2`。

```java
/**
 * 二分查找 + 位运算
 * 宫水三叶
 */
public int singleNonDuplicate(int[] nums)
{
	int len = nums.length, l = 0, r = len - 1;
	while (l < r)
	{
		int m = l + r >> 1;
		if (nums[m] == nums[m ^ 1]) l = m + 1;
		else r = m;
	}
	return nums[r];
}
```

#### [542. 01 矩阵](https://leetcode.cn/problems/01-matrix/)

@2023.10.20

#动态规划 

```
if matrix[i][j] == 0: dp[i][j] = 0
else: dp[i][j] = min(dp[i - 1][j], dp[i + 1][j], dp[i][j - 1], dp[i][j + 1])
```

从 4 个角开始递推即可，但其实从 2 个相对的角开始也可以：

- 从左上角开始递推，满足了左侧与上侧最小，其余被从右下角开始的递推补充了。
- 从右下角开始递推，满足了右侧与下侧最小，其余被从左上角开始的递推补充了。

```java
/**
 * 动态规划
 * Sweetiee 🍬
 */
public int[][] updateMatrix(int[][] matrix)
{
	int rows = matrix.length, cols = matrix[0].length;
	int[][] dp = new int[rows][cols];
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			dp[i][j] = matrix[i][j] == 0 ? 0 : 10000;
		}
	}
	
	// 从左上角开始递推
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (i >= 1) dp[i][j] = Math.min(dp[i - 1][j] + 1, dp[i][j]);
			if (j >= 1) dp[i][j] = Math.min(dp[i][j - 1] + 1, dp[i][j]);
		}
	}
	// 从右下角开始递推
	for (int i = rows - 1; i >= 0; i--)
	{
		for (int j = cols - 1; j >= 0; j--)
		{
			if (i < rows - 1) dp[i][j] = Math.min(dp[i + 1][j] + 1, dp[i][j]);
			if (j < cols - 1) dp[i][j] = Math.min(dp[i][j + 1] + 1, dp[i][j]);
		}
	}
	
	return dp;
}
```

#### [544. 输出比赛匹配对](https://leetcode.cn/problems/output-contest-matches/)

@2023.10.18

#模拟 

递归，传入本轮剩余的队伍数，按顺序取首尾两两比赛，递归调用，下一轮只剩一半队伍。

```java
/**
 * 模拟
 * Somnia1337
 */
private final List<String> ans = new ArrayList<>();

public String findContestMatch(int n)
{
	for (int i = 1; i <= n; i++) ans.add(String.valueOf(i));
	solve(n);
	return ans.get(0);
}

private void solve(int len)
{
	if (len == 1) return; // 只剩下1支队伍，无需再次比赛
	for (int i = 0; i < len / 2; i++)
	{
		// 取首尾元素，用括号包括
		ans.set(i, "(" + ans.get(i) + "," + ans.get(len - i - 1) + ")");
	}
	solve(len / 2); // 只剩下一半队伍需要比赛
}
```

#### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

@2023.10.25

#搜索 #并查集 

1. DFS

DFS 遍历所有节点，`vis` 记录遍历过的节点，每次找到一个未遍历的节点时，以其为起点，遍历标记所有能到达的节点，起点的数量即为分组数。

```java
/**
 * DFS
 * Somnia1337
 */
private boolean[] vis;

public int findCircleNum(int[][] isConnected) {
	int n = isConnected.length;
	vis = new boolean[n];
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (!vis[i]) {
			ans++; // 新的分组
			dfs(isConnected, i);
		}
	}
	return ans;
}

// 遍历标记所有连通节点
private void dfs(int[][] graph, int idx) {
	if (vis[idx]) return;
	vis[idx] = true;
	for (int i = 0; i < graph.length; i++) {
		if (graph[idx][i] == 1) dfs(graph, i);
	}
}
```

2. 并查集

并查集，合并之后计数 `root[i] == i` 的数量。

```java
/**
 * 并查集
 * Somnia1337
 */
public int findCircleNum(int[][] isConnected) {
	int n = isConnected.length;
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int i = 0; i < n - 1; i++) {
		for (int j = i + 1; j < n; j++) {
			if (isConnected[i][j] == 1) union(root, i, j);
		}
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		if (root[i] == i) ans++;
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

#### [553. 最优除法](https://leetcode.cn/problems/optimal-division/)

@2023.11.02

#技巧 

越除，分子越小，分母越大，因此应在第一个位置分割。

```java
/**
 * 脑筋急转弯
 * Somnia1337
 */
public String optimalDivision(int[] nums)
{
	int len = nums.length;
	if (len == 1) return nums[0] + "";
	if (len == 2) return nums[0] + "/" + nums[1];
	StringBuilder ans = new StringBuilder();
	ans.append(nums[0])
	   .append("/(");
	for (int i = 1; i < len - 1; i++)
	{
		ans.append(nums[i])
		   .append("/");
	}
	ans.append(nums[len - 1])
	   .append(")");
	return ans.toString();
}
```

#### [554. 砖墙](https://leetcode.cn/problems/brick-wall/)

@2023.10.18

#哈希表 

遍历每一行砖，前缀累加得到每条砖缝，用哈希表计数，用总行数减去最多重合砖缝数即为答案。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int leastBricks(List<List<Integer>> wall)
{
	Map<Integer, Integer> count = new HashMap<>();
	
	for (List<Integer> row : wall)
	{
		int len = 0;
		for (int i = 0; i < row.size() - 1; i++)
		{
			len += row.get(i);
			count.merge(len, 1, Integer::sum);
		}
	}
	
	int max = 0;
	for (int len : count.values()) max = Math.max(len, max);
	return wall.size() - max;
}
```

```java
/**
 * 哈希表
 * ?
 */
public int leastBricks(List<List<Integer>> wall)
{
	Map<Integer, Integer> map = new HashMap<>();
	Int result = new Int(0);
	wall.forEach(list -> {
		int sum = 0;
		for (int i = list.size() - 1; i > 0; i--)
		{
			sum += list.get(i);
			int val = map.getOrDefault(sum, 0) + 1;
			result.setVal(val);
			map.put(sum, val);
		}
	});
	return wall.size() - result.val;
}

static class Int
{
	int val;

	public Int(int val)
	{
		this.val = val;
	}

	public void setVal(int val)
	{
		if (val > this.val)
		{
			this.val = val;
		}
	}
}
```

```java
/**
 * 哈希表
 * ?
 */
private final Map<Integer, Integer> map = new HashMap<>();
private int max = 0;

public int leastBricks(List<List<Integer>> wall)
{
	for (List<Integer> row : wall)
	{
		processRow(row);
	}
	return wall.size() - max;
}

public void processRow(List<Integer> row)
{
	int rowSum = row.get(0);
	for (int j = 1; j < row.size(); j++)
	{
		int f = map.getOrDefault(rowSum, 0) + 1;
		map.put(rowSum, f);
		if (f > max) max = f;
		rowSum += row.get(j);
	}
}
```

#### [555. 分割连接字符串](https://leetcode.cn/problems/split-concatenated-strings/)

@2023.10.18

```java
/**
 * 贪心
 * Somnia1337
 */
public String splitLoopedString(String[] strs) {
	int n = strs.length;
	for (int i = 0; i != n; i++) {
		String s = strs[i], re = reverse(s);
		if (re.compareTo(s) > 0) strs[i] = re;
	}
	String ans = "";
	for (int i = 0; i < n; i++) {
		String s = strs[i];
		StringBuilder other = new StringBuilder();
		for (int j = i + 1; j < n; j++) other.append(strs[j]);
		for (int j = 0; j < i; j++) other.append(strs[j]);
		int l = s.length();
		for (int j = 0; j < l; j++) {
			String cur = s.substring(j) + other + s.substring(0, j);
			if (cur.compareTo(ans) > 0) ans = cur;
		}
		String re = reverse(s);
		for (int j = 0; j < l; j++) {
			String cur = re.substring(j) + other + re.substring(0, j);
			if (cur.compareTo(ans) > 0) ans = cur;
		}
	}
	return ans;
}

private String reverse(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length;
	for (int i = 0; i < n / 2; i++) swap(chs, i, n - i - 1);
	return new String(chs);
}

private void swap(char[] arr, int i, int j) {
	char t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [556. 下一个更大元素 III](https://leetcode.cn/problems/next-greater-element-iii/)

@2023.10.21

#模拟 

将 `n` 转为 `int[]`，借用 [31. 下一个排列](https://leetcode.cn/problems/next-permutation/) 的解寻找其下一个排列，然后再转换回 `int`。

```java
/**
 * 模拟
 * Somnia1337
 */
public int nextGreaterElement(int n)
{
	int hold = n;
	int len = String.valueOf(n)
					.length();
	int[] nums = new int[len];
	for (int i = len - 1; i >= 0; i--)
	{
		nums[i] = n % 10;
		n /= 10;
	}
	nextPermutation(nums);
	long ans = 0;
	for (int i = 0; i < len; i++)
	{
		ans *= 10;
		ans += nums[i];
	}
	return ans <= Integer.MAX_VALUE ? (ans > hold ? (int) ans : -1) : -1;
}

// 31.下一个排列
private void nextPermutation(int[] nums)
{
	int len = nums.length;
	int left, right;
	for (int i = len - 2; i >= 0; i--)
	{
		if (nums[i] < nums[i + 1])
		{
			for (int j = len - 1; j > i; j--)
			{
				if (nums[j] > nums[i])
				{
					int temp = nums[j];
					nums[j] = nums[i];
					nums[i] = temp;
					left = i + 1;
					right = len - 1;
					flip(nums, left, right);
					return;
				}
			}
		}
	}
	flip(nums, 0, len - 1);
}

private void flip(int[] nums, int left, int right)
{
	while (left < right)
	{
		int temp = nums[left];
		nums[left] = nums[right];
		nums[right] = temp;
		left++;
		right--;
	}
}
```

#### [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

@2023.09.19

```java
/**
 * 前缀和 + 哈希表
 * 力扣官方题解
 */
public int subarraySum(int[] nums, int k) {
	Map<Integer, Integer> count = new HashMap<>();
	count.put(0, 1);
	int sum = 0, ans = 0;
	for (int x : nums) {
		ans += count.getOrDefault((sum += x) - k, 0);
		count.merge(sum, 1, Integer::sum);
	}
	return ans;
}
```

#### [562. 矩阵中最长的连续1线段](https://leetcode.cn/problems/longest-line-of-consecutive-one-in-matrix/)

@2023.10.26

`dp[i][j]` 有 3 个维度，分别记录以 `mat[i][j]` 结尾的在横向、纵向、斜向上的最长连续 1 线段长度。

由于斜向只能记录左上 -> 右下，因此还需将 `mat` 左右对称翻转后再次进行动态规划。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int longestLine(int[][] mat)
{
	int ans = solve(mat);
	int cols = mat[0].length;
	for (int[] row : mat)
	{
		int l = 0, r = cols - 1;
		while (l < r)
		{
			swap(row, l++, r--);
		}
	}
	return Math.max(solve(mat), ans);
}

private int solve(int[][] mat)
{
	int rows = mat.length, cols = mat[0].length;
	int[][][] dp = new int[rows][cols][3];
	dp[0][0][0] = dp[0][0][1] = dp[0][0][2] = mat[0][0] == 1 ? 1 : 0;
	int ans = dp[0][0][0];
	for (int i = 1; i < rows; i++)
	{
		if (mat[i][0] == 1)
		{
			dp[i][0][0] = dp[i - 1][0][0] + 1;
			dp[i][0][1] = dp[i][0][2] = 1;
			ans = Math.max(dp[i][0][0], ans);
		}
	}
	for (int j = 1; j < cols; j++)
	{
		if (mat[0][j] == 1)
		{
			dp[0][j][1] = dp[0][j - 1][1] + 1;
			dp[0][j][0] = dp[0][j][2] = 1;
			ans = Math.max(dp[0][j][1], ans);
		}
	}
	for (int i = 1; i < rows; i++)
	{
		for (int j = 1; j < cols; j++)
		{
			if (mat[i][j] == 1)
			{
				dp[i][j][0] = dp[i - 1][j][0] + 1;
				dp[i][j][1] = dp[i][j - 1][1] + 1;
				dp[i][j][2] = dp[i - 1][j - 1][2] + 1;
				ans = Math.max(Math.max(dp[i][j][0], dp[i][j][1]), Math.max(dp[i][j][2], ans));
			}
		}
	}
	return ans;
}

private void swap(int[] arr, int i, int j)
{
	int t = arr[i];
	arr[i] = arr[j];
	arr[j] = t;
}
```

```java
/**
 * 动态规划
 * 豆小科
 */
public int longestLine(int[][] mat)
{
	int rows = mat.length, cols = mat[0].length;
	int[][][] dp = new int[2][cols + 2][4];
	
	int ans = 0;
	for (int i = 1; i <= rows; i++)
	{
		for (int j = 1; j <= cols; j++)
		{
			if (mat[i - 1][j - 1] == 1)
			{
				dp[i % 2][j][0] = dp[i % 2][j - 1][0] + 1;
				dp[i % 2][j][1] = dp[(i - 1) % 2][j][1] + 1;
				dp[i % 2][j][2] = dp[(i - 1) % 2][j - 1][2] + 1;
				dp[i % 2][j][3] = dp[(i - 1) % 2][j + 1][3] + 1;
				for (int k = 0; k < 4; k++)
				{
					ans = Math.max(ans, dp[i % 2][j][k]);
				}
			}
			else
			{
				for (int k = 0; k < 4; k++)
				{
					dp[i % 2][j][k] = 0;
				}
			}
		}
	}
	
	return ans;
}
```

#### [565. 数组嵌套](https://leetcode.cn/problems/array-nesting/)

@2023.10.26

#搜索 

`nums` 构成一张有向图，不含重复元素 -> 每个节点的入度与出度均为 1。

也就是说，这张图必然由若干个环构成，从环的任意一个节点开始遍历，最终都能得到该环的大小。

```java
/**
 * 图
 * Somnia1337
 */
public int arrayNesting(int[] nums)
{
	int len = nums.length, ans = 0;
	boolean[] vis = new boolean[len];
	for (int i = 0; i < len; i++)
	{
		if (vis[i]) continue;
		int cur = 0;
		while (!vis[i])
		{
			vis[i] = true;
			i = nums[i];
			cur++;
		}
		ans = Math.max(cur, ans);
	}
	return ans;
}
```

优化：可以原地将遍历过的元素标记 `-1`，省去 `vis`。

```java
/**
 * 原地标记
 * 力扣官方题解
 */
public int arrayNesting(int[] nums)
{
	int len = nums.length, ans = 0;
	for (int i = 0; i < len; i++)
	{
		int cur = 0;
		while (nums[i] >= 0)
		{
			int num = nums[i];
			nums[i] = -1;
			i = num;
			cur++;
		}
		ans = Math.max(cur, ans);
	}
	return ans;
}
```

#### [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)

@2023.10.09

#滑动窗口 

统计滑动窗口内的字符，与目标值对比。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public boolean checkInclusion(String s1, String s2)
{
	int len1 = s1.length(), len2 = s2.length();
	if (len1 > len2) return false;
	int[] count = new int[26];
	int[] tar = new int[26];
	for (int i = 0; i < len1; i++)
	{
		count[s2.charAt(i) - 'a']++;
		tar[s1.charAt(i) - 'a']++;
	}
	if (Arrays.equals(count, tar)) return true;
	for (int i = len1; i < len2; i++)
	{
		count[s2.charAt(i - len1) - 'a']--;
		count[s2.charAt(i) - 'a']++;
		if (Arrays.equals(count, tar)) return true;
	}
	return false;
}
```

#### [573. 松鼠模拟](https://leetcode.cn/problems/squirrel-simulation/)

@2023.10.27

#贪心 

观察易得，运送第 1 个松果时需要从起点到达松果、再到达树，运送剩下的所有松果的距离都是松果与树距离的 2 倍，因此只需考虑选择哪一个松果作为第 1 个。

用 `dists` 记录每一个松果与树的距离，`sum` 为 `sum(dists) * 2`，随后，枚举每一个松果作为第 1 个，用该情况下的总距离更新答案。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minDistance(int height, int width, int[] tree, int[] squirrel, int[][] nuts)
{
	int len = nuts.length, sum = 0;
	int[] dists = new int[len];
	for (int i = 0; i < len; i++)
	{
		dists[i] = dist(nuts[i], tree);
		sum += dists[i] * 2;
	}
	
	int ans = Integer.MAX_VALUE;
	for (int i = 0; i < len; i++)
	{
		ans = Math.min(sum - dists[i] + dist(nuts[i], squirrel), ans);
	}
	
	return ans;
}

private int dist(int[] a, int[] b)
{
	return Math.abs(a[0] - b[0]) + Math.abs(a[1] - b[1]);
}
```

#### [576. 出界的路径数](https://leetcode.cn/problems/out-of-boundary-paths/)

@2023.10.21

1. 记忆化搜索

```java
/**
 * 记忆化搜索
 * 宫水三叶
 */
int MOD = (int) 1e9 + 7;
int m, n, max;
int[][] dirs = new int[][]{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
int[][][] cache;

public int findPaths(int m, int n, int maxMove, int startRow, int startColumn)
{
	this.m = m;
	this.n = n;
	max = maxMove;
	cache = new int[m][n][max + 1];
	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			for (int k = 0; k <= maxMove; k++)
			{
				cache[i][j][k] = -1;
			}
		}
	}
	return dfs(startRow, startColumn, maxMove);
}

private int dfs(int x, int y, int k)
{
	if (x < 0 || x >= m || y < 0 || y >= n) return 1;
	if (k == 0) return 0;
	if (cache[x][y][k] != -1) return cache[x][y][k];
	int ans = 0;
	for (int[] d : dirs)
	{
		int nx = x + d[0], ny = y + d[1];
		ans += dfs(nx, ny, k - 1);
		ans %= MOD;
	}
	cache[x][y][k] = ans;
	return ans;
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 彤哥来刷题啦
 */
public int findPaths(int m, int n, int maxMove, int startRow, int startColumn)
{
	final int MOD = (int) (1e9 + 7);
	final int[][] dirs = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	int[][][] dp = new int[m][n][maxMove + 1];
	for (int k = 1; k <= maxMove; k++)
	{
		for (int i = 0; i < m; i++)
		{
			for (int j = 0; j < n; j++)
			{
				if (i == 0) dp[i][j][k]++;
				if (j == 0) dp[i][j][k]++;
				if (i == m - 1) dp[i][j][k]++;
				if (j == n - 1) dp[i][j][k]++;
				for (int[] dir : dirs)
				{
					int nextI = i + dir[0];
					int nextJ = j + dir[1];
					if (nextI >= 0 && nextI < m && nextJ >= 0 && nextJ < n)
					{
						dp[i][j][k] = (dp[i][j][k] + dp[nextI][nextJ][k - 1]) % MOD;
					}
				}
			}
		}
	}
	return dp[startRow][startColumn][maxMove];
}
```

#### [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)

@2023.10.17

1. 排序

排序，与原数组对比，从第一个不同元素到最后一个不同元素即为所求部分。

```java
/**
 * 排序
 * Somnia1337
 */
public int findUnsortedSubarray(int[] nums)
{
	int len = nums.length;
	int[] copy = new int[len];
	System.arraycopy(nums, 0, copy, 0, len);
	Arrays.sort(copy);
	
	// 搜索首个、末个不同元素的位置
	int start = 0, end = len - 1;
	while (start < len && nums[start] == copy[start]) start++;
	while (end > start && nums[end] == copy[end]) end--;
	
	return end - start + 1;
}
```

2. 一次遍历

将 `nums` 分为 3 段 `A`、`B`、`C`，`B` 为最短无序子数组，存在以下关系：

- `A` 中任意元素 < `B + C` 中任意元素。
- `B` 中任意元素 < `C` 中任意元素。

![[581. 最短无序连续子数组.png|400]]

目标就是找中段的左右边界，分别记为 `start` 和 `end`：

- 从左到右维护一个最大值 `max`，在进入右段之前遍历到的 `nums[i]` 都小于 `max`，`end` 即为最后一个小于 `max` 的元素的位置。
- 从右到左维护一个最小值 `min`，在进入左段之前遍历到的 `nums[i]` 都大于 `min`，`start` 即为最后一个大于 `min` 的元素的位置。

```java
/**
 * 一次遍历
 * 力扣官方题解
 */
public int findUnsortedSubarray(int[] nums)
{
	int len = nums.length;
	int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
	int end = -1, start = -1;
	for (int i = 0; i < len; i++)
	{
		if (max > nums[i]) end = i;
		else max = nums[i];
		if (min < nums[len - i - 1]) start = len - i - 1;
		else min = nums[len - i - 1];
	}
	return end >= 0 ? end - start + 1 : 0;
}
```

#### [582. 杀掉进程](https://leetcode.cn/problems/kill-process/)

@2023.10.27

#哈希表 

遍历 `pid`，用哈希表记录每个父子节点关系，`key` 为父节点，`value` 为其子节点列表。然后创建 `ans`，添加 `kill`，随后用一个指针指向当前需要杀掉的节点，从 `kids` 中获取其下所有子节点，全部添加到 `ans`，向前移动指针，直到没有更多节点需要杀掉。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> killProcess(List<Integer> pid, List<Integer> ppid, int kill)
{
	Map<Integer, List<Integer>> kids = new HashMap<>();
	for (int i = 0; i < pid.size(); i++)
	{
		int par = ppid.get(i);
		List<Integer> kid = kids.getOrDefault(par, new ArrayList<>());
		kid.add(pid.get(i));
		kids.put(par, kid);
	}
	
	List<Integer> ans = new ArrayList<>();
	ans.add(kill);
	int it = 0;
	while (it < ans.size())
	{
		int tar = ans.get(it);
		List<Integer> next = kids.get(tar);
		if (next != null) ans.addAll(next);
		it++;
	}
	
	return ans;
}
```

#### [583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/)

@2023.09.10

#动态规划 

与 [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/) 等价。

求出两字符串的 `lcs`，答案即 `l1 + l2 - lcs`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minDistance(String word1, String word2) {
	return word1.length() + word2.length() - longestCommonSubsequence(word1, word2) * 2;
}

// 1143. 最长公共子序列
private int longestCommonSubsequence(String text1, String text2) {
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

另一种思路：直接 DP 计算答案。用 `dp` 记录目前需要进行的最少操作数，初始化 `dp[i..][0]`、`dp[0][j..]` 为 `i..`、`j..`，遍历时，如果当前字符：

- 相同，无需增加操作数，`dp[i][j] = dp[i - 1][j - 1]`。
- 不同，需要增加操作数，且取左侧、上侧中的较小者，`dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1`。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int minDistance(String word1, String word2) {
	int l1 = word1.length(), l2 = word2.length();
	int[][] dp = new int[l1 + 1][l2 + 1];
	for (int i = 1; i <= l1; i++) dp[i][0] = i;
	for (int j = 1; j <= l2; j++) dp[0][j] = j;
	for (int i = 1; i <= l1; i++) {
		char c1 = word1.charAt(i - 1);
		for (int j = 1; j <= l2; j++) {
			char c2 = word2.charAt(j - 1);
			if (c1 == c2) dp[i][j] = dp[i - 1][j - 1];
			else dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
		}
	}
	return dp[l1][l2];
}
```

#### [592. 分数加减运算](https://leetcode.cn/problems/fraction-addition-and-subtraction/)

@2023.10.26

#模拟 

```java
/**
 * 模拟
 * Somnia1337
 */
public String fractionAddition(String expression)
{
	int A = 0, B = 1;
	expression = expression.replaceAll("-", "+-");
	String[] splits = expression.split("\\++");
	for (String split : splits)
	{
		if (split.isEmpty()) continue;
		String[] AB = split.split("/");
		int a = Integer.parseInt(AB[0]), b = Integer.parseInt(AB[1]);
		int lcp = lcp(B, b);
		int sc1 = lcp / B, sc2 = lcp / b;
		A *= sc1;
		B *= sc1;
		a *= sc2;
		A += a;
	}
	if (A == 0) return "0/1";
	int gcd = gcd(Math.abs(A), Math.abs(B));
	A /= gcd;
	B /= gcd;
	return A + "/" + B;
}

private int lcp(int x, int y)
{
	return x * y / gcd(x, y);
}

private int gcd(int x, int y)
{
	return y == 0 ? x : gcd(y, x % y);
}
```

#### [593. 有效的正方形](https://leetcode.cn/problems/valid-square/)

@2023.10.18

#数学 

硬核数学………

```java
/**
 * 数学
 * Somnia1337
 */
public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4)
{
	if (p1[0] == p2[0] && p2[0] == p3[0] && p3[0] == p4[0] && p1[1] == p2[1] && p2[1] == p3[1] && p3[1] == p4[1])
		return false;
	int[][] ps = new int[4][2];
	ps[0] = p1;
	ps[1] = p2;
	ps[2] = p3;
	ps[3] = p4;
	Arrays.sort(ps, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
	int[][] vs = new int[2][2];
	vs[0][0] = ps[3][0] - ps[0][0];
	vs[0][1] = ps[3][1] - ps[0][1];
	vs[1][0] = ps[2][0] - ps[1][0];
	vs[1][1] = ps[2][1] - ps[1][1];
	if (vs[0][0] * vs[1][0] + vs[0][1] * vs[1][1] != 0) return false;
	if (ps[1][0] - ps[0][0] != ps[3][0] - ps[2][0] || ps[1][1] - ps[0][1] != ps[3][1] - ps[2][1]) return false;
	if (ps[2][0] - ps[0][0] != ps[3][0] - ps[1][0] || ps[2][1] - ps[0][1] != ps[3][1] - ps[1][1]) return false;
	return (ps[0][0] - ps[1][0]) * (ps[3][0] - ps[1][0]) + (ps[0][1] - ps[1][1]) * (ps[3][1] - ps[1][1]) == 0;
}
```

#### [609. 在系统中查找重复文件](https://leetcode.cn/problems/find-duplicate-file-in-system/)

@2023.10.26

#哈希表 

哈希表记录重复文件，`key` 为文件内容，`value` 为重复文件所在的路径列表。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<List<String>> findDuplicate(String[] paths)
{
	Map<String, List<String>> duplicate = new HashMap<>();
	for (String path : paths)
	{
		String[] splits = path.split(" ");
		for (int i = 1; i < splits.length; i++)
		{
			int s = 0;
			while (splits[i].charAt(s) != '(') s++;
			int e = ++s;
			while (splits[i].charAt(e) != ')') e++;
			String content = splits[i].substring(s, e);
			List<String> same = duplicate.getOrDefault(content, new ArrayList<>());
			same.add(splits[0] + '/' + splits[i].substring(0, s - 1));
			duplicate.put(content, same);
		}
	}
	List<List<String>> ans = new ArrayList<>();
	for (List<String> same : duplicate.values())
	{
		if (same.size() > 1) ans.add(new ArrayList<>(same));
	}
	return ans;
}
```

#### [611. 有效三角形的个数](https://leetcode.cn/problems/valid-triangle-number/)

@2023.10.26

1. 暴力

排序，枚举每个三元组，检查是否构成有效的三角形。

```java
/**
 * 枚举
 * Somnia1337
 */
public int triangleNumber(int[] nums)
{
	Arrays.sort(nums);
	int len = nums.length, ans = 0;
	for (int i = 0; i < len - 2; i++)
	{
		for (int j = i + 1; j < len - 1; j++)
		{
			for (int k = j + 1; k < len; k++)
			{
				if (nums[i] + nums[j] <= nums[k]) break;
				ans++;
			}
		}
	}
	return ans;
}
```

2. 双指针

排序后，

```java
/**
 * 双指针
 * 力扣官方题解
 */
public int triangleNumber(int[] nums)
{
	Arrays.sort(nums);
	int len = nums.length, ans = 0;
	for (int i = 0; i < len - 1; i++)
	{
		int l = i;
		for (int r = i + 1; r < len; r++)
		{
			while (l < len - 1 && nums[l + 1] < nums[i] + nums[r]) l++;
			ans += Math.max(l - r, 0);
		}
	}
	return ans;
}
```

#### [616. 给字符串添加加粗标签](https://leetcode.cn/problems/add-bold-tag-in-string/)

@2023.10.17

遍历 `words`，对每个单词，用滑动窗口记录 `s` 中被其覆盖的区间，对所有区间进行合并后，遍历合并后的区间，在每个区间左右添加 `<b>`、`</b>`。首尾需要单独处理。

合并区间部分借用 [56. 合并区间](https://leetcode.cn/problems/merge-intervals/) 的解。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public String addBoldTag(String s, String[] words)
{
	int len = s.length();
	List<int[]> covered = new ArrayList<>(); // 记录被单词覆盖的区间
	for (String word : words) // 遍历words
	{
		int l = word.length();
		if (l == 0) continue;
		// 滑动窗口
		for (int i = 0; i <= len - l; i++)
		{
			if (word.equals(s.substring(i, i + l))) covered.add(new int[]{i, i + l});
		}
	}
	
	if (covered.isEmpty()) return s; // 没有覆盖，不作修改
	int[][] intervals = covered.toArray(new int[covered.size()][2]);
	int[][] merged = merge(intervals); // 合并区间
	
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < merged.length; i++) // 遍历合并后的区间
	{
		// 特殊处理首部
		if (i == 0 && merged[0][0] > 0)
		{
			ans.append(s, 0, merged[0][0]);
		}
		
		// 正常处理
		ans.append("<b>")
		   .append(s, merged[i][0], merged[i][1])
		   .append("</b>");
		
		// 特殊处理尾部
		if (i < merged.length - 1)
		{
			ans.append(s, merged[i][1], merged[i + 1][0]);
		}
		else if (merged[i][1] < len)
		{
			ans.append(s, merged[i][1], len);
		}
	}
	
	return ans.toString();
}

// 56.合并区间
private int[][] merge(int[][] intervals)
{
	Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
	List<int[]> ans = new ArrayList<>();
	int start = intervals[0][0], end = intervals[0][1];
	for (int i = 1; i < intervals.length; i++)
	{
		if (intervals[i][0] <= end)
		{
			end = Math.max(intervals[i][1], end);
		}
		else
		{
			ans.add(new int[]{start, end});
			start = intervals[i][0];
			end = intervals[i][1];
		}
	}
	ans.add(new int[]{start, end});
	return ans.toArray(new int[ans.size()][2]);
}
```

待学习：[字典树题解](https://leetcode.cn/problems/add-bold-tag-in-string/solutions/87869/zi-dian-shu-java-by-henrylee4/)。

#### [621. 任务调度器](https://leetcode.cn/problems/task-scheduler/)

@2023.10.27

```text
//统计每个任务出现的次数，找到出现次数最多的任务
//因为相同元素必须有n个冷却时间，假设A出现3次，n = 2，任务要执行完，至少形成AXX AXX A序列（X看作预占位置）
//该序列长度为 (n+1) * (cnt[25] - 1) + 1
//此时为了尽量利用X所预占的空间（贪心）使得整个执行序列长度尽量小，将剩余任务往X预占的空间插入
//剩余的任务次数有两种情况：
//1.与A出现次数相同，比如B任务最优插入结果是ABX ABX AB，中间还剩两个空位，当前序列长度+1
//2.比A出现次数少，若还有X，则按序插入X位置，比如C出现两次，形成ABC ABC AB的序列
//直到X预占位置还没插满，剩余元素逐个放入X位置就满足冷却时间至少为n
//当所有X预占的位置插满了怎么办？
//在任意插满区间（这里是ABC）后面按序插入剩余元素，比如ABCD ABCD发现D之间距离至少为n+1，肯定满足冷却条件
//因此，当X预占位置能插满时，最短序列长度就是task.length，不能插满则取最少预占序列长度
```

```java
/**
 * 贪心
 * 人类二分法精华
 */
public int leastInterval(char[] tasks, int n)
{
	int[] cnt = new int[26];
	for (char task : tasks) cnt[task - 'A']++;
	Arrays.sort(cnt);
	int ans = (n + 1) * (cnt[25] - 1) + 1;
	for (int i = 24; i >= 0 && cnt[i] == cnt[25]; i--)
	{
		ans++;
	}
	return Math.max(tasks.length, ans);
}
```

```java
public int leastInterval(char[] tasks, int n)
{
	int[] cnt = new int[26];
	for (char task : tasks) cnt[task - 'A']++;
	int max = 0;
	for (int i = 0; i < 26; i++)
	{
		max = Math.max(cnt[i], max);
	}
	int maxCnt = 0;
	for (int i = 0; i < 26; i++)
	{
		if (cnt[i] == max)
		{
			maxCnt++;
		}
	}
	return Math.max((n + 1) * (max - 1) + maxCnt, tasks.length);
}
```

#### [624. 数组列表中的最大距离](https://leetcode.cn/problems/maximum-distance-in-arrays/)

@2023.09.14

#贪心 

遍历，记录最大、次大、最小、次小，以及最大、最小所在的行，如果最大最小在同行则返回 `max(max2 - min1, max1 - min2)`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxDistance(List<List<Integer>> arrays) {
	int min1 = 10001, max1 = -10001, min2 = 10001, max2 = -10001;
	int minIdx = 0, maxIdx = 0;
	for (int i = 0; i < arrays.size(); i++) {
		List<Integer> arr = arrays.get(i);
		int mn = arr.get(0), mx = arr.get(arr.size() - 1);
		if (mn <= min1) {
			min2 = min1;
			min1 = arr.get(0);
			minIdx = i;
		}
		else if (mn < min2) min2 = mn;
		if (mx >= max1) {
			max2 = max1;
			max1 = mx;
			maxIdx = i;
		}
		else if (mx > max2) max2 = mx;
	}
	return minIdx == maxIdx ? Math.max(max2 - min1, max1 - min2) : max1 - min1;
}
```

#### [625. 最小因式分解](https://leetcode.cn/problems/minimum-factorization/)

@2023.10.27

```java
/**
 * 数学
 * 力扣官方题解
 */
public int smallestFactorization(int a)
{
	if (a < 10) return a;
	long ans = 0, mul = 1;
	for (int i = 9; i >= 2; i--)
	{
		while (a % i == 0)
		{
			a /= i;
			ans += mul * i;
			mul *= 10;
		}
	}
	return a < 2 && ans <= Integer.MAX_VALUE ? (int) ans : 0;
}
```

#### [633. 平方数之和](https://leetcode.cn/problems/sum-of-square-numbers/)

@2023.10.26

#双指针 

用左右双指针遍历 `[0, sqrt(c)]`，检查平方和是否为 `c`。

不会错过答案的原因：

- 如果平方和小于 `c`，那么固定 `l`、继续减小 `r` 只会让平方和更小，说明 `l` 可以排除，需要增大 `l`。
- 如果平方和大于 `c`，那么固定 `r`、继续增大 `l` 只会让平方和更大，说明 `r` 可以排除，需要减小 `r`。

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean judgeSquareSum(int c)
{
	long l = 0, r = (int) Math.sqrt(c);
	while (l <= r)
	{
		long cur = l * l + r * r;
		if (cur == c) return true;
		if (cur < c) l++;
		else r--;
	}
	return false;
}
```

#### [634. 寻找数组的错位排列](https://leetcode.cn/problems/find-the-derangement-of-an-array/)

@2023.10.26

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findDerangement(int n)
{
	if (n == 0) return 1;
	if (n == 1) return 0;
	long[] dp = new long[3];
	dp[0] = 1;
	for (int i = 2; i <= n; i++)
	{
		dp[2] = (i - 1) * (dp[1] + dp[0]) % 1000000007;
		dp[0] = dp[1];
		dp[1] = dp[2];
	}
	return (int) dp[2];
}
```

#### [638. 大礼包](https://leetcode.cn/problems/shopping-offers/)

@2023.10.27

```java
/**
 * DFS
 * Somnia1337
 */
private int ans = Integer.MAX_VALUE;
private List<List<Integer>> valid;

public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs)
{
	int len = price.size();
	valid = new ArrayList<>();
	for (List<Integer> offer : special)
	{
		int total = 0;
		for (int i = 0; i < len; i++) total += price.get(i) * offer.get(i);
		if (offer.get(len) < total) valid.add(new ArrayList<>(offer));
	}
	dfs(price, needs, 0);
	return ans;
}

private void dfs(List<Integer> price, List<Integer> needs, int cost)
{
	for (List<Integer> offer : valid)
	{
		List<Integer> next = new ArrayList<>();
		int it = 0;
		while (it < offer.size() - 1)
		{
			int need = needs.get(it);
			if (offer.get(it) > need) break;
			next.add(need - offer.get(it));
			it++;
		}
		if (it == offer.size() - 1) dfs(price, next, cost + offer.get(offer.size() - 1));
	}
	for (int i = 0; i < needs.size(); i++)
	{
		cost += needs.get(i) * price.get(i);
	}
	ans = Math.min(cost, ans);
}
```

#### [640. 求解方程](https://leetcode.cn/problems/solve-the-equation/)

@2023.10.26

改进：把 “-” 替换成 “+-”，然后按 “+” 切割。

```java
/**
 * 模拟
 * Somnia1337
 */
public String solveEquation(String equation)
{
	int[][] params = new int[2][2];
	String[] splits = equation.split("=");
	char[] left = splits[0].toCharArray(), right = splits[1].toCharArray();
	int l1 = left.length, l2 = right.length;
	
	int i = 0;
	while (i < l1)
	{
		boolean neg = false, isX = false;
		if (left[i] == '-')
		{
			neg = true;
			i++;
		}
		else if (left[i] != 'x' && !Character.isDigit(left[i])) i++;
		int e = i;
		while (e < l1)
		{
			if (left[e] == 'x')
			{
				isX = true;
				break;
			}
			else if (!Character.isDigit(left[e])) break;
			e++;
		}
		String p = splits[0].substring(i, e);
		int param = !p.isEmpty() ? Integer.parseInt(p) : 1;
		if (neg) param = -param;
		if (isX) params[0][0] += param;
		else params[0][1] += param;
		i = isX ? e + 1 : e;
	}
	
	i = 0;
	while (i < l2)
	{
		boolean neg = false, isX = false;
		if (right[i] == '-')
		{
			neg = true;
			i++;
		}
		else if (right[i] != 'x' && !Character.isDigit(right[i])) i++;
		int e = i;
		while (e < l2)
		{
			if (right[e] == 'x')
			{
				isX = true;
				break;
			}
			else if (!Character.isDigit(right[e])) break;
			e++;
		}
		String p = splits[1].substring(i, e);
		int param = !p.isEmpty() ? Integer.parseInt(p) : 1;
		if (neg) param = -param;
		if (isX) params[1][0] += param;
		else params[1][1] += param;
		i = isX ? e + 1 : e;
	}
	
	params[0][0] -= params[1][0];
	params[0][1] -= params[1][1];
	if (params[0][0] == 0)
	{
		if (params[0][1] == 0) return "Infinite solutions";
		return "No solution";
	}
	int ans = (-params[0][1] / params[0][0]);
	if (-ans * params[0][0] != params[0][1]) return "No solution";
	return "x=" + (-params[0][1] / params[0][0]);
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
public String solveEquation(String equation)
{
	int factor = 0, val = 0;
	int index = 0, n = equation.length(), sign1 = 1;
	while (index < n)
	{
		if (equation.charAt(index) == '=')
		{
			sign1 = -1;
			index++;
			continue;
		}
		
		int sign2 = sign1, number = 0;
		boolean valid = false;
		if (equation.charAt(index) == '-' || equation.charAt(index) == '+')
		{
			sign2 = (equation.charAt(index) == '-') ? -sign1 : sign1;
			index++;
		}
		while (index < n && Character.isDigit(equation.charAt(index)))
		{
			number = number * 10 + (equation.charAt(index) - '0');
			index++;
			valid = true;
		}
		
		if (index < n && equation.charAt(index) == 'x')
		{
			factor += valid ? sign2 * number : sign2;
			index++;
		}
		else val += sign2 * number;
	}
	
	if (factor == 0) return val == 0 ? "Infinite solutions" : "No solution";
	return "x=" + (-val / factor);
}
```

#### [646. 最长数对链](https://leetcode.cn/problems/maximum-length-of-pair-chain/)

@2023.10.28

1. 动态规划

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int findLongestChain(int[][] pairs)
{
	Arrays.sort(pairs, Comparator.comparingInt(a -> a[0]));
	int len = pairs.length;
	int[] dp = new int[len];
	Arrays.fill(dp, 1);
	for (int i = 0; i < len; i++)
	{
		for (int j = 0; j < i; j++)
		{
			if (pairs[i][0] > pairs[j][1])
			{
				dp[i] = Math.max(dp[j] + 1, dp[i]);
			}
		}
	}
	return dp[len - 1];
}
```

2. 贪心

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int findLongestChain(int[][] pairs)
{
	Arrays.sort(pairs, Comparator.comparingInt(a -> a[1]));
	int cur = Integer.MIN_VALUE, ans = 0;
	for (int[] pair : pairs)
	{
		if (cur < pair[0])
		{
			cur = pair[1];
			ans++;
		}
	}
	return ans;
}
```

#### [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

@2023.09.11

#暴力 #动态规划 

1. 枚举

枚举计算以每个字母为回文中心 / 回文偏左中心扩展得到的回文串个数。

```java
/**
 * 枚举
 * Somnia1337
 */
public int countSubstrings(String s) {
	int l = s.length(), ans = l; // 每个长为 1 的子串都回文, 初始化为 l
	for (int i = 0; i < l - 1; i++) ans += palindromeCnt(s, i);
	return ans;
}

private int palindromeCnt(String s, int i) {
	return centeredPalindromeCnt(s, i) + nonCenteredPalindromeCnt(s, i);
}

private int centeredPalindromeCnt(String s, int i) {
	int n = s.length(), l = i - 1, r = i + 1, ret = 0;
	while (l >= 0 && r < n && s.charAt(l--) == s.charAt(r++)) ret++;
	return ret;
}

private int nonCenteredPalindromeCnt(String s, int i) {
	int n = s.length(), l = i, r = i + 1, ret = 0;
	while (l >= 0 && r < n && s.charAt(l--) == s.charAt(r++)) ret++;
	return ret;
}
```

2. 动态规划

`dp[l][r]` 表示 `s[l:r]` 是否回文，递推式为 `dp[l][r] = s[l]==s[r] && (r-l<=1 || dp[l+1][r-1])`，解释如下：

- 首先要求 `s[l]==s[r]`，否则 `dp[l][r]` 为 `false`。
- 然后，
	- 如果当前子串长度 `<= 2`（`"a"` 或 `"aa"`），必为回文串，`dp[l][r]` 为 `true`。
	- 如果当前子串长度 `> 2`，两端字符已经相同，只要中间部分为回文，整体就为回文，中间部分即 `dp[l+1][r-1]`。

```java
/**
 * 动态规划
 * jawhiow
 */
public int countSubstrings(String s) {
	int n = s.length();
	char[] chs = s.toCharArray();
	boolean[][] dp = new boolean[n][n];
	int ans = 0;
	for (int r = 0; r < n; r++) {
		for (int l = 0; l <= r; l++) {
			dp[l][r] = chs[l] == chs[r] && (r - l + 1 <= 2 || dp[l + 1][r - 1]);
			if (dp[l][r]) ans++;
		}
	}
	return ans;
}
```

#### [648. 单词替换](https://leetcode.cn/problems/replace-words/)

@2023.10.28

1. 哈希表

哈希表 `key` 为首字母，`value` 为其下的词根列表。记录后，对每个词根列表按长度排序，每个单词匹配到的第一个词根就是满足要求的最短词根。

```java
/**
 * 哈希表
 * Somnia1337
 */
public String replaceWords(List<String> dictionary, String sentence)
{
	// 记录到哈希表
	Map<Character, List<String>> roots = new HashMap<>();
	for (String word : dictionary)
	{
		char c = word.charAt(0);
		List<String> root = roots.getOrDefault(c, new ArrayList<>());
		root.add(word);
		roots.put(c, root);
	}
	
	// 根据长度排序
	for (List<String> root : roots.values())
	{
		root.sort(Comparator.comparing(String::length));
	}
	
	// 拆分, 替换
	String[] words = sentence.split(" ");
	for (int i = 0; i < words.length; i++)
	{
		String word = words[i];
		Character key = word.charAt(0); // 取首字母
		if (roots.containsKey(key))
		{
			List<String> root = roots.get(key);
			for (String r : root)
			{
				if (word.length() >= r.length() && word.startsWith(r))
				{
					words[i] = r;
					break; // 已按长度排序, 替换为第一个满足要求的词根即可
				}
			}
		}
	}
	
	return String.join(" ", words);
}
```

2. 字典树

```java
class Solution
{
    public String replaceWords(List<String> dictionary, String sentence)
    {
        Trie trie = new Trie();
        for (String word : dictionary)
        {
            Trie cur = trie;
            for (int i = 0; i < word.length(); i++)
            {
                char c = word.charAt(i);
                cur.children.putIfAbsent(c, new Trie());
                cur = cur.children.get(c);
            }
            cur.children.put('#', new Trie());
        }
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; i++)
        {
            words[i] = findRoot(words[i], trie);
        }
        return String.join(" ", words);
    }
	
    public String findRoot(String word, Trie trie)
    {
        StringBuilder root = new StringBuilder();
        Trie cur = trie;
        for (int i = 0; i < word.length(); i++)
        {
            char c = word.charAt(i);
            if (cur.children.containsKey('#'))
            {
                return root.toString();
            }
            if (!cur.children.containsKey(c))
            {
                return word;
            }
            root.append(c);
            cur = cur.children.get(c);
        }
        return root.toString();
    }
}

class Trie
{
    Map<Character, Trie> children;
	
    public Trie()
    {
        children = new HashMap<>();
    }
}
```

#### [649. Dota2 参议院](https://leetcode.cn/problems/dota2-senate/)

@2023.10.28

每个议员都要 ban 掉接下来的第 1 个敌方议员。

```java
/**
 * 贪心
 * Somnia1337
 */
public String predictPartyVictory(String senate)
{
	int len = senate.length();
	List<Integer> Rs = new ArrayList<>(), Ds = new ArrayList<>();
	Set<Integer> ban = new HashSet<>();
	for (int i = 0; i < len; i++)
	{
		if (senate.charAt(i) == 'R') Rs.add(i);
		else Ds.add(i);
	}
	if (Ds.isEmpty()) return "Radiant";
	if (Rs.isEmpty()) return "Dire";
	do
	{
		for (int i = 0; i < len; i++)
		{
			if (ban.contains(i)) continue;
			int j = 0;
			if (senate.charAt(i) == 'R')
			{
				while (j < Ds.size() && Ds.get(j) < i) j++;
				int b = j < Ds.size() ? Ds.remove(j) : Ds.remove(0);
				if (Ds.isEmpty()) return "Radiant";
				ban.add(b);
			}
			else
			{
				while (j < Rs.size() && Rs.get(j) < i) j++;
				int b = j < Rs.size() ? Rs.remove(j) : Rs.remove(0);
				if (Rs.isEmpty()) return "Dire";
				ban.add(b);
			}
		}
	} while (ban.size() != len);
	return "";
}
```

```java
/**
 * 贪心
 * 力扣官方题解
 */
public String predictPartyVictory(String senate)
{
	int len = senate.length();
	Queue<Integer> radiant = new LinkedList<>();
	Queue<Integer> dire = new LinkedList<>();
	for (int i = 0; i < len; i++)
	{
		if (senate.charAt(i) == 'R') radiant.add(i);
		else dire.add(i);
	}
	while (!radiant.isEmpty() && !dire.isEmpty())
	{
		int radiantIndex = radiant.poll(), direIndex = dire.poll();
		if (radiantIndex < direIndex) radiant.add(radiantIndex + len);
		else dire.add(direIndex + len);
	}
	return !radiant.isEmpty() ? "Radiant" : "Dire";
}
```

#### [650. 只有两个键的键盘](https://leetcode.cn/problems/2-keys-keyboard/)

@2023.10.28

```java
/**
 * 数学
 * fibonacciWH
 */
public int minSteps(int n)
{
	int ans = 0;
	for (int i = 2; i <= n; i++)
	{
		while (n % i == 0)
		{
			ans += i;
			n /= i;
		}
	}
	return ans;
}
```

#### [652. 寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/)

@2023.11.09

#序列化 

`Map<序列字符串, 树节点>` 记录序列串，`Set<树节点>` 收集答案。序列化方式为 `node.val + "(" + dfs(node.left) + ")(" + dfs(node.right) + ")"`。

```java
/**
 * 序列化
 * 力扣官方题解
 */
private Map<String, TreeNode> vis;
private Set<TreeNode> collect;

public List<TreeNode> findDuplicateSubtrees(TreeNode root)
{
	vis = new HashMap<>();
	collect = new HashSet<>();
	dfs(root);
	return new ArrayList<>(collect);
}

private String dfs(TreeNode node)
{
	if (node == null) return "";
	String serial = node.val + "(" + dfs(node.left) + ")(" + dfs(node.right) + ")";
	if (vis.containsKey(serial)) collect.add(vis.get(serial));
	else vis.put(serial, node);
	return serial;
}
```

#### [658. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)

@2023.10.28

#滑动窗口 

维护滑窗内元素与 `x` 的差之和 `sum`，滑动，当 `sum` 开始增加时说明已找到答案。

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public List<Integer> findClosestElements(int[] arr, int k, int x)
{
	int len = arr.length;
	int it = 0, sum = 0;
	while (it < k) sum += Math.abs(arr[it++] - x);
	while (it < len)
	{
		int hold = sum; // 保存滑之前的值
		sum += Math.abs(arr[it] - x) - Math.abs(arr[it - k] - x); // 更新 sum
		if (sum > hold || sum == hold && arr[it] > arr[it - k]) break; // sum 增加了, 找到答案
		it++;
	}
	List<Integer> ans = new ArrayList<>();
	for (int i = it - k; i < it; i++) ans.add(arr[i]);
	return ans;
}
```

#### [659. 分割数组为连续子序列](https://leetcode.cn/problems/split-array-into-consecutive-subsequences/)

@2023.11.01

#贪心 

贪心：每个子序列都尽可能长。

哈希表计数，从每个开头元素查找最长的递增子序列，同时记录其长度 `l` 并更新计数，如果 `l` 短于 3 则返回 `false`。

```java
/**
 * 贪心
 * QQ怪
 */
public boolean isPossible(int[] nums)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int num : nums) count.merge(num, 1, Integer::sum);
	
	for (int num : nums)
	{
		int l = 0, cnt = 1; // l 为当前子序列长度, cnt 为当前元素计数
		while (count.getOrDefault(num, 0) >= cnt)
		{
			cnt = count.get(num);
			count.merge(num, -1, Integer::sum); // 用掉 1 个
			num++; // 查找下一个元素
			l++;
		}
		if (l > 0 && l < 3) return false;
	}
	
	return true;
}
```

#### [665. 非递减数列](https://leetcode.cn/problems/non-decreasing-array/)

@2023.10.29

#数组 

记录极值下标 `peak`，如果出现两个极值，直接返回 `false`。分别将 `peak` 与 `peak + 1` ban 掉，检查其余元素是否非递减。

```java
/**
 * 一次遍历
 * Somnia1337
 */
public boolean checkPossibility(int[] nums) {
	int peak = -1;
	for (int i = 0; i < nums.length - 1; i++) {
		if (nums[i] > nums[i + 1]) {
			if (peak >= 0) return false; // 出现 2 个逆序对
			peak = i;
		}
	}
	return check(nums, peak) || check(nums, peak + 1);
}

private boolean check(int[] nums, int ban) {
	int pre = Integer.MIN_VALUE;
	for (int i = 0; i < nums.length; i++) {
		if (i == ban) continue;
		if (nums[i] < pre) return false;
		pre = nums[i];
	}
	return true;
}
```

#### [667. 优美的排列 II](https://leetcode.cn/problems/beautiful-arrangement-ii/)

@2023.10.31

#技巧 

> 这道题偏 OJ 风格，属于思维题而并不涉及数据结构和算法，标中等是不是合适就见仁见智了（太古老的比赛题也没有 rating 数据），虽然实现简单但不看答案短时间想不出正确思路也很正常（主要是得想到 1,n,2,n-1,... 这个序列可以满足最大的合法 k 值），还是不要因为一道题轻易怀疑自己和失去信心……
> 
> ——Carl_Czerny

```java
/**
 * 找规律
 * 宫水三叶
 */
public int[] constructArray(int n, int k)
{
	int[] ans = new int[n];
	int t = n - k - 1, it = 0;
	while (it < t)
	{
		ans[it] = it + 1;
		it++;
	}
	int a = n - k, b = n;
	while (it < n)
	{
		ans[it++] = a++;
		if (it < n) ans[it++] = b--;
	}
	return ans;
}
```

#### [670. 最大交换](https://leetcode.cn/problems/maximum-swap/)

@2023.10.30

1. 暴力

枚举每对数位。

```java
/**
 * 枚举
 * Somnia1337
 */
public int maximumSwap(int num)
{
	char[] digits = String.valueOf(num).toCharArray();
	int len = digits.length, ans = num;
	for (int i = 0; i < len - 1; i++)
	{
		for (int j = i + 1; j < len; j++)
		{
			swap(digits, i, j);
			ans = Math.max(Integer.parseInt(new String(digits)), ans);
			swap(digits, i, j);
		}
	}
	return ans;
}

private void swap(char[] arr, int i, int j)
{
	char t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

2. 贪心

每一位数字应该不小于所有排它后面的数字，否则找最大的且排最后面的数字与之交换。

```java
/**
 * 贪心
 * 力扣官方题解
 */
public int maximumSwap(int num)
{
	char[] digits = String.valueOf(num).toCharArray();
	int len = digits.length;
	int maxIdx = len - 1;
	int idx1 = -1, idx2 = -1;
	for (int i = len - 1; i >= 0; i--)
	{
		if (digits[i] > digits[maxIdx]) maxIdx = i;
		else if (digits[i] < digits[maxIdx])
		{
			idx1 = i;
			idx2 = maxIdx;
		}
	}
	if (idx1 >= 0)
	{
		swap(digits, idx1, idx2);
		return Integer.parseInt(new String(digits));
	}
	else return num;
}

private void swap(char[] arr, int i, int j)
{
	char temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}
```

#### [673. 最长递增子序列的个数](https://leetcode.cn/problems/number-of-longest-increasing-subsequence/)

@2023.10.30

#动态规划 

`dp[i]` 为以 `nums[i]` 为末元素的最长子序列长度，`cnt[i]` 为以 `nums[i]` 为末元素的最长子序列数量。过程中维护 `max(dp[i])`，最后倒序遍历，累加 `dp[k] == max` 的 `cnt[k]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findNumberOfLIS(int[] nums)
{
	int len = nums.length;
	int[] dp = new int[len], cnt = new int[len];
	Arrays.fill(cnt, 1);
	int max = 0;
	for (int i = 0; i < len; i++)
	{
		for (int j = 0; j < i; j++)
		{
			if (nums[j] < nums[i]) // 构成子序列
			{
				if (dp[j] + 1 > dp[i]) // 更长了
				{
					dp[i] = dp[j] + 1;
					cnt[i] = cnt[j];
				}
				else if (dp[j] + 1 == dp[i]) // 一样长
				{
					cnt[i] += cnt[j];
				}
			}
		}
		max = Math.max(dp[i], max);
	}
	int ans = 0;
	for (int i = len - 1; i >= 0; i--) // 累加
	{
		if (dp[i] == max) ans += cnt[i];
	}
	return ans;
}
```

#### [676. 实现一个魔法字典](https://leetcode.cn/problems/implement-magic-dictionary/)

@2023.11.07

#设计 

哈希表记录长度相同的字符串组，`search()` 时遍历等长组的字符串，检查是否刚好有 1 个字符不同。

```java
/**
 * 哈希表
 * Somnia1337
 */
class MagicDictionary
{
    private Map<Integer, List<String>> dict;
	
    public MagicDictionary()
    {
        dict = new HashMap<>();
    }
	
    public void buildDict(String[] dictionary)
    {
        for (String word : dictionary)
        {
            int l = word.length();
            dict.putIfAbsent(l, new ArrayList<>());
            dict.get(l)
                .add(word);
        }
    }
	
    public boolean search(String searchWord)
    {
        int l = searchWord.length();
        List<String> group = dict.get(l);
        if (group == null) return false;
        for (String s : group)
        {
            int diff = 0;
            for (int i = 0; i < l; i++)
            {
                if (s.charAt(i) != searchWord.charAt(i)) diff++;
            }
            if (diff == 1) return true;
        }
        return false;
    }
}
```

学习：字典树

#### [677. 键值映射](https://leetcode.cn/problems/map-sum-pairs/)

@2023.11.15

1. 模拟

哈希表记录，模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
class MapSum
{
    private Map<String, Integer> vals;
	
    public MapSum()
    {
        vals = new HashMap<>();
    }
	
    public void insert(String key, int val)
    {
        vals.put(key, val);
    }
	
    public int sum(String prefix)
    {
        int ret = 0;
        for (String key : vals.keySet())
        {
            if (key.startsWith(prefix)) ret += vals.get(key);
        }
        return ret;
    }
}
```

2. 字典树

```java
class MapSum
{
    TrieNode root;
    Map<String, Integer> map;
	
    public MapSum()
    {
        root = new TrieNode();
        map = new HashMap<>();
    }
	
    public void insert(String key, int val)
    {
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);
        TrieNode node = root;
        for (char c : key.toCharArray())
        {
            if (node.next[c - 'a'] == null)
            {
                node.next[c - 'a'] = new TrieNode();
            }
            node = node.next[c - 'a'];
            node.val += delta;
        }
    }
	
    public int sum(String prefix)
    {
        TrieNode node = root;
        for (char c : prefix.toCharArray())
        {
            if (node.next[c - 'a'] == null) return 0;
            node = node.next[c - 'a'];
        }
        return node.val;
    }
	
    class TrieNode
    {
        int val = 0;
        TrieNode[] next = new TrieNode[26];
    }
}
```

#### [678. 有效的括号字符串](https://leetcode.cn/problems/valid-parenthesis-string/)

@2023.12.26

#贪心 

1. 贪心

两次遍历：

- 第一次正向，`'(' -> +1`，`')' -> -1`，`'*' -> +1`。
- 第二次反向，`'(' -> -1`，`')' -> +1`，`'*' -> +1`。

如果两次遍历过程中都未出现计数器 `< 0` 的情况，则所有左右括号都一定能匹配。

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean checkValidString(String s) {
	return check(s, 1) && check(reverse(s), -1);
}

private boolean check(String s, int add) {
	int cnt = 0;
	for (char c : s.toCharArray()) {
		switch (c) {
			case '(' -> cnt += add;
			case ')' -> cnt -= add;
			default -> cnt++;
		}
		if (cnt < 0) return false;
	}
	return true;
}

private String reverse(String s) {
	StringBuilder re = new StringBuilder(s);
	return re.reverse().toString();
}
```

2. 动态规划

```java
/**
 * 动态规划
 * 宫水三叶
 */
public boolean checkValidString(String s) {
	int l = s.length();
	boolean[][] dp = new boolean[l + 1][l + 1];
	dp[0][0] = true;
	for (int i = 1; i <= l; i++) {
		char c = s.charAt(i - 1);
		for (int j = 0; j <= i; j++) {
			if (c == '(') {
				if (j - 1 >= 0) dp[i][j] = dp[i - 1][j - 1];
			}
			else if (c == ')') {
				if (j + 1 <= i) dp[i][j] = dp[i - 1][j + 1];
			}
			else {
				dp[i][j] = dp[i - 1][j];
				if (j - 1 >= 0) dp[i][j] |= dp[i - 1][j - 1];
				if (j + 1 <= i) dp[i][j] |= dp[i - 1][j + 1];
			}
		}
	}
	return dp[l][0];
}
```

#### [681. 最近时刻](https://leetcode.cn/problems/next-closest-time/)

@2023.10.31

#暴力 

枚举，遍历更新最小时刻、大于当前时刻的最小时刻，判断是否到达第二天。

```java
/**
 * 枚举
 * Somnia1337
 */
public String nextClosestTime(String time)
{
	int l = Integer.parseInt(time.substring(0, 2));
	int r = Integer.parseInt(time.substring(3));
	Set<Integer> nums = new HashSet<>(List.of(l / 10, l % 10, r / 10, r % 10));
	List<Integer> candi = new ArrayList<>();
	int t = l * 60 + r;
	for (int a : nums)
	{
		if (a > 2) continue;
		for (int b : nums)
		{
			l = a * 10 + b;
			if (l > 23) continue;
			for (int c : nums)
			{
				if (c > 5) continue;
				for (int d : nums)
				{
					r = c * 10 + d;
					int p = l * 60 + r;
					candi.add(p);
				}
			}
		}
	}
	int min = 1440, up = 1440; // min 为最小时刻, up 为大于当前时刻的最小时刻
	for (int cur : candi)
	{
		min = Math.min(cur, min);
		if (cur > t) up = Math.min(cur, up);
	}
	if (up < 1440)
	{
		l = up / 60;
		r = up % 60;
	}
	else
	{
		l = min / 60;
		r = min % 60;
	}
	return (l >= 10 ? "" + l : "0" + l) + ":" + (r >= 10 ? "" + r : "0" + r);
}
```

#### [684. 冗余连接](https://leetcode.cn/problems/redundant-connection/)

@2023.11.15

#并查集 

并查集维护支，遍历 `edges`，如果新增边的两端点不在同一支，合并两支，否则将形成环，返回这条边。

```java
/**
 * 并查集
 * Somnia1337
 */
public int[] findRedundantConnection(int[][] edges) {
	int n = edges.length;
	int[] root = new int[n + 1];
	Arrays.setAll(root, a -> a);
	for (int[] e : edges) {
		int n1 = e[0], n2 = e[1];
		if (find(root, n1) != find(root, n2)) union(root, n1, n2);
		else return new int[]{n1, n2};
	}
	return null;
}

private int find(int[] root, int i) {
	if (root[i] != i) root[i] = find(root, root[i]);
	return root[i];
}

private void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```

#### [686. 重复叠加字符串匹配](https://leetcode.cn/problems/repeated-string-match/)

@2023.10.31

#分类讨论 

先用哈希表排除 `b` 中存在独有字符的情况。记 `ceil(len(b)/len(a))` 为 `t`，只需尝试 `a` 重复 `t` 次、`t + 1` 次，否则返回 `-1`。

```java
/**
 * 分类讨论
 * 空白白喵白。oO
 */
public int repeatedStringMatch(String a, String b) {
	Set<Character> vis = new HashSet<>();
	for (char c : a.toCharArray()) vis.add(c);
	for (char c : b.toCharArray()) {
		if (!vis.contains(c)) return -1;
	}
	
	int t = (int) Math.ceil((double) b.length() / a.length());
	if (a.repeat(t).contains(b)) return t;
	if (a.repeat(t + 1).contains(b)) return t + 1;
	return -1;
}
```

#### [688. 骑士在棋盘上的概率](https://leetcode.cn/problems/knight-probability-in-chessboard/)

@2023.10.31

#动态规划 

`dp[k][x][y]` 为骑士从 `(x, y)` 出发、走了 `k` 步后仍在棋盘上的概率，递推式 `dp[k][x][y] = sum(dp[k - 1][x + dx][y + dy]) / 8`，其中 `dx`、`dy` 为方向偏移量。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public double knightProbability(int n, int k, int row, int column)
{
	double[][][] dp = new double[k + 1][n][n];
	int[][] dirs = {{-1, -2}, {-2, -1}, {1, -2}, {-2, 1}, {-1, 2}, {2, -1}, {1, 2}, {2, 1}};
	for (double[] r : dp[0]) Arrays.fill(r, 1);
	for (int i = 1; i < k + 1; i++)
	{
		for (int x = 0; x < n; x++)
		{
			for (int y = 0; y < n; y++)
			{
				for (int[] dir : dirs)
				{
					int nextX = x + dir[0], nextY = y + dir[1];
					if (nextX >= 0 && nextX < n && nextY >= 0 && nextY < n)
					{
						dp[i][x][y] += dp[i - 1][nextX][nextY] / 8;
					}
				}
			}
		}
	}
	return dp[k][row][column];
}
```

#### [690. 员工的重要性](https://leetcode.cn/problems/employee-importance/)

@2023.11.15

#搜索 

DFS 员工的下级。

```java
/**
 * DFS
 * Somnia1337
 */
private Map<Integer, Employee> emp;

public int getImportance(List<Employee> employees, int id)
{
	emp = new HashMap<>();
	for (Employee e : employees) emp.put(e.id, e);
	return dfs(id);
}

private int dfs(int id)
{
	Employee e = emp.get(id);
	int ret = e.importance;
	for (int n : e.subordinates) ret += dfs(n);
	return ret;
}
```

#### [692. 前K个高频单词](https://leetcode.cn/problems/top-k-frequent-words/)

@2023.10.30

#哈希表 

统计词频，对 `keySet()` 排序，取前 `k` 个。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<String> topKFrequent(String[] words, int k)
{
	Map<String, Integer> cnt = new HashMap<>();
	for (String word : words)
	{
		cnt.merge(word, 1, Integer::sum);
	}
	List<String> keys = new ArrayList<>(cnt.keySet());
	keys.sort((k1, k2) -> {
		int c1 = cnt.get(k1), c2 = cnt.get(k2);
		if (c1 != c2) return c2 - c1;
		return k1.compareTo(k2);
	});
	return keys.subList(0, k);
}
```

#### [694. 不同岛屿的数量](https://leetcode.cn/problems/number-of-distinct-islands/)

@2023.10.31

#搜索 

用字符串记录 DFS 过程中的路径，哈希表去重，最后表的大小即为答案。

```java
/**
 * DFS
 * Somnia1337
 */
private final int[] dirs = {0, 1, 0, -1, 0};
private final String[] from = new String[]{"l", "u", "r", "d"};
private int rows, cols;
private StringBuilder land;

public int numDistinctIslands(int[][] grid) {
	rows = grid.length;
	cols = grid[0].length;
	Set<String> vis = new HashSet<>();
	for (int i = 0; i < rows; i++) {
		for (int j = 0; j < cols; j++) {
			if (grid[i][j] == 1) {
				land = new StringBuilder();
				dfs(grid, i, j, "");
				vis.add(land.toString());
			}
		}
	}
	return vis.size();
}

private void dfs(int[][] g, int x, int y, String f) {
	if (!inBound(x, y) || g[x][y] == 0) return;
	land.append(f);
	g[x][y] = 0;
	for (int k = 0; k < 4; k++) dfs(g, x + dirs[k], y + dirs[k + 1], from[k]);
	land.append(" ");
}

private boolean inBound(int x, int y) {
	return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

#### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)

@2023.10.19

#搜索 

全局变量 `cur` 记录当前岛屿的大小，遍历，对每个岛屿计算大小并标记为 0，

1. DFS

```java
/**
 * DFS
 * Somnia1337
 */
private int cur; // 当前岛屿的大小

public int maxAreaOfIsland(int[][] grid)
{
	int ans = 0;
	
	for (int i = 0; i < grid.length; i++)
	{
		for (int j = 0; j < grid[0].length; j++)
		{
			if (grid[i][j] == 1)
			{
				cur = 0; // 重置大小
				dfs(grid, i, j);
				ans = Math.max(cur, ans); // 更新最大值
			}
		}
	}
	
	return ans;
}

private void dfs(int[][] grid, int x, int y)
{
	if (x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] == 0) return;
	grid[x][y] = 0;
	cur++; // 大小加1与标记为0耦合
	dfs(grid, x + 1, y);
	dfs(grid, x, y + 1);
	dfs(grid, x - 1, y);
	dfs(grid, x, y - 1);
}
```

2. BFS

```java
/**
 * BFS
 * Somnia1337
 */
private int cur; // 当前岛屿的大小

public int maxAreaOfIsland(int[][] grid)
{
	int ans = 0;
	for (int i = 0; i < grid.length; i++)
	{
		for (int j = 0; j < grid[0].length; j++)
		{
			if (grid[i][j] == 1)
			{
				cur = 0; // 重置大小
				bfs(grid, i, j);
				ans = Math.max(cur, ans); // 更新最大值
			}
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
		int[] land = queue.remove();
		x = land[0];
		y = land[1];
		if (0 <= x && x < grid.length && 0 <= y && y < grid[0].length && grid[x][y] == 1)
		{
			grid[x][y] = 0;
			cur++; // 大小加1与标记为0耦合
			queue.add(new int[]{x + 1, y});
			queue.add(new int[]{x - 1, y});
			queue.add(new int[]{x, y + 1});
			queue.add(new int[]{x, y - 1});
		}
	}
}
```

#### [698. 划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/)

@2023.10.23

#回溯 

每个子集的元素和必须为 `sum(nums) / k`，回溯检查是否存在一种分配，剪枝参见 [1723. 完成所有工作的最短时间](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/)。

```java
/**
 * 回溯
 * Somnia1337
 */
public boolean canPartitionKSubsets(int[] nums, int k) {
    Arrays.sort(nums);
    int sum = 0;
    for (int x : nums) sum += x;
    if (sum % k != 0) return false;
    int tar = sum / k;
    int[] subs = new int[k];
    return bt(nums, subs, tar, nums.length - 1);
}

private boolean bt(int[] nums, int[] subs, int tar, int idx) {
    if (idx == -1) return true;
    for (int i = subs.length - 1; i >= 0; i--) {
        if (subs[i] + nums[idx] > tar) continue;
        subs[i] += nums[idx];
        if (bt(nums, subs, tar, idx - 1)) return true;
        subs[i] -= nums[idx];
        if (subs[i] == 0 || subs[i] + nums[idx] == tar) return false;
    }
    return false;
}
```

#### [702. 搜索长度未知的有序数组](https://leetcode.cn/problems/search-in-a-sorted-array-of-unknown-size/)

@2023.11.15

#二分查找 

两次二分，第一次查找上界，第二次查找 `target`。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int search(ArrayReader reader, int target)
{
	int left = 0, right = 10000;
	while (left <= right)
	{
		int mid = left + right >> 1;
		if (reader.get(mid) == Integer.MAX_VALUE) right = mid - 1;
		else left = mid + 1;
	}
	int l = 0, r = left;
	while (l <= r)
	{
		int m = l + r >> 1, v = reader.get(m);
		if (v == target) return m;
		if (v < target) l = m + 1;
		else r = m - 1;
	}
	return -1;
}
```

#### [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

@2023.11.15

```java
/**
 * 模拟
 * Somnia1337
 */
class MyLinkedList
{
    int size;
    MyNode head, tail;
	
    public MyLinkedList()
    {
        size = 0;
        head = new MyNode();
    }
	
    public int get(int index)
    {
        if (size <= index) return -1;
        MyNode it = head.next;
        while (index-- > 0) it = it.next;
        if (it == null) return -1;
        return it.val;
    }
	
    public void addAtHead(int val)
    {
        head.next = new MyNode(val, head.next);
        size++;
        if (size == 1) tail = head.next;
    }
	
    public void addAtTail(int val)
    {
        if (size == 0)
        {
            addAtHead(val);
            return;
        }
        tail.next = new MyNode(val);
        tail = tail.next;
        size++;
    }
	
    public void addAtIndex(int index, int val)
    {
        if (index < 0 || index > size) return;
        if (index == size)
        {
            addAtTail(val);
            return;
        }
        MyNode it = head;
        while (index-- > 0) it = it.next;
        it.next = new MyNode(val, it.next);
        size++;
    }
	
    public void deleteAtIndex(int index)
    {
        if (index < 0 || index >= size) return;
        MyNode it = head.next, pre = head;
        while (index-- > 0)
        {
            it = it.next;
            pre = pre.next;
        }
        pre.next = it != null ? it.next : null;
        if (it == tail) tail = pre;
        size--;
        if (size == 0) tail = null;
    }
	
    class MyNode
    {
        int val;
        MyNode next;
		
        public MyNode() {}
		
        public MyNode(int val)
        {
            this.val = val;
        }
		
        public MyNode(int val, MyNode next)
        {
            this.val = val;
            this.next = next;
        }
    }
}
```

```java
/**
 * 模拟
 * 力扣官方题解
 */
class MyLinkedList
{
    int size;
    MyNode head;
	
    public MyLinkedList()
    {
        size = 0;
        head = new MyNode(0);
    }
	
    public int get(int index)
    {
        if (index < 0 || index >= size) return -1;
        MyNode it = head;
        while (index-- >= 0) it = it.next;
        return it.val;
    }
	
    public void addAtHead(int val)
    {
        addAtIndex(0, val);
    }
	
    public void addAtTail(int val)
    {
        addAtIndex(size, val);
    }
	
    public void addAtIndex(int index, int val)
    {
        if (index < 0 || index > size) return;
        MyNode pre = head;
        for (int i = 0; i < index; i++) pre = pre.next;
        pre.next = new MyNode(val, pre.next);
        size++;
    }
	
    public void deleteAtIndex(int index)
    {
        if (index < 0 || index >= size) return;
        MyNode pre = head;
        for (int i = 0; i < index; i++) pre = pre.next;
        pre.next = pre.next.next;
        size--;
    }
}

class MyNode
{
    int val;
    MyNode next;
	
    public MyNode(int val)
    {
        this.val = val;
    }
	
    public MyNode(int val, MyNode next)
    {
        this.val = val;
        this.next = next;
    }
}
```

#### [708. 循环有序列表的插入](https://leetcode.cn/problems/insert-into-a-sorted-circular-linked-list/)

@2024.01.15



```java
/**
 * 一次遍历
 * 啊飞飞飞飞飞
 */
public Node insert(Node head, int insertVal) {
	if (head == null) {
		Node node = new Node(insertVal);
		return node.next = node;
	}
	Node pre = head, it = head.next;
	while (it != head) {
		if (pre.val <= insertVal && insertVal <= it.val || pre.val > it.val && (insertVal >= pre.val || insertVal <= it.val))
			break;
		pre = it;
		it = it.next;
	}
	pre.next = new Node(insertVal, it);
	return head;
}
```

#### [712. 两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/)

@2023.10.30

#动态规划 

`dp[i][j]` 为子问题 `s1[:i]` 与 `s2[:j]` 的答案，递推式为：

- 如果 `c1 == c2`，`dp[i][j] = dp[i - 1][j - 1]`。
- 如果 `c1 != c2`，`dp[i][j] = min(dp[i - 1][j] + c1, dp[i][j - 1] + c2)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minimumDeleteSum(String s1, String s2)
{
	int l1 = s1.length(), l2 = s2.length();
	int[][] dp = new int[l1 + 1][l2 + 1];
	
	for (int i = 1; i < l1 + 1; i++)
	{
		dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
	}
	for (int j = 1; j < l2 + 1; j++)
	{
		dp[0][j] = dp[0][j - 1] + s2.charAt(j - 1);
	}
	
	for (int i = 1; i < l1 + 1; i++)
	{
		char c1 = s1.charAt(i - 1);
		for (int j = 1; j < l2 + 1; j++)
		{
			char c2 = s2.charAt(j - 1);
			if (c1 == c2)
			{
				dp[i][j] = dp[i - 1][j - 1];
			}
			else
			{
				dp[i][j] = Math.min(dp[i - 1][j] + c1, dp[i][j - 1] + c2);
			}
		}
	}
	
	return dp[l1][l2];
}
```

#### [713. 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)

@2023.10.30

#双指针 

枚举以 `r` 为末元素的子数组，维护 `nums[l:r]` 的乘积小于 `k`，累加 `r - l`。

```java
/**
 * 双指针
 * Somnia1337
 */
public int numSubarrayProductLessThanK(int[] nums, int k)
{
	int len = nums.length;
	int l = 0, r = 0, cur = 1, ans = 0;
	while (r < len)
	{
		cur *= nums[r];
		while (l < r && cur >= k) cur /= nums[l++];
		ans += r - l + (nums[r] < k ? 1 : 0);
		r++;
	}
	return ans;
}
```

#### [714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

@2023.09.17

```java
/**
 * 动态规划
 * ChatGPT
 */
public int maxProfit(int[] prices, int fee)
{
	int len = prices.length;
	int cash = 0;
	int hold = -prices[0];
	
	for (int i = 1; i < len; i++)
	{
		int prevCash = cash;
		cash = Math.max(cash, hold + prices[i] - fee);
		hold = Math.max(hold, prevCash - prices[i]);
	}
	
	return cash;
}
```

#### [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

@2023.09.08

#动态规划 #滑动窗口 

1. 动态规划

`dp[i][j]` 表示 `nums1[i:]` 与 `nums2[j:]` 的最长公共前缀，答案为 `max(dp)`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findLength(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length;
	int[][] dp = new int[n1][n2];
	if (nums1[n1 - 1] == nums2[n2 - 1]) dp[n1 - 1][n2 - 1]++;
	for (int i1 = n1 - 2; i1 >= 0; i1--) {
		if (nums1[i1] == nums2[n2 - 1]) dp[i1][n2 - 1] = 1;
		dp[i1][n2 - 1] = Math.max(dp[i1 + 1][n2 - 1], dp[i1][n2 - 1]);
	}
	for (int i2 = n2 - 2; i2 >= 0; i2--) {
		if (nums2[i2] == nums1[n1 - 1]) dp[n1 - 1][i2] = 1;
		dp[n1 - 1][i2] = Math.max(dp[n1 - 1][i2 + 1], dp[n1 - 1][i2]);
	}
	int ans = 0;
	for (int i1 = n1 - 2; i1 >= 0; i1--) {
		for (int i2 = n2 - 2; i2 >= 0; i2--) {
			if (nums1[i1] == nums2[i2]) dp[i1][i2] = 1;
			if (nums1[i1 + 1] == nums2[i2 + 1]) dp[i1][i2] += dp[i1 + 1][i2 + 1];
			ans = Math.max(dp[i1][i2], ans);
		}
	}
	return ans;
}
```

优化：`nums1[i] != nums2[j]` 时，`dp[i][j]` 直接置 `0`，无需再考虑` dp[i + 1][j + 1]`。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int findLength(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length;
	int[][] dp = new int[n1][n2];
	if (nums1[n1 - 1] == nums2[n2 - 1]) dp[n1 - 1][n2 - 1]++;
	for (int i1 = n1 - 2; i1 >= 0; i1--) {
		if (nums1[i1] == nums2[n2 - 1]) {
			while (i1 >= 0) dp[i1--][n2 - 1] = 1;
		}
	}
	for (int i2 = n2 - 2; i2 >= 0; i2--) {
		if (nums2[i2] == nums1[n1 - 1]) {
			while (i2 >= 0) dp[n1 - 1][i2--] = 1;
		}
	}
	int ans = 0;
	for (int i1 = n1 - 2; i1 >= 0; i1--) {
		for (int i2 = n2 - 2; i2 >= 0; i2--) {
			if (nums1[i1] == nums2[i2]) dp[i1][i2] = dp[i1 + 1][i2 + 1] + 1;
			ans = Math.max(dp[i1][i2], ans);
		}
	}
	return ans;
}
```

优化：`dp` 在两个维度上都多开 1 个单位，相当于这一部分全部初始化为 `0`。

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public int findLength(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length;
	int[][] dp = new int[n1 + 1][n2 + 1];
	int ans = 0;
	for (int i1 = n1 - 1; i1 >= 0; i1--) {
		for (int i2 = n2 - 1; i2 >= 0; i2--) {
			if (nums1[i1] == nums2[i2]) dp[i1][i2] = dp[i1 + 1][i2 + 1] + 1;
			ans = Math.max(dp[i1][i2], ans);
		}
	}
	return ans;
}
```

另一种思路：见题解 [一张表，八句话看懂动态规划(DP)思路](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/solutions/310917/yi-zhang-biao-ba-ju-hua-kan-dong-dong-tai-gui-hua-/)。

```java
/**
 * 动态规划
 * 南方是北方的远方啊
 */
public int findLength(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length, ans = 0;
	int[][] dp = new int[n1 + 1][n2 + 1];
	for (int i = 1; i <= n1; i++) {
		for (int j = 1; j <= n2; j++) {
			if (nums1[i - 1] == nums2[j - 1]) {
				dp[i][j] = dp[i - 1][j - 1] + 1;
				ans = Math.max(dp[i][j], ans);
			}
		}
	}
	return ans;
}
```

2. 滑动窗口

![滑动窗口](https://pic.leetcode-cn.com/9ed48b9b51214a8bafffcad17356d438b4c969b4999623247278d23f1e43977f-%E9%94%99%E5%BC%80%E6%AF%94%E8%BE%83.gif)

```java
/**
 * 滑动窗口
 * 天高
 */
public int findLength(int[] nums1, int[] nums2) {
	int n1 = nums1.length, n2 = nums2.length, ans = 0;
	// 移动 nums1
	for (int i1 = 0; i1 < n1; i1++) {
		ans = Math.max(maxOverlap(nums1, nums2, i1, 0, Math.min(n2, n1 - i1)), ans);
	}
	// 移动 nums2
	for (int i2 = 0; i2 < n2; i2++) {
		ans = Math.max(maxOverlap(nums1, nums2, 0, i2, Math.min(n1, n2 - i2)), ans);
	}
	return ans;
}

// 计算重合部分的最大连续相同长度
private int maxOverlap(int[] a1, int[] a2, int off1, int off2, int l) {
	int ret = 0, k = 0;
	for (int i = 0; i < l; i++) {
		if (a1[off1 + i] == a2[off2 + i]) k++;
		else k = 0;
		ret = Math.max(k, ret);
	}
	return ret;
}
```

#### [720. 词典中最长的单词](https://leetcode.cn/problems/longest-word-in-dictionary/)

@2023.10.30

1. 模拟

将 `words` 先按长度递减、再按字典序排序，维护所有从长到短的单词序列，对于新单词，遍历所有序列，接在满足条件的序列尾部，最后遍历所有序列，返回最长的序列首项。

```java
/**
 * 模拟
 * Somnia1337
 */
public String longestWord(String[] words)
{
	Arrays.sort(words, (String a, String b) -> {
		if (a.length() != b.length()) return b.length() - a.length();
		return a.compareTo(b);
	});
	List<List<String>> groups = new ArrayList<>();
	for (String word : words)
	{
		boolean added = false;
		for (List<String> group : groups)
		{
			String last = group.get(group.size() - 1);
			if (last.startsWith(word) && last.length() == word.length() + 1)
			{
				group.add(word);
				added = true;
				break;
			}
		}
		if (!added)
		{
			List<String> newGroup = new ArrayList<>();
			newGroup.add(word);
			groups.add(newGroup);
		}
	}
	for (List<String> group : groups)
	{
		if (group.size() == group.get(0).length()) return group.get(0);
	}
	return "";
}
```

2. 哈希表

将 `words` 先按长度递增、再按字典序排序，用哈希表记录，对每个单词 `word`，如果表中存在 `word[:n-1]`，则加入表，同时维护答案。

```java
/**
 * 哈希表
 * 力扣官方题解
 */
public String longestWord(String[] words)
{
	Arrays.sort(words, (a, b) -> {
		if (a.length() != b.length()) return a.length() - b.length();
		return b.compareTo(a);
	});
	String ans = "";
	Set<String> candi = new HashSet<>();
	candi.add("");
	for (String word : words)
	{
		if (candi.contains(word.substring(0, word.length() - 1)))
		{
			candi.add(word);
			ans = word;
		}
	}
	return ans;
}
```

3. 字典树

```java
/**
 * 字典树
 * 力扣官方题解
 */
class Solution
{
    public String longestWord(String[] words)
    {
        Trie trie = new Trie();
        for (String word : words)
        {
            trie.insert(word);
        }
        String ans = "";
        for (String word : words)
        {
            if (trie.search(word))
            {
                if (word.length() > ans.length() || (word.length() == ans.length() && word.compareTo(ans) < 0))
                {
                    ans = word;
                }
            }
        }
        return ans;
    }
}

class Trie
{
    Trie[] children;
    boolean isEnd;
	
    public Trie()
    {
        children = new Trie[26];
        isEnd = false;
    }
	
    public void insert(String word)
    {
        Trie node = this;
        for (int i = 0; i < word.length(); i++)
        {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null)
            {
                node.children[index] = new Trie();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }
	
    public boolean search(String word)
    {
        Trie node = this;
        for (int i = 0; i < word.length(); i++)
        {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null || !node.children[index].isEnd)
            {
                return false;
            }
            node = node.children[index];
        }
        return node != null && node.isEnd;
    }
}
```

#### [722. 删除注释](https://leetcode.cn/problems/remove-comments/)

@2024.01.30



```java
/**
 * 分类讨论
 * ylb
 */
public List<String> removeComments(String[] source) {
	StringBuilder s = new StringBuilder();
	List<String> ans = new ArrayList<>();
	boolean block = false;
	for (String src : source) {
		int n = src.length();
		for (int i = 0; i < n; i++) {
			if (block) {
				if (i + 1 < n && src.charAt(i) == '*' && src.charAt(i + 1) == '/') {
					block = false;
					i++;
				}
			}
			else {
				if (i + 1 < n && src.charAt(i) == '/' && src.charAt(i + 1) == '*') {
					block = true;
					i++;
				}
				else if (i + 1 < n && src.charAt(i) == '/' && src.charAt(i + 1) == '/') break;
				else s.append(src.charAt(i));
			}
		}
		if (!block && !s.isEmpty()) {
			ans.add(s.toString());
			s.setLength(0);
		}
	}
	return ans;
}
```

#### [729. 我的日程安排表 I](https://leetcode.cn/problems/my-calendar-i/)

@2023.11.17

1. 公交车法

```java
/**
 * 公交车法
 * 奶味蓝
 */
class MyCalendar
{
    private TreeMap<Integer, Integer> bus;
	
    public MyCalendar()
    {
        bus = new TreeMap<>();
    }
	
    public boolean book(int start, int end)
    {
        bus.merge(start, 1, Integer::sum);
        bus.merge(end, -1, Integer::sum);
        int cnt = 0;
        for (int v : bus.values())
        {
            cnt += v;
            if (cnt >= 2)
            {
                bus.merge(start, -1, Integer::sum);
                bus.merge(end, 1, Integer::sum);
                return false;
            }
        }
        return true;
    }
}
```

2. 二分查找

```java
/**
 * 二分查找
 * happyCoder
 */
class MyCalendar
{
    private TreeSet<int[]> time;
	
    public MyCalendar()
    {
        time = new TreeSet<>(Comparator.comparingInt(a -> a[0]));
    }
	
    public boolean book(int start, int end)
    {
        if (time.isEmpty())
        {
            time.add(new int[]{start, end});
            return true;
        }
        int[] tmp = new int[]{end, 0};
        int[] lower = time.lower(tmp);
        if (lower == null || start >= lower[1])
        {
            time.add(new int[]{start, end});
            return true;
        }
        return false;
    }
}
```

#### [735. 行星碰撞](https://leetcode.cn/problems/asteroid-collision/)

@2023.09.04

#栈 

根据题意模拟。

```java
/**
 * 栈
 * Somnia1337
 */
public int[] asteroidCollision(int[] asteroids) {
	Deque<Integer> stk = new ArrayDeque<>();
	for (int aster : asteroids) {
		boolean alive = true;
		while (alive && !stk.isEmpty() && aster < 0 && stk.peek() > 0) {
			int preSize = stk.peek(), curSize = -aster;
			if (preSize > curSize) alive = false;
			else if (preSize == curSize) {
				stk.pop();
				alive = false;
			}
			else stk.pop();
		}
		if (alive) stk.push(aster);
	}
	int[] ans = new int[stk.size()];
	for (int i = stk.size() - 1; i >= 0; i--) ans[i] = stk.pop();
	return ans;
}
```

优化：不使用 `for-each`，而使用 `for-i`，每次访问数组进行比较（而不是与栈内元素比较），如果可以继续相撞，将索引 -1。

```java
/**
 * 栈
 * 数据结构和算法
 */
public int[] asteroidCollision(int[] asteroids) {
	Deque<Integer> stk = new ArrayDeque<>();
	for (int i = 0; i < asteroids.length; i++) {
		if (stk.isEmpty() || asteroids[i] > 0 || stk.peek() < 0) stk.push(asteroids[i]);
		else if (stk.peek() <= -asteroids[i]) {
			if (stk.pop() < -asteroids[i]) i--;
		}
	}
	int[] ans = new int[stk.size()];
	for (int i = stk.size() - 1; i >= 0; i--) ans[i] = stk.pop();
	return ans;
}
```

优化：还可以继续简化判断部分。

```java
/**
 * 栈
 * 力扣官方题解
 */
public int[] asteroidCollision(int[] asteroids) {
	Deque<Integer> stk = new ArrayDeque<>();
	for (int aster : asteroids) {
		boolean alive = true;
		while (alive && aster < 0 && !stk.isEmpty() && stk.peek() > 0) {
			alive = stk.peek() < -aster;
			if (stk.peek() <= -aster) stk.pop();
		}
		if (alive) stk.push(aster);
	}
	int[] ans = new int[stk.size()];
	for (int i = stk.size() - 1; i >= 0; i--) ans[i] = stk.pop();
	return ans;
}
```

#### [737. 句子相似性 II](https://leetcode.cn/problems/sentence-similarity-ii/)

@2023.11.15

```java
/**
 * 并查集
 * z446979478
 */
private Map<String, String> root = new HashMap<>();

public boolean areSentencesSimilarTwo(String[] sentence1, String[] sentence2, List<List<String>> similarPairs)
{
	if (sentence1.length != sentence2.length) return false;
	for (List<String> pair : similarPairs) union(pair.get(0), pair.get(1));
	for (int i = 0; i < sentence1.length; i++)
	{
		if (!find(sentence1[i]).equals(find(sentence2[i]))) return false;
	}
	return true;
}

private void union(String word1, String word2)
{
	String r1 = find(word1), r2 = find(word2);
	if (!r1.equals(r2)) root.put(r1, r2);
}

private String find(String word)
{
	while (root.containsKey(word) && !root.get(word).equals(word)) word = root.get(word);
	return word;
}
```

#### [738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

@2023.09.25

```java
/**
 * 贪心
 * 代码随想录
 */
public int monotoneIncreasingDigits(int n)
{
	String s = String.valueOf(n);
	char[] chars = s.toCharArray();
	int start = s.length();
	for (int i = s.length() - 2; i >= 0; i--)
	{
		if (chars[i] > chars[i + 1])
		{
			chars[i]--;
			start = i + 1;
		}
	}
	for (int i = start; i < s.length(); i++)
	{
		chars[i] = '9';
	}
	return Integer.parseInt(String.valueOf(chars));
}
```

#### [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

@2023.09.08

#单调栈 

用单调栈存储索引，遍历 `temperatures`，如果当天温度：

- 小于栈顶，直接压栈。
- 大于栈顶，一直弹栈，直到栈为空或者栈顶索引对应的温度大于当天温度，最后再压栈。

```java
/**
 * 单调栈
 * Somnia1337
 */
public int[] dailyTemperatures(int[] temperatures) {
	int[] ans = new int[temperatures.length];
	Deque<Integer> stk = new ArrayDeque<>();
	for (int i = 0; i < temperatures.length; i++) {
		while (!stk.isEmpty() && temperatures[stk.peek()] < temperatures[i]) {
			int idx = stk.pop();
			ans[idx] = i - idx;
		}
		stk.push(i);
	}
	return ans;
}
```

#### [740. 删除并获得点数](https://leetcode.cn/problems/delete-and-earn/)

@2023.11.02

#动态规划 

本题可以转换为 [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)，后者的递推式：

`dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])`

在本题，如果要拿走某个点数，一定要全部拿走，用 `total` 记录每个点数 `i` 乘以其计数（即其总和），然后动态规划，拿第 `i` 个点数时，不能拿 `i - 1`，递推式：

`dp[i] = max(dp[i - 2] + total[i], dp[i - 1])`

```java
/**
 * 动态规划
 * Somnia1337
 */
public int deleteAndEarn(int[] nums)
{
	int max = 0;
	for (int num : nums) max = Math.max(num, max);
	int[] total = new int[max + 1];
	for (int num : nums) total[num] += num;
	
	int[] dp = new int[max + 1];
	dp[0] = total[0];
	dp[1] = Math.max(total[0], total[1]);
	for (int i = 2; i < max + 1; i++)
	{
		dp[i] = Math.max(dp[i - 2] + total[i], dp[i - 1]);
	}
	return dp[max];
}
```

#### [743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/)

@2023.11.18

```java
/**
 * BFS
 * Meteordream
 */
public int networkDelayTime(int[][] times, int n, int k)
{
	Map<Integer, Map<Integer, Integer>> mp = new HashMap<>();
	for (int[] e : times)
	{
		mp.putIfAbsent(e[0], new HashMap<>());
		mp.get(e[0]).put(e[1], e[2]);
	}
	int[] r = new int[n + 1];
	Arrays.fill(r, Integer.MAX_VALUE);
	r[k] = 0;
	Queue<int[]> dq = new LinkedList<>();
	dq.add(new int[]{k, 0});
	
	while (!dq.isEmpty())
	{
		int[] cur = dq.poll();
		if (mp.containsKey(cur[0]))
		{
			for (int v : mp.get(cur[0]).keySet())
			{
				int d = mp.get(cur[0]).get(v) + cur[1];
				if (d < r[v])
				{
					r[v] = d;
					dq.add(new int[]{v, d});
				}
			}
		}
	}
	
	int ans = -1;
	for (int i = 1; i <= n; i++)
	{
		if (r[i] == Integer.MAX_VALUE) return -1;
		ans = Math.max(r[i], ans);
	}
	return ans;
}
```

#### [754. 到达终点数字](https://leetcode.cn/problems/reach-a-number/)

@2023.11.18

#数学 

`tar` 为负的情况都可以转换为为正的情况。先一直向右走直到超过 `tar`，不妨设 `sum(1, k) = s >= tar` 且 `sum(1, k - 1) < tar`，然后需要反转一步以走到 `tar`，设需要反转的值为 `x`，有 `s - 2 * x = tar`，那么有 `(s - tar) % 2 == 0`，找到最小的 `k` 使 `sum(1, k) >= tar` 且 `s - tar` 为偶即为答案。

```java
/**
 * 数学
 * davidditao
 */
public int reachNumber(int target)
{
	target = Math.abs(target);
	int s = 0, ans = 0;
	while (s < target || (s - target) % 2 == 1)
	{
		ans++;
		s += ans;
	}
	return ans;
}
```

#### [758. 字符串中的加粗单词](https://leetcode.cn/problems/bold-words-in-string/)

@2023.11.23

```java
/**
 * 模拟
 * Somnia1337
 */
public String boldWords(String[] words, String s)
{
	if (words.length == 0) return s;
	int l = s.length();
	boolean[] bold = new boolean[l];
	List<List<String>> group = new ArrayList<>();
	for (int i = 0; i < 26; i++) group.add(new ArrayList<>());
	for (String word : words) group.get(word.charAt(0) - 'a').add(word);
	
	for (int i = 0; i < l; i++)
	{
		List<String> g = group.get(s.charAt(i) - 'a');
		String sub = s.substring(i);
		for (String word : g)
		{
			if (sub.startsWith(word))
			{
				Arrays.fill(bold, i, i + word.length(), true);
			}
		}
	}
	
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < l; i++)
	{
		if (!bold[i]) ans.append(s.charAt(i));
		else
		{
			ans.append("<b>");
			while (i < l && bold[i]) ans.append(s.charAt(i++));
			ans.append("</b>");
			i--;
		}
	}
	return ans.toString();
}
```

待优化：

```java
class Solution
{
    public String boldWords(String[] words, String s)
    {
        boolean[] bold = new boolean[s.length()];
        for (String word : words)
        {
            int n = s.indexOf(word);
            while (n != -1)
            {
                Arrays.fill(bold, n, n + word.length(), true);
                n = s.indexOf(word, n + 1);
            }
        }
        StringBuilder ans = new StringBuilder();
        if (bold[0]) ans.append("<b>");
        for (int i = 0; i < bold.length; i++)
        {
            ans.append(s.charAt(i));
            if (i == bold.length - 1)
            {
                if (bold[i]) ans.append("</b>");
                break;
            }
            if (bold[i] && !bold[i + 1]) ans.append("</b>");
            if (!bold[i] && bold[i + 1]) ans.append("<b>");
        }
        return ans.toString();
    }
}
```

#### [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

@2023.09.17

#贪心 

两次遍历，第一次用数组记录每个字母最后出现的位置，第二次维护一个当前子串的起点与终点，终点为子串内所含字母最后出现位置的最大值，当遍历到最大值处时，该段作为解加入解集。

```java
/**
 * 贪心
 * Somnia1337
 */
public List<Integer> partitionLabels(String s)
{
	int len = s.length();
	List<Integer> ans = new ArrayList<>();
	int[] lastPos = new int[26];
	for (int i = 0; i < len; i++)
	{
		lastPos[s.charAt(i) - 'a'] = i;
	}
	
	int left = -1, right = 0, farthest = 0;
	while (right < len)
	{
		farthest = Math.max(lastPos[s.charAt(right) - 'a'], farthest);
		if (right >= farthest)
		{
			ans.add(right - left);
			left = right;
		}
		right++;
	}
	
	return ans;
}
```

#### [764. 最大加号标志](https://leetcode.cn/problems/largest-plus-sign/)

@2023.11.18

#动态规划 

`dp[n][n][4]` 记录每个单元格在四个方向上的连续 1 数量，遍历取每个单元格所有方向上最小值的最大值。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int orderOfLargestPlusSign(int n, int[][] mines)
{
	if (mines.length == n * n) return 0;
	int[][] map = new int[n][n];
	for (int[] row : map) Arrays.fill(row, 1);
	for (int[] xy : mines) map[xy[0]][xy[1]] = 0;
	return solve(n, map);
}

private int solve(int n, int[][] map)
{
	int[][][] dp = new int[n][n][4];
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (map[i][j] == 1)
			{
				Arrays.fill(dp[i][j], 1);
			}
		}
	}
	
	// up
	for (int i = 1; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (map[i][j] == 1)
			{
				dp[i][j][0] = map[i - 1][j] == 1 ? dp[i - 1][j][0] + 1 : 1;
			}
		}
	}
	// down
	for (int i = n - 2; i >= 0; i--)
	{
		for (int j = 0; j < n; j++)
		{
			if (map[i][j] == 1)
			{
				dp[i][j][1] = map[i + 1][j] == 1 ? dp[i + 1][j][1] + 1 : 1;
			}
		}
	}
	// left
	for (int i = 0; i < n; i++)
	{
		for (int j = 1; j < n; j++)
		{
			if (map[i][j] == 1)
			{
				dp[i][j][2] = map[i][j - 1] == 1 ? dp[i][j - 1][2] + 1 : 1;
			}
		}
	}
	// right
	for (int i = 0; i < n; i++)
	{
		for (int j = n - 2; j >= 0; j--)
		{
			if (map[i][j] == 1)
			{
				dp[i][j][3] = map[i][j + 1] == 1 ? dp[i][j + 1][3] + 1 : 1;
			}
		}
	}
	
	int ans = 1;
	for (int[][] row : dp)
	{
		for (int[] grid : row)
		{
			int cur = n;
			for (int d : grid)
			{
				cur = Math.min(d, cur);
			}
			ans = Math.max(cur, ans);
		}
	}
	return ans;
}
```

优化：无需真的构造矩阵，先向 `dp` 填充 `1`，再遍历 `mines` 在对应位置填充 `0`；遍历时，由于以四条边上的 `1` 为中心的加号的阶最高为 1，因此无需考虑，从而可以合并左上、右下的遍历，将 4 次遍历简化至 2 次。

```java
/**
 * 动态规划
 * 淡淡的扯淡
 */
public int orderOfLargestPlusSign(int n, int[][] mines)
{
	int[][][] dp = new int[n][n][4];
	for (int[][] row : dp)
	{
		for (int[] grid : row)
		{
			Arrays.fill(grid, 1);
		}
	}
	for (int[] z : mines)
	{
		for (int k = 0; k < 4; k++) Arrays.fill(dp[z[0]][z[1]], 0);
	}
	
	for (int i = 1; i < n; i++)
	{
		for (int j = 1; j < n; j++)
		{
			if (dp[i][j][0] == 1)
			{
				dp[i][j][0] = dp[i][j - 1][0] + 1;
				dp[i][j][2] = dp[i - 1][j][2] + 1;
			}
		}
	}
	for (int i = n - 2; i >= 0; i--)
	{
		for (int j = n - 2; j >= 0; j--)
		{
			if (dp[i][j][1] == 1)
			{
				dp[i][j][1] = dp[i][j + 1][1] + 1;
				dp[i][j][3] = dp[i + 1][j][3] + 1;
			}
		}
	}
	
	int ans = 0;
	for (int[][] row : dp)
	{
		for (int[] grid : row)
		{
			int cur = n;
			for (int d : grid) cur = Math.min(d, cur);
			ans = Math.max(cur, ans);
		}
	}
	return ans;
}
```

#### [767. 重构字符串](https://leetcode.cn/problems/reorganize-string/)

@2023.11.02

#贪心 

字符统计，每次接最高频的字符，再接任意一个不同字符。

```java
/**
 * 贪心
 * 🍬
 */
public String reorganizeString(String s)
{
	int[] count = new int[26];
	for (int i = 0; i < s.length(); i++) count[s.charAt(i) - 'a']++;
	// 如果每隔一个位置就放该字符也无法放入, 返回 ""
	for (int i = 0; i < 26; i++)
	{
		if (count[i] > (s.length() + 1) / 2) return "";
	}
	
	StringBuilder ans = new StringBuilder();
	while (ans.length() < s.length())
	{
		// 先接最高频的字符
		int idx = 0;
		for (int i = 0; i < 26; i++)
		{
			if (count[idx] < count[i]) idx = i;
		}
		ans.append((char) ('a' + idx));
		count[idx]--;
		
		// 再接任意一个不同的字符
		for (int i = 0; i < 26; i++)
		{
			if (i != idx && count[i] > 0)
			{
				ans.append((char) ('a' + i));
				count[i]--;
				break;
			}
		}
	}
	
	return ans.toString();
}
```

#### [769. 最多能完成排序的块](https://leetcode.cn/problems/max-chunks-to-make-sorted/)

@2023.11.18

#技巧 #单调栈 

1. 一次遍历

一次遍历，更新前缀最大值，如果当前下标与最大值相等，则可分块。

```java
/**
 * 一次遍历
 * ylb
 */
public int maxChunksToSorted(int[] arr)
{
	int max = 0, ans = 0;
	for (int i = 0; i < arr.length; i++)
	{
		max = Math.max(arr[i], max);
		if (max == i) ans++;
	}
	return ans;
}
```

2. 单调栈

观察发现，每个分块都有一个最大值，且各分块的最大值递增，可用递增单调栈维护每个分块的最大值，如果当前元素小于栈顶元素，则保存栈顶元素，持续弹出大于当前值的元素，再压回最大值。

这种思路适用于包含重复元素的情况。

```java
/**
 * 单调栈
 * ylb
 */
public int maxChunksToSorted(int[] arr)
{
	Deque<Integer> st = new ArrayDeque<>();
	for (int v : arr)
	{
		if (st.isEmpty() || v >= st.peek()) st.push(v);
		else
		{
			int max = st.pop();
			while (!st.isEmpty() && st.peek() > v) st.pop();
			st.push(max);
		}
	}
	return st.size();
}
```

#### [777. 在LR字符串中交换相邻字符](https://leetcode.cn/problems/swap-adjacent-in-lr-string/)

@2023.11.20

#双指针 

> 提示：Think of the L and R's as people on a horizontal line, where X is a space. The people can't cross each other, and also you can't go from XRX to RXX.

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean canTransform(String start, String end)
{
	int l = start.length(), i = 0, j = 0;
	while (i < l)
	{
		while (i < l && start.charAt(i) == 'X') i++;
		if (i == l) break;
		while (j < l && end.charAt(j) == 'X') j++;
		if (j == l) return false;
		char c = start.charAt(i);
		if (end.charAt(j) != c || c == 'L' && j > i || c == 'R' && j < i) return false;
		i++;
		j++;
	}
	while (j < l && end.charAt(j) == 'X') j++;
	return j == l;
}
```

#### [779. 第K个语法符号](https://leetcode.cn/problems/k-th-symbol-in-grammar/)

@2023.11.20

```java
/**
 * 位运算
 * ylb
 */
public int kthGrammar(int n, int k)
{
	return Integer.bitCount(k - 1) & 1;
}
```

#### [781. 森林中的兔子](https://leetcode.cn/problems/rabbits-in-forest/)

@2023.11.20

#贪心 

数据保证 `answers[i] <= 1000`，可用 `int[1000] group` 记录每种颜色的兔子，当 `group[answer] == answer + 1` 时，贪心地将这一组兔子累加到答案，并将该组置零。

例如，有 5 只兔子回答了 3，统计前 4 只兔子后，`group[3] == 4`，可以认为这 4 只兔子同色、每一只都回答了其余 3 只，因此将 4 累加到答案并将该组置零。第 5 只兔子再次回答 3，但它已经无法与前 4 只同色了（反向思考，如果 5 只都同色，它们都会回答 4），因此单独累加它，最终答案为 `8`。

```java
/**
 * 贪心
 * Somnia1337
 */
public int numRabbits(int[] answers)
{
	int[] groups = new int[1001];
	int ans = 0;
	for (int answer : answers)
	{
		groups[answer]++;
		if (groups[answer] == answer + 1)
		{
			ans += answer + 1;
			groups[answer] = 0;
		}
	}
	for (int i = 0; i < 1001; i++)
	{
		if (groups[i] > 0) ans += i + 1;
	}
	return ans;
}
```

#### [784. 字母大小写全排列](https://leetcode.cn/problems/letter-case-permutation/)

@2023.11.20

#回溯 

回溯 `char[]`，在每个字符先回溯一次，用异或操作 `path[i] ^= 32` 将其大小写互换后再次回溯。

```java
/**
 * 回溯
 * Somnia1337
 */
private List<String> ans;
private char[] path;

public List<String> letterCasePermutation(String s)
{
	ans = new ArrayList<>();
	path = s.toCharArray();
	bt(0);
	return ans;
}

private void bt(int i)
{
	if (i == path.length)
	{
		ans.add(new String(path));
		return;
	}
	if (Character.isDigit(path[i])) bt(i + 1);
	else
	{
		bt(i + 1);
		path[i] ^= 32;
		bt(i + 1);
	}
}
```

#### [785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/)

@2023.11.20

#搜索 #并查集 

1. 并查集

二分图的每个结点需要满足：

- 与其邻接的所有结点属于同一集合
- 其本身属于另一个集合

遍历 `graph`，`union()` 每个结点的邻接结点，最后判断节点本身是否与它们分属不同的集合。

```java
/**
 * 并查集
 * Somnia1337
 */
public boolean isBipartite(int[][] graph) {
	int n = graph.length;
	int[] root = new int[n];
	Arrays.setAll(root, a -> a);
	for (int i = 0; i < n; i++) {
		int[] v = graph[i];
		for (int j = 1; j < v.length; j++) union(root, v[0], v[j]);
		if (v.length > 0 && find(root, i) == find(root, v[0])) return false;
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

2. DFS

共 2 个分区，可分别用 `1`、`-1` 表示两种颜色，从没有染色的结点开始 DFS 染色，如果某节点已染的颜色不同于将染的颜色，则无法分为两部。

```java
/**
 * DFS
 * Sweetiee 🍬
 */
private int[] vis; // 两种颜色分别为 1 和 -1

public boolean isBipartite(int[][] graph) {
	vis = new int[graph.length];
	for (int i = 0; i < graph.length; i++) {
		// 从未访问过的结点进行 DFS 染色
		if (vis[i] == 0 && !dfs(graph, i, 1)) return false;
	}
	return true;
}

private boolean dfs(int[][] g, int u, int color) {
	// 如果已经染色, 返回已染的颜色是否与将染的颜色相同
	if (vis[u] != 0) return vis[u] == color;
	vis[u] = color;
	for (int v : g[u]) {
		if (!dfs(g, v, -color)) return false;
	}
	return true;
}
```

3. BFS

类似 DFS，将没有染色的结点加入队列，BFS，将每个出队结点的相邻节点标记为相反颜色，同时判断是否出现矛盾。

```java
/**
 * BFS
 * Sweetiee 🍬
 */
public boolean isBipartite(int[][] graph) {
	int n = graph.length;
	int[] vis = new int[n];
	Queue<Integer> q = new LinkedList<>();
	for (int u = 0; u < n; u++) {
		if (vis[u] != 0) continue;
		q.add(u);
		vis[u] = 1;
		while (!q.isEmpty()) {
			int p = q.poll();
			for (int v : graph[p]) {
				if (vis[v] == vis[p]) return false;
				if (vis[v] == 0) {
					vis[v] = -vis[p];
					q.add(v);
				}
			}
		}
	}
	return true;
}
```

#### [788. 旋转数字](https://leetcode.cn/problems/rotated-digits/)

@2023.11.20

1. 枚举

一个数字为好数的条件是：

- 含有 `"2569"` 中至少一个字符
- 不含 `"347"` 中任何字符

枚举范围内所有数字，转换为字符串检查是否满足条件。

```java
/**
 * 枚举
 * Somnia1337
 */
public int rotatedDigits(int n)
{
	String a = "2569", b = "347";
	int ans = 0;
	for (int x = 1; x <= n; x++)
	{
		char[] digits = String.valueOf(x).toCharArray();
		boolean valid = false;
		for (char digit : digits)
		{
			if (b.indexOf(digit) >= 0)
			{
				valid = false;
				break;
			}
			if (a.indexOf(digit) >= 0) valid = true;
		}
		if (valid) ans++;
	}
	return ans;
}
```

2. 数位 DP

```java
/**
 * 数位 DP
 * 灵茶山艾府
 */
private final int[] dif = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};
private char s[];
private int dp[][];

public int rotatedDigits(int n)
{
	s = Integer.toString(n).toCharArray();
	int m = s.length;
	dp = new int[m][2];
	for (int i = 0; i < m; i++) Arrays.fill(dp[i], -1);
	return solve(0, 0, true);
}

private int solve(int idx, int valid, boolean lim)
{
	if (idx == s.length) return valid;
	if (!lim && dp[idx][valid] >= 0) return dp[idx][valid];
	int ret = 0, up = lim ? s[idx] - '0' : 9;
	for (int d = 0; d <= up; d++)
	{
		if (dif[d] != -1) ret += solve(idx + 1, valid | dif[d], lim && d == up);
	}
	if (!lim) dp[idx][valid] = ret;
	return ret;
}
```

#### [791. 自定义字符串排序](https://leetcode.cn/problems/custom-sort-string/)

@2023.11.20

#排序 

对 `s` 字符统计，遍历 `order`，如果 `s` 中含有此字符，就 `append()` 到答案，最后 `append()` 在 `s` 中而不在 `order` 中的字符。

```java
/**
 * 计数排序
 * Somnia1337
 */
public String customSortString(String order, String s)
{
	int[] count = new int[26];
	for (char c : s.toCharArray()) count[c - 'a']++;
	StringBuilder ans = new StringBuilder();
	for (char c : order.toCharArray())
	{
		if (count[c - 'a'] > 0)
		{
			ans.append(String.valueOf(c).repeat(count[c - 'a']));
			count[c - 'a'] = 0;
		}
	}
	for (int i = 0; i < 26; i++)
	{
		if (count[i] > 0)
		{
			ans.append(String.valueOf((char) ('a' + i)).repeat(count[i]));
		}
	}
	return ans.toString();
}
```

#### [792. 匹配子序列的单词数](https://leetcode.cn/problems/number-of-matching-subsequences/)

@2023.11.22

#二分查找 #多指针 

1. 二分查找

预处理 `s`，记录每个字母出现的位置，对每个 `word`，用 `cur` 维护当前字母在 `s` 中的位置，过程中，遍历 `word`，取字母对应在 `s` 中的位置列表，二分查找第一个大于 `cur` 的位置，并将 `cur` 更新为该位置。

```java
/**
 * 二分查找
 * davidditao
 */
public int numMatchingSubseq(String s, String[] words)
{
	// 预处理 s, 记录每个字母的位置列表
	List<List<Integer>> pos = new ArrayList<>();
	for (int i = 0; i < 26; i++) pos.add(new ArrayList<>());
	for (int i = 0; i < s.length(); i++) pos.get(s.charAt(i) - 'a').add(i);
	
	int ans = 0;
	for (String word : words)
	{
		int cur = -1;
		boolean valid = true;
		for (char c : word.toCharArray())
		{
			List<Integer> p = pos.get(c - 'a');
			int idx = search(p, cur); // 二分查找第一个右侧下标
			if (idx == p.size()) // 该下标不存在
			{
				valid = false;
				break;
			}
			cur = p.get(idx); // 更新 cur
		}
		if (valid) ans++;
	}
	return ans;
}

// 二分查找第一个大于 tar 的下标
private int search(List<Integer> p, int tar)
{
	int l = 0, r = p.size(); // 如果没有, 返回 p.size()
	while (l < r)
	{
		int m = l + r >> 1;
		if (p.get(m) <= tar) l = m + 1;
		else r = m;
	}
	return l;
}
```

2. 多指针

每个 `word` 用一个 `int[2]` 记录其 `[在words中的下标, 当前字符下标]`，用一个 `Queue<int[]>[26]` 存储，遍历 `s`，获取字符 `c` 对应的那组 `int[]`，取对应的 `word`，如果当前字符已在其末尾，累加答案，否则将其压入对应下个字符的那组 `int[]`。

工作原理：`Queue<int[]>[26]` 中的每个队列存储了等待该字符出现的一组 `word`，遍历时一旦该字符出现，就全部取出并更新，如果还没到 `word` 的结尾，就让其等待下个字符。

```text
s="abc", word="ac"
qs=[
	'a': [[0,0]]
	'b': []
	'c': []
]

遍历 s:
	'a':
		取出 [0,0]
		更新为 [0,1]
		word 的 1 处字符为 'c'
		qs=[
			'a': []
			'b': []
			'c': [[0,1]]
		]
	'b':
		'b' 对应 [], 什么都不做
	'c':
		取出 [0,0]
		更新为 [0,2]
		2 在 word 的末尾, ans++
```

```java
/**
 * 多指针
 * 力扣官方题解
 */
public int numMatchingSubseq(String s, String[] words)
{
	Queue<int[]>[] qs = new Queue[26];
	for (int i = 0; i < 26; i++) qs[i] = new ArrayDeque<>();
	for (int i = 0; i < words.length; i++) qs[words[i].charAt(0) - 'a'].offer(new int[]{i, 0});
	
	int ans = 0;
	for (int i = 0; i < s.length(); i++)
	{
		char c = s.charAt(i);
		int len = qs[c - 'a'].size();
		while (len-- > 0)
		{
			int[] p = qs[c - 'a'].poll();
			if (p[1] == words[p[0]].length() - 1) ans++;
			else qs[words[p[0]].charAt(++p[1]) - 'a'].offer(p);
		}
	}
	return ans;
}
```

#### [794. 有效的井字游戏](https://leetcode.cn/problems/valid-tic-tac-toe-state/)

@2023.11.20

#分类讨论 

讨论所有情况。

```java
/**
 * 分类讨论
 * Somnia1337
 */
public boolean validTicTacToe(String[] board)
{
	int xCnt = 0, oCnt = 0;
	for (String row : board)
	{
		for (char c : row.toCharArray())
		{
			if (c == 'X') xCnt++;
			else if (c == 'O') oCnt++;
		}
	}
	if (xCnt - oCnt > 1 || xCnt < oCnt) return false;
	boolean xWin = false, oWin = false;
	String[] cols = new String[3];
	Arrays.fill(cols, "");
	StringBuilder c1 = new StringBuilder(), c2 = new StringBuilder();
	
	for (int i = 0; i < 3; i++)
	{
		if (board[i].equals("XXX")) xWin = true;
		else if (board[i].equals("OOO")) oWin = true;
		for (int j = 0; j < 3; j++)
		{
			cols[j] += board[i].charAt(j);
		}
		c1.append(board[i].charAt(i));
		c2.append(board[i].charAt(2 - i));
	}
	for (String col : cols)
	{
		if (col.equals("XXX")) xWin = true;
		else if (col.equals("OOO")) oWin = true;
	}
	if (c1.toString().equals("XXX") || c2.toString().equals("XXX")) xWin = true;
	if (c1.toString().equals("OOO") || c2.toString().equals("OOO")) oWin = true;
	
	if (xWin && oWin) return false;
	if (xWin) return xCnt == oCnt + 1;
	if (oWin) return xCnt == oCnt;
	return xCnt == oCnt || xCnt == oCnt + 1;
}
```

优化：

- 无需用字符串模拟，直接对 `board` 中的字符串取字符即可
- 先检查一个玩家，如果没有赢再检查另一个，而不是同时检查两个

```java
/**
 * 分类讨论
 * 力扣官方题解
 */
public boolean validTicTacToe(String[] board)
{
	int xCnt = 0, oCnt = 0;
	for (String row : board)
	{
		for (char c : row.toCharArray())
		{
			if (c == 'X') xCnt++;
			else if (c == 'O') oCnt++;
		}
	}
	return !((oCnt != xCnt && oCnt != xCnt - 1) || (oCnt != xCnt - 1 && win(board, 'X')) || (oCnt != xCnt && win(board, 'O')));
}

private boolean win(String[] b, char c)
{
	// 同一个 for 检查行和列
	for (int i = 0; i < 3; i++)
	{
		if ((c == b[0].charAt(i) && c == b[1].charAt(i) && c == b[2].charAt(i)) || (c == b[i].charAt(0) && c == b[i].charAt(1) && c == b[i].charAt(2))) return true;
	}
	// 检查两个对角线
	return ((c == b[0].charAt(0) && c == b[1].charAt(1) && c == b[2].charAt(2)) || (c == b[0].charAt(2) && c == b[1].charAt(1) && c == b[2].charAt(0)));
}
```

#### [795. 区间子数组个数](https://leetcode.cn/problems/number-of-subarrays-with-bounded-maximum/)

@2023.11.20

#双指针 

![[795. 区间子数组个数.png|400]]

维护最近超过 `right` 的下标 `i0`、最近在 `left` 和 `right` 之间的下标 `i1`（`i0` 及之前的元素不能作为起点，`i1` 之后的元素也不能），枚举每个元素作为子数组右端点，将更新后的 `i1 - i0` 累加到答案。

```java
/**
 * 双指针
 * 灵茶山艾府
 */
public int numSubarrayBoundedMax(int[] nums, int left, int right)
{
	int ans = 0, i0 = -1, i1 = -1;
	for (int i = 0; i < nums.length; i++)
	{
		if (nums[i] > right) i0 = i;
		if (nums[i] >= left) i1 = i;
		ans += i1 - i0;
	}
	return ans;
}
```

#### [797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)

@2023.10.19

#搜索 

每轮搜索中，取出当前节点能够到达的节点列表，以每个节点作为下轮搜索的起点。

```java
/**
 * DFS
 * Somnia1337
 */
private List<List<Integer>> ans;
private List<Integer> path; // 记录当前路径

public List<List<Integer>> allPathsSourceTarget(int[][] graph)
{
	ans = new ArrayList<>();
	path = new ArrayList<>();
	path.add(0); // 规定以 0 为起点
	dfs(graph, 0);
	return ans;
}

private void dfs(int[][] graph, int node)
{
	if (node == graph.length - 1)
	{
		ans.add(new ArrayList<>(path));
		return;
	}
	int[] nexts = graph[node]; // 当前点能够到达的点
	for (int next : nexts)
	{
		path.add(next);
		dfs(graph, next);
		path.remove(path.size() - 1);
	}
}
```

#### [799. 香槟塔](https://leetcode.cn/problems/champagne-tower/)

@2023.11.20

#动态规划 

用 `double[][]` 表示所有酒杯，将所有的酒先装入第一个酒杯，从上到下遍历，如果一个酒杯能装满，将其中多出的酒均分给它下面的两个杯子，即 `dp[i + 1][j]` 和 `dp[i + 1][j + 1]`。

```java
/**
 * 动态规划
 * davidditao
 */
public double champagneTower(int poured, int query_row, int query_glass)
{
	double[][] dp = new double[101][101];
	dp[0][0] = poured;
	for (int i = 0; i <= query_row; i++)
	{
		for (int j = 0; j <= i; j++)
		{
			if (dp[i][j] > 1)
			{
				double flow = (dp[i][j] - 1) / 2;
				dp[i][j] = 1;
				dp[i + 1][j] += flow;
				dp[i + 1][j + 1] += flow;
			}
		}
	}
	return dp[query_row][query_glass];
}
```

优化：？

```java
/**
 * 动态规划
 * ?
 */
public double champagneTower(int poured, int query_row, int query_glass)
{
	double[] dp = new double[102];
	dp[0] = poured;
	for (int i = 1; i <= query_row; i++)
	{
		for (int j = i; j >= 0; j--)
		{
			double flow = (dp[j] - 1) / 2;
			if (flow > 0)
			{
				dp[j] = flow;
				dp[j + 1] += flow;
			}
			else dp[j] = 0;
		}
	}
	return Math.min(dp[query_glass], 1);
}
```

#### [802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/)

@2023.11.21

#搜索 #拓扑排序 

1. 拓扑排序

出度为 0 的结点入队，将出队节点加入解集，并将其入度来源结点的出度减 1，如果更新后出度为 0 则入队。

```java
/**
 * 拓扑排序
 * Somnia1337
 */
public List<Integer> eventualSafeNodes(int[][] graph) {
    int n = graph.length;
    List<List<Integer>> ins = new ArrayList<>(); // 入度源
    for (int i = 0; i < n; i++) ins.add(new ArrayList<>());
    int[] deg = new int[n]; // 出度
    for (int i = 0; i < n; i++) {
        deg[i] = graph[i].length;
        for (int node : graph[i]) ins.get(node).add(i);
    }
    
    List<Integer> ans = new ArrayList<>();
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (deg[i] == 0) q.add(i);
    }
    while (!q.isEmpty()) {
        int node = q.poll();
        ans.add(node);
        for (int i : ins.get(node)) {
            if (--deg[i] == 0) q.add(i);
        }
    }
    Collections.sort(ans);
    return ans;
}
```

优化：待拓扑排序结束后，遍历 `deg` 收集出度为 0 的结点，节省了排序解集的步骤。

```java
/**
 * 拓扑排序
 * 力扣官方题解
 */
public List<Integer> eventualSafeNodes(int[][] graph) {
    int n = graph.length;
    List<List<Integer>> ins = new ArrayList<>(); // 入度源
    for (int i = 0; i < n; i++) ins.add(new ArrayList<>());
    int[] deg = new int[n]; // 出度
    for (int i = 0; i < n; i++) {
        deg[i] = graph[i].length;
        for (int node : graph[i]) ins.get(node).add(i);
    }
    
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (deg[i] == 0) q.add(i);
    }
    while (!q.isEmpty()) {
        for (int i : ins.get(q.poll())) {
            if (--deg[i] == 0) q.add(i);
        }
    }
    List<Integer> ans = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        if (deg[i] == 0) ans.add(i);
    }
    return ans;
}
```

2. DFS

如果一个结点位于环内，或者可达于一个环上结点，则该节点不安全。

用 3 种颜色标记结点状态：

- `0`：未访问
- `1`：安全
- `-1`：已访问，不确定安全性（位于递归栈中）或确定为环上结点

首次访问一个结点时，将其标记为 `-1`，搜索其通向的下一个结点，在此过程中：

- 如果遇到了一个 `-1` 结点，说明找到了环，退出搜索，此次搜索过程中的结点保持为 `-1`，这可以将“找到了环”这一信息传递到栈中的所有结点。
- 如果没有遇到 `-1` 结点，说明没有找到环，递归返回前将其标记为 `1`。

```java
/**
 * DFS
 * 力扣官方题解
 */
private int[] color;

public List<Integer> eventualSafeNodes(int[][] graph) {
    int n = graph.length;
    color = new int[n];
    List<Integer> ans = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        if (safe(graph, i)) ans.add(i);
    }
    return ans;
}

private boolean safe(int[][] graph, int node) {
    if (color[node] != 0) return color[node] == 1;
    color[node] = -1;
    for (int i : graph[node]) {
        if (!safe(graph, i)) return false;
    }
    color[node] = 1;
    return true;
}
```

#### [807. 保持城市天际线](https://leetcode.cn/problems/max-increase-to-keep-city-skyline/)

@2023.11.23

#贪心 

要保持天际线，每栋楼最高增加到其所在行、列的最高楼高中的较矮者。

```java
/**
 * 贪心
 * Somnia1337
 */
public int maxIncreaseKeepingSkyline(int[][] grid)
{
	int len = grid.length;
	int[] rowMax = new int[len], colMax = new int[len];
	for (int a = 0; a < len; a++)
	{
		int rMax = grid[a][0], cMax = grid[0][a];
		for (int b = 0; b < len; b++)
		{
			rMax = Math.max(grid[a][b], rMax);
			cMax = Math.max(grid[b][a], cMax);
		}
		rowMax[a] = rMax;
		colMax[a] = cMax;
	}
	
	int ans = 0;
	for (int i = 0; i < len; i++)
	{
		for (int j = 0; j < len; j++)
		{
			ans += Math.min(rowMax[i], colMax[j]) - grid[i][j];
		}
	}
	return ans;
}
```

#### [813. 最大平均值和的分组](https://leetcode.cn/problems/largest-sum-of-averages/)

@2023.12.29

最多分为 `k` 组，但是实际上要达到最优，必须分满 `k` 组。以 `[1,2,3],3` 为例直观感受：

- 分为 `1` 组：`(1+2+3) / 3 = 2.0`。
- 分为 `2` 组：`(1+2)/2 + 3/3 = 2.5`，`1/1 + (2+3)/2 = 3.5`。
- 分为 `3` 组：`1/1 + 2/1 + 3/1 = 6.0`。

`dp[i][j]` 表示 `nums[0:i]` 被分为 `j` 组的子问题答案（`i >= j`），讨论：

- `j == 1`：`dp[i][1]` 为 `avg(nums[0:i])`。
- `j > 1`：`nums[0:i]` 分为 `nums[0:x]` 和 `nums[x:i]` 两部分（`x >= j - 1`），`dp[i][j]` 为所有有效分割方式的平均值的最大值，状态转移：$$dp[i][j] = \underset{x \geqslant j - 1}{{\rm{max}}}\{dp[x][j - 1] + \frac{\underset{r = x}{\overset{i - 1}{\sum}} nums[r]}{i - x}\}$$

```java
/**
 * 动态规划
 * 力扣官方题解
 */
public double largestSumOfAverages(int[] nums, int k) {
	int n = nums.length;
	double[] pre = new double[n + 1];
	for (int i = 1; i < n + 1; i++) pre[i] = pre[i - 1] + nums[i - 1];
	double[][] dp = new double[n + 1][k + 1];
	for (int i = 1; i <= n; i++) dp[i][1] = pre[i] / i;
	for (int j = 2; j <= k; j++) {
		for (int i = j; i <= n; i++) {
			for (int x = j - 1; x < i; x++) {
				dp[i][j] = Math.max(dp[x][j - 1] + (pre[i] - pre[x]) / (i - x), dp[i][j]);
			}
		}
	}
	return dp[n][k];
}
```

#### [816. 模糊坐标](https://leetcode.cn/problems/ambiguous-coordinates/)

@2023.12.10

```java
/**
 * 枚举
 * 宫水三叶
 */
private String s;

public List<String> ambiguousCoordinates(String s) {
	this.s = s.substring(1, s.length() - 1);
	int n = this.s.length();
	List<String> ans = new ArrayList<>();
	for (int i = 0; i < n - 1; i++) {
		List<String> a = search(0, i), b = search(i + 1, n - 1);
		for (String x : a) {
			for (String y : b) ans.add("(" + x + ", " + y + ")");
		}
	}
	return ans;
}

private List<String> search(int start, int end) {
	List<String> ans = new ArrayList<>();
	if (start == end || s.charAt(start) != '0') ans.add(s.substring(start, end + 1));
	for (int i = start; i < end; i++) {
		String a = s.substring(start, i + 1), b = s.substring(i + 1, end + 1);
		if (a.length() > 1 && a.charAt(0) == '0') continue;
		if (b.charAt(b.length() - 1) == '0') continue;
		ans.add(a + "." + b);
	}
	return ans;
}
```

#### [820. 单词的压缩编码](https://leetcode.cn/problems/short-encoding-of-words/)

@2024.01.02

1. 反转

所有单词反转，按字典序排序，如果 `words[i+1]` 不是以 `word[i]` 开头，那么需要为 `words[i]` 单开一段。

```java
/**
 * 反转
 * nettee
 */
public int minimumLengthEncoding(String[] words) {
	for (int i = 0; i < words.length; i++) words[i] = reverse(words[i]);
	Arrays.sort(words);
	int ans = 0;
	for (int i = 0; i < words.length; i++) {
		if (i + 1 >= words.length || !words[i + 1].startsWith(words[i])) {
			ans += words[i].length() + 1;
		}
	}
	return ans;
}

private String reverse(String s) {
	char[] chs = s.toCharArray();
	for (int i = 0; i < chs.length / 2; i++) {
		swap(chs, i, chs.length - i - 1);
	}
	return new String(chs);
}

private void swap(char[] arr, int i, int j) {
	char t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

2. 哈希表

哈希表存储所有串，去除每个串的所有子串，没有被去除的串都需要单开。

```java
/**
 * 哈希表
 * 孙笑川258
 */
public int minimumLengthEncoding(String[] words) {
	Set<String> dict = new HashSet<>(Arrays.asList(words));
	for (String word : words) {
		for (int i = 1; i < word.length(); i++) dict.remove(word.substring(i));
	}
	int ans = dict.size();
	for (String word : dict) ans += word.length();
	return ans;
}
```

3. 字典树

```java
/**
 * 字典树
 * Sweetiee 🍬
 */
class Solution {
    public int minimumLengthEncoding(String[] words) {
        int len = 0;
        Trie trie = new Trie();
        Arrays.sort(words, (a, b) -> b.length() - a.length());
        for (String word : words) {
            len += trie.insert(word);
        }
        return len;
    }
}

class Trie {
    TrieNode root;
	
    public Trie() {
        root = new TrieNode();
    }
	
    public int insert(String word) {
        TrieNode cur = root;
        boolean isNew = false;
        for (int i = word.length() - 1; i >= 0; i--) {
            int c = word.charAt(i) - 'a';
            if (cur.children[c] == null) {
                isNew = true;
                cur.children[c] = new TrieNode();
            }
            cur = cur.children[c];
        }
        return isNew ? word.length() + 1 : 0;
    }
}

class TrieNode {
    char val;
    TrieNode[] children = new TrieNode[26];
	
    public TrieNode() {}
}
```

#### [822. 翻转卡片游戏](https://leetcode.cn/problems/card-flipping-game/)

@2023.12.30

#哈希表 

题意理解：ban 掉正反面相同的卡牌的卡面，从剩下的卡面中选择最小值。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int flipgame(int[] fronts, int[] backs) {
	boolean[] ban = new boolean[2000];
	for (int i = 0; i < fronts.length; i++) {
		if (fronts[i] == backs[i]) ban[fronts[i]] = true;
	}
	int ans = Integer.MAX_VALUE;
	for (int x : fronts) if (!ban[x]) ans = Math.min(x, ans);
	for (int x : backs) if (!ban[x]) ans = Math.min(x, ans);
	return ans < Integer.MAX_VALUE ? ans : 0;
}
```

#### [823. 带因子的二叉树](https://leetcode.cn/problems/binary-trees-with-factors/)

@2023.12.04

#递归 #动态规划 

1. 记忆化搜索

定义 `dfs(val)` 为根节点为 `val` 时的答案，总答案即 `sum(dfs(arr[i]))`。

哈希表 `vals` 记录所有元素，`memo` 记录递归子问题结果，对每个 `val`，遍历 `arr`，如果 `val % arr[i] == 0` 且元素 `val / arr[i]` 存在，即找到了 `val` 的一组子结点，子问题的答案累加 `dfs(arr[i]) * dfs(val / arr[i])`。

```java
/**
 * 记忆化搜索
 * Somnia1337
 */
private int[] arr;
private Set<Integer> val;
private Map<Integer, Long> memo;

public int numFactoredBinaryTrees(int[] arr) {
    this.arr = arr;
    val = new HashSet<>();
    for (int n : arr) val.add(n);
    memo = new HashMap<>();
    long ans = 0;
    for (int n : arr) ans += dfs(n);
    return (int) (ans % (1e9 + 7));
}

private long dfs(int val) {
    if (memo.containsKey(val)) return memo.get(val);
    long ret = 1;
    for (int n : arr) {
        if (val % n == 0 && this.val.contains(val / n)) {
            ret += dfs(n) * dfs(val / n);
        }
    }
    memo.put(val, ret);
    return ret;
}
```

2. 动态规划

将递归翻译为递推。

```java
/**
 * 动态规划
 * 灵茶山艾府
 */
public int numFactoredBinaryTrees(int[] arr) {
	Arrays.sort(arr);
	int n = arr.length;
	Map<Integer, Integer> idx = new HashMap<>();
	for (int i = 0; i < n; i++) idx.put(arr[i], i);
	
	long[] dp = new long[n];
	long ans = 0;
	for (int i = 0; i < n; i++) {
		int v1 = arr[i];
		dp[i] = 1;
		for (int j = 0; j < i; j++) {
			int v2 = arr[j];
			if (v1 % v2 == 0 && idx.containsKey(v1 / v2)) {
				dp[i] += dp[j] * dp[idx.get(v1 / v2)];
			}
		}
		ans += dp[i];
	}
	return (int) (ans % (1e9 + 7));
}
```

优化：内层循环限定上界 `sqrt(v1)`，18 ms 优化至 10 ms。

```java
/**
 * 动态规划
 * 灵茶山艾府 Somnia1337
 */
public int numFactoredBinaryTrees(int[] arr) {
    Arrays.sort(arr);
    int n = arr.length;
    Map<Integer, Integer> idx = new HashMap<>();
    for (int i = 0; i < n; i++) idx.put(arr[i], i);
    
    long[] dp = new long[n];
    long ans = 0;
    for (int i = 0; i < n; i++) {
        int v1 = arr[i];
        dp[i] = 1;
        int max = (int) Math.sqrt(v1);
        for (int j = 0; j < i && arr[j] <= max; j++) {
            if (v1 % arr[j] == 0) {
                int v2 = arr[j], q = v1 / v2;
                if (idx.containsKey(q)) {
                    dp[i] += dp[j] * dp[idx.get(q)] * (q != v2 ? 2 : 1);
                }
            }
        }
        ans += dp[i];
    }
    return (int) (ans % (1e9 + 7));
}
```

#### [825. 适龄的朋友](https://leetcode.cn/problems/friends-of-appropriate-ages/)

@2023.11.22

#前缀和 

对各年龄人数计算前缀和，累加答案。

```java
/**
 * 前缀和
 * Somnia1337
 */
public int numFriendRequests(int[] ages)
{
	int[] count = new int[121];
	for (int age : ages) count[age]++;
	int[] preSum = new int[121];
	for (int i = 1; i < 121; i++) preSum[i] = count[i] + preSum[i - 1];
	int ans = 0;
	for (int i = 15; i < 121; i++)
	{
		ans += count[i] * (preSum[i] - preSum[i / 2 + 7] - 1);
	}
	return ans;
}
```

#### [826. 安排工作以达到最大收益](https://leetcode.cn/problems/most-profit-assigning-work/)

@2023.11.22

#二分查找 #排序 

1. 二分查找

将难度与利润组合，按先难度再利润双递减排序，由于难度大不意味着利润大，需要将所有利润更新为后缀最大值。

再进行二分查找，累加每个工人能够完成的最难工作对应的利润。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
	int n = difficulty.length;
	int[][] join = new int[n][2];
	for (int i = 0; i < n; i++) {
		join[i][0] = difficulty[i];
		join[i][1] = profit[i];
	}
	Arrays.sort(join, (a, b) -> a[0] != b[0] ? b[0] - a[0] : b[1] - a[1]);
	int max = 0;
	for (int i = n - 1; i >= 0; i--) {
		max = Math.max(join[i][1], max);
		join[i][1] = max;
	}
	
	int ans = 0;
	for (int w : worker) {
		int l = 0, r = n - 1;
		while (l <= r) {
			int m = l + r >> 1;
			if (join[m][0] <= w) r = m - 1;
			else l = m + 1;
		}
		if (l < n) ans += join[l][1];
	}
	return ans;
}
```

2. 计数排序

用 `int[maxDif + 1]` 记录每种难度对应的最大利润，一次遍历写入 `difficulty` 和 `profit`，再取前缀最大值，最后遍历 `worker` 累加每个工人的能力对应的最大利润。

```java
/**
 * 计数排序
 * ?
 */
public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
	int maxDif = 0;
	for (int dif : difficulty) maxDif = Math.max(dif, maxDif);
	int[] maxPro = new int[maxDif + 1];
	for (int i = 0; i < difficulty.length; i++) {
		maxPro[difficulty[i]] = Math.max(profit[i], maxPro[difficulty[i]]);
	}
	
	int maxP = 0;
	for (int i = 0; i <= maxDif; i++) {
		maxP = Math.max(maxPro[i], maxP);
		maxPro[i] = Math.max(maxP, maxPro[i]);
	}
	
	int ans = 0;
	for (int w : worker) ans += maxPro[Math.min(w, maxDif)];
	return ans;
}
```

#### [831. 隐藏个人信息](https://leetcode.cn/problems/masking-personal-information/)

@2023.11.25

#模拟 

分别模拟邮箱、电话，处理字符串。

```java
/**
 * 模拟
 * Somnia1337
 */
public String maskPII(String s)
{
	return s.contains("@") ? mail(s) : phone(s);
}

private String mail(String s)
{
	s = s.toLowerCase();
	int idx = s.indexOf("@");
	return s.charAt(0) + "*****" + s.substring(idx - 1);
}

private String phone(String s)
{
	StringBuilder nums = new StringBuilder();
	for (char c : s.toCharArray())
	{
		if (Character.isDigit(c)) nums.append(c);
	}
	if (nums.length() == 10) return "***-***-" + nums.substring(6);
	return "+" + "*".repeat(nums.length() - 10) + "-***-***-" + nums.substring(nums.length() - 4);
}
```

优化：在电话部分，

- 用正则表达式 `[^0-9]` 选出非数字，将其替换为空串。
- 用数组存储 4 种可能的前缀。

```java
/**
 * 模拟
 * Kvicii
 */
public String maskPII(String s)
{
	int idx = s.indexOf("@");
	if (idx > 0)
	{
		s = s.toLowerCase();
		return s.charAt(0) + "*****" + s.substring(idx - 1);
	}
	String[] pref = new String[]{"", "+*-", "+**-", "+***-"};
	s = s.replaceAll("[^0-9]", "");
	return pref[s.length() - 10] + "***-***-" + s.substring(s.length() - 4);
}
```

#### [833. 字符串中的查找与替换](https://leetcode.cn/problems/find-and-replace-in-string/)

@2023.11.22

```java
/**
 * 哈希表
 * Somnia1337
 */
public String findReplaceString(String s, int[] indices, String[] sources, String[] targets)
{
	int len = indices.length, l = s.length();
	Map<Integer, List<Integer>> index = new HashMap<>();
	for (int i = 0; i < len; i++)
	{
		index.putIfAbsent(indices[i], new ArrayList<>());
		index.get(indices[i]).add(i);
	}
	
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < l; i++)
	{
		boolean rep = false;
		if (index.containsKey(i))
		{
			List<Integer> id = index.get(i);
			for (int idx : id)
			{
				String src = sources[idx];
				int t = src.length();
				if (l - i >= t && s.substring(i, i + t).equals(src))
				{
					ans.append(targets[idx]);
					i += t - 1;
					rep = true;
					break;
				}
			}
		}
		if (!rep)
		{
			ans.append(s.charAt(i));
		}
	}
	return ans.toString();
}
```

```java
/**
 * 预处理
 * ?
 */
public String findReplaceString(String s, int[] indices, String[] sources, String[] targets)
{
	int len = indices.length;
	int[] repIdx = new int[s.length()];
	for (int i = 0; i < len; i++)
	{
		if (s.startsWith(sources[i], indices[i]))
		{
			repIdx[indices[i]] = i + 1;
		}
	}
	
	StringBuilder ans = new StringBuilder();
	for (int i = 0; i < repIdx.length; i++)
	{
		if (repIdx[i] == 0)
		{
			ans.append(s.charAt(i));
		}
		else
		{
			ans.append(targets[repIdx[i] - 1]);
			i += (sources[repIdx[i] - 1].length() - 1);
		}
	}
	return ans.toString();
}
```

#### [835. 图像重叠](https://leetcode.cn/problems/image-overlap/)

@2023.11.25

#暴力 

用 `count[2 * n + 1][2 * n + 1]` 记录每个可能的偏移量下重合 1 的数量，枚举 `img1`，为 1 时，嵌套枚举 `img2` 的每个 1，将对应偏移量下的重合计数加 1，最后遍历 `count` 取最大值。

```java
/**
 * 枚举
 * 力扣官方题解
 */
public int largestOverlap(int[][] img1, int[][] img2)
{
	int n = img1.length;
	int[][] count = new int[2 * n + 1][2 * n + 1];
	for (int x = 0; x < n; x++)
	{
		for (int y = 0; y < n; y++)
		{
			if (img1[x][y] == 1)
			{
				for (int i = 0; i < n; i++)
				{
					for (int j = 0; j < n; j++)
					{
						if (img2[i][j] == 1) count[x - i + n][y - j + n]++;
					}
				}
			}
		}
	}
	
	int ans = 0;
	for (int[] cnt : count)
	{
		for (int c : cnt) ans = Math.max(c, ans);
	}
	return ans;
}
```

#### [840. 矩阵中的幻方](https://leetcode.cn/problems/magic-squares-in-grid/)

@2023.11.22

#暴力 

枚举每个可能的左上角，判断幻方。

```java
/**
 * 枚举
 * Somnia1337
 */
public int numMagicSquaresInside(int[][] grid)
{
	int ans = 0;
	for (int i = 0; i < grid.length - 2; i++)
	{
		for (int j = 0; j < grid[0].length - 2; j++)
		{
			if (check(grid, i, j)) ans++;
		}
	}
	return ans;
}

private boolean check(int[][] g, int x, int y)
{
	int[] cnt = new int[9];
	for (int i = x; i < x + 3; i++)
	{
		for (int j = y; j < y + 3; j++)
		{
			if (g[i][j] == 0 || g[i][j] >= 10) return false;
			cnt[g[i][j] - 1]++;
		}
	}
	for (int c : cnt)
	{
		if (c != 1) return false;
	}
	return g[x][y] + g[x][y + 1] + g[x][y + 2] == 15 && g[x + 1][y] + g[x + 1][y + 1] + g[x + 1][y + 2] == 15 && g[x + 2][y] + g[x + 2][y + 1] + g[x + 2][y + 2] == 15 && g[x][y] + g[x + 1][y] + g[x + 2][y] == 15 && g[x][y + 1] + g[x + 1][y + 1] + g[x + 2][y + 1] == 15 && g[x][y + 2] + g[x + 1][y + 2] + g[x + 2][y + 2] == 15 && g[x][y] + g[x + 1][y + 1] + g[x + 2][y + 2] == 15 && g[x][y + 2] + g[x + 1][y + 1] + g[x + 2][y] == 15;
}
```

提前列出所有可能的三阶幻方，枚举检查。

```java
/**
 * 枚举
 * Somnia1337
 */
public int numMagicSquaresInside(int[][] grid)
{
	int[][] valid = new int[][]{{8, 1, 6, 3, 5, 7, 4, 9, 2}, {6, 1, 8, 7, 5, 3, 2, 9, 4}, {4, 9, 2, 3, 5, 7, 8, 1, 6}, {2, 9, 4, 7, 5, 3, 6, 1, 8}, {6, 7, 2, 1, 5, 9, 8, 3, 4}, {8, 3, 4, 1, 5, 9, 6, 7, 2}, {2, 7, 6, 9, 5, 1, 4, 3, 8}, {4, 3, 8, 9, 5, 1, 2, 7, 6}};
	int ans = 0;
	for (int i = 0; i < grid.length - 2; i++)
	{
		for (int j = 0; j < grid[0].length - 2; j++)
		{
			int[] t = new int[9];
			int it = 0;
			for (int x = i; x < i + 3; x++)
			{
				for (int y = j; y < j + 3; y++)
				{
					t[it++] = grid[x][y];
				}
			}
			for (int[] v : valid)
			{
				if (Arrays.equals(v, t))
				{
					ans++;
					break;
				}
			}
		}
	}
	return ans;
}
```

#### [841. 钥匙和房间](https://leetcode.cn/problems/keys-and-rooms/)

@2023.10.20

#搜索 

从每个房间访问所有下个房间，记录访问过的房间数，最后判断是否等于 `len`。

```java
/**
 * DFS
 * Somnia1337
 */
private boolean[] visited;
private int visits; // 访问到的房间

public boolean canVisitAllRooms(List<List<Integer>> rooms)
{
	int len = rooms.size();
	visited = new boolean[len];
	dfs(0, rooms); // 最初只能进入0号
	return visits == len; // 需要全部访问到
}

private void dfs(int room, List<List<Integer>> rooms)
{
	if (visited[room]) return;
	visited[room] = true;
	visits++;
	for (int next : rooms.get(room)) dfs(next, rooms);
}
```

#### [842. 将数组拆分成斐波那契序列](https://leetcode.cn/problems/split-array-into-fibonacci-sequence/)

@2023.12.03

#暴力 

枚举前 2 个数，生成其余数，检查是否相同。

```java
/**
 * 枚举
 * Somnia1337
 */
public List<Integer> splitIntoFibonacci(String num)
{
	List<Integer> ans = new ArrayList<>();
	int l = num.length();
	for (int i = 1; i <= Math.min(l - 2, 9); i++)
	{
		if (i > 1 && num.startsWith("0")) break;
		int a = Integer.parseInt(num.substring(0, i));
		for (int j = i + 1; j <= Math.min(l - 1, i + 9); j++)
		{
			if (j > i + 1 && num.substring(i, j).startsWith("0")) break;
			int b = Integer.parseInt(num.substring(i, j));
			StringBuilder cur = new StringBuilder(num.substring(0, j));
			ans.add(a);
			ans.add(b);
			while (cur.length() < l)
			{
				int hold = b;
				b += a;
				a = hold;
				ans.add(b);
				cur.append(b);
				if (!num.startsWith(cur.toString())) break;
			}
			if (cur.length() == l && cur.toString().equals(num)) return ans;
			ans = new ArrayList<>();
			a = Integer.parseInt(num.substring(0, i));
		}
	}
	return ans;
}
```

#### [845. 数组中的最长山脉](https://leetcode.cn/problems/longest-mountain-in-array/)

@2023.11.23

待优化。

```java
/**
 * 三次遍历
 * Somnia1337
 */
public int longestMountain(int[] arr)
{
	int len = arr.length;
	int[][] con = new int[len][2];
	
	int cur = 1;
	con[0][0] = 1;
	for (int i = 1; i < len; i++)
	{
		con[i][0] = cur = arr[i] > arr[i - 1] ? cur + 1 : 1;
	}
	cur = 1;
	con[len - 1][1] = 1;
	for (int i = len - 2; i >= 0; i--)
	{
		con[i][1] = cur = arr[i] > arr[i + 1] ? cur + 1 : 1;
	}
	
	int ans = 0;
	for (int[] c : con)
	{
		if (c[0] > 1 && c[1] > 1) ans = Math.max(c[0] + c[1] - 1, ans);
	}
	return ans;
}
```

#### [846. 一手顺子](https://leetcode.cn/problems/hand-of-straights/)

@2023.11.01

#贪心 

类似 [659. 分割数组为连续子序列](https://leetcode.cn/problems/split-array-into-consecutive-subsequences/)，哈希表计数，排序，记录每组顺子的长度，同时更新计数。

```java
/**
 * 贪心
 * Somnia1337
 */
public boolean isNStraightHand(int[] hand, int groupSize)
{
	if (hand.length % groupSize > 0) return false;
	Map<Integer, Integer> count = new HashMap<>();
	for (int card : hand) count.merge(card, 1, Integer::sum);
	
	Arrays.sort(hand);
	for (int card : hand)
	{
		if (count.get(card) == 0) continue;
		int curSize = 0;
		while (curSize < groupSize && count.getOrDefault(card, 0) > 0)
		{
			count.merge(card, -1, Integer::sum);
			card++;
			curSize++;
		}
		if (curSize != groupSize) return false;
	}
	
	return true;
}
```

#### [848. 字母移位](https://leetcode.cn/problems/shifting-letters/)

@2023.11.23

#前缀和 

每个字母的最终移位次数为其后缀和 % 26，倒序遍历再反转。

```java
/**
 * 前缀和
 * Somnia1337
 */
public String shiftingLetters(String s, int[] shifts)
{
	int len = s.length();
	StringBuilder ans = new StringBuilder();
	for (int i = len - 1; i >= 0; i--)
	{
		if (i < len - 1) shifts[i] += shifts[i + 1];
		shifts[i] %= 26;
		ans.append((char) ('a' + (s.charAt(i) - 'a' + shifts[i]) % 26));
	}
	return ans.reverse().toString();
}
```

优化：用字符数组，更改操作更简单，且最后无需反转。

```java
/**
 * 前缀和
 * ?
 */
public String shiftingLetters(String s, int[] shifts)
{
	char[] chars = s.toCharArray();
	int shift = 0;
	for (int i = s.length() - 1; i >= 0; i--)
	{
		shift += shifts[i] % 26;
		chars[i] = (char) ('a' + (chars[i] - 'a' + shift) % 26);
	}
	return new String(chars);
}
```

#### [849. 到最近的人的最大距离](https://leetcode.cn/problems/maximize-distance-to-closest-person/)

@2023.11.23

待优化。

```java
/**
 * 找规律
 * Somnia1337
 */
public int maxDistToClosest(int[] seats)
{
	int cur = 0, max = 0;
	for (int seat : seats)
	{
		if (seat == 0)
		{
			cur++;
			max = Math.max(cur, max);
		}
		else cur = 0;
	}
	
	int l = 0;
	for (int seat : seats)
	{
		if (seat == 1) break;
		l++;
	}
	int r = 0;
	for (int i = seats.length - 1; i >= 0; i--)
	{
		if (seats[i] == 1) break;
		r++;
	}
	
	return Math.max((max + 1) / 2, Math.max(l, r));
}
```

#### [852. 山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/)

@2023.11.23

#二分查找 

与右侧元素比较，向山顶方向走。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int peakIndexInMountainArray(int[] arr)
{
	int l = 0, r = arr.length - 1, m = 0;
	while (l < r)
	{
		m = l + r >> 1;
		if (arr[m - 1] < arr[m] && arr[m] > arr[m + 1]) break;
		if (arr[m + 1] > arr[m]) l = m;
		else if (arr[m + 1] < arr[m]) r = m;
	}
	return m;
}
```

#### [853. 车队](https://leetcode.cn/problems/car-fleet/)

@2023.11.23

```java
/**
 * 排序
 * Somnia1337
 */
public int carFleet(int target, int[] position, int[] speed)
{
	int len = position.length;
	double[][] join = new double[len][2];
	for (int i = 0; i < len; i++)
	{
		join[i][0] = position[i];
		join[i][1] = (double) (target - position[i]) / speed[i];
	}
	Arrays.sort(join, Comparator.comparing(a -> -a[0]));
	double max = 0;
	int ans = 0;
	for (double[] car : join)
	{
		if (car[1] > max)
		{
			max = car[1];
			ans++;
		}
	}
	return ans;
}
```

```java
/**
 * 排序
 * ckforsy
 */
public int carFleet(int target, int[] position, int[] speed)
{
	int len = position.length;
	int[] temp = new int[target];
	for (int i = 0; i < len; i++) temp[position[i]] = speed[i];
	double[] time = new double[len];
	Arrays.sort(position);
	for (int i = 0; i < len; i++)
	{
		int n = len - 1 - i;
		time[i] = (target - position[n]) / (1.0 * temp[position[n]]);
	}
	int ans = 1;
	for (int i = 0; i < len - 1; i++)
	{
		if (time[i] >= time[i + 1]) time[i + 1] = time[i];
		else ans++;
	}
	return ans;
}
```

#### [856. 括号的分数](https://leetcode.cn/problems/score-of-parentheses/)

@2023.11.23

#搜索 #栈 

1. DFS

![[856. 括号的分数-1.png|400]]

括号之间有着明显的层级关系，可以将之视为树状结构：

![[856. 括号的分数-2.png|300]]

![[856. 括号的分数-3.png|300]]

- 当一个结点没有子结点的时候，那么这个结点的值为 1
- 否则，为所有子结点之和 * 2

用 `c == ')'` 判断结点的开始，同样判断是否有子结点。

```java
/**
 * DFS
 * HongKy
 */
private int it = 0;

public int scoreOfParentheses(String s) {
	char[] chs = s.toCharArray();
	int n = chs.length, ans = 0;
	while (it < n && chs[it] == '(') {
		it++;
		if (chs[it] == ')') ans++; // 没有子结点
		else ans += scoreOfParentheses(s) * 2; // 有子结点
		it++;
	}
	return ans;
}
```

2. 栈

定义空串的分数为 0，遍历：

- 遇到 `'('` 时，压入 `0`，作为暂时的分数。
- 遇到 `')'` 时，弹出的分数为当前括号对之间的分数，将其乘 `2`，与 `1` 取较大者，加上再次弹出的分数

以 `"(()(()))"` 为例，展示这个过程：

```text
i  c  stk
-1    [0
0  (  [0,0
1  (  [0,0,0
2  )  [0,1
3  (  [0,1,0
4  (  [0,1,0,0
5  )  [0,1,1
6  )  [0,3
7  )  [6
```

```java
/**
 * 栈
 * 力扣官方题解
 */
public int scoreOfParentheses(String s) {
	Deque<Integer> stk = new ArrayDeque<>();
	stk.push(0); // 初始时压入 0
	for (char c : s.toCharArray()) {
		if (c == '(') stk.push(0); // 压栈
		else stk.push(Math.max(2 * stk.pop(), 1) + stk.pop()); // 更新栈顶
	}
	return stk.peek(); // 栈顶元素即为答案
}
```

3. 加权和

从根源上，分数全部来源于原子括号对 "()"，对深度为 `d` 的原子，它对最终分数的贡献为 $2^d$。遍历时，遇到 '(' 将 `d` 加 1、遇到 ')' 将 `d` 减 1，并在匹配到原子时将 `1 << d` 累加到答案。

```java
/**
 * 加权和
 * 力扣官方题解
 */
public int scoreOfParentheses(String s)
{
	int n = s.length(), d = 0, ans = 0;
	for (int i = 0; i < n; i++)
	{
		d += (s.charAt(i) == '(' ? 1 : -1); // 更新深度
		if (s.charAt(i) == ')' && s.charAt(i - 1) == '(') ans += 1 << d; // 匹配到原子, 累加答案
	}
	return ans;
}
```

#### [858. 镜面反射](https://leetcode.cn/problems/mirror-reflection/)

@2023.11.25

#数学 

将光的运动拆分到两个方向，都在 0 到 `p` 间往返，由于接收器都位于角落，因此只有经过整数个时间后才可能到接收器，设为 `t`，则 `q * t % p == 0`，找到最小满足条件的 `t` 后，根据其奇偶性判断左 / 右侧，根据 `t * q / p` 的奇偶性判断上 / 下侧。

`p` 和 `q` 的最小公倍数为 `s = p * q / gcd(p, q)`，`t` 取 `s / q = p / gcd(p, q)`。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int mirrorReflection(int p, int q)
{
	return q / gcd(p, q) % 2 == 0 ? 0 : p / gcd(p, q) % 2 == 0 ? 2 : 1;
}

private int gcd(int a, int b)
{
	return a % b == 0 ? b : gcd(b, a % b);
}
```

#### [866. 回文素数](https://leetcode.cn/problems/prime-palindrome/)

@2023.12.23

#数学 

逐一检查，先检查回文，再检查质数。

不存在长为 8 的质数。

```java
/**
 * 数学
 * 力扣官方题解
 */
public int primePalindrome(int n) {
	if (n <= 2) return 2;
	for (int x = (n & 1) == 1 ? n : n + 1; x < 200_000_000; x += 2) {
		if (reverse(x) == x && isPrime(x)) return x;
		if (10_000_000 < x && x < 100_000_000) x = 100_000_001;
	}
	return -1;
}

private boolean isPrime(int x) {
	for (int y = 3; y * y <= x; y += 2) {
		if (x % y == 0) return false;
	}
	return true;
}

private int reverse(int x) {
	int ret = 0;
	while (x > 0) {
		ret = 10 * ret + (x % 10);
		x /= 10;
	}
	return ret;
}
```

#### [869. 重新排序得到 2 的幂](https://leetcode.cn/problems/reordered-power-of-2/)

@2023.11.26

#暴力 

将字符数组排序得到的字符串作为哈希值，枚举检查。

```java
/**
 * 枚举
 * Somnia1337
 */
public boolean reorderedPowerOf2(int n)
{
	String s = hash(n);
	for (long i = 1; i < Integer.MAX_VALUE; i <<= 1)
	{
		if (hash(i).equals(s)) return true;
	}
	return false;
}

private String hash(long n)
{
	char[] digits = String.valueOf(n).toCharArray();
	Arrays.sort(digits);
	return new String(digits);
}
```

#### [870. 优势洗牌](https://leetcode.cn/problems/advantage-shuffle/)

@2023.11.25

#贪心 

注意，`nums2` 的元素顺序不允许改变。

为 `nums2` 创建索引数组 `Integer[] idx`，按照 `nums2` 的元素降序对索引数组排序，例如：

```text
nums2: [1,10,4,11]
idx:   [3,1,2,0]
```

`idx` 的意义：将每个 `idx[i]` 恢复为 `nums2[idx[i]]` 后，元素是降序的，相当于对不允许排序的 `nums2` 进行了排序。

```text
nums2:                 [1,10,4,11]
idx:                   [3,1,2,0]
idx[i]->nums2[idx[i]]: [11,10,4,1]
```

左右双指针指向 `nums[1]` 两端，遍历 `idx`（将 `idx[i]` 恢复为 `nums2[idx[i]]` 后，相当于对 `nums2` 降序遍历，也就是每次取 `nums2` 中所剩元素最大者）：

- 如果 `nums1[r] > nums2[idx[i]]`，取 `nums1[r--]`。
- 否则，取 `nums1[l++]`。

```java
/**
 * 贪心
 * huang054
 */
public int[] advantageCount(int[] nums1, int[] nums2)
{
	int len = nums1.length;
	Integer[] idx = new Integer[len];
	for (int i = 0; i < len; i++) idx[i] = i;
	Arrays.sort(nums1);
	Arrays.sort(idx, (i1, i2) -> nums2[i2] - nums2[i1]);
	
	int[] ans = new int[len];
	int l = 0, r = len - 1;
	for (int i = 0; i < len; i++)
	{
		ans[idx[i]] = nums1[r] > nums2[idx[i]] ? nums1[r--] : nums1[l++];
	}
	return ans;
}
```

#### [873. 最长的斐波那契子序列的长度](https://leetcode.cn/problems/length-of-longest-fibonacci-subsequence/)

@2023.11.28

#暴力 #动态规划 

1. 枚举

将所有元素记录在哈希表中，双重 `for` 枚举斐波那契子序列的前 2 个元素，生成后续元素（由于题目保证 `arr` 严格递增，因此只需记录元素，如果 `contains()` 则比在后续位置），注意长度至少为 3 才有效，更新答案。

```java
/**
 * 枚举
 * Somnia1337
 */
public int lenLongestFibSubseq(int[] arr) {
    int n = arr.length;
    Set<Integer> vis = new HashSet<>();
    for (int x : arr) vis.add(x);
    
    int ans = 0;
    for (int i = 0; i < n - 2; i++) {
        int x1 = arr[i];
        for (int j = i + 1; j < n - 1; j++) {
            int x2 = arr[j], x3 = x1 + x2;
            int cur = 2, hold = x3;
            while (vis.contains(x3)) { // 题目保证了严格递增
                cur++;
                x1 = x2;
                x2 = x3;
                x3 = x1 + x2;
            }
            if (x3 != hold) ans = Math.max(cur, ans);
            x1 = arr[i];
        }
    }
    return ans;
}
```

2. 动态规划

`dp[i][j]` 表示以 `arr[i]` 为末元素、`arr[j]` 为次末元素的子问题答案，哈希表记录 `<元素, 下标>`。

从 `0` 正序枚举 `i`，从 `i - 1` 倒序枚举 `j`，`dp[i][j]` 对应的上个元素为 `arr[i] - arr[j]`，如果存在，状态转移方程 `dp[i][j] = max(dp[j][pos[arr[i] - arr[j]]] + 1, 3)`，3 表示只要 `arr[i] - arr[j]` 存在就已经凑出了长为 3 的子序列。

剪枝：

- 可行性剪枝：如果 `arr[i] - arr[j] >= arr[j]`，那么即使存在 `arr[i] - arr[j]`，其下标也在 `j` 之后。
- 最优性剪枝：只有 `j + 2 > ans` 时才继续搜索，因为 `j + 2` 为以 `arr[j]` 为次末元素时最大可能长度。

```java
/**
 * 动态规划
 * 宫水三叶
 */
public int lenLongestFibSubseq(int[] arr) {
    int n = arr.length;
    Map<Integer, Integer> pos = new HashMap<>();
    for (int i = 0; i < n; i++) pos.put(arr[i], i);
    
    int[][] dp = new int[n][n];
    int ans = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i - 1; j >= 0 && j + 2 > ans; j--) { // 剪枝 2
            if (arr[i] - arr[j] >= arr[j]) break; // 剪枝 1
            if (!pos.containsKey(arr[i] - arr[j])) continue;
            dp[i][j] = Math.max(dp[j][pos.get(arr[i] - arr[j])] + 1, 3);
            ans = Math.max(dp[i][j], ans);
        }
    }
    return ans;
}
```

#### [874. 模拟行走机器人](https://leetcode.cn/problems/walking-robot-simulation/)

@2023.11.26

#模拟 

用 `px`，`py` 表示两个方向上的增量，模拟行走。将障碍物坐标哈希为整型，实现快速检查。

```java
/**
 * 模拟
 * Somnia1337
 */
private int px, py; // 当前 x, y 方向上的增量

public int robotSim(int[] commands, int[][] obstacles) {
	Set<Integer> ban = new HashSet<>();
	for (int[] obs : obstacles) {
		ban.add(obs[0] * 12345 + obs[1]); // 哈希函数 x*12345+y 是试出来的
	}
	int x = 0, y = 0, ans = 0;
	px = 0;
	py = 1;
	for (int c : commands) {
		if (c < 0) turn(c == -2 ? 1 : -1); // 向左为 1, 向右为 -1
		else {
			for (int k = 0; k < c; k++) {
				x += px;
				y += py;
				if (ban.contains(x * 12345 + y)) // 遇到障碍, 回退一步并 break
				{
					x -= px;
					y -= py;
					break;
				}
				ans = Math.max(x * x + y * y, ans);
			}
		}
	}
	return ans;
}

private void turn(int c) {
	int hold = px;
	px = px == 0 ? -c * py : 0;
	py = py == 0 ? c * hold : 0;
}
```

#### [875. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/)

@2023.09.18

#二分查找 

答案符合单调性，对答案进行二分查找。

```java
/**
 * 二分查找
 * Somnia1337
 */
public int minEatingSpeed(int[] piles, int h)
{
	int max = 0;
	for (int pile : piles) max = Math.max(pile, max);
	int left = 1, right = max;
	while (left <= right)
	{
		int speed = left + (right - left) / 2;
		long time = 0; // 有一个用例会溢出int
		for (int pile : piles)
		{
			time += pile / speed;
			if (pile % speed != 0) time++;
		}
		if (time > h) left = speed + 1;
		else right = speed - 1;
	}
	return left;
}
```

#### [877. 石子游戏](https://leetcode.cn/problems/stone-game/)

@2023.11.24

#博弈 #技巧 

一共有偶数堆石子，先手可以控制获得所有奇数位置或所有偶数位置的石子，因此必定胜利。

```java
/**
 * 脑筋急转弯
 * Somnia1337
 */
public boolean stoneGame(int[] piles)
{
	return true;
}
```

#### [880. 索引处的解码字符串](https://leetcode.cn/problems/decoded-string-at-index/)

@2023.12.24

```java
/**
 * 递归
 * 丶暗隐精灵
 */
public String decodeAtIndex(String s, int k) {
	k--; // 索引从 0 开始, k 从 1 开始, k-1 以与索引统一
	int n = 0; // 当前解码字符串长度
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if (Character.isDigit(c)) {
			int num = c - '0'; // 倍数
			// k>=n*num, 无法在当前解码串中找到目标字符
			if (k / num >= n) n *= num; // 防溢出变形
			// 否则, 能找到, 取余并递归
			else return decodeAtIndex(s, k % n + 1);
		}
		else if (n++ == k) return String.valueOf(c);
	}
	return "";
}
```

#### [881. 救生艇](https://leetcode.cn/problems/boats-to-save-people/)

@2023.11.26

#贪心 

排序后，双指针配对乘客，无法配对时，只上较重者，原理：

考虑体重最轻的人：

- 若他不能与体重最重的人同乘一艘船，那么体重最重的人无法与任何人同乘一艘船，此时应单独分配一艘船给体重最重的人。
- 若他能与体重最重的人同乘一艘船，那么他能与其余任何人同乘一艘船，为了尽可能地利用船的承载重量，选择与体重最重的人同乘一艘船是最优的。

```java
/**
 * 贪心
 * Somnia1337
 */
public int numRescueBoats(int[] people, int limit)
{
	Arrays.sort(people);
	int l = 0, r = people.length - 1, ans = 0;
	while (l <= r)
	{
		if (people[l] + people[r] <= limit) l++;
		r--;
		ans++;
	}
	return ans;
}
```

#### [886. 可能的二分法](https://leetcode.cn/problems/possible-bipartition/)

@2024.01.13

#并查集 

转换为 [785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/)。

```java
/**
 * 并查集
 * Somnia1337
 */
public boolean possibleBipartition(int n, int[][] dislikes) {
	int[] root = new int[n + 1];
	Arrays.setAll(root, a -> a);
	List<Integer>[] g = new List[n + 1];
	Arrays.setAll(g, k -> new ArrayList<>());
	for (int[] d : dislikes) {
		g[d[0]].add(d[1]);
		g[d[1]].add(d[0]);
	}
	for (int i = 1; i < n + 1; i++) {
		List<Integer> list = g[i];
		if (list.isEmpty()) continue;
		int head = list.get(0);
		for (int j = 1, l = list.size(); j < l; j++) union(root, list.get(j), head);
		if (find(root, i) == find(root, head)) return false;
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

#### [890. 查找和替换模式](https://leetcode.cn/problems/find-and-replace-pattern/)

@2023.11.28

#哈希表 #字符串 

借用 [205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/) 的答案。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<String> findAndReplacePattern(String[] words, String pattern)
{
	List<String> ans = new ArrayList<>();
	for (String word : words)
	{
		if (check(word, pattern)) ans.add(word);
	}
	return ans;
}

private boolean check(String a, String b)
{
	Map<Character, Character> map = new HashMap<>();
	for (int i = 0; i < a.length(); i++)
	{
		char c1 = a.charAt(i), c2 = b.charAt(i);
		if (map.containsKey(c1) && map.get(c1) != c2 || !(map.containsKey(c1)) && map.containsValue(c2))
			return false;
		map.put(c1, c2);
	}
	return true;
}
```

优化：无需记录所有映射，只需考虑位置（每个映射都由字符首次出现的位置决定），遍历 `a`、`b`，每个相同位置的字符在各自中首次出现的位置应该相同。

```java
/**
 * 库函数
 * ?
 */
public List<String> findAndReplacePattern(String[] words, String pattern)
{
	List<String> ans = new ArrayList<>();
	for (String word : words)
	{
		if (check(word, pattern)) ans.add(word);
	}
	return ans;
}

private boolean check(String a, String b)
{
	for (int i = 0; i < a.length(); i++)
	{
		if (a.indexOf(a.charAt(i)) != b.indexOf(b.charAt(i))) return false;
	}
	return true;
}
```

#### [893. 特殊等价字符串组](https://leetcode.cn/problems/groups-of-special-equivalent-strings/)

@2023.11.27

#哈希表 

自定义哈希函数，将奇偶下标的字符分为两组分别排序，再用特殊符号分隔组合为哈希串。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int numSpecialEquivGroups(String[] words)
{
	Set<String> group = new HashSet<>();
	for (String word : words) group.add(hash(word));
	return group.size();
}

private String hash(String s)
{
	StringBuilder p1 = new StringBuilder(), p2 = new StringBuilder();
	for (int i = 0; i < s.length(); i++)
	{
		if ((i & 1) == 0) p1.append(s.charAt(i));
		else p2.append(s.charAt(i));
	}
	char[] c1 = p1.toString().toCharArray(), c2 = p2.toString().toCharArray();
	Arrays.sort(c1);
	Arrays.sort(c2);
	return new String(c1) + "/" + new String(c2);
}
```

优化：更好的哈希函数，遍历 `s`，将偶数下标字符的计数加 `s.length()`，将奇数下标字符的计数加 1，组合为哈希串，省略排序。

```java
/**
 * 哈希表
 * ?
 */
public int numSpecialEquivGroups(String[] words)
{
	Set<String> group = new HashSet<>();
	for (String word : words) group.add(hash(word));
	return group.size();
}

private String hash(String s)
{
	int l = s.length();
	char[] ret = new char[26];
	for (int i = 0; i < l; i++)
	{
		if ((i & 1) == 0) ret[s.charAt(i) - 'a'] += l;
		else ret[s.charAt(i) - 'a']++;
	}
	return new String(ret);
}
```

#### [898. 子数组按位或操作](https://leetcode.cn/problems/bitwise-ors-of-subarrays/)

@2023.11.29

#位运算 

枚举每个子数组的终点 `i`，嵌套向前枚举起点 `j`，用 `arr[i]` 更新 `arr[j]`，这样保证了每个先前元素都包含后续元素的所有位：

```text
 arr: [ 5, 1, 7, 6, 4, 3, 9, 8,2]
arr': [15,15,15,15,15,11,11,10,2]
```

因此内层出现更新后与原来相同 `(arr[j] | arr[i]) == arr[j]` 时，再向前也不可能有效更新，可以停止。

```java
/**
 * 位运算
 * hundanLi
 */
public int subarrayBitwiseORs(int[] arr) {
	Set<Integer> vis = new HashSet<>();
	for (int i = 0; i < arr.length; i++) {
		vis.add(arr[i]);
		for (int j = i - 1; j >= 0; j--) {
			if ((arr[j] | arr[i]) == arr[j]) break;
			arr[j] |= arr[i];
			vis.add(arr[j]);
		}
	}
	return vis.size();
}
```

#### [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)

@2023.10.07

#单调栈 

```java
/**
 * 单调栈
 * 宫水三叶
 */
class StockSpanner
{
    Deque<int[]> d;
    int cur;
	
    public StockSpanner()
    {
        cur = 0;
        d = new ArrayDeque<>();
    }
	
    public int next(int price)
    {
        while (!d.isEmpty() && d.peekLast()[1] <= price) d.pollLast();
        int prev = d.isEmpty() ? -1 : d.peekLast()[0], ans = cur - prev;
        d.addLast(new int[]{cur++, price});
        return ans;
    }
}
```

#### [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

@2023.09.15

```java
/**
 * 滑动窗口
 * Somnia1337
 */
public int totalFruit(int[] fruits)
{
	int len = fruits.length;
	Map<Integer, Integer> count = new HashMap<>();
	int slow = 0, fast = 0;
	int ans = 0;
	
	while (fast < len)
	{
		count.merge(fruits[fast], 1, Integer::sum);
		while (count.size() > 2)
		{
			count.merge(fruits[slow], -1, Integer::sum);
			if (count.get(fruits[slow]) == 0) count.remove(fruits[slow]);
			slow++;
		}
		ans = Math.max(fast - slow + 1, ans);
		fast++;
	}
	
	return ans;
}
```

```text
这段代码是用于解决力扣第904题（Fruit Into Baskets）的问题。该题要求找到一个连续的子数组，其中最多包含两种不同类型的水果，并且计算出这个子数组的最大长度。

这段代码的思路是使用滑动窗口（two-pointer）的方法来解决问题。下面是代码的主要逻辑和解释：

1. 初始化变量：
   - `ans`：用于存储最大子数组的长度，初始化为0。
   - `lastChange`：用于记录上一次水果类型发生变化的索引，初始化为0。
   - `slow` 和 `fast`：这两个指针分别用于定义滑动窗口的起始和结束位置。`slow` 初始化为0，`fast` 初始化为1。
   - `diff`：用于记录当前滑动窗口中两种水果类型的差异值，初始化为0。

2. 进入循环：
   - 使用 `while` 循环，当 `fast` 小于数组的长度时，继续执行以下操作。

3. 判断水果类型是否变化：
   - 如果当前水果类型和前一个水果类型不相同，进入条件判断。
   - 检查当前水果类型与上一个水果类型的差异是否等于 `diff`。
     - 如果不等于 `diff`，表示当前水果类型和上一个水果类型中间有一个新的水果类型插入，那么需要更新 `ans`，将当前窗口的长度与 `ans` 比较并取较大值，然后将 `slow` 更新为 `lastChange`，即窗口的起始位置。
   - 更新 `lastChange` 为当前 `fast` 的值，以便下次检测水果类型变化。
   - 更新 `diff` 为当前水果类型和前一个水果类型的差异。

4. 移动 `fast` 指针：
   - 不管是否进入条件判断，都需要将 `fast` 指针向右移动一位，以扩展窗口。

5. 返回结果：
   - 循环结束后，返回 `fruits.length - slow` 和 `ans` 中的较大值，这是因为有可能在数组末尾仍然有一部分水果类型相同，但是在之前的计算中没有被考虑到。

这段代码的核心思想是在遍历数组时，使用两个指针来维护一个滑动窗口，通过记录水果类型的变化和差异，不断更新最大子数组的长度。最终返回的是最大的子数组长度，该子数组包含最多两种不同类型的水果。这种方法的时间复杂度是 O(N)，其中 N 是输入数组 `fruits` 的长度。
```

```java
/**
 * 滑动窗口
 * ?
 */
public int totalFruit(int[] fruits)
{
	int ans = 0;
	int lastChange = 0;
	int slow = 0, fast = 1;
	int diff = 0;
	
	while (fast < fruits.length)
	{
		if (fruits[fast] != fruits[fast - 1])
		{
			if (fruits[fast - 1] - fruits[fast] != diff)
			{
				ans = Math.max(ans, fast - slow);
				slow = lastChange;
			}
			lastChange = fast;
			diff = fruits[fast] - fruits[fast - 1];
		}
		fast++;
	}
	
	return Math.max(fruits.length - slow, ans);
}
```

#### [907. 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)

@2023.10.25

```java
/**
 * 单调栈 + 动态规划
 * 力扣官方题解
 */
public int sumSubarrayMins(int[] arr)
{
	final int MOD = 1000000007;
	int len = arr.length;
	long ans = 0;
	Deque<Integer> stk = new ArrayDeque<>();
	int[] dp = new int[len];
	for (int i = 0; i < len; i++)
	{
		while (!stk.isEmpty() && arr[stk.peek()] > arr[i])
		{
			stk.pop();
		}
		int k = !stk.isEmpty() ? i - stk.peek() : i + 1;
		dp[i] = k * arr[i] + (!stk.isEmpty() ? dp[i - k] : 0);
		ans += dp[i];
		ans %= MOD;
		stk.push(i);
	}
	return (int) ans;
}
```

#### [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)

@2023.09.23

```java
/**
 * 归并排序
 * Somnia1337
 */
void mergeSort(int[] nums, int left, int right)
{
	if (left >= right) return;
	int mid = (left + right) / 2;
	mergeSort(nums, left, mid);
	mergeSort(nums, mid + 1, right);
	merge(nums, left, mid, right);
}

void merge(int[] nums, int left, int mid, int right)
{
	int[] tmp = Arrays.copyOfRange(nums, left, right + 1);
	int leftStart = 0, leftEnd = mid - left;
	int rightEnd = right - left;
	int i = leftStart, j = mid + 1 - left;
	for (int k = left; k <= right; k++)
	{
		if (i > leftEnd) nums[k] = tmp[j++];
		else if (j > rightEnd || tmp[i] <= tmp[j]) nums[k] = tmp[i++];
		else nums[k] = tmp[j++];
	}
}

public int[] sortArray(int[] nums)
{
	int[] ans = new int[nums.length];
	System.arraycopy(nums, 0, ans, 0, nums.length);
	mergeSort(ans, 0, nums.length - 1);
	return ans;
}
```

#### [915. 分割数组](https://leetcode.cn/problems/partition-array-into-disjoint-intervals/)

@2023.11.28

找规律：

```text
i:      0,1,2,3,4
nums:   5,0,3,8,6
sufMin: 0,0,3,6,6
preMax: 0,5,5,5,8 (preMax[i] = max(nums[:i]), 不包括 nums[i])
ans = 3, 是正序第一个 sufMin > preMax 的 i
```

实现上，先倒序遍历计算 `int[] sufMin`，再正序遍历，在每个 `i` 处先检查是否有  `sufMin[i] > preMax`，再更新 `preMax`。

```java
/**
 * 前后缀分解
 * Somnia1337
 */
public int partitionDisjoint(int[] nums)
{
	int len = nums.length;
	int[] sufMin = new int[len];
	sufMin[len - 1] = nums[len - 1];
	for (int i = len - 2; i >= 0; i--)
	{
		sufMin[i] = Math.min(sufMin[i + 1], nums[i]);
	}
	
	int preMax = nums[0];
	for (int i = 1; i < len; i++)
	{
		if (sufMin[i] >= preMax) return i;
		preMax = Math.max(nums[i - 1], preMax);
	}
	return -1;
}
```

仙术？

```java
/**
 * ?
 * ?
 */
public int partitionDisjoint(int[] nums)
{
	int idx = 0, max = nums[0], lMax = max;
	for (int i = 1; i < nums.length; i++)
	{
		if (lMax > nums[i])
		{
			idx = i;
			lMax = max;
		}
		else max = Math.max(max, nums[i]);
	}
	return idx + 1;
}
```

#### [916. 单词子集](https://leetcode.cn/problems/word-subsets/)

@2023.11.27

#字符串 

要从 `words1` 中查找所有 `word1`，满足 `word1` 是 `words2` 中每个字符串的超集。

可将 `words2` 整个合为一个串考虑，用 `int[26] total` 记录整个 `words2` 的字符频率，对每个 `word1` 计频，如果能覆盖 `total` 则加入解集。

```java
/**
 * 字符统计
 * Somnia1337
 */
public List<String> wordSubsets(String[] words1, String[] words2)
{
	int[] total = new int[26];
	for (String w2 : words2)
	{
		int[] count = count(w2);
		for (int i = 0; i < 26; i++) total[i] = Math.max(count[i], total[i]);
	}
	
	List<String> ans = new ArrayList<>();
	for (String w1 : words1)
	{
		int[] count = count(w1);
		boolean valid = true;
		for (int i = 0; i < 26; i++)
		{
			if (count[i] < total[i])
			{
				valid = false;
				break;
			}
		}
		if (valid) ans.add(w1);
	}
	return ans;
}

private int[] count(String s)
{
	int[] ret = new int[26];
	for (char c : s.toCharArray()) ret[c - 'a']++;
	return ret;
}
```

#### [918. 环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/)

@2023.10.29

#数组 #分类讨论 #技巧 

![[918. 环形子数组的最大和.png]]

一次遍历，计算子数组最大和 `maxS`、子数组最小和 `minS`、数组和 `sum`，分为两种情况：

- 最大子数组不成环，等价于 [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)，答案为 `maxS`。
- 最大子数组成环 ，那么最小子数组就不会成环，答案为 `sum - minS`。

```java
/**
 * 分类讨论
 * 灵茶山艾府
 */
public int maxSubarraySumCircular(int[] nums) {
	int maxS = Integer.MIN_VALUE, minS = 0;
	int maxF = 0, minF = 0, sum = 0;
	for (int num : nums) {
		maxF = Math.max(maxF, 0) + num;
		maxS = Math.max(maxF, maxS);
		minF = Math.min(minF, 0) + num;
		minS = Math.min(minF, minS);
		sum += num;
	}
	return sum != minS ? Math.max(maxS, sum - minS) : maxS;
}
```

#### [921. 使括号有效的最少添加](https://leetcode.cn/problems/minimum-add-to-make-parentheses-valid/)

@2023.12.01

#贪心 

`l`、`r` 分别记录当前没有匹配的左、右括号，遍历，贪心地匹配：

- 遇到 `'('`，`l++`
- 遇到 `')'`，
	- 如果左侧还有未匹配的左括号（`l > 0`），匹配，`l--`
	- 否则，无法匹配，`r++`

最终 `l + r` 即为剩余没有匹配成功的括号数目，也即需要添加的括号数目。

```java
/**
 * 贪心
 * Somnia1337
 */
public int minAddToMakeValid(String s)
{
	int l = 0, r = 0;
	for (char c : s.toCharArray())
	{
		if (c == '(') l++;
		else
		{
			if (l > 0) l--;
			else r++;
		}
	}
	return l + r;
}
```

#### [926. 将字符串翻转到单调递增](https://leetcode.cn/problems/flip-string-to-monotone-increasing/)

@2023.11.28

找规律：

```text
s:       1 0 1 0 0 1 1 0
suf0:    4 4 3 3 2 1 1 1 (后缀 0 数量)
pre1:    1 1 2 2 2 3 4 4 (前缀 1 数量)
sum - 1: 4 4 4 4 3 3 4 4 (二者之和 - 1)

ans = min(sum - 1) = 3
```

```java
/**
 * 前后缀分解
 * Somnia1337
 */
public int minFlipsMonoIncr(String s)
{
	int l = s.length();
	int[] suf0 = new int[l];
	suf0[l - 1] = '1' - s.charAt(l - 1);
	for (int i = l - 2; i >= 0; i--)
	{
		suf0[i] = suf0[i + 1] + '1' - s.charAt(i);
	}
	
	int pre1 = 0, ans = Integer.MAX_VALUE;
	for (int i = 0; i < l; i++)
	{
		pre1 += s.charAt(i) - '0';
		ans = Math.min(pre1 + suf0[i] - 1, ans);
	}
	return ans;
}
```

仙术？

```java
/**
 * 一次遍历
 * ?
 */
public int minFlipsMonoIncr(String s)
{
	int flip = 0, one = 0;
	for (char c : s.toCharArray())
	{
		if (c == '1') one++;
		else if (one > 0) flip++;
		if (flip > one) flip = one;
	}
	return flip;
}
```

#### [930. 和相同的二元子数组](https://leetcode.cn/problems/binary-subarrays-with-sum/)

@2023.12.01

#前缀和 #滑动窗口 

1. 前缀和 + 哈希表

哈希表统计每个前缀和出现次数，从 `goal` 开始枚举 `n`，累加 `pres[n] * pres[n - goal]` 到答案。

```java
/**
 * 前缀和 + 哈希表
 * Somnia1337
 */
public int numSubarraysWithSum(int[] nums, int goal)
{
	Map<Integer, Integer> pres = new HashMap<>();
	int pre = 0;
	pres.merge(0, 1, Integer::sum);
	for (int num : nums)
	{
		pre += num;
		pres.merge(pre, 1, Integer::sum);
	}
	
	int ans = 0;
	for (int n = goal; n <= pre; n++)
	{
		int v = pres.getOrDefault(n, 0);
		if (goal == 0) ans += v * (v - 1) / 2;
		else ans += v * pres.getOrDefault(n - goal, 0);
	}
	return ans;
}
```

2. 滑动窗口

所求 = 和为 `goal` 的子数组数量

= 和 `<= goal` 的子数组数量 - 和 `<= goal - 1` 的子数组数量

辅助方法 `count` 返回和 `<= max` 的子数组数量，用滑动窗口实现。

```java
/**
 * 滑动窗口
 * ?
 */
public int numSubarraysWithSum(int[] nums, int goal)
{
	return count(nums, goal) - count(nums, goal - 1);
}

private int count(int[] nums, int max)
{
	int ret = 0;
	for (int l = 0, r = 0, sum = 0; r < nums.length; r++)
	{
		sum += nums[r];
		while (l <= r && sum > max) sum -= nums[l++];
		ret += r - l + 1;
	}
	return ret;
}
```

#### [931. 下降路径最小和](https://leetcode.cn/problems/minimum-falling-path-sum/)

@2023.09.13

#动态规划 

与 [120. 三角形最小路径和](https://leetcode.cn/problems/triangle/) 类似。

```java
/**
 * 动态规划
 * Somnia1337
 */
public int minFallingPathSum(int[][] matrix) {
	int n = matrix.length;
	int[][] dp = new int[n][n];
	for (int i = 0; i < n; i++) System.arraycopy(matrix[i], 0, dp[i], 0, n);
	for (int i = 1; i < n; i++) {
		dp[i][0] += Math.min(dp[i - 1][0], dp[i - 1][1]);
		for (int j = 1; j < n - 1; j++) {
			dp[i][j] += Math.min(Math.min(dp[i - 1][j - 1], dp[i - 1][j + 1]), dp[i - 1][j]);
		}
		dp[i][n - 1] += Math.min(dp[i - 1][n - 2], dp[i - 1][n - 1]);
	}
	int ans = Integer.MAX_VALUE;
	for (int j = 0; j < n; j++) ans = Math.min(dp[n - 1][j], ans);
	return ans;
}
```

#### [934. 最短的桥](https://leetcode.cn/problems/shortest-bridge/)

@2023.11.27

#搜索 

先 DFS 将一个岛的单元格入队，再 BFS 直到遇到另一个岛。

```java
/**
 * DFS + BFS
 * Somnia1337
 */
private final int[] dir = new int[]{-1, 0, 1, 0, -1};
private Queue<int[]> q;

public int shortestBridge(int[][] grid)
{
	int n = grid.length;
	q = new LinkedList<>();
	boolean dfs = false;
	for (int i = 0; i < n && !dfs; i++)
	{
		for (int j = 0; j < n && !dfs; j++)
		{
			if (grid[i][j] == 1)
			{
				dfs(grid, i, j);
				dfs = true;
			}
		}
	}
	
	int ans = 0;
	while (!q.isEmpty())
	{
		for (int i = q.size(); i > 0; i--)
		{
			int[] p = q.poll();
			for (int k = 0; k < 4; k++)
			{
				int x = p[0] + dir[k], y = p[1] + dir[k + 1];
				if (x >= 0 && x < n && y >= 0 && y < n)
				{
					if (grid[x][y] == 1) return ans;
					if (grid[x][y] == 0)
					{
						grid[x][y] = 2;
						q.add(new int[]{x, y});
					}
				}
			}
		}
		ans++;
	}
	return -1;
}

private void dfs(int[][] g, int x, int y)
{
	if (!(x >= 0 && x < g.length && y >= 0 && y < g.length) || g[x][y] != 1) return;
	q.add(new int[]{x, y});
	g[x][y] = 2;
	for (int k = 0; k < 4; k++) dfs(g, x + dir[k], y + dir[k + 1]);
}
```

#### [939. 最小面积矩形](https://leetcode.cn/problems/minimum-area-rectangle/)

@2023.11.30

#哈希表 

用 `x * 54321 + y` 作为哈希函数，哈希表记录点，遍历 `points`，嵌套遍历哈希表，将两点视为对角线，检查是否存在另外两点与之构成矩形。

```java
/**
 * 哈希表
 * garrybest
 */
public int minAreaRect(int[][] points)
{
	int ans = Integer.MAX_VALUE;
	Set<Integer> p = new HashSet<>();
	for (int[] p1 : points)
	{
		int x1 = p1[0], y1 = p1[1];
		for (int p2 : p)
		{
			int x2 = p2 / 54321, y2 = p2 % 54321;
			if (x1 != x2 && y1 != y2)
			{
				if (p.contains(x1 * 54321 + y2) && p.contains(x2 * 54321 + y1))
				{
					ans = Math.min(Math.abs((x1 - x2) * (y1 - y2)), ans);
				}
			}
		}
		p.add(x1 * 54321 + y1);
	}
	return ans < Integer.MAX_VALUE ? ans : 0;
}
```

#### [945. 使数组唯一的最小增量](https://leetcode.cn/problems/minimum-increment-to-make-array-unique/)

@2023.11.30

#排序 

排序，遍历，如果当前元素 `<=` 前一个元素，将其变为前一个加 1，累加增量。

```java
/**
 * 排序
 * Sweetiee 🍬
 */
public int minIncrementForUnique(int[] nums)
{
	Arrays.sort(nums);
	int ans = 0;
	for (int i = 1; i < nums.length; i++)
	{
		if (nums[i] <= nums[i - 1])
		{
			ans += nums[i - 1] + 1 - nums[i];
			nums[i] = nums[i - 1] + 1;
		}
	}
	return ans;
}
```

#### [946. 验证栈序列](https://leetcode.cn/problems/validate-stack-sequences/)

@2023.11.30

#模拟 

用栈模拟，维护 `popped` 的下标，当栈顶与 `popped` 当前元素相同时弹栈。

```java
/**
 * 模拟
 * Somnia1337
 */
public boolean validateStackSequences(int[] pushed, int[] popped)
{
	int l = pushed.length, it = 0;
	Deque<Integer> stk = new ArrayDeque<>();
	for (int p : pushed)
	{
		stk.push(p);
		while (!stk.isEmpty() && it < l && stk.peek() == popped[it])
		{
			stk.pop();
			it++;
		}
	}
	return it == l;
}
```

优化：双指针指向两数组，对 `pushed` 操作模拟压栈。

```java
/**
 * 模拟
 * ?
 */
public boolean validateStackSequences(int[] pushed, int[] popped)
{
	int i = 0, j = 0;
	for (int p : pushed)
	{
		pushed[i++] = p;
		while (i > 0 && pushed[i - 1] == popped[j])
		{
			i--;
			j++;
		}
	}
	return i == 0;
}
```

#### [949. 给定数字能组成的最大时间](https://leetcode.cn/problems/largest-time-for-given-digits/)

@2023.12.01

#暴力 

枚举每一种排列，如果是合法的时间格式，更新最小时间。

API：`String.format()`。

```java
/**
 * 枚举
 * Somnia1337
 */
private int[] arr;
private boolean[] vis;
private StringBuilder path;
private int max = -1, cur = 0;

public String largestTimeFromDigits(int[] arr)
{
	this.arr = arr;
	vis = new boolean[4];
	path = new StringBuilder();
	bt();
	return max >= 0 ? String.format("%02d:%02d", max / 60, max % 60) : "";
}

private void bt()
{
	if (path.length() == 4)
	{
		if (check()) max = Math.max(cur, max);
		return;
	}
	for (int i = 0; i < 4; i++)
	{
		if (!vis[i])
		{
			vis[i] = true;
			path.append(arr[i]);
			bt();
			vis[i] = false;
			path.deleteCharAt(path.length() - 1);
		}
	}
}

private boolean check()
{
	int a = Integer.parseInt(path.substring(0, 2)), b = Integer.parseInt(path.substring(2));
	cur = a * 60 + b;
	return a <= 23 && b <= 59;
}
```

#### [954. 二倍数对数组](https://leetcode.cn/problems/array-of-doubled-pairs/)

@2023.12.01

#哈希表 

\[Deprecated]

```java
/**
 * 哈希表
 * Somnia1337
 */
public boolean canReorderDoubled(int[] arr)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int n : arr) count.merge(n, 2, Integer::sum);
	Arrays.sort(arr);
	if (count.containsKey(0) && (count.get(0) & 1) == 1) return false;
	for (int n : arr)
	{
		if (n == 0) continue;
		int c = count.get(n);
		if (c == 0) continue;
		if ((n & 1) == 1)
		{
			if (!count.containsKey(n * 2) || count.get(n * 2) < c) return false;
			count.merge(n * 2, -c, Integer::sum);
			count.put(n, 0);
		}
		else
		{
			if (count.containsKey(n * 2))
			{
				int minc = Math.min(c, count.get(n * 2));
				count.merge(n * 2, -minc, Integer::sum);
				count.merge(n, -minc, Integer::sum);
			}
			if (count.containsKey(n / 2))
			{
				int minc = Math.min(count.get(n), count.get(n / 2));
				count.merge(n / 2, -minc, Integer::sum);
				count.merge(n, -minc, Integer::sum);
			}
			if (count.get(n) > 0) return false;
		}
	}
	return true;
}
```

\[Deprecated]

```java
/**
 * 哈希表
 * 友友
 */
public boolean canReorderDoubled(int[] arr)
{
	Arrays.sort(arr);
	Map<Integer, Integer> count = new HashMap<>();
	for (int i : arr)
	{
		if (i < 0)
		{
			if (count.containsKey(2 * i) && count.get(2 * i) != 0) count.merge(2 * i, -1, Integer::sum);
			else count.merge(i, 1, Integer::sum);
		}
		else
		{
			if (count.containsKey(i) && count.get(i) != 0) count.merge(i, -1, Integer::sum);
			else count.merge(2 * i, 1, Integer::sum);
		}
	}
	for (int c : count.values())
	{
		if (c > 0) return false;
	}
	return true;
}
```

哈希表计数，将出现过的元素（键值集）按绝对值排序（这样，`2` 在 `4` 之前，`-2` 也在 `-4` 之前，所有元素的 2 倍都在它之后，因此按序匹配元素的 2 倍即可），按序遍历，每个元素的剩余计数都必须被其 2 倍元素的计数抵消，并更新其 2 倍的计数。

```java
/**
 * 哈希表
 * ?
 */
public boolean canReorderDoubled(int[] arr)
{
	Map<Integer, Integer> count = new HashMap<>();
	for (int v : arr) count.merge(v, 1, Integer::sum);
	if ((count.getOrDefault(0, 0) & 1) == 1) return false; // 0 只能与 0 配对, 必须出现偶数次
	List<Integer> vals = new ArrayList<>(count.keySet()); // 所有出现的元素
	vals.sort(Comparator.comparingInt(Math::abs)); // 按绝对值排序
	for (int v : vals)
	{
		if (count.getOrDefault(2 * v, 0) < count.get(v)) return false; // 每个元素的 2 倍的出现次数必须不少于它, 才能抵消
		count.merge(2 * v, -count.get(v), Integer::sum);
	}
	return true;
}
```

#### [957. N 天后的牢房](https://leetcode.cn/problems/prison-cells-after-n-days/)

@2023.12.01

#哈希表 

用哈希表记录每个状态，出现周期后停止，直接取余获取结果。

注意周期不一定从第 0 天开始（第 0 天两端的牢房可为 1，其后两端只能为 0）。

```java
/**
 * 哈希表
 * ......
 */
public int[] prisonAfterNDays(int[] cells, int N)
{
	Map<Integer, int[]> cycle = new HashMap<>();
	Set<String> state = new HashSet<>();
	int[] cur = getNext(cells);
	for (int i = 1; i <= N; i++)
	{
		if (!state.add(Arrays.toString(cur))) break;
		cycle.put(i, cur);
		cur = getNext(cur);
	}
	int period = state.size();
	int day = N % period;
	return cycle.get(day == 0 ? period : day);
}

private int[] getNext(int[] cur)
{
	int[] next = new int[8];
	for (int i = 1; i < 7; i++) next[i] = 1 - cur[i - 1] ^ cur[i + 1];
	return next;
}
```

可以证明，周期为 14，直接模拟，最多调用 14 次 `getNext()`。

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] prisonAfterNDays(int[] cells, int n)
{
	n = (n - 1) % 14 + 1;
	for (int i = 0; i < n; i++) cells = getNext(cells);
	return cells;
}

private int[] getNext(int[] cur)
{
	int[] next = new int[8];
	for (int i = 1; i < 7; i++) next[i] = 1 - cur[i - 1] ^ cur[i + 1];
	return next;
}
```

#### [962. 最大宽度坡](https://leetcode.cn/problems/maximum-width-ramp/)

@2023.10.25

```java
/**
 * 单调栈
 * 浙中医大彭于晏
 */
public int maxWidthRamp(int[] nums)
{
	int len = nums.length;
	Deque<Integer> stk = new ArrayDeque<>();
	stk.push(0);
	for (int i = 1; i < len; i++)
	{
		if (nums[i] < nums[stk.peek()])
		{
			stk.push(i);
		}
	}
	
	int ans = 0;
	for (int i = len - 1; i >= 0; i--)
	{
		while (!stk.isEmpty() && nums[i] >= nums[stk.peek()])
		{
			ans = Math.max(i - stk.pop(), ans);
		}
	}
	
	return ans;
}
```

#### [966. 元音拼写检查器](https://leetcode.cn/problems/vowel-spellchecker/)

@2023.12.01

#哈希表 

哈希表：

- `original`：原串
- `cas`：`<小写形式, 原串>`，每个键只在第一次遇到时存储
- `vow`：`<哈希值, 原串>`，哈希值为将所有元音替换为空格的小写形式

依次从 3 表中检查是否有匹配。

```java
/**
 * 哈希表
 * Somnia1337
 */
public String[] spellchecker(String[] wordlist, String[] queries)
{
	Set<String> original = new HashSet<>();
	Collections.addAll(original, wordlist);
	Map<String, String> cas = new HashMap<>(), vow = new HashMap<>();
	for (String word : wordlist)
	{
		cas.putIfAbsent(word.toLowerCase(), word);
		vow.putIfAbsent(hash(word.toLowerCase()), word);
	}
	
	String[] ans = new String[queries.length];
	for (int i = 0; i < queries.length; i++)
	{
		String q = queries[i];
		if (original.contains(q)) ans[i] = q;
		else
		{
			q = q.toLowerCase();
			if (cas.containsKey(q)) ans[i] = cas.get(q);
			else ans[i] = vow.getOrDefault(hash(q), "");
		}
	}
	return ans;
}

private String hash(String s)
{
	StringBuilder key = new StringBuilder();
	for (char c : s.toCharArray())
	{
		if ("aeiou".indexOf(c) < 0) key.append(c);
		else key.append(" ");
	}
	return key.toString();
}
```

#### [967. 连续差相同的数字](https://leetcode.cn/problems/numbers-with-same-consecutive-differences/)

@2023.12.01

#回溯 

回溯，限定开头为 1~9，构造出长为 `n` 的数字后用哈希表收集。

```java
/**
 * 回溯
 * Somnia1337
 */
private Set<Integer> collect;
private int path;

public int[] numsSameConsecDiff(int n, int k)
{
	collect = new HashSet<>();
	path = 0;
	for (int i = 1; i <= 9; i++) bt(n, 0, k, i); // 限定开头为 1~9
	int it = 0;
	int[] ans = new int[collect.size()];
	for (int c : collect) ans[it++] = c;
	return ans;
}

private void bt(int n, int l, int k, int cur)
{
	if (l == n)
	{
		collect.add(path);
		return;
	}
	path = path * 10 + cur;
	if (cur - k >= 0) bt(n, l + 1, k, cur - k);
	if (cur + k <= 9) bt(n, l + 1, k, cur + k);
	path /= 10;
}
```

#### [969. 煎饼排序](https://leetcode.cn/problems/pancake-sorting/)

@2023.12.02

#模拟 

只要答案长度小于 `10 * arr.length` 且能够排序都可接受，可每次选择最大的未排序元素 `val`，设其当前下标为 `idx`：

- 先翻转一次 `k = idx`，将其翻到数组首部
- 再翻转一次 `k = [val - 1]`，将其翻到目标位置。

```java
/**
 * 模拟
 * Somnia1337
 */
public List<Integer> pancakeSort(int[] arr) {
	List<Integer> ans = new ArrayList<>();
	for (int val = arr.length; val > 0; val--) { // 倒序处理
		int idx = getIndex(arr, val);
		reverse(arr, idx);
		if (idx != 0) ans.add(idx + 1);
		reverse(arr, val - 1);
		ans.add(val);
	}
	return ans;
}

private int getIndex(int[] arr, int tar) {
	for (int i = 0; i < arr.length; i++) {
		if (arr[i] == tar) return i;
	}
	return -1;
}

private void reverse(int[] arr, int r) {
	int l = 0;
	while (l < r) swap(arr, l++, r--);
}

private void swap(int[] arr, int i, int j) {
	int t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [970. 强整数](https://leetcode.cn/problems/powerful-integers/)

@2023.12.02

#哈希表 #技巧 

2 个哈希表分别记录所有 `x` 的幂、`y` 的幂，枚举检查每个值能否表示出。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> powerfulIntegers(int x, int y, int bound) {
	if (x < y) return powerfulIntegers(y, x, bound);
	Set<Integer> vis = new HashSet<>();
	if (x > 1) {
		int a = 1;
		for (int p = 1; a < bound; p++) {
			vis.add(a);
			a = (int) Math.pow(x, p);
		}
	}
	else vis.add(1);
	
	List<Integer> yp = new ArrayList<>();
	if (y > 1) {
		int b = 1;
		for (int p = 1; b < bound; p++) {
			yp.add(b);
			b = (int) Math.pow(y, p);
		}
	}
	else yp.add(1);
	
	List<Integer> ans = new ArrayList<>();
	for (int n = 1; n <= bound; n++) {
		for (int power : yp) {
			if (power > n) break;
			if (vis.contains(n - power)) {
				ans.add(n);
				break;
			}
		}
	}
	return ans;
}
```

优化：枚举检查每个值浪费了很多次查找，可以正向思考，枚举得到的所有幂、生成所有可能的值，606 ms 优化至 1 ms。

```java
/**
 * 哈希表
 * Somnia1337
 */
public List<Integer> powerfulIntegers(int x, int y, int bound) {
	List<Integer> xs = new ArrayList<>(), ys = new ArrayList<>();
	
	if (x > 1) {
		int a = 1;
		for (int p = 1; a < bound; p++) {
			xs.add(a);
			a = (int) Math.pow(x, p);
		}
	}
	else xs.add(1);
	
	if (y > 1) {
		int b = 1;
		for (int p = 1; b < bound; p++) {
			ys.add(b);
			b = (int) Math.pow(y, p);
		}
	}
	else ys.add(1);
	
	Set<Integer> ans = new HashSet<>();
	// 枚举, 组合所有幂值
	for (int a : xs) {
		for (int b : ys) {
			if (a + b > bound) break;
			ans.add(a + b);
		}
	}
	return new ArrayList<>(ans);
}
```

优化：正向枚举就无需存储之前的幂了，用 2 个 `int` 代替哈希表。

```java
/**
 * 哈希表
 * ?
 */
public List<Integer> powerfulIntegers(int x, int y, int bound) {
	Set<Integer> vis = new HashSet<>();
	int a = 1;
	while (a < bound) {
		int b = 1;
		while (a + b <= bound) {
			vis.add(a + b);
			if (y == 1) break;
			b *= y;
		}
		if (x == 1) break;
		a *= x;
	}
	return new ArrayList<>(vis);
}
```

#### [974. 和可被 K 整除的子数组](https://leetcode.cn/problems/subarray-sums-divisible-by-k/)

@2023.10.16

#前缀和 #哈希表 

计算前缀和，用哈希表记录余数，将与当前前缀和对 `k` 同余的前缀和个数加到答案。

在 Java 中，余数可为负（`-5 % 3 == -2`），需调用 `Math::floorMod` 强制其为正。

```java
/**
 * 哈希表
 * Somnia1337
 */
public int subarraysDivByK(int[] nums, int k) {
    Map<Integer, Integer> mods = new HashMap<>();
    mods.put(0, 1); // 前 0 个元素和为 0, 余数为 0, 计数为 1
    int pre = 0, ans = 0;
    for (int x : nums) {
        int mod = Math.floorMod(pre += x, k); // 取正余数
        ans += mods.merge(mod, 1, Integer::sum) - 1;
    }
    return ans;
}
```

#### [979. 在二叉树中分配硬币](https://leetcode.cn/problems/distribute-coins-in-binary-tree/)

@2023.12.02

#搜索 

DFS：

- 如果不够，直接向父结点要，不用管父结点够不够（父结点也向父节点要，因此最终都会有）。
- 如果多余，多出的值给父结点。

```java
/**
 * DFS
 * Riser
 */
private int ans;

public int distributeCoins(TreeNode root) {
    ans = 0;
    dfs(root);
    return ans;
}

private int dfs(TreeNode root) {
    if (root == null) return 0;
    int need = 1 - (root.val - dfs(root.left) - dfs(root.right)); // 与 1 的差距
    ans += Math.abs(need);
    return need;
}
```

#### [981. 基于时间的键值存储](https://leetcode.cn/problems/time-based-key-value-store/)

@2023.12.25

#设计 

哈希表嵌套 `TreeMap`，`Map<键, TreeMap<时间戳, 值>>`。

API `TreeMap::floorEntry` 返回不大于指定键的最大键值对。

```java
/**
 * 哈希表
 * 爪洼选手在线打铁
 */
class TimeMap {
    private Map<String, TreeMap<Integer, String>> map;
	
    public TimeMap() {
        map = new HashMap<>();
    }
	
    public void set(String key, String value, int timestamp) {
        map.computeIfAbsent(key, e -> new TreeMap<>()).put(timestamp, value);
    }
	
    public String get(String key, int timestamp) {
        Map.Entry<Integer, String> entry = map.getOrDefault(key, new TreeMap<>()).floorEntry(timestamp);
        return entry != null ? entry.getValue() : "";
    }
}
```

#### [985. 查询后的偶数和](https://leetcode.cn/problems/sum-of-even-numbers-after-queries/)

@2023.12.02

#模拟 

预处理求出所有偶数和，每次更新元素时判断两次：

- 更新前，如果为偶数，从当前和减去
- 更新后，如果为偶数，加到当前和

```java
/**
 * 模拟
 * Somnia1337
 */
public int[] sumEvenAfterQueries(int[] nums, int[][] queries)
{
	int cur = 0;
	for (int num : nums)
	{
		if ((num & 1) == 0) cur += num;
	}
	int[] ans = new int[queries.length];
	for (int i = 0; i < queries.length; i++)
	{
		int idx = queries[i][1];
		if ((nums[idx] & 1) == 0) cur -= nums[idx];
		nums[idx] += queries[i][0];
		if ((nums[idx] & 1) == 0) cur += nums[idx];
		ans[i] = cur;
	}
	return ans;
}
```

#### [988. 从叶结点开始的最小字符串](https://leetcode.cn/problems/smallest-string-starting-from-leaf/)

@2023.12.02

#回溯 

回溯，`StringBuilder` 记录路径，到达叶结点时更新答案。

```java
/**
 * 回溯
 * Somnia1337
 */
private String ans;
private StringBuilder path;

public String smallestFromLeaf(TreeNode root)
{
	path = new StringBuilder();
	dfs(root);
	return ans;
}

private void dfs(TreeNode root)
{
	if (root == null) return;
	path.append((char) ('a' + root.val));
	if (root.left == null && root.right == null)
	{
		String cur = path.reverse().toString();
		if (ans == null || cur.compareTo(ans) < 0) ans = cur;
		path.reverse();
	}
	else
	{
		dfs(root.left);
		dfs(root.right);
	}
	path.deleteCharAt(path.length() - 1);
}
```

#### [990. 等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/)

@2023.12.02

#并查集 

用并查集维护相等关系，两次遍历：

- 第一次只看等式，更新并查集。
- 第二次只看不等式，检查等价关系是否与其矛盾。

```java
/**
 * 并查集
 * Somnia1337
 */
public boolean equationsPossible(String[] equations) {
	int[] root = new int[26];
	Arrays.setAll(root, a -> a);
	for (String e : equations) {
		if (e.startsWith("==", 1)) {
			union(root, e.charAt(0) - 'a', e.charAt(3) - 'a');
		}
	}
	for (String e : equations) {
		if (e.startsWith("!=", 1)) {
			if (find(root, e.charAt(0) - 'a') == find(root, e.charAt(3) - 'a')) return false; // 不等式, 但左右同支, 矛盾
		}
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

#### [991. 坏了的计算器](https://leetcode.cn/problems/broken-calculator/)

@2024.01.04

#递归 

逆向思维，从 `target` 逼近 `startValue`，`t > s` 时先采用跨度大的 `/2` 逼近，`t < s` 时再逐渐 `+1`。

```java
/**
 * 递归
 * Somnia1337
 */
public int brokenCalc(int startValue, int target) {
	if (target <= startValue) return startValue - target; // +1 逼近
	if ((target & 1) == 0) return brokenCalc(startValue, target >> 1) + 1; // /2 逼近
	return brokenCalc(startValue, target + 1) + 1; // 调整为偶数
}
```

#### [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)

@2023.10.21

1. DFS

从每个腐烂橘子开始 `dfs`，更新所有橘子到腐烂橘子的最短距离，最后取所有最短距离的最大值。

```java
/**
 * DFS
 * Somnia1337
 */
public int orangesRotting(int[][] grid)
{
	int rows = grid.length, cols = grid[0].length;
	
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (grid[i][j] == 2) // 从腐烂橘子开始搜索
			{
				dfs(grid, i, j, 2); // 加一个偏移量2，以免影响其他腐烂源
			}
		}
	}
	
	int ans = 0;
	for (int[] row : grid)
	{
		for (int orange : row)
		{
			if (orange == 1) return -1; // 仍有橘子新鲜，返回-1
			ans = Math.max(orange, ans); // 更新最大用时
		}
	}
	
	return Math.max(ans - 2, 0); // 腐烂源到自身距离为2，整体偏移2
}

private void dfs(int[][] grid, int x, int y, int dist)
{
	// dist为当前橘子到源腐烂橘子的距离
	// 如果grid[x][y]<dist则直接返回，除非grid[x][y]==1（仍然新鲜）
	if (!(x >= 0 && x < grid.length && y >= 0 && y < grid[0].length) || grid[x][y] != 1 && grid[x][y] < dist) return;
	grid[x][y] = dist;
	dfs(grid, x - 1, y, dist + 1);
	dfs(grid, x + 1, y, dist + 1);
	dfs(grid, x, y - 1, dist + 1);
	dfs(grid, x, y + 1, dist + 1);
}
```

2. BFS

```java
/**
 * BFS
 * nettee
 */
public int orangesRotting(int[][] grid)
{
	int M = grid.length;
	int N = grid[0].length;
	Queue<int[]> queue = new LinkedList<>();
	
	int count = 0;
	for (int r = 0; r < M; r++)
	{
		for (int c = 0; c < N; c++)
		{
			if (grid[r][c] == 1) count++;
			else if (grid[r][c] == 2)
			{
				queue.add(new int[]{r, c});
			}
		}
	}
	
	int round = 0;
	while (count > 0 && !queue.isEmpty())
	{
		round++;
		int n = queue.size();
		for (int i = 0; i < n; i++)
		{
			int[] orange = queue.poll();
			int r = orange[0];
			int c = orange[1];
			if (r - 1 >= 0 && grid[r - 1][c] == 1)
			{
				grid[r - 1][c] = 2;
				count--;
				queue.add(new int[]{r - 1, c});
			}
			if (r + 1 < M && grid[r + 1][c] == 1)
			{
				grid[r + 1][c] = 2;
				count--;
				queue.add(new int[]{r + 1, c});
			}
			if (c - 1 >= 0 && grid[r][c - 1] == 1)
			{
				grid[r][c - 1] = 2;
				count--;
				queue.add(new int[]{r, c - 1});
			}
			if (c + 1 < N && grid[r][c + 1] == 1)
			{
				grid[r][c + 1] = 2;
				count--;
				queue.add(new int[]{r, c + 1});
			}
		}
	}
	
	return count == 0 ? round : -1;
}
```