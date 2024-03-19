```text
题库: 3 11 15 49 53 56 62 72 75 128 189 198 238 279 287 300 322 416 438 560
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

@2024.02.17

```rust
pub fn length_of_longest_substring(s: String) -> i32 {
	let chs = s.as_bytes();
	let (mut l, mut ans) = (0, 0);
	let mut count = vec![0; 128];
	for r in 0..s.len() {
		count[chs[r] as usize] += 1;
		while count[chs[r] as usize] > 1 {
			count[chs[l] as usize] -= 1;
			l += 1;
		}
		ans = ans.max((r - l + 1) as i32);
	}
	ans
}
```

#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

@2024.02.25

```rust
pub fn longest_palindrome(s: String) -> String {
	let chs: Vec<char> = s.chars().collect();
	let (mut st, mut ed) = (0, 0);
	for i in 0..s.len() {
		let l = Self::exp(&chs, i as i32, i).max(Self::exp(&chs, i as i32, i + 1));
		if l > ed - st {
			st = i - (l - 1) / 2;
			ed = i + l / 2;
		}
	}
	s[st..=ed].to_string()
}

fn exp(chs: &Vec<char>, mut l: i32, mut r: usize) -> usize {
	let n = chs.len();
	while l >= 0 && r < n && chs[l as usize] == chs[r] {
		l -= 1;
		r += 1;
	}
	r - l as usize - 1
}
```

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

@2024.02.15

```rust
pub fn max_area(height: Vec<i32>) -> i32 {
	let (mut l, mut r, mut ans) = (0, height.len() - 1, 0);
	while l < r {
		ans = ans.max((r - l) as i32 * (height[l].min(height[r])));
		if height[l] < height[r] { l += 1; } else { r -= 1; }
	}
	ans
}
```

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

@2024.02.15

```rust
pub fn three_sum(mut nums: Vec<i32>) -> Vec<Vec<i32>> {
	nums.sort_unstable();
	let (n, mut ans) = (nums.len(), vec![]);
	for i in 0..n - 2 {
		if i > 0 && nums[i - 1] == nums[i] { continue; }
		let (tar, mut l, mut r) = (-nums[i], i + 1, n - 1);
		while l < r {
			if nums[l] + nums[r] == tar {
				ans.push(vec![nums[i], nums[l], nums[r]]);
				while l < r && nums[l + 1] == nums[l] { l += 1; }
				while r > l && nums[r - 1] == nums[r] { r -= 1; }
				l += 1;
				r -= 1;
			} else if nums[l] + nums[r] < tar { l += 1; } else { r -= 1; }
		}
	}
	ans
}
```

#### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

@2024.02.14

```rust
pub fn group_anagrams(strs: Vec<String>) -> Vec<Vec<String>> {
	let mut group = HashMap::new();
	for s in strs.iter() {
		let mut k = s.clone().into_bytes();
		k.sort();
		group.entry(k).or_insert(vec![]).push(s.clone());
	}
	group.values().cloned().collect()
}
```

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

@2024.02.19

```rust
pub fn max_sub_array(nums: Vec<i32>) -> i32 {
	let (mut sum, mut ans) = (0, i32::MIN);
	for &x in nums.iter() {
		sum += x;
		ans = ans.max(sum);
		sum = 0.max(sum);
	}
	ans
}
```

#### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

@2024.02.19

```rust
pub fn merge(mut intervals: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
	intervals.sort();
	let (mut cur, mut ans) = (vec![intervals[0][0], intervals[0][1]], vec![]);
	for itv in intervals.iter() {
		if itv[0] > cur[1] {
			ans.push(cur);
			cur = itv.clone();
		} else if itv[1] > cur[1] { cur[1] = itv[1]; }
	}
	ans.push(cur);
	ans
}
```

#### [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

@2024.02.24

```rust
pub fn unique_paths(m: i32, n: i32) -> i32 {
	let (m, n) = (m as usize, n as usize);
	let mut dp = vec![vec![0; n]; m];
	for i in 0..m { dp[i][0] = 1; }
	for j in 0..n { dp[0][j] = 1; }
	for i in 1..m {
		for j in 1..n {
			dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
		}
	}
	dp[m - 1][n - 1]
}
```

#### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

@2024.02.25

```rust
pub fn min_distance(word1: String, word2: String) -> i32 {
	let word2 = word2.chars().collect::<Vec<_>>();
	let mut dp = (0..=word2.len() as i32).collect::<Vec<_>>();
	for (i, c1) in word1.chars().enumerate() {
		let mut pre = i as i32;
		dp[0] = pre + 1;
		for (j, &c2) in word2.iter().enumerate() {
			let cur = dp[j + 1];
			if c1 == c2 {
				dp[j + 1] = pre;
			} else {
				dp[j + 1] = dp[j + 1].min(dp[j]) + 1;
				dp[j + 1] = dp[j + 1].min(pre + 1);
			}
			pre = cur;
		}
	}
	dp[word2.len()]
}
```

#### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

@2024.02.22

```rust
pub fn sort_colors(nums: &mut Vec<i32>) {
	let (mut p0, mut p2) = (0, nums.len() - 1);
	let mut i = 0;
	for i in 0..=p2 {
		// 持续将 2 换至右侧, 直到非 2
		while i <= p2 && nums[i] == 2 {
			Self::swap(nums, i, p2);
			if p2 == 0 { break; }
			p2 -= 1;
		}
		// 持续将 0 换至左侧, 直到非 0
		while i >= p0 && nums[i] == 0 {
			Self::swap(nums, i, p0);
			p0 += 1;
		}
	}
}

