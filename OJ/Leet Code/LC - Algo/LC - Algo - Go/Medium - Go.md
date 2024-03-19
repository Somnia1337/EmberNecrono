```text
题库: 3 5 7 11 15 16 2415
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```go
func lengthOfLongestSubstring(s string) int {
	n, l, r, ans := len(s), 0, 0, 0
	count := make([]int, 128)
	for r < n {
		count[s[r]]++
		for count[s[r]] > 1 {
			count[s[l]]--
			l++
		}
		r++
		ans = max(r-l, ans)
	}
	return ans
}
```

#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

```go
func longestPalindrome(s string) string {
	ans := ""
	mx := 0
	for i := range s {
		a, b := palCentered(s, i), palDecentered(s, i)
		if a > mx {
			mx = a
			ans = s[i-a/2 : i+a/2+1]
		}
		if b > mx {
			mx = b
			ans = s[i-b/2+1 : i+b/2+1]
		}
	}
	return ans
}

func palCentered(s string, i int) int {
	n, l, r := len(s), i-1, i+1
	for l >= 0 && r < n {
		if s[l] != s[r] {
			break
		}
		l--
		r++
	}
	return r - l - 1
}

func palDecentered(s string, i int) int {
	n, l, r := len(s), i, i+1
	for l >= 0 && r < n {
		if s[l] != s[r] {
			break
		}
		l--
		r++
	}
	return r - l - 1
}
```

#### [7. 整数反转](https://leetcode.cn/problems/reverse-integer/)

```go
func reverse(x int) int {
	ans := 0
	for x != 0 {
		if ans < math.MinInt32/10 || ans > math.MaxInt32/10 {
			return 0
		}
		ans = ans*10 + x%10
		x /= 10
	}
	return ans
}
```

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

```go
func maxArea(height []int) int {
	l, r, ans := 0, len(height)-1, 0
	for l < r {
		ans = max(min(height[l], height[r])*(r-l), ans)
		if height[l] < height[r] {
			l++
		} else {
			r--
		}
	}
	return ans
}
```

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

```go
func threeSum(nums []int) [][]int {
	slices.Sort(nums)
	ans := make([][]int, 0)
	for i := 0; i < len(nums)-1; i++ {
		l, r := i+1, len(nums)-1
		for l < r {
			t := nums[i] + nums[l] + nums[r]
			if t > 0 {
				r--
			} else if t < 0 {
				l++
			} else {
				ans = append(ans, []int{nums[i], nums[l], nums[r]})
				for l < r && nums[l+1] == nums[l] {
					l++
				}
				for r > l && nums[r-1] == nums[r] {
					r--
				}
				l++
				r--
			}
		}
		for i < len(nums)-1 && nums[i+1] == nums[i] {
			i++
		}
	}
	return ans
}
```

#### [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

```go
func threeSumClosest(nums []int, target int) int {
	slices.Sort(nums)
	mn, ans := math.MaxFloat32, 0
	for i := 0; i < len(nums)-2; i++ {
		l, r := i+1, len(nums)-1
		for l < r {
			s := nums[l] + nums[r] + nums[i]
			if s == target {
				return target
			}
			df := math.Abs(float64(target - s))
			if df < mn {
				mn = df
				ans = s
			}
			if s < target {
				l++
			} else {
				r--
			}
		}
	}
	return ans
}
```

#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

```go
var buttons = []string{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}

func letterCombinations(digits string) (ans []string) {
	n := len(digits)
	if n == 0 {
		return
	}
	path := make([]byte, n)
	var dfs func(int)
	dfs = func(i int) {
		if i == n {
			ans = append(ans, string(path))
			return
		}
		for _, c := range buttons[digits[i]-'2'] {
			path[i] = byte(c)
			dfs(i + 1)
		}
	}
	dfs(0)
	return
}
```

#### [2415. 反转二叉树的奇数层](https://leetcode.cn/problems/reverse-odd-levels-of-binary-tree/)

```go
func reverseOddLevels(root *TreeNode) *TreeNode {
	dfs(root.Left, root.Right, true)
	return root
}

func dfs(node1 *TreeNode, node2 *TreeNode, odd bool) {
	if node1 == nil {
		return
	}
	if odd {
		node1.Val, node2.Val = node2.Val, node1.Val
	}
	dfs(node1.Left, node2.Right, !odd)
	dfs(node1.Right, node2.Left, !odd)
}
```