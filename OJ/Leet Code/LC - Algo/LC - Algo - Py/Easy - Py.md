#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

```python
"""
哈希表
Somnia1337
"""
def twoSum(self, nums: List[int], target: int) -> List[int]:
	map = dict()
	for i, num in enumerate(nums):
		if target - num in map:
			return [map[target - num], i]
		map[nums[i]] = i
	return []
```

#### [9. 回文数](https://leetcode.cn/problems/palindrome-number/)

```python
"""
字符串
Somnia1337
"""
def isPalindrome(self, x: int) -> bool:
	s = str(x)
	n = len(s)
	for i in range(n // 2):
		if s[i] != s[n - 1 - i]:
			return False
	return True
```

```python
"""
字符串
Levi_Ackerman
"""
def isPalindrome(self, x: int) -> bool:
	return str(x)[::-1] == str(x)
```

#### [14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

```python
"""
纵向扫描
悠远的苍穹
"""
def longestCommonPrefix(self, strs: List[str]) -> str:
	if not strs:
		return ""
	str0 = min(strs)
	str1 = max(strs)
	for i in range(len(str0)):
		if str0[i] != str1[i]:
			return str0[:i]
	return str0
```

```python
"""
纵向扫描
java牛牛
"""
def longestCommonPrefix(self, strs: List[str]) -> str:
	ans = ""
	for i in zip(*strs):
		if len(set(i)) == 1:
			ans += i[0]
		else:
			break
	return ans
```

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

```python
"""
栈
Krahets
"""
def isValid(self, s: str) -> bool:
	dict = {"{": "}", "[": "]", "(": ")", "?": "?"}
	stack = ["?"]
	for c in s:
		if c in dict:
			stack.append(c)
		elif dict[stack.pop()] != c:
			return False
	return len(stack) == 1
```

#### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

```python
"""
双指针
好饭不怕晚
"""
def removeDuplicates(self, nums: List[int]) -> int:
	if not nums:
		return 0
	i = 0
	for j in range(1, len(nums)):
		if nums[i] != nums[j]:
			i += 1
			nums[i] = nums[j]
	return i + 1
```

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

```python
"""
记忆化搜索
Somnia1337
"""
@cache
def climbStairs(self, n: int) -> int:
	if n <= 2:
		return n
	return self.climbStairs(n - 1) + self.climbStairs(n - 2)
```

#### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

```python
"""
位运算
?
"""
def singleNumber(self, nums: List[int]) -> int:
	return reduce(xor, nums)
```

#### [1640. 能否连接形成数组](https://leetcode.cn/problems/check-array-formation-through-concatenation/)

```python
"""
lambda
清风Python
"""
def canFormArray(self, arr, pieces):
	d = {j: i for i, j in enumerate(arr)}
	return (
		list(chain.from_iterable(sorted(pieces, key=lambda x: d.get(x[0], 0))))
		== arr
	)
```

#### [2103. 环和杆](https://leetcode.cn/problems/rings-and-rods/)

```python
"""
位运算
Somnia1337
"""
def countPoints(self, rings: str) -> int:
	ps = [0] * 10
	for i in range(0, len(rings), 2):
		p = int(rings[i + 1])
		if rings[i] == "R":
			ps[p] |= 1
		elif rings[i] == "G":
			ps[p] |= 1 << 1
		else:
			ps[p] |= 1 << 2
	return ps.count(7)
```

> `all()` 只有在 `iteration` 均为 `True` 时返回 `True`，`sum()` 返回 `iteration` 中 `True` 的个数。

```python
"""
位运算
Alex
"""
def countPoints(self, rings: str) -> int:
	return sum(
		all(rings.count(f"{rgb}{num}") for rgb in "RGB")
		for num in "0123456789"
	)
```

#### [2562. 找出数组的串联值](https://leetcode.cn/problems/find-the-array-concatenation-value/)

```python
"""
模拟
Somnia1337
"""
def findTheArrayConcVal(self, nums: List[int]) -> int:
	ans = 0
	l, r = 0, len(nums) - 1
	while l <= r:
		if l < r:
			a, b = nums[l], nums[r]
			offset = len(str(b))
			ans += a * 10**offset + b
		else:
			ans += nums[l]
		l += 1
		r -= 1
	return ans
```

#### [2652. 倍数求和](https://leetcode.cn/problems/sum-multiples/)

```python
"""
直接计算
Somnia1337
"""
def sumOfMultiples(self, n: int) -> int:
	return sum(i for i in range(1, n + 1) if i % 3 == 0 or i % 5 == 0 or i % 7 == 0)
```

```python
"""
数学
灵茶山艾府
"""
def sumOfMultiples(self, n: int) -> int:
	def s(m: int) -> int:
		return n // m * (n // m + 1) // 2 * m
	
	return s(3) + s(5) + s(7) - s(15) - s(21) - s(35) + s(105)
```

#### [2656. K 个元素的最大和](https://leetcode.cn/problems/maximum-sum-with-exactly-k-elements/)

@2023.11.15

```python
"""
数学
Somnia1337
"""
class Solution:
    def maximizeSum(self, nums: List[int], k: int) -> int:
        return (max(nums) * 2 + k - 1) * k // 2
```

#### [2678. 老人的数目](https://leetcode.cn/problems/number-of-senior-citizens/)

```python
"""
字符串
ylb
"""
def countSeniors(self, details: List[str]) -> int:
	return sum(int(x[11:13]) > 60 for x in details)
```

#### [2788. 按分隔符拆分字符串](https://leetcode.cn/problems/split-strings-by-separator/)

```python
"""
库函数
ylb
"""
def splitWordsBySeparator(self, words: List[str], separator: str) -> List[str]:
	return [s for w in words for s in w.split(separator) if s]
```