fn swap(arr: &mut Vec<i32>, i: usize, j: usize) {
	let t = arr[j];
	arr[j] = arr[i];
	arr[i] = t;
}
```

#### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

@2024.02.14

```rust
fn longest_consecutive(nums: Vec<i32>) -> i32 {
	let (mut vis, mut ans) = (HashSet::new(), 0);
	for &x in nums.iter() { vis.insert(x); }
	for &x in vis.iter() {
		if vis.contains(&(x - 1)) { continue; }
		let mut cur = 1;
		while vis.contains(&(x + cur)) { cur += 1; }
		ans = ans.max(cur);
	}
	ans
}
```

#### [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

@2024.02.19

```rust
pub fn rotate(nums: &mut Vec<i32>, k: i32) {
	for _ in 0..(k as usize % nums.len()) {
		let n = nums.pop().unwrap();
		nums.insert(0, n);
	}
}
```

```rust
pub fn rotate(nums: &mut Vec<i32>, k: i32) {
	let n = nums.len();
	let k = k as usize % n;
	nums.reverse();
	nums[0..k].reverse();
	nums[k..n].reverse();
}
```

```rust
pub fn rotate(nums: &mut Vec<i32>, k: i32) {
	let k = k as usize % nums.len();
	nums.rotate_right(k);
}
```

#### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

@2024.02.22

```rust
pub fn rob(nums: Vec<i32>) -> i32 {
	let (mut a, mut b) = (0, 0);
	for x in nums.iter() {
		let hold = b;
		b = b.max(a + x);
		a = hold;
	}
	b
}
```

#### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

@2024.02.20

```rust
pub fn product_except_self(nums: Vec<i32>) -> Vec<i32> {
	let n = nums.len();
	let (mut l, mut r, mut ans) = (1, 1, vec![1; n]);
	for i in 0..n {
		ans[i] *= l;
		ans[n - i - 1] *= r;
		l *= nums[i];
		r *= nums[n - i - 1];
	}
	ans
}
```

#### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

@2024.02.23

```rust
pub fn num_squares(n: i32) -> i32 {
	let n = n as usize;
	let mut dp = vec![0; n + 1];
	for i in 1..n + 1 {
		let (mut min, mut j) = (i32::MAX, 1);
		while j * j <= i {
			min = min.min(dp[i - j * j] + 1);
			j += 1;
		}
		dp[i] = min;
	}
	dp[n]
}
```

```rust
pub fn num_squares(n: i32) -> i32 {
	if Self::is_sq(n) { return 1; }
	if Self::check_4(n) { return 4; }
	let mut i = 1;
	while i * i <= n {
		if Self::is_sq(n - i * i) { return 2; }
		i += 1;
	}
	3
}

