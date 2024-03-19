### [# 1790](https://codeforces.com/contest/1790)

#### [A. Polycarp and the Day of Pi](https://codeforces.com/contest/1790/problem/A)

@2024.01.01

```java
out.println(check(in.readLine()));
```

```java
private static int check(String p) {
	String ac = "314159265358979323846264338327";
	for (int i = 0; i < p.length(); i++) {
		if (p.charAt(i) != ac.charAt(i)) return i;
	}
	return p.length();
}
```

#### [B. Taisia and Dice](https://codeforces.com/contest/1790/problem/B)

@2024.01.01

```java
StringTokenizer st = new StringTokenizer(in.readLine());
int n = Integer.parseInt(st.nextToken()), s = Integer.parseInt(st.nextToken()), r = Integer.parseInt(st.nextToken());
int a = s - r, b = r / (n - 1), l = r % (n - 1);
StringBuilder ans = new StringBuilder();
ans.append(a).append(" ");
for (int i = 0; i < n - l - 1; i++) ans.append(b).append(" ");
for (int i = 0; i < l; i++) ans.append(b + 1).append(" ");
out.println(ans);
```

#### [C. Premutation](https://codeforces.com/contest/1790/problem/C)

@2024.01.01

```java
int n = Integer.parseInt(in.readLine());
int[][] rk = new int[n][2];
for (int i = 0; i < n; i++) rk[i][1] = i + 1;
for (int i = 0; i < n; i++) {
	StringTokenizer st = new StringTokenizer(in.readLine());
	int[] v = new int[n - 1];
	for (int j = 0; j < n - 1; j++) v[j] = Integer.parseInt(st.nextToken());
	for (int j = 0; j < n - 1; j++) rk[v[j] - 1][0] += j;
}
Arrays.sort(rk, Comparator.comparingInt(a -> a[0]));
StringBuilder ans = new StringBuilder();
for (int i = 0; i < n; i++) ans.append(rk[i][1]).append(" ");
out.println(ans);
```

#### [E. Vlad and a Pair of Numbers](https://codeforces.com/contest/1790/problem/E)

@2024.01.01

```java
int m = Integer.parseInt(in.readLine()), and = m >> 1;
if ((m & 1) == 1 || (m & and) > 0) out.println(-1);
else out.println((m | and) + " " + and);
```