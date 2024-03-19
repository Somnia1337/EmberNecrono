```text
题库: 1 9 20 26 28 35 58 66 67 69 70 88 121 125 136 190 191 202 205 217 219 231 242 243 252 258 263 266 268 283 292 293 303 326 338 342 344 345 349 350 367 374 383 387 389 392 409 412 414 415 441 448 455 459 461 463 476 482 485 492 495 496 500 504 977 1108 1394 1450 1455 1464 1470 1480
LCR: 120
```

#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

```go
func twoSum(nums []int, target int) []int {
	idx := make(map[int]int, 0)
	for j, x := range nums {
		if i, ok := idx[target-x]; ok {
			return []int{i, j}
		}
		idx[x] = j
	}
	return nil
}
```

#### [9. 回文数](https://leetcode.cn/problems/palindrome-number/)

```go
func isPalindrome(x int) bool {
	s := strconv.Itoa(x)
	l := len(s)
	for i := 0; i < l; i++ {
		if s[i] != s[l-1-i] {
			return false
		}
	}
	return true
}
```

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

```go
func isValid(s string) bool {
	n := len(s)
	if n%2 == 1 {
		return false
	}
	pairs := map[byte]byte{
		')': '(',
		']': '[',
		'}': '{',
	}
	stk := []byte{}
	for i := 0; i < n; i++ {
		if pairs[s[i]] > 0 {
			if len(stk) == 0 || stk[len(stk)-1] != pairs[s[i]] {
				return false
			}
			stk = stk[:len(stk)-1]
		} else {
			stk = append(stk, s[i])
		}
	}
	return len(stk) == 0
}
```

#### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

```go
func removeDuplicates(nums []int) int {
	n, l, r, ans := len(nums), 0, 0, len(nums)
	for r < n {
		nums[l] = nums[r]
		r++
		for ; r < n && nums[r] == nums[l]; r++ {
			ans--
		}
		l++
	}
	return ans
}
```

```go
func removeDuplicates(nums []int) int {
	n, l := len(nums), 1
	for r := 1; r < n; r++ {
		if nums[r] != nums[r-1] {
			nums[l] = nums[r]
			l++
		}
	}
	return l
}
```

#### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

```go
func strStr(haystack string, needle string) int {
	n1, n2 := len(haystack), len(needle)
	for i := 0; i < n1-n2+1; i++ {
		if haystack[i:i+n2] == needle {
			return i
		}
	}
	return -1
}
```

#### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

```go
func searchInsert(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		m := (l + r) >> 1
		if nums[m] == target {
			return m
		}
		if nums[m] < target {
			l = m + 1
		} else {
			r = m - 1
		}
	}
	return l
}
```

#### [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

```go
func lengthOfLastWord(s string) int {
	i := len(s) - 1
	for s[i] == ' ' {
		i--
	}
	ans := 0
	for ; i >= 0 && s[i] != ' '; i-- {
		ans++
	}
	return ans
}
```

```go
func lengthOfLastWord(s string) int {
	list := strings.Split(s, " ")
	tail := len(list) - 1
	for list[tail] == "" {
		tail--
	}
	return len(list[tail])
}
```

#### [66. 加一](https://leetcode.cn/problems/plus-one/)

```go
func plusOne(digits []int) []int {
	n := len(digits)
	for i := n - 1; i >= 0; i-- {
		digits[i] = (digits[i] + 1) % 10
		if digits[i] > 0 {
			return digits
		}
	}
	digits = make([]int, n+1)
	digits[0] = 1
	return digits
}
```

#### [67. 二进制求和](https://leetcode.cn/problems/add-binary/)

```go
func addBinary(a string, b string) string {
	ai, _ := new(big.Int).SetString(a, 2)
	bi, _ := new(big.Int).SetString(b, 2)
	ai.Add(ai, bi)
	return ai.Text(2)
}
```

```go
func addBinary(a string, b string) string {
	n1, n2 := len(a), len(b)
	n := max(n1, n2)
	ans := ""
	carry := 0
	for i := 0; i < n; i++ {
		if i < n1 {
			carry += int(a[n1-i-1] - '0')
		}
		if i < n2 {
			carry += int(b[n2-i-1] - '0')
		}
		ans = strconv.Itoa(carry&1) + ans
		carry >>= 1
	}
	if carry > 0 {
		ans = "1" + ans
	}
	return ans
}
```

