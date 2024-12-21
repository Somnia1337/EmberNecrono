### 数学

#### 最大公约数 GCD

```rust
fn gcd(mut a: i32, mut b: i32) -> i32 {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    a
}
```

#### 最小公倍数 LCM

```rust
fn lcm(a: i32, b: i32) -> i32 {
    (a / gcd(a, b)) * b
}

fn gcd(mut a: i32, mut b: i32) -> i32 {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    a
}
```

#### 带模快速幂

```rust
/// 带模快速幂 i32
fn pow(mut x: i64, mut p: i32, m: i64) -> i32 {
    let mut ret = 1i64;

    x %= m;

    while p > 0 {
        if p % 2 == 1 {
            ret = (ret * x) % m;
        }
        x = (x * x) % m;
        p >>= 1;
    }

    ret as i32
}
```

```rust
/// 带模快速幂 i64
fn pow(mut x: i64, mut p: i64, m: i64) -> i64 {
    let mut ret = 1;

    x %= m;

    while p > 0 {
        if p % 2 == 1 {
            ret = (ret * x) % m;
        }
        x = (x * x) % m;
        p >>= 1;
    }

    ret
}
```

### 搜索

#### 图的邻接表

```rust
/// 构造邻接表
fn next(edges: &[Vec<i32>], n: usize) -> Vec<Vec<i32>> {
    let mut next = vec![vec![]; n];

    for e in edges {
        let (x, y) = (e[0], e[1]);
        next[x as usize].push(y);
        next[y as usize].push(x);
    }

    next
}
```

#### `k` 步内可达的结点

```rust
/// BFS 搜索从 start 开始, k 步可达的结点数量,
/// start 本身也算一个
fn bfs_k_reachable(next: &[Vec<i32>], start: usize, k: i32) -> usize {
    let mut vis = vec![false; next.len()];
    let mut q = VecDeque::new();
    let mut reach = 0;

    vis[start] = true;
    q.push_back(start);

    for _ in 0..=k {
        if q.is_empty() {
            return reach;
        }

        let cnt = q.len();
        reach += cnt;

        for _ in 0..cnt {
            let x = q.pop_front().unwrap();
            for y in &next[x] {
                let y = *y as usize;
                if !vis[y] {
                    vis[y] = true;
                    q.push_back(y);
                }
            }
        }
    }

    reach
}
```

#### 检查新的 `x` 和 `y`

```rust
fn check_xy(x: i32, y: i32, rows: i32, cols: i32) -> bool {
    x >= 0 && x < rows && y >= 0 && y < cols
}
```

#### 转向

- 4 向：

```rust
let dirs = [0, 1, 0, -1, 0];
```

```rust
for k in 0..4 {
    let nx = x + dirs[k];
    let ny = y + dirs[k + 1];
    if check_xy(nx, ny, rows, cols) && /* ... */ {
        // ...
    }
}
```

- 8 向：

```rust
let dirs = [
    [-1, -1],
    [-1, 0],
    [-1, 1],
    [0, -1],
    [0, 1],
    [1, -1],
    [1, 0],
    [1, 1],
];
```

```rust
for dir in &dirs {
    let nx = x + dir[0];
    let ny = y + dir[1];
    if check_xy(nx, rows, ny, cols) && /* ... */ {
        // ...
    }
}
```

### 并查集

```rust
fn find(root: &mut [usize], i: usize) -> usize {
    if root[i] == i {
        i
    } else {
        root[i] = find(root, root[i]);
        root[i]
    }
}

fn union(root: &mut [usize], x: usize, y: usize) {
    root[find(root, x)] = find(root, y);
}
```