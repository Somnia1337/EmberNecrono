```text
题库: 1 20 35 70 136 283
```

#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

@2024.02.14

```rust
pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
	let mut vis = HashMap::new();
	for (i, &x) in nums.iter().enumerate() {
		if let Some(&j) = vis.get(&(target - x)) {
			return vec![i as i32, j as i32];
		}
		vis.insert(x, i);
	}
	unreachable!()
}
```

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

@2024.02.24

```rust
pub fn is_valid(s: String) -> bool {
	let pair: HashMap<char, char> = [(')', '('), (']', '['), ('}', '{')].iter().cloned().collect();
	let mut stk = vec!['0'];
	for c in s.chars() {
		match c {
			'(' | '[' | '{' => stk.push(c),
			r => if stk.pop().unwrap() != *pair.get(&r).unwrap() { return false; },
		}
	}
	stk.len() == 1
}
```

#### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

@2024.02.20

```rust
pub fn search_insert(nums: Vec<i32>, target: i32) -> i32 {
	let (mut l, mut r) = (0, nums.len() - 1);
	while l <= r {
		let m = l + r >> 1;
		if nums[m] == target { return m as i32; }
		if nums[m] < target {
			l = m + 1;
		} else {
			if m == 0 { break; }
			r = m - 1;
		}
	}
	l as i32
}
```

```rust
pub fn search_insert(nums: Vec<i32>, target: i32) -> i32 {
	nums.binary_search(&target).unwrap_or_else(|x| x) as i32
}
```

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

@2024.02.18

```rust
pub fn climb_stairs(n: i32) -> i32 {
	if n <= 2 { return n; }
	let (mut a, mut b) = (1, 2);
	for _ in 2..n {
		let t = b;
		b += a;
		a = t;
	}
	b
}
```

#### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

@2024.02.21

```rust
pub fn single_number(nums: Vec<i32>) -> i32 {
	let mut xor = 0;
	for x in nums { xor ^= x; }
	xor
}
```

#### [169. 多数元素](https://leetcode.cn/problems/majority-element/)

@2024.02.21

```rust
pub fn majority_element(nums: Vec<i32>) -> i32 {
	let mut nums = nums.clone();
	nums.sort_unstable();
	nums[nums.len() / 2]
}
```

```rust
pub fn majority_element(nums: Vec<i32>) -> i32 {
	let (mut cnt, mut ans) = (0, nums[0]);
	for &x in nums.iter() {
		cnt += if ans == x { 1 } else { -1 };
		if cnt < 0 {
			ans = x;
			cnt = 0;
		}
	}
	ans
}
```

#### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

@2024.02.15

```rust
pub fn move_zeroes(nums: &mut Vec<i32>) {
	let mut l = 0;
	for r in 0..nums.len() {
		if nums[r] != 0 {
			nums[l] = nums[r];
			if l != r { nums[r] = 0; }
			l += 1;
		}
	}
}
```