#### [69. x 的平方根](https://leetcode.cn/problems/sqrtx/)

```go
func mySqrt(x int) int {
	l, r := 0, x
	for l <= r {
		m := (l + r) >> 1
		v := m * m
		if v == x {
			return m
		}
		if v < x {
			l = m + 1
		} else {
			r = m - 1
		}
	}
	return r
}
```

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

```go
var memo = make(map[int]int, 0)

func climbStairs(n int) int {
	return help(n)
}

func help(n int) int {
	if n <= 2 {
		return n
	}
	if memo[n] > 0 {
		return memo[n]
	}
	memo[n] = help(n-1) + help(n-2)
	return memo[n]
}
```

```go
func climbStairs(n int) int {
	dp := make([]int, n+1)
	dp[0], dp[1] = 1, 1
	for i := 2; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}
	return dp[n]
}
```

```go
func climbStairs(n int) int {
	if n <= 2 {
		return n
	}
	a, b := 1, 2
	for i := 3; i <= n; i++ {
		hold := b
		b += a
		a = hold
	}
	return b
}
```

#### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	i, j, k := m-1, n-1, m+n-1
	for i >= 0 && j >= 0 {
		if nums1[i] >= nums2[j] {
			nums1[k] = nums1[i]
			i--
		} else {
			nums1[k] = nums2[j]
			j--
		}
		k--
	}
	for j >= 0 {
		nums1[k] = nums2[j]
		k--
		j--
	}
}
```

#### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

```go
func maxProfit(prices []int) int {
	minP, ans := prices[0], 0
	for i := range prices {
		cur := prices[i] - minP
		if prices[i] < minP {
			minP = prices[i]
		} else if cur > ans {
			ans = cur
		}
	}
	return ans
}
```

```go
func maxProfit(prices []int) int {
	minP, ans := prices[0], 0
	for _, p := range prices {
		ans = max(ans, p-minP)
		minP = min(minP, p)
	}
	return ans
}
```

#### [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

```go
func isPalindrome(s string) bool {
	s = strings.ToLower(s)
	i, j := 0, len(s)-1
	for i < j {
		if !check(s[i]) {
			i++
			continue
		}
		if !check(s[j]) {
			j--
			continue
		}
		if s[i] != s[j] {
			return false
		}
		i++
		j--
	}
	return true
}

func check(c byte) bool {
	return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')
}
```

#### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

```go
func singleNumber(nums []int) int {
	x := 0
	for _, num := range nums {
		x ^= num
	}
	return x
}
```

#### [190. 颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)

```go
func reverseBits(num uint32) uint32 {
	var ans uint32 = 0
	for i := 0; i < 32 && num > 0; i++ {
		ans |= num & 1 << (31 - i)
		num >>= 1
	}
	return ans
}
```

```go
func reverseBits(num uint32) uint32 {
	var ans uint32
	for i := 0; i < 32; i++ {
		ans = (ans << 1) | (num & 1)
		num >>= 1
	}
	return ans
}
```

#### [191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)

```go
func hammingWeight(num uint32) int {
	ans := 0
	for num > 0 {
		if (num & 1) == 1 {
			ans++
		}
		num >>= 1
	}
	return ans
}
```

```go
func hammingWeight(num uint32) int {
	ans := 0
	for num > 0 {
		num &= num - 1
		ans++
	}
	return ans
}
```

#### [202. 快乐数](https://leetcode.cn/problems/happy-number/)

```go
func isHappy(n int) bool {
	mp := make(map[int]bool)
	for n != 1 && !mp[n] {
		n, mp[n] = step(n), true
	}
	return n == 1
}

func step(x int) int {
	ret := 0
	for x > 0 {
		ret += (x % 10) * (x % 10)
		x /= 10
	}
	return ret
}
```

```go
func isHappy(n int) bool {
	s, f := n, step(n)
	for f != 1 && f != s {
		s = step(s)
		f = step(step(f))
	}
	return f == 1
}

