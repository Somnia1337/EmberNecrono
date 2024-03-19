#### [78. 子集](https://leetcode.cn/problems/subsets/)

```python
"""
回溯
秋水中的鱼
"""
def subsets(self, nums: List[int]) -> List[List[int]]:
	ans = [[]]
	n = len(nums)
	for i in range(n):
		for j in range(len(ans)):
			ans.append(ans[j] + [nums[i]])
	return ans
```

```python
"""
回溯
?
"""
def subsets(self, nums: List[int]) -> List[List[int]]:
	ans = []
	n = len(nums)
	
	def helper(i, tmp):
		ans.append(tmp)
		for j in range(i, n):
			helper(j + 1, tmp + [nums[j]])
	
	helper(0, [])
	return ans
```

#### [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

```python
"""
库函数
Somnia1337
"""
def reverseWords(self, s: str) -> str:
	return " ".join(s.strip().split()[::-1])
```

#### [179. 最大数](https://leetcode.cn/problems/largest-number/)

```python
"""
排序
maomao
"""
def largestNumber(self, nums: List[int]) -> str:
	def compare(x, y):
		return int(y + x) - int(x + y)
	
	nums = sorted(map(str, nums), key=cmp_to_key(compare))
	return "0" if nums[0] == "0" else "".join(nums)
```

#### [241. 为运算表达式设计优先级](https://leetcode.cn/problems/different-ways-to-add-parentheses/)

```python
"""
分治
小毛驴
"""
def diffWaysToCompute(self, expression: str) -> List[int]:
	if len(expression) <= 2:
		return [int(expression)]
	
	ret = []
	for i in range(len(expression)):
		if not expression[i].isdigit():
			left = self.diffWaysToCompute(expression[0:i])
			right = self.diffWaysToCompute(expression[i + 1 :])
			for a in left:
				for b in right:
					if expression[i] == "+":
						ret.append(a + b)
					elif expression[i] == "-":
						ret.append(a - b)
					else:
						ret.append(a * b)
	
	return ret
```

#### [274. H 指数](https://leetcode.cn/problems/h-index/)

@2023.10.12

```python
"""
找规律
Somnia1337
"""
def hIndex(self, citations: List[int]) -> int:
	citations.sort()
	return max(min(len(citations) - i, citations[i]) for i in range(0, len(citations)))
```

#### [275. H 指数 II](https://leetcode.cn/problems/h-index-ii/)

@2023.10.30

```python
"""
找规律
Somnia1337
"""
def hIndex(self, citations: List[int]) -> int:
	return max(min(len(citations) - i, citations[i]) for i in range(0, len(citations)))
```

```python
"""
二分查找
Somnia1337
"""
def hIndex(self, citations: List[int]) -> int:
	l, r = 1, len(citations)
	while l <= r:
		mid = (l + r) // 2
		if citations[-mid] >= mid:
			l = mid + 1
		else:
			r = mid - 1
	return r
```

#### [306. 累加数](https://leetcode.cn/problems/additive-number/)

```python
"""
字符串
Aganzo
"""
def isAdditiveNumber(self, num: str) -> bool:
	def check(a, b):
		if (a.startswith("0") and a != "0") or (b.startswith("0") and b != "0"):
			return False
		temp = a + b
		a, b = b, str(int(b) + int(a))
		temp += b
		while len(temp) < len(num):
			a, b = b, str(int(b) + int(a))
			temp += b
		return num == temp
	
	for j in range(1, len(num) - 1):
		for i in range(j):
			if check(num[: i + 1], num[i + 1 : j + 1]):
				return True
	return False
```

#### [365. 水壶问题](https://leetcode.cn/problems/water-and-jug-problem/)

```python
"""
数学
力扣官方题解
"""
def canMeasureWater(self, x: int, y: int, z: int) -> bool:
	if x + y < z:
		return False
	if x == 0 or y == 0:
		return z == 0 or x + y == z
	return z % math.gcd(x, y) == 0
```

#### [368. 最大整除子集](https://leetcode.cn/problems/largest-divisible-subset/)

```python
"""
动态规划
maomao
"""
def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
	nums.sort()
	f = [[x] for x in nums]
	for j in range(len(nums)):
		for i in range(j):
			if nums[j] % nums[i] == 0 and len(f[i]) + 1 > len(f[j]):
				f[j] = f[i] + [nums[j]]
	return max(f, key=len)
```

#### [372. 超级次方](https://leetcode.cn/problems/super-pow/)

```python
"""
库函数
Benhao
"""
def superPow(self, a: int, b: List[int]) -> int:
	return pow(a, int("".join(map(str, b))), 1337)
```

