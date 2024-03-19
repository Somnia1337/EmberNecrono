#### .01

🟡1780 / [2369. 检查数组是否存在有效划分](https://leetcode.cn/problems/check-if-there-is-a-valid-partition-for-the-array/) / #动态规划 

```java
public boolean validPartition(int[] nums) {
	int n = nums.length;
	boolean[] dp = new boolean[n + 1];
	dp[0] = true;
	for (int i = 1; i < n; i++) {
		if (dp[i - 1] && nums[i] == nums[i - 1] || i > 1 && dp[i - 2] && (nums[i] == nums[i - 1] && nums[i] == nums[i - 2] || nums[i] == nums[i - 1] + 1 && nums[i] == nums[i - 2] + 2)) {
			dp[i + 1] = true;
		}
	}
	return dp[n];
}
```

#### .02

🟡1477 / [2368. 受限条件下可到达节点的数目](https://leetcode.cn/problems/reachable-nodes-with-restrictions/) / #树 #搜索 

```java
private List<Integer>[] adj;
private Set<Integer> ban;
private boolean[] vis;

public int reachableNodes(int n, int[][] edges, int[] restricted) {
	adj = new ArrayList[n];
	Arrays.setAll(adj, e -> new ArrayList());
	for (int[] e : edges) {
		adj[e[0]].add(e[1]);
		adj[e[1]].add(e[0]);
	}
	ban = new HashSet<>();
	for (int x : restricted) ban.add(x);
	vis = new boolean[n];
	dfs(0);
	
	int ans = 0;
	for (boolean b : vis) {
		if (b) ans++;
	}
	return ans;
}

private void dfs(int node) {
	vis[node] = true;
	for (int next : adj[node]) {
		if (!ban.contains(next) && !vis[next]) dfs(next);
	}
}
```

#### .03

🟢N/A / [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/) / #设计 

```java
class MyStack {
    private Queue<Integer> q;
    
    public MyStack() {
        q = new LinkedList<>();
    }
    
    public void push(int x) {
        q.offer(x);
        int size = q.size();
        while (--size > 0) q.offer(q.poll());
    }
    
    public int pop() {
        return q.poll();
    }
    
    public int top() {
        return q.peek();
    }
    
    public boolean empty() {
        return q.isEmpty();
    }
}
```

#### .04

🟢N/A / [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/) / #设计 

```java
class MyQueue {
    private Deque<Integer> stk1;
    private Deque<Integer> stk2;
    
    public MyQueue() {
        stk1 = new ArrayDeque<>();
        stk2 = new ArrayDeque<>();
    }
    
    public void push(int x) {
        stk1.push(x);
    }
    
    public int pop() {
        move();
        return stk2.pop();
    }
    
    public int peek() {
        move();
        return stk2.peek();
    }
    
    public boolean empty() {
        return stk1.isEmpty() && stk2.isEmpty();
    }
    
    private void move() {
        while (stk2.isEmpty()) {
            while (!stk1.isEmpty()) {
                stk2.push(stk1.pop());
            }
        }
    }
}
```

#### .05

🟡2095 / [1976. 到达目的地的方案数](https://leetcode.cn/problems/number-of-ways-to-arrive-at-destination/) / #搜索 

```java
public int countPaths(int n, int[][] roads) {
	final int MOD = 1000000007;
	long[][] g = new long[n][n];
	for (long[] row : g) Arrays.fill(row, Long.MAX_VALUE / 2);
	for (int[] r : roads) {
		int u = r[0], v = r[1], d = r[2];
		g[u][v] = g[v][u] = d;
	}
	long[] dis = new long[n];
	Arrays.fill(dis, 1, n, Long.MAX_VALUE / 2);
	int[] f = new int[n];
	f[0] = 1;
	boolean[] done = new boolean[n];
	while (true) {
		int x = -1;
		for (int i = 0; i < n; i++) {
			if (!done[i] && (x < 0 || dis[i] < dis[x])) x = i;
		}
		if (x == n - 1) return f[n - 1];
		done[x] = true;
		for (int y = 0; y < n; y++) {
			long newDis = dis[x] + g[x][y];
			if (newDis < dis[y]) {
				dis[y] = newDis;
				f[y] = f[x];
			}
			else if (newDis == dis[y]) f[y] = (f[y] + f[x]) % MOD;
		}
	}
}
```