func step(x int) int {
	ret := 0
	for x > 0 {
		ret += (x % 10) * (x % 10)
		x /= 10
	}
	return ret
}
```

#### [205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

```go
func isIsomorphic(a string, b string) bool {
	if len(a) != len(b) {
		return false
	}
	for i := 0; i < len(a); i++ {
		if strings.IndexByte(a, a[i]) != strings.IndexByte(b, b[i]) {
			return false
		}
	}
	return true
}
```

#### [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)

```go
func containsDuplicate(nums []int) bool {
	mp := make(map[int]bool)
	for _, num := range nums {
		if mp[num] {
			return true
		}
		mp[num] = true
	}
	return false
}
```

#### [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

```go
func containsNearbyDuplicate(nums []int, k int) bool {
	cnt := make(map[int]int)
	n := len(nums)
	for i := 0; i < min(n, k+1); i++ {
		cnt[nums[i]]++
		if cnt[nums[i]] > 1 {
			return true
		}
	}
	for i := k + 1; i < n; i++ {
		cnt[nums[i-k-1]]--
		cnt[nums[i]]++
		if cnt[nums[i]] > 1 {
			return true
		}
	}
	return false
}
```

#### [231. 2 的幂](https://leetcode.cn/problems/power-of-two/)

```go
func isPowerOfTwo(n int) bool {
	return n > 0 && 1<<31%n == 0
}
```

#### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

```go
func isAnagram(s string, t string) bool {
	count := func(s string) [26]int {
		c := [26]int{}
		for _, ch := range s {
			c[ch-'a']++
		}
		return c
	}
	
	return len(s) == len(t) && count(s) == count(t)
}
```

#### [243. 最短单词距离](https://leetcode.cn/problems/shortest-word-distance/)

```go
func shortestDistance(wordsDict []string, word1 string, word2 string) int {
	n := len(wordsDict)
	a, b, ans := -1, -1, n
	for i := 0; i < n; i++ {
		word := wordsDict[i]
		if word == word1 {
			a = i
		} else if word == word2 {
			b = i
		}
		if a >= 0 && b >= 0 {
			ans = min(abs(b-a), ans)
		}
	}
	return ans
}

func abs(x int) int {
	if x >= 0 {
		return x
	}
	return -x
}
```

#### [252. 会议室](https://leetcode.cn/problems/meeting-rooms/)

```go
func canAttendMeetings(intervals [][]int) bool {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})
	
	for i := 0; i < len(intervals)-1; i++ {
		if intervals[i][1] > intervals[i+1][0] {
			return false
		}
	}
	return true
}
```

#### [258. 各位相加](https://leetcode.cn/problems/add-digits/)

```go
func addDigits(num int) int {
	return (num-1)%9 + 1
}
```

#### [263. 丑数](https://leetcode.cn/problems/ugly-number/)

```go
func isUgly(n int) bool {
	if n == 0 {
		return false
	}
	for n%5 == 0 {
		n /= 5
	}
	for n%3 == 0 {
		n /= 3
	}
	for n%2 == 0 {
		n /= 2
	}
	return n == 1
}
```

#### [266. 回文排列](https://leetcode.cn/problems/palindrome-permutation/)

```go
func canPermutePalindrome(s string) bool {
	count := [128]int{}
	oddCnt := 0
	for i := 0; i < len(s); i++ {
		count[s[i]]++
		if count[s[i]]%2 == 1 {
			oddCnt++
		} else {
			oddCnt--
		}
	}
	return oddCnt <= 1
}
```

#### [268. 丢失的数字](https://leetcode.cn/problems/missing-number/)

```go
func missingNumber(nums []int) int {
	s, e := 0, len(nums)
	for i := 0; i < len(nums); i++ {
		s += nums[i]
		e += i
	}
	return e - s
}
```

#### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

```go
func moveZeroes(nums []int) {
	n := len(nums)
	for l := 0; l < n; l++ {
		if nums[l] == 0 {
			r := l + 1
			for r < n && nums[r] == 0 {
				r++
			}
			if r < n {
				nums[l], nums[r] = nums[r], nums[l]
			} else {
				break
			}
		}
	}
}
```

```go
func moveZeroes(nums []int) {
	for l, r := 0, 0; r < len(nums); r++ {
		if nums[r] != 0 {
			nums[r], nums[l] = nums[l], nums[r]
			l++
		}
	}
}
```

#### [292. Nim 游戏](https://leetcode.cn/problems/nim-game/)

```go
func canWinNim(n int) bool {
	return n%4 > 0
}
```

#### [293. 翻转游戏](https://leetcode.cn/problems/flip-game/)

```go
func generatePossibleNextMoves(currentState string) []string {
	chs, ans := []byte(currentState), make([]string, 0)
	for i := 0; i < len(chs)-1; i++ {
		if chs[i] == '+' && chs[i+1] == '+' {
			t := make([]byte, len(chs))
			copy(t, chs)
			t[i], t[i+1] = '-', '-'
			ans = append(ans, string(t))
		}
	}
	return ans
}
```

#### [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)

```go
type NumArray struct {
	pSum []int
}