```python
"""
快速幂
Benhao
"""
def superPow(self, a: int, b: List[int]) -> int:
	MOD = 1337
	
	def dfs(i):
		if i == -1:
			return 1
		return quickPow(dfs(i - 1), 10) * quickPow(a, b[i]) % MOD
	
	def quickPow(x, y):
		ans = 1
		x %= MOD
		while y:
			if y & 1:
				ans = ans * x % MOD
			x = x * x % MOD
			y >>= 1
		return ans
	
	a %= MOD
	return dfs(len(b) - 1)
```

#### [395. 至少有 K 个重复字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/)

```python
"""
分治
gyx2110
"""
def longestSubstring(self, s: str, k: int) -> int:
	for c in set(s):
		if s.count(c) < k:
			return max(self.longestSubstring(t, k) for t in s.split(c))
	return len(s)
```

#### [453. 最小操作次数使数组元素相等](https://leetcode.cn/problems/minimum-moves-to-equal-array-elements/)

```python
"""
正难则反
Somnia1337
"""
def minMoves(self, nums: List[int]) -> int:
	m = min(nums)
	ans = 0
	for n in nums:
		ans += n - m
	return ans
```

```python
"""
正难则反
生气的老虎虎虎生风
"""
def minMoves(self, nums: List[int]) -> int:
	return sum(nums) - len(nums) * min(nums)
```

#### [462. 最小操作次数使数组元素相等 II](https://leetcode.cn/problems/minimum-moves-to-equal-array-elements-ii/)

```python
"""
数学
Somnia1337
"""
def minMoves2(self, nums: List[int]) -> int:
	nums = sorted(nums)
	ans, tar = 0, nums[len(nums) // 2]
	for num in nums:
		ans += abs(tar - num)
	return ans
```

```python
"""
数学
力扣官方题解
"""
def minMoves2(self, nums: List[int]) -> int:
	nums.sort()
	return sum(abs(num - nums[len(nums) // 2]) for num in nums)
```

```python
"""
数学
Benhao
"""
def minMoves2(self, nums: List[int]) -> int:
	return (
		sum(abs(mid - num) for num in nums)
		if (mid := sorted(nums)[len(nums) // 2]) != inf
		else inf
	)
```

#### [477. 汉明距离总和](https://leetcode.cn/problems/total-hamming-distance/)

```python
"""
位运算
Somnia1337
"""
def totalHammingDistance(self, nums: List[int]) -> int:
	ans = 0
	for i in range(0, 31):
		z = 0
		for n in nums:
			z += (n >> i) & 1
		ans += z * (len(nums) - z)
	return ans
```

#### [486. 预测赢家](https://leetcode.cn/problems/predict-the-winner/)

```python
"""
DFS
Somnia1337
"""
def predictTheWinner(self, nums: List[int]) -> bool:
	def solve(l: int, r: int, a: int, b: int, turn: bool) -> bool:
		if l > r:
			return a >= b
		if turn:
			return solve(l + 1, r, a + nums[l], b, False) or solve(l, r - 1, a + nums[r], b, False)
		return solve(l + 1, r, a, b + nums[l], True) and solve(l, r - 1, a, b + nums[r], True)
	
	if len(nums) == 1:
		return True
	return solve(0, len(nums) - 1, 0, 0, True)
```

#### [524. 通过删除字母匹配到字典里最长单词](https://leetcode.cn/problems/longest-word-in-dictionary-through-deleting/)

```python
"""
自定义排序
typingMonkey
"""
def findLongestWord(self, s: str, d: List[str]) -> str:
	def f(c):
		i = 0
		for j in c:
			if (i := s.find(j, i)) == -1:
				return True
			i += 1
	
	d.sort(key=lambda x: (-len(x), x))
	return next((c for c in d if not f(c)), "")
```

#### [525. 连续数组](https://leetcode.cn/problems/contiguous-array/)

```python
"""
前缀和 + 哈希表
Somnia1337
"""
def findMaxLength(self, nums: List[int]) -> int:
	indices = {0: -1}  # 定义-1处的前缀和为0
	preSum = 0
	ans = 0
	
	for i in range(len(nums)):
		preSum += 1 if nums[i] == 1 else -1  # 计算前缀和
		if preSum in indices:
			ans = max(ans, i - indices[preSum])
		else:
			indices[preSum] = i
	
	return ans
```

#### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

@2023.10.30

```python
"""
并查集
谁是谁的小王子
"""
def findCircleNum(self, isConnected: List[List[int]]) -> int:
	n = len(isConnected)
	pre = [-1] * n
	
	def find(root):
		son = root
		while pre[root] >= 0:
			root = pre[root]
		while son != root:
			tmp = pre[son]
			pre[son] = root
			son = tmp
		return root
	
	def union(root1, root2):
		if pre[root2] < pre[root1]:
			pre[root2] += pre[root1]
			pre[root1] = root2
		else:
			pre[root1] += pre[root2]
			pre[root2] = root1
	
	for i in range(n):
		for j in range(i + 1, n):
			if isConnected[i][j] == 1:
				root1 = find(i)
				root2 = find(j)
				if root1 != root2:
					union(root1, root2)
	
	ans = 0
	for i in range(n):
		if pre[i] < 0:
			ans += 1
	return ans
```

