### 技巧

- 难题：会暴写暴，不会暴输出用例
- 检查项：
	- 尽量用 `long`
	- `a * b * c` 的取余写法：`((a % M) * (b % M) % M) * (c % M) % M`（注意 `a * b` 之后额外的一次 `% M`）

### 默写

```java
import java.util.*;
import java.io.*;

public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		PrintWriter out = new PrintWriter(System.out);
		
		out.flush();
		out.close();
	}
	
	private int gcd(int a, int b) {
		return b == 0 ? a : gcd(b, a % b);
	}
	
	private int lcp(int a, int b) {
		return a * b / gcd(a, b);
	}
	
	private static long pow(long x, long p, int M) {
		long r = 1;
		while (p > 0) {
			if ((p & 1) == 1) {
				r = ((r % M) * (x % M)) % M;
			}
			x = ((x % M) * (x % M)) % M;
			p >>= 1;
		}
		return r;
	}
	
	private static double pow(double x, int p) {
		if (p < 0) {
			x = 1 / x;
			p = -p;
		}
		double ret = 1;
		while (p > 0) {
			if (p % 2 == 1)
				ret *= x;
			x *= x;
			p /= 2;
		}
		return ret;
	}
	
	private static int find(int[] root, int i) {
		return root[i] == i ? i : (root[i] = find(root, root[i]));
	}
	
	private static void union(int[] root, int x, int y) {
		root[find(root, x)] = find(root, y);
	}
	
	private void floyd(int[][] g) {
		for (int k = 0; k < g.length; k++) {
			for (int i = 0; i < g.length; i++) {
				for (int j = 0; j < g.length; j++) {
					if (g[i][k] < Integer.MAX_VALUE && g[k][j] < Integer.MAX_VALUE) {
						g[i][j] = Math.min(g[i][k] + g[k][j], g[i][j]);
					}
				}
			}
		}
	}
	
	static class TrieNode {
		TrieNode[] children;
		boolean isWord;
		
		TrieNode() {
			this.children = new TrieNode[26];
			this.isWord = false;
		}
		
		private static TrieNode buildTrie(String[] words) {
			TrieNode root = new TrieNode();
			for (String word : words) {
				TrieNode node = root;
				for (char c : word.toCharArray()) {
					if (node.children[c - 'A'] == null)
						node.children[c - 'A'] = new TrieNode();
					node = node.children[c - 'A'];
				}
				node.isWord = true;
			}
			return root;
		}
	}
}
```