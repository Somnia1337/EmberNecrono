### 技巧

- 难题：会暴写暴，不会暴输出用例。

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