#### [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)

```python
"""
排序
Somnia1337
"""
def findUnsortedSubarray(self, nums: List[int]) -> int:
	sorted_nums = sorted(nums)
	start, end = 0, len(nums) - 1
	
	while start < len(nums) and nums[start] == sorted_nums[start]:
		start += 1
	while end > start and nums[end] == sorted_nums[end]:
		end -= 1
	
	return end - start + 1
```

```python
"""
一次遍历
力扣官方题解
"""
def findUnsortedSubarray(self, nums: List[int]) -> int:
	n = len(nums)
	maxn, right = float("-inf"), -1
	minn, left = float("inf"), -1
	
	for i in range(n):
		if maxn > nums[i]:
			right = i
		else:
			maxn = nums[i]
		if minn < nums[n - i - 1]:
			left = n - i - 1
		else:
			minn = nums[n - i - 1]
	
	return 0 if right == -1 else right - left + 1
```

#### [609. 在系统中查找重复文件](https://leetcode.cn/problems/find-duplicate-file-in-system/)

```python
"""
哈希表
azhi
"""
def findDuplicate(self, paths: List[str]) -> List[List[str]]:
	tmp = defaultdict(list)
	
	for f in paths:
		f = f.split(" ")
		for t in f[1:]:
			t = t.split("(")
			tmp[t[1]].append(f[0] + "/" + t[0])
	
	return [i for i in tmp.values() if len(i) > 1]
```

#### [621. 任务调度器](https://leetcode.cn/problems/task-scheduler/)

```python
"""
贪心
MingZhang
"""
def leastInterval(self, tasks: List[str], n: int) -> int:
	dict_count = list(collections.Counter(tasks).values())
	maxn = max(dict_count)
	max_cnt = dict_count.count(maxn)
	return max((maxn - 1) * (n + 1) + max_cnt, len(tasks))
```

#### [634. 寻找数组的错位排列](https://leetcode.cn/problems/find-the-derangement-of-an-array/)

```python
"""
动态规划
Somnia1337
"""
def findDerangement(self, n: int) -> int:
	f = [0 for _ in range(0, 3)]
	f[0] = 1
	if n == 0:
		return 1
	if n == 1:
		return 0
	for i in range(2, n + 1):
		f[2] = (i - 1) * (f[1] + f[0]) % 1000000007
		f[0] = f[1]
		f[1] = f[2]
	return f[2]
```

#### [684. 冗余连接](https://leetcode.cn/problems/redundant-connection/)

```python
"""
并查集
Yuanhao
"""
class Solution:
	def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
	n = len(edges)
	root = list(range(n + 1))
	
	def find(i: int) -> int:
		if root[i] != i:
			root[i] = find(root[i])
		return root[i]
	
	def union(n1, n2):
		root[find(n1)] = find(n2)
	
	for n1, n2 in edges:
		if find(n1) != find(n2):
			union(n1, n2)
		else:
			return [n1, n2]
	return []
```

#### [686. 重复叠加字符串匹配](https://leetcode.cn/problems/repeated-string-match/)

```python
"""
哈希表
空白白喵白。oO
"""
def repeatedStringMatch(self, a: str, b: str) -> int:
	aS, bS = set(a), set(b)
	if aS & bS != bS:
		return -1
	
	times = math.ceil(len(b) / len(a))
	if b in a * times:
		return times
	elif b in a * (times + 1):
		return times + 1
	else:
		return -1
```

#### [737. 句子相似性 II](https://leetcode.cn/problems/sentence-similarity-ii/)

@2023.11.15

```python
"""
并查集
两面包夹芝士
"""
class Solution:
    def areSentencesSimilarTwo(self, sentence1: List[str], sentence2: List[str], similarPairs: List[List[str]]) -> bool:
        def find(i):
            if i not in root:
                root[i] = i
            elif root[i] != i:
                root[i] = find(root[i])
            return root[i]
		
        def union(i, j):
            ri, rj = find(i), find(j)
            if ri != rj:
                root[rj] = ri
        
		m, n = len(sentence1), len(sentence2)
        if m != n:
            return False
        root = {}
        for i, j in similarPairs:
            union(i, j)
        for i in range(n):
            if find(sentence2[i]) != find(sentence1[i]):
                return False
        return True
```

#### [791. 自定义字符串排序](https://leetcode.cn/problems/custom-sort-string/)