func Constructor(nums []int) NumArray {
	nA := NumArray{
		pSum: make([]int, len(nums)+1),
	}
	for i := 1; i < len(nums)+1; i++ {
		nA.pSum[i] = nA.pSum[i-1] + nums[i-1]
	}
	return nA
}

func (this *NumArray) SumRange(i int, j int) int {
	return this.pSum[j+1] - this.pSum[i]
}
```

#### [326. 3 的幂](https://leetcode.cn/problems/power-of-three/)

```go
func isPowerOfThree(n int) bool {
	return n > 0 && 1162261467%n == 0
}
```

#### [338. 比特位计数](https://leetcode.cn/problems/counting-bits/)

```go
func countBits(n int) []int {
	ans := make([]int, n+1)
	for x := 1; x <= n; x++ {
		ans[x] = ans[x>>1] + x&1
	}
	return ans
}
```

#### [342. 4的幂](https://leetcode.cn/problems/power-of-four/)

```go
func isPowerOfFour(n int) bool {
	return n > 0 && n&(n-1) == 0 && n%3 == 1
}
```

#### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

```go
func reverseString(s []byte) {
	n := len(s)
	for i := 0; i < n/2; i++ {
		s[i], s[n-i-1] = s[n-i-1], s[i]
	}
}
```

#### [345. 反转字符串中的元音字母](https://leetcode.cn/problems/reverse-vowels-of-a-string/)

```go
func reverseVowels(s string) string {
	v := "AEIOUaeiou"
	n := len(s)
	l, r := 0, n-1
	chs := []byte(s)
	for ; l < r; l, r = l+1, r-1 {
		for l < r && strings.IndexByte(v, chs[l]) < 0 {
			l++
		}
		for l < r && strings.IndexByte(v, chs[r]) < 0 {
			r--
		}
		chs[l], chs[r] = chs[r], chs[l]
	}
	return string(chs)
}
```

#### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

```go
func intersection(nums1 []int, nums2 []int) []int {
	mp := make(map[int]bool)
	for _, x1 := range nums1 {
		mp[x1] = true
	}
	ans := make([]int, 0)
	for _, x2 := range nums2 {
		if mp[x2] {
			ans = append(ans, x2)
			mp[x2] = false
		}
	}
	return ans
}
```

#### [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

```go
func intersect(nums1 []int, nums2 []int) []int {
	mp := make(map[int]int)
	for _, x1 := range nums1 {
		mp[x1]++
	}
	ans := make([]int, 0)
	for _, x2 := range nums2 {
		if mp[x2] > 0 {
			ans = append(ans, x2)
			mp[x2]--
		}
	}
	return ans
}
```

#### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

```go
func isPerfectSquare(num int) bool {
	l, r := 0, num
	for l <= r {
		m := (l + r) >> 1
		sq := m * m
		if sq == num {
			return true
		}
		if sq < num {
			l = m + 1
		} else {
			r = m - 1
		}
	}
	return false
}
```

#### [374. 猜数字大小](https://leetcode.cn/problems/guess-number-higher-or-lower/)

```go
func guessNumber(n int) int {
	l, r := 1, n
	for l <= r {
		m := (l + r) >> 1
		if guess(m) <= 0 {
			r = m - 1
		} else {
			l = m + 1
		}
	}
	return l
}
```

#### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

```go
func canConstruct(ransomNote string, magazine string) bool {
	count := make([]int, 26)
	for _, c := range magazine {
		count[c-'a']++
	}
	for _, c := range ransomNote {
		if count[c-'a'] == 0 {
			return false
		}
		count[c-'a']--
	}
	return true
}
```

#### [387. 字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/)

```go
func firstUniqChar(s string) int {
	count := make([]int, 26)
	for _, c := range s {
		count[c-'a']++
	}
	for i, c := range s {
		if count[c-'a'] == 1 {
			return i
		}
	}
	return -1
}
```

#### [389. 找不同](https://leetcode.cn/problems/find-the-difference/)

```go
func findTheDifference(s string, t string) byte {
	count := make([]int, 26)
	for i, _ := range s {
		count[s[i]-'a']--
		count[t[i]-'a']++
	}
	count[t[len(s)]-'a']++
	for i, c := range count {
		if c == 1 {
			return byte('a' + i)
		}
	}
	return byte(0)
}
```

#### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

```go
func isSubsequence(s string, t string) bool {
	l1, l2 := len(s), len(t)
	if l1 > l2 {
		return false
	}
	for i1, i2 := 0, 0; i1 < l1; i1, i2 = i1+1, i2+1 {
		for i2 < l2 && t[i2] != s[i1] {
			i2++
		}
		if i2 == l2 {
			return false
		}
	}
	return true
}
```

#### [409. 最长回文串](https://leetcode.cn/problems/longest-palindrome/)

```go
func longestPalindrome(s string) int {
	count := make([]int, 128)
	for _, c := range s {
		count[c]++
	}
	oddCnt, ans := 0, 0
	for _, n := range count {
		if (n & 1) == 1 {
			oddCnt++
		}
		ans += n
	}
	if oddCnt > 1 {
		return ans - oddCnt + 1
	}
	return ans
}
```

#### [412. Fizz Buzz](https://leetcode.cn/problems/fizz-buzz/)

```go
func fizzBuzz(n int) []string {
	ans := make([]string, n)
	for i := 1; i <= n; i++ {
		if i%15 == 0 {
			ans[i-1] = "FizzBuzz"
		} else if i%3 == 0 {
			ans[i-1] = "Fizz"
		} else if i%5 == 0 {
			ans[i-1] = "Buzz"
		} else {
			ans[i-1] = strconv.Itoa(i)
		}
	}
	return ans
}
```

#### [414. 第三大的数](https://leetcode.cn/problems/third-maximum-number/)

```go
func thirdMax(nums []int) int {
	min := math.MinInt64
	a, b, c := min, min, min
	for _, n := range nums {
		if n > a {
			c = b
			b = a
			a = n
		} else if b < n && n < a {
			c = b
			b = n
		} else if c < n && n < b {
			c = n
		}
	}
	if a == min || b == min || c == min {
		return a
	}
	return c
}
```

#### [415. 字符串相加](https://leetcode.cn/problems/add-strings/)

```go
func addStrings(num1 string, num2 string) string {
	ans := ""
	l1, l2 := len(num1), len(num2)
	car := 0
	for i1, i2 := l1-1, l2-1; i1 >= 0 || i2 >= 0 || car > 0; {
		cur := car
		if i1 >= 0 {
			cur += int(num1[i1] - '0')
			i1--
		}
		if i2 >= 0 {
			cur += int(num2[i2] - '0')
			i2--
		}
		ans = string(cur%10+'0') + ans
		car = cur / 10
	}
	if car > 0 {
		ans = string(car+'0') + ans
	}
	return ans
}
```

#### [434. 字符串中的单词数](https://leetcode.cn/problems/number-of-segments-in-a-string/)

API：`strings.Fields()`。

```go
func countSegments(s string) int {
	return len(strings.Fields(s))
}
```

#### [441. 排列硬币](https://leetcode.cn/problems/arranging-coins/)

```go
func arrangeCoins(n int) int {
	l, r := 1, n
	for l <= r {
		m := (l + r) >> 1
		c := (m + 1) * m >> 1
		if c <= n {
			l = m + 1
		} else {
			r = m - 1
		}
	}
	return r
}
```

#### [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

```go
func findDisappearedNumbers(nums []int) []int {
	i := 0
	for i < len(nums) {
		if nums[i] == i+1 {
			i++
			continue
		}
		if nums[i] == nums[nums[i]-1] {
			i++
			continue
		}
		nums[nums[i]-1], nums[i] = nums[i], nums[nums[i]-1]
	}
	ans := make([]int, 0)
	for i, x := range nums {
		if x != i+1 {
			ans = append(ans, i+1)
		}
	}
	return ans
}
```

#### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

```go
func findContentChildren(g []int, s []int) int {
	slices.Sort(g)
	slices.Sort(s)
	j, ans := 0, 0
	for i := 0; i < len(g); i, j, ans = i+1, j+1, ans+1 {
		for j < len(s) && s[j] < g[i] {
			j++
		}
		if j == len(s) {
			break
		}
	}
	return ans
}
```

#### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

```go
func repeatedSubstringPattern(s string) bool {
	l := len(s)
	for n := 1; n <= l/2; n++ {
		if l%n > 0 {
			continue
		}
		if strings.Repeat(s[:n], l/n) == s {
			return true
		}
	}
	return false
}
```

#### [461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)

```go
func hammingDistance(x int, y int) int {
	ans := 0
	for x > 0 || y > 0 {
		if x&1 != y&1 {
			ans++
		}
		x >>= 1
		y >>= 1
	}
	return ans
}
```

```go
func hammingDistance(x int, y int) int {
	xor, ans := x^y, 0
	for xor != 0 {
		if xor&1 != 0 {
			ans++
		}
		xor >>= 1
	}
	return ans
}
```

#### [463. 岛屿的周长](https://leetcode.cn/problems/island-perimeter/)

```go
func islandPerimeter(grid [][]int) int {
	rows, cols, land, connect := len(grid), len(grid[0]), 0, 0
	for _, row := range grid {
		for j, g := range row {
			if g == 1 {
				land++
				if j < cols-1 && row[j+1] == 1 {
					connect++
				}
			}
		}
	}
	for j := 0; j < cols; j++ {
		for i := 0; i < rows-1; i++ {
			if grid[i][j] == 1 && grid[i+1][j] == 1 {
				connect++
			}
		}
	}
	return land*4 - connect*2
}
```

#### [476. 数字的补数](https://leetcode.cn/problems/number-complement/)

```go
func findComplement(num int) int {
	x := 1
	for x < num {
		x = (x+1)*2 - 1
	}
	return x ^ num
}
```

```go
func findComplement(num int) int {
	return (1<<bits.Len32(uint32(num)) - 1) ^ num
}
```

#### [482. 密钥格式化](https://leetcode.cn/problems/license-key-formatting/)

```go
func licenseKeyFormatting(s string, k int) string {
	valid := strings.Join(strings.Split(strings.ToUpper(s), "-"), "")
	if len(valid) == 0 {
		return ""
	}
	l1 := len(valid) % k
	if l1 == 0 {
		l1 = k
	}
	build := strings.Builder{}
	build.WriteString(valid[0:l1] + "-")
	for i := l1; i < len(valid); i += k {
		build.WriteString(valid[i:i+k] + "-")
	}
	ans := build.String()
	return ans[0 : len(ans)-1]
}
```

#### [485. 最大连续 1 的个数](https://leetcode.cn/problems/max-consecutive-ones/)

```go
func findMaxConsecutiveOnes(nums []int) int {
	cur, ans := 0, 0
	for _, x := range nums {
		if x == 1 {
			cur++
			ans = max(cur, ans)
		} else {
			cur = 0
		}
	}
	return ans
}
```

#### [492. 构造矩形](https://leetcode.cn/problems/construct-the-rectangle/)

```go
func constructRectangle(area int) []int {
	w := int(math.Sqrt(float64(area)))
	for area%w > 0 {
		w--
	}
	return []int{area / w, w}
}
```

#### [495. 提莫攻击](https://leetcode.cn/problems/teemo-attacking/)

```go
func findPoisonedDuration(timeSeries []int, duration int) int {
	ans := duration
	for i := 1; i < len(timeSeries); i++ {
		ans += min(timeSeries[i]-timeSeries[i-1], duration)
	}
	return ans
}
```

#### [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
	n1, n2 := len(nums1), len(nums2)
	mp := make(map[int]int)
	for i, x := range nums2 {
		mp[x] = i
	}
	ans := make([]int, n1)
	for i, x := range nums1 {
		ans[i] = -1
		for idx := mp[x]; idx < n2; idx++ {
			if nums2[idx] > x {
				ans[i] = nums2[idx]
				break
			}
		}
	}
	return ans
}
```

