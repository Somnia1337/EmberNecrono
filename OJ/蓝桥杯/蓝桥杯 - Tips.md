### 技巧

- 难题：会暴写暴，不会暴输出用例
- 检查项：
	- 全篇不要有 `int`
	- `a * b * c` 的取余写法：`((a % M) * (b % M) % M) * (c % M) % M`（注意 `a * b` 之后额外的一次 `% M`）

### 默写

#### I/O

```java
BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
PrintWriter out = new PrintWriter(System.out);

out.flush();
out.close();
```

#### 快速幂

```java
private static long pow(long x, long p) {
	long ret = 1;
	while (p > 0) {
		if ((p & 1) == 1) {
			ret = ((ret % MOD) * (x % MOD)) % MOD;
		}
		x = ((x % MOD) * (x % MOD)) % MOD;
		p >>= 1;
	}
	return ret;
}
```

```java
long ret = 1;
while (p > 0) {
	if ((p & 1) == 1) {
		ret = ret * x;
	}
	x = x * x;
	p >>= 1;
}
return ret;
```

#### 并查集

```java
private static int find(int[] root, int i) {
	if (root[i] != i) {
		root[i] = find(root, root[i]);
	}
	return root[i];
}

private static void union(int[] root, int x, int y) {
	root[find(root, x)] = find(root, y);
}
```