```python
"""
自定义排序
灵茶山艾府
"""
def customSortString(self, order: str, s: str) -> str:
	val = {c: i for i, c in enumerate(order, 1)}
	return "".join(sorted(s, key=lambda c: val.get(c, 0)))
```

#### [846. 一手顺子](https://leetcode.cn/problems/hand-of-straights/)

```python
"""
贪心
Somnia1337
"""
def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
	if len(hand) % groupSize > 0:
		return False
	count = Counter(hand)
	hand.sort()
	for card in hand:
		if count[card] == 0:
			continue
		curSize = 0
		while curSize < groupSize and count.get(card, 0) > 0:
			count[card] -= 1
			card += 1
			curSize += 1
		if curSize != groupSize:
			return False
	return True
```

#### [856. 括号的分数](https://leetcode.cn/problems/score-of-parentheses/)

```python
"""
库函数
lsc
"""
def scoreOfParentheses(self, s: str) -> int:
	return eval(s.replace(")(", ")+(").replace("()", "1").replace("(", "2*("))
```

#### [1969. 数组元素的最小非零乘积](https://leetcode.cn/problems/minimum-non-zero-product-of-the-array-elements/)

```python
"""
s数学
Somnia1337
"""
def minNonZeroProduct(self, p: int) -> int:
	MOD = 1000000007
	return pow((1 << p) - 2, (1 << (p - 1)) - 1, MOD) * (((1 << p) - 1) % MOD) % MOD
```

#### [2171. 拿出最少数目的魔法豆](https://leetcode.cn/problems/removing-minimum-number-of-magic-beans/)

```python
"""
前缀和
灵茶山艾府
"""
def minimumRemoval(self, beans: List[int]) -> int:
	return sum(beans) - max((len(beans) - i) * v for i, v in enumerate(sorted(beans)))
```

#### [2992. 自整除排列的数量](https://leetcode.cn/problems/number-of-self-divisible-permutations/)

```python
"""
回溯
丁飞
"""
def selfDivisiblePermutationCount(self, n: int) -> int:
	def dfs(idx):
		if idx == 0:
			self.ans += 1
		else:
			for x in list(lst):
				if gcd(idx, x) == 1:
					lst.remove(x)
					dfs(idx - 1)
					lst.add(x)
	
	lst, self.ans = set(range(1, n + 1)), 0
	dfs(n)
	return self.ans
```

#### [2997. 使数组异或和等于 K 的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-xor-equal-to-k/)

```python
"""
位运算
Somnia1337
"""
def minOperations(self, nums: List[int], k: int) -> int:
	return (reduce(xor, nums) ^ k).bit_count()
```

#### [2998. 使 X 和 Y 相等的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-x-and-y-equal/)

1. BFS

```python
"""
BFS
Somnia1337
"""
def minimumOperationsToMakeEqual(self, x: int, y: int) -> int:
	dp = {x: 0}
	q = [x]
	while q:
		p = q.pop(0)
		if p == y:
			return dp[p]
		next = [p + 1, p - 1]
		if p % 5 == 0:
			next.append(p / 5)
		if p % 11 == 0:
			next.append(p / 11)
		for v in next:
			if v not in dp or dp[v] > dp[p] + 1:
				dp[v] = dp[p] + 1
				q.append(v)
	return -1
```

2. 记忆化搜索

```python
"""
记忆化搜索
lsf
"""
def minimumOperationsToMakeEqual(self, x: int, y: int) -> int:
	@cache
	def f(x):
		if x <= y:
			return y - x
		t = x - y
		for i in (11, 5):
			d, m = divmod(x, i)
			t = min(t, m + 1 + f(d), i - m + 1 + f(d + 1))
		return t
	
	return f(x)
```

#### [3002. 移除后集合的最多元素数](https://leetcode.cn/problems/maximum-size-of-a-set-after-removals/)

```python
"""
哈希表
?
"""
def maximumSetSize(self, nums1: List[int], nums2: List[int]) -> int:
	n = len(nums1)
	s, s1, s2 = set(), set(), set()
	
	for i in nums1:
		s.add(i)
		s1.add(i)
	for i in nums2:
		s.add(i)
		s2.add(i)
	
	a, b = len(s), min(n // 2, len(s1)) + min(n // 2, len(s2))
	return min(min(a, b), n)
```

#### [3012. 通过操作使数组长度最小](https://leetcode.cn/problems/minimize-length-of-array-using-operations/)

```python
"""
脑筋急转弯
Somnia1337
"""
def minimumArrayLength(self, nums: List[int]) -> int:
	mn = min(nums)
	for x in nums:
		if x % mn: return 1
	x = nums.count(mn)
	return (x + 1) // 2
```