#### [500. 键盘行](https://leetcode.cn/problems/keyboard-row/)

```go
func findWords(words []string) (ans []string) {
	const rowIdx = "12210111011122000010020202"
next:
	for _, word := range words {
		idx := rowIdx[unicode.ToLower(rune(word[0]))-'a']
		for _, ch := range word[1:] {
			if rowIdx[unicode.ToLower(ch)-'a'] != idx {
				continue next
			}
		}
		ans = append(ans, word)
	}
	return
}
```

#### [504. 七进制数](https://leetcode.cn/problems/base-7/)

```go
func convertToBase7(num int) string {
	if num == 0 {
		return "0"
	}
	s := []byte{}
	neg := num < 0
	if neg {
		num = -num
	}
	for num > 0 {
		s = append(s, '0'+byte(num%7))
		num /= 7
	}
	if neg {
		s = append(s, '-')
	}
	for i, l := 0, len(s); i < l/2; i++ {
		s[i], s[l-i-1] = s[l-i-1], s[i]
	}
	return string(s)
}
```

#### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

```go
func sortedSquares(nums []int) []int {
	ans := make([]int, len(nums))
	i, l, r := len(nums)-1, 0, len(nums)-1
	for i >= 0 {
		if math.Abs(float64(nums[l])) > math.Abs(float64(nums[r])) {
			ans[i] = nums[l] * nums[l]
			l++
		} else {
			ans[i] = nums[r] * nums[r]
			r--
		}
		i--
	}
	return ans
}
```

