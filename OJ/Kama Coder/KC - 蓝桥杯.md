### [# 10](https://kamacoder.com/contest.php?cid=1030)

### [# 11](https://kamacoder.com/contest.php?cid=1033)

#### [A. 冶炼金属](https://kamacoder.com/problem.php?id=1109)

```java
int T = Integer.parseInt(in.readLine()), min = Integer.MIN_VALUE, max = Integer.MAX_VALUE;
while (T-- > 0) {
	StringTokenizer st = new StringTokenizer(in.readLine());
	int a = Integer.parseInt(st.nextToken()), b = Integer.parseInt(st.nextToken());
	min = (int) Math.max(Math.ceil((double) a / (b + 1)), min);
	max = Math.min(a / b, max);
}
out.println(min + " " + max);
```

#### [C. 接龙数列](https://kamacoder.com/problempage.php?pid=1111)

```java
int n = Integer.parseInt(in.readLine());
int[] nums = new int[n];
StringTokenizer st = new StringTokenizer(in.readLine());
for (int i = 0; i < n; i++) nums[i] = Integer.parseInt(st.nextToken());
int[] dp = new int[n];
Arrays.fill(dp, 1);
int max = 0;
for (int i = 1; i < n; i++) {
	for (int j = i - 1; j >= 0; j--) {
		if (check(nums[j], nums[i])) dp[i] = Math.max(dp[j] + 1, dp[i]);
	}
	max = Math.max(dp[i], max);
}
out.println(n - max);
```

#### [E. 子串简写](https://kamacoder.com/problem.php?id=1113)

```java
int k = Integer.parseInt(in.readLine());
StringTokenizer st = new StringTokenizer(in.readLine());
char[] chs = st.nextToken().toCharArray();
char c1 = st.nextToken().charAt(0), c2 = st.nextToken().charAt(0);
int l = 0, r = k - 1, cnt = 0, ans = 0;
while (r < chs.length) {
	if (chs[l++] == c1) cnt++;
	if (chs[r++] == c2) ans += cnt;
}
out.println(ans);
```

#### [F. 整数删除](https://kamacoder.com/problem.php?id=1114)

```java
StringTokenizer st = new StringTokenizer(in.readLine());
int n = Integer.parseInt(st.nextToken()), k = Integer.parseInt(st.nextToken());
int[] nums = new int[n];
st = new StringTokenizer(in.readLine());
for (int i = 0; i < n; i++) nums[i] = Integer.parseInt(st.nextToken());
while (k-- > 0) {
	int min = Integer.MAX_VALUE, idx = -1;
	for (int i = 0; i < n; i++) {
		if (nums[i] < min) {
			min = nums[i];
			idx = i;
		}
	}
	int l = idx - 1, r = idx + 1;
	while (l >= 0 && nums[l] == Integer.MAX_VALUE) l--;
	if (l >= 0) nums[l] += nums[idx];
	while (r < n && nums[r] == Integer.MAX_VALUE) r++;
	if (r < n) nums[r] += nums[idx];
	nums[idx] = Integer.MAX_VALUE;
}
StringBuilder ans = new StringBuilder();
for (int x : nums) {
	if (x < Integer.MAX_VALUE) ans.append(x).append(" ");
}
out.println(ans);
```