```text
329 458 1153 1392 1416 2646 2845 3031 100246
```

#### [329. 矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/)

```python
"""

Somnia1337
"""
def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
	def dfs(x: int, y: int) -> int:
		if vis[x][y] > 0:
			return vis[x][y]
		ret = max((dfs(x + dirs[k], y + dirs[k + 1]) for k in range(4) if 0 <= x + dirs[k] < rows and 0 <= y + dirs[k + 1] < cols and matrix[x + dirs[k]][y + dirs[k + 1]] < matrix[x][y]), default=0)
		vis[x][y] = ret + 1
		return ret + 1
	
	dirs = [0, 1, 0, -1, 0]
	rows, cols, ans = len(matrix), len(matrix[0]), 1
	vis = [[0 for _ in range(cols)] for _ in range(rows)]
	for i in range(rows):
		for j in range(cols):
			if not vis[i][j]:
				ans = max(dfs(i, j), ans)
	return ans
```

#### [458. 可怜的小猪](https://leetcode.cn/problems/poor-pigs/)

```python
"""
数学
powcai
"""
def poorPigs(self, buckets: int, minutesToDie: int, minutesToTest: int) -> int:
	pigs = 0
	while (minutesToTest // minutesToDie + 1) ** pigs < buckets:
		pigs += 1
	return pigs
```

#### [1153. 字符串转化](https://leetcode.cn/problems/string-transforms-into-another-string/)

```python
"""
库函数
lan
"""
def canConvert(self, str1: str, str2: str) -> bool:
	return str1 == str2 or (len(set(zip(str1, str2))) == len(set(str1)) and len(set(str2)) < 26)
```

#### [1392. 最长快乐前缀](https://leetcode.cn/problems/longest-happy-prefix/)

```python
"""
暴力
roeexu
"""
def longestPrefix(self, s: str) -> str:
	n = len(s)
	for i in range(n - 1, 0, -1):
		if s.endswith(s[:i]):
			return s[-i:]
	return ""
```

#### [1416. 恢复数组](https://leetcode.cn/problems/restore-the-array/)

```python
"""
记忆化搜索
草莓奶昔🍓
"""
def numberOfArrays(self, s: str, k: int) -> int:
	@cache
	def dfs(i):
		if i == len(s):
			return 1
		if s[i] == '0':
			return 0
		ret = 0
		for j in range(i, len(s)):
			if int(s[i: j + 1]) > k:
				break
			ret += dfs(j + 1) % MOD
		return ret
	
	MOD = 1000000007
	return dfs(0) % MOD
```

#### [2646. 最小化旅行的价格总和](https://leetcode.cn/problems/minimize-the-total-price-of-the-trips/)

```python
"""
DFS
灵茶山艾府
"""
def minimumTotalPrice(
	self, n: int, edges: List[List[int]], price: List[int], trips: List[List[int]]
) -> int:
	g = [[] for _ in range(n)]
	for x, y in edges:
		g[x].append(y)
		g[y].append(x)
	
	cnt = [0] * n
	for start, end in trips:
		
		def dfs(x: int, fa: int) -> bool:  # type: ignore
			if x == end:
				cnt[x] += 1
				return True  # 找到 end
			for y in g[x]:
				if y != fa and dfs(y, x):
					cnt[x] += 1  # x 是 end 的祖先节点，也就在路径上
					return True
			return False  # 未找到 end
		
		dfs(start, -1)

	# 类似 337. 打家劫舍 III
	def dfs(x: int, fa: int) -> (int, int):  # type: ignore
		not_halve = price[x] * cnt[x]  # x 不变
		halve = not_halve // 2  # x 减半
		for y in g[x]:
			if y != fa:
				nh, h = dfs(y, x)  # 计算 y 不变/减半的最小价值总和
				not_halve += min(nh, h)  # x 不变，那么 y 可以不变或者减半，取这两种情况的最小值
				halve += nh  # x 减半，那么 y 只能不变
		return not_halve, halve
	
	return min(dfs(0, -1))
```

#### [2845. 统计趣味子数组的数目](https://leetcode.cn/problems/count-of-interesting-subarrays/)

```python
"""
前缀和
灵茶山艾府
"""
def countInterestingSubarrays(self, nums: List[int], mod: int, k: int) -> int:
	cnt = Counter([0])
	ans = pre = 0
	for x in nums:
		pre += x % mod == k
		ans += cnt[(pre - k) % mod]
		cnt[pre % mod] += 1
	return ans
```

#### [100203. 将单词恢复初始状态所需的最短时间 II](https://leetcode.cn/problems/minimum-time-to-revert-word-to-initial-state-ii/)

```python
"""
枚举
RockyHaotianDu
"""
def minimumTimeToInitialState(self, word: str, k: int) -> int:
	ans, s = 1, word[k:]
	while not word.startswith(s):
		s = s[k:]
		ans += 1
	return ans
```

#### [100246. 将元素分配到两个数组中 II](https://leetcode.cn/problems/distribute-elements-into-two-arrays-ii/)

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