#### [1108. IP 地址无效化](https://leetcode.cn/problems/defanging-an-ip-address/)

```go
func defangIPaddr(address string) string {
	return strings.ReplaceAll(address, ".", "[.]")
}
```

#### [1394. 找出数组中的幸运数](https://leetcode.cn/problems/find-lucky-integer-in-an-array/)

```go
func findLucky(arr []int) int {
	mp := make(map[int]int)
	for _, x := range arr {
		mp[x]++
	}
	ans := -1
	for k, v := range mp {
		if k == v {
			ans = max(v, ans)
		}
	}
	return ans
}
```

#### [1450. 在既定时间做作业的学生人数](https://leetcode.cn/problems/number-of-students-doing-homework-at-a-given-time/)

```go
func busyStudent(startTime []int, endTime []int, queryTime int) int {
	ans := 0
	for i := range startTime {
		if queryTime >= startTime[i] && queryTime <= endTime[i] {
			ans++
		}
	}
	return ans
}
```

#### [1455. 检查单词是否为句中其他单词的前缀](https://leetcode.cn/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence/)

```go
func isPrefixOfWord(sentence string, searchWord string) int {
	sp := strings.Split(sentence, " ")
	for i, s := range sp {
		if strings.HasPrefix(s, searchWord) {
			return i + 1
		}
	}
	return -1
}
```

