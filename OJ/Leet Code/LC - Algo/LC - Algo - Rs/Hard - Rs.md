```text
题库: 41 42 239
```

#### [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)

@2024.02.20

```rust
pub fn first_missing_positive(nums: Vec<i32>) -> i32 {
	let n = nums.len();
	let mut nums = nums;
	for i in 0..n {
		while nums[i] > 0 && nums[i] <= n as i32 && nums[(nums[i] - 1) as usize] != nums[i] {
			let idx = (nums[i] - 1) as usize;
			nums.swap(i, idx);
		}
	}
	for i in 0..n {
		if nums[i] != (i + 1) as i32 { return (i + 1) as i32; }
	}
	(n + 1) as i32
}
```

#### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

@2024.02.17

```rust
pub fn trap(height: Vec<i32>) -> i32 {
	let (mut y, mut ans) = (1, 0);
	loop {
		let mut cur = -1;
		for i in 0..height.len() {
			if height[i] >= y {
				if cur > -1 { ans += i as i32 - cur - 1; }
				cur = i as i32;
			}
		}
		if cur == -1 { break; }
		y += 1;
	}
	ans
}
```

```rust
pub fn trap(height: Vec<i32>) -> i32 {
	let (mut l, mut r) = (0, height.len() - 1);
	let (mut l_max, mut r_max) = (0, 0);
	let mut ans = 0;
	while l < r {
		l_max = l_max.max(height[l]);
		r_max = r_max.max(height[r]);
		if l_max < r_max {
			ans += l_max - height[l];
			l += 1;
		} else {
			ans += r_max - height[r];
			r -= 1;
		}
	}
	ans
}
```

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

@2024.02.18

```rust
pub fn max_sliding_window(nums: Vec<i32>, k: i32) -> Vec<i32> {
	let (k, mut q, mut ans) = (k as usize, VecDeque::new(), Vec::new());
	for (i, &x) in nums.iter().enumerate() {
		while !q.is_empty() && nums[*q.back().unwrap()] <= x { q.pop_back(); }
		q.push_back(i);
		if i - q[0] >= k { q.pop_front(); }
		if i >= k - 1 { ans.push(nums[q[0]]); }
	}
	ans
}
```