fn is_sq(x: i32) -> bool {
	let y = (x as f64).sqrt() as i32;
	y * y == x
}

fn check_4(mut x: i32) -> bool {
	while x % 4 == 0 { x /= 4; }
	x % 8 == 7
}
```

#### [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

@2024.02.24

```rust
pub fn find_duplicate(nums: Vec<i32>) -> i32 {
	let mut vis = vec![false; nums.len() + 1];
	for &x in nums.iter() {
		if vis[x as usize] { return x; }
		vis[x as usize] = true;
	}
	0
}
```

```rust
pub fn find_duplicate(nums: Vec<i32>) -> i32 {
	let (mut slow, mut fast) = (0, 0);
	loop {
		slow = nums[slow] as usize;
		fast = nums[nums[fast] as usize] as usize;
		if slow == fast { break; }
	}
	slow = 0;
	while slow != fast {
		slow = nums[slow] as usize;
		fast = nums[fast] as usize;
	}
	slow as i32
}
```

#### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

@2024.02.23

```rust
pub fn length_of_lis(nums: Vec<i32>) -> i32 {
	let (n, mut ans) = (nums.len(), 1);
	let mut dp = vec![1; n];
	for r in 1..n {
		for l in 0..r {
			if nums[r] > nums[l] { dp[r] = dp[r].max(dp[l] + 1); }
		}
		ans = ans.max(dp[r]);
	}
	ans
}
```

```rust
pub fn length_of_lis(nums: Vec<i32>) -> i32 {
	let mut dp = vec![];
	for &x in nums.iter() {
		dp.binary_search(&x).err().map(|idx| {
			if idx == dp.len() { dp.push(x); } else { dp[idx] = x; }
		});
	}
	dp.len() as i32
}
```

#### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

@2024.02.22

```rust
pub fn coin_change(coins: Vec<i32>, amount: i32) -> i32 {
	let mut dp = vec![i32::MAX; (amount + 1) as usize];
	dp[0] = 0;
	for &coin in coins.iter() {
		for i in coin..=amount {
			if dp[(i - coin) as usize] < i32::MAX {
				dp[i as usize] = dp[i as usize].min(dp[(i - coin) as usize] + 1);
			}
		}
	}
	if dp[amount as usize] < i32::MAX { dp[amount as usize] } else { -1 }
}
```

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

@2024.02.23

```rust
pub fn can_partition(nums: Vec<i32>) -> bool {
	let sum: i32 = nums.iter().sum();
	if sum % 2 == 1 { return false; }
	let half = sum / 2;
	let sum = sum as usize;
	let mut dp: Vec<i32> = vec![0; sum / 2 + 1];
	for &x in nums.iter() {
		for j in (1..=half).rev() {
			if j - x >= 0 {
				let j = j as usize;
				dp[j] = dp[j].max(dp[j - x as usize] + x);
			}
		}
		if dp[sum / 2] == half { return true; }
	}
	dp[sum / 2] == half
}
```

#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

@2024.02.17

```rust
pub fn find_anagrams(s: String, p: String) -> Vec<i32> {
	let (mut p_cnt, mut ans) = ([0; 26], vec![]);
	for c in p.bytes() { p_cnt[(c - b'a') as usize] += 1; }
	for (i, w) in s.as_bytes().windows(p.len()).enumerate() {
		if i == 0 {
			for &b in w.iter() { p_cnt[(b - b'a') as usize] -= 1; }
		} else {
			p_cnt[(*w.last().unwrap() - b'a') as usize] -= 1;
		}
		if p_cnt.iter().all(|&cnt| cnt == 0) { ans.push(i as i32); }
		p_cnt[(*w.first().unwrap() - b'a') as usize] += 1;
	}
	ans
}
```

#### [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

@2024.02.18

```rust
pub fn subarray_sum(nums: Vec<i32>, k: i32) -> i32 {
	let mut count = HashMap::new();
	count.insert(0, 1);
	let (mut sum, mut ans) = (0, 0);
	for &x in nums.iter() {
		sum += x;
		ans += count.get(&(sum - k)).unwrap_or(&0);
		*count.entry(sum).or_insert(0) += 1;
	}
	ans
}
```