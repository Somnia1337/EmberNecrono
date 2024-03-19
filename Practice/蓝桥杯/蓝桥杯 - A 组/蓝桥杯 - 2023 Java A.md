[蓝桥杯 - 2023 Java A](https://www.dotcpp.com/oj/train/1096/)

#### [C. 平均](https://www.dotcpp.com/oj/problem3179.html)

```java
int n = Integer.parseInt(in.readLine());
int[] count = new int[10];
List<Integer>[] cost = new ArrayList[10];
Arrays.setAll(cost, k -> new ArrayList<>());
StringTokenizer st;
for (int i = 0; i < n; i++) {
	st = new StringTokenizer(in.readLine());
	int x = Integer.parseInt(st.nextToken());
	count[x]++;
	cost[x].add(Integer.parseInt(st.nextToken()));
}
long ans = 0;
for (int x = 0; x < 10; x++) {
	if (count[x] > n / 10) {
		int dif = count[x] - n / 10;
		List<Integer> c = cost[x];
		Collections.sort(c);
		for (int i = 0; i < dif; i++) ans += c.get(i);
	}
}
out.println(ans);
```

#### [D. 棋盘](https://www.dotcpp.com/oj/problem3180.html)

```java
StringTokenizer st = new StringTokenizer(in.readLine());
int n = Integer.parseInt(st.nextToken()), m = Integer.parseInt(st.nextToken());
int[][] board = new int[n][n];
while (m-- > 0) {
	st = new StringTokenizer(in.readLine());
	int x1 = Integer.parseInt(st.nextToken()), y1 = Integer.parseInt(st.nextToken());
	int x2 = Integer.parseInt(st.nextToken()), y2 = Integer.parseInt(st.nextToken());
	for (int i = x1 - 1; i < x2; i++) {
		for (int j = y1 - 1; j < y2; j++) board[i][j] = 1 - board[i][j];
	}
}
for (int[] row : board) {
	for (int x : row) out.print(x);
	out.println();
}
```

#### [E. 互质数的个数](https://www.dotcpp.com/oj/problem3162.html)

```java
private static final int MOD = 998244353;

public static void main(String... args) throws IOException {
	BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
	PrintWriter out = new PrintWriter(System.out);
	
	StringTokenizer st = new StringTokenizer(in.readLine());
	long a = Long.parseLong(st.nextToken());
	long b = Long.parseLong(st.nextToken());
	long x = pow(a, b);
	long ans = x;
	
	for (int i = 2; i <= x / i; i++) {
		if (x % i == 0) {
			while (x % i == 0) {
				x /= i;
				x %= MOD;
			}
			ans -= ans / i;
			ans %= MOD;
		}
	}
	if (x > 1) ans -= ans / x;
	out.println(ans % MOD);
	
	out.flush();
	out.close();
}

private static long pow(long x, long p) {
	long ret = 1;
	while (p > 0) {
		if (p % 2 == 1) ret = ((ret % MOD) * (x % MOD)) % MOD;
		x = ((x % MOD) * (x % MOD)) % MOD;
		p >>= 1;
	}
	return ret;
}
```

#### [F. 阶乘的和](https://www.dotcpp.com/oj/problem3166.html)

```java
int n = Integer.parseInt(in.readLine());
int[] a = new int[n];
int min = Integer.MAX_VALUE, minCnt = 0;
StringTokenizer st = new StringTokenizer(in.readLine());
for (int i = 0; i < n; i++) {
	a[i] = Integer.parseInt(st.nextToken());
	if (a[i] < min) {
		min = a[i];
		minCnt = 1;
	}
	else if (a[i] == min) minCnt++;
}
while (minCnt % (min + 1) == 0) {
	minCnt /= ++min;
	for (int i = 0; i < n; i++) {
		if (a[i] == min) minCnt++;
	}
}
out.println(min);
```