#### [1464. 数组中两元素的最大乘积](https://leetcode.cn/problems/maximum-product-of-two-elements-in-an-array/)

```go
func maxProduct(nums []int) int {
	slices.Sort(nums)
	return max((nums[0]-1)*(nums[1]-1), (nums[len(nums)-1]-1)*(nums[len(nums)-2]-1))
}
```

#### [1470. 重新排列数组](https://leetcode.cn/problems/shuffle-the-array/)

```go
func shuffle(nums []int, n int) []int {
	ans := make([]int, n*2)
	p1, p2 := 0, n
	for i := 0; i < n*2; i += 2 {
		ans[i] = nums[p1]
		ans[i+1] = nums[p2]
		p1++
		p2++
	}
	return ans
}
```

#### [1480. 一维数组的动态和](https://leetcode.cn/problems/running-sum-of-1d-array/)

```go
func runningSum(nums []int) []int {
	ans := make([]int, len(nums))
	ans[0] = nums[0]
	for i := 1; i < len(nums); i++ {
		ans[i] = ans[i-1] + nums[i]
	}
	return ans
}
```

#### [LCR 120. 寻找文件副本](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```go
func findRepeatDocument(documents []int) int {
	for i := 0; i < len(documents); i++ {
		for documents[i] != i {
			if documents[i] == documents[documents[i]] {
				return documents[i]
			}
			documents[i], documents[documents[i]] = documents[documents[i]], documents[i]
		}
	}
	return -1
}
```