#### [C. 求和](https://www.dotcpp.com/oj/problem2664.html)

```java
int n = Integer.parseInt(in.readLine());
StringTokenizer st = new StringTokenizer(in.readLine());
long pre = 0, ans = 0;
for (int i = 0; i < n; i++) {
	int cur = Integer.parseInt(st.nextToken());
	ans += pre * cur;
	pre += cur;
}
out.println(ans);
```

#### [D. GCD](https://www.dotcpp.com/oj/problem2682.html)

```java
StringTokenizer st = new StringTokenizer(in.readLine());
long a = Long.parseLong(st.nextToken()), b = Long.parseLong(st.nextToken()), c = b - a;
out.println(c - a % c);
```

#### [F. 全排列的价值](https://www.dotcpp.com/oj/problem2687.html)

```java
final int MOD = 998244353;
int n = Integer.parseInt(in.readLine());
long ans = (long) n * (n - 1) / 2 % MOD;
for (int i = 3; i <= n; i++) ans = (ans * (i % MOD)) % MOD;
out.println(ans);
```