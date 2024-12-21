## 🏆 周赛

### [# 426](https://leetcode.cn/contest/weekly-contest-426/)

@2024.12.01

|      全国排名      | 得分  |        用时        |   同分首位    |    竞赛分数    |
| :------------: | :-: | :--------------: | :-------: | :--------: |
| 412/2447 16.9% | 18✨ | `01:53:42` (+25) | 1 `12:50` | 1647 (+81) |

#### [T1. 仅含置位位的最小整数](https://leetcode.cn/problems/smallest-number-with-all-set-bits/)

将 `n` 的各二进制位置 `1`，即 `(1 << m) - 1`，其中 `m` 为 `n` 的二进制位数。

```rust
impl Solution {
    pub fn smallest_number(n: i32) -> i32 {
        (1 << (32 - n.leading_zeros())) - 1
    }
}
```

#### [T2. 识别数组中的最大异常值](https://leetcode.cn/problems/identify-the-largest-outlier-in-an-array/)

从大到小枚举异常值，检查剩余元素能否分为 `sum()` 相同的两组，且其中一组只有一个元素。

```rust
use std::collections::HashMap;

impl Solution {
    pub fn get_largest_outlier(mut nums: Vec<i32>) -> i32 {
        // 检查: 移除 banned 后, 是否存在一个元素 A,
        // 使得剩余元素(移除 banned 和 A)之和为 A
        fn check(sum: i32, banned: i32, count: &HashMap<i32, usize>) -> bool {
            let sum = sum - banned;
            if sum % 2 != 0 {
                return false;
            }

            let tar = sum / 2;
            *count.get(&tar).unwrap_or(&0) >= if tar == banned { 2 } else { 1 }
        }

        // 倒序排序
        nums.sort_unstable_by(|a, b| b.cmp(a));

        // 求和
        let sum: i32 = nums.iter().sum();

        // 计数
        let mut count = HashMap::new();
        for &x in &nums {
            let e = count.entry(x).or_insert(0);
            *e += 1;
        }

        // 枚举异常值
        for &x in &nums {
            if check(sum, x, &count) {
                return x;
            }
        }

        unreachable!()
    }
}
```

#### [T3. 连接两棵树后最大目标节点数目 I](https://leetcode.cn/problems/maximize-the-number-of-target-nodes-after-connecting-trees-i/)

对图 1 的每个结点，BFS 计算其在图 1 中 `k` 步可达结点数量，加上图 2 中 `k-1` 步可达的最大结点数量（事先对图 2 的每个起点计算一次，取最大）即为 `ans[i]`。

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn max_target_nodes(edges1: Vec<Vec<i32>>, edges2: Vec<Vec<i32>>, k: i32) -> Vec<i32> {
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

        // 邻接表
        let next1 = next(&edges1, edges1.len() + 1);
        let next2 = next(&edges2, edges2.len() + 1);

        // 计算图 2 中 k-1 步可达的最大结点数量(只计算 1 次)
        let reach2_max = (0..next2.len())
            .map(|i| bfs_k_reachable(&next2, i, k - 1))
            .max()
            .unwrap_or(1);

        let mut ans = vec![];

        // 对图 1 的每个结点, 计算其在图 1 中 k 步可达结点数量,
        // 加上已计算的图 2 的最大值
        for i in 0..next1.len() {
            let a = bfs_k_reachable(&next1, i, k) + reach2_max;
            ans.push(a as i32);
        }

        ans
    }
}
```

#### [T4. 连接两棵树后最大目标节点数目 II](https://leetcode.cn/problems/maximize-the-number-of-target-nodes-after-connecting-trees-ii/)

定义距离为偶数的两个结点是“可达”的。

观察发现：每个图都可以分为两组，每组内的任意两结点“可达”，两组间的任何两结点“不可达”。

BFS 对两图各遍历一次，标记每个结点所属分组。

因为总可以控制图 1 和图 2 的连接结点，使得对于任意的 `i`，总能让 `i` 与图 2 中较大的那一组结点“可达”。

每个 `ans[i]` 即为：图 1 中与 `i` 属于同一分组的结点数量，加上图 2 中较大分组的结点数量。

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn max_target_nodes(edges1: Vec<Vec<i32>>, edges2: Vec<Vec<i32>>) -> Vec<i32> {
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

        /// BFS 将结点分为两组, 每组内的任意结点间距离均为偶数,
        /// 返回两个 set 分别包含每组的结点编号,
        /// start 结点代表的组在第一个 set 中
        fn bfs_bisect(next: &[Vec<i32>], start: usize) -> (HashSet<usize>, HashSet<usize>) {
            let mut vis = vec![false; next.len()];
            let mut q = VecDeque::new();
            let mut even = false;
            let mut g = (HashSet::new(), HashSet::new());

            vis[start] = true;
            q.push_back(start);
            g.0.insert(start);

            loop {
                if q.is_empty() {
                    return g;
                }

                let cnt = q.len();

                for _ in 0..cnt {
                    let x = q.pop_front().unwrap();
                    for y in &next[x] {
                        let y = *y as usize;
                        if !vis[y] {
                            vis[y] = true;
                            if even {
                                g.0.insert(y);
                            } else {
                                g.1.insert(y);
                            }
                            q.push_back(y);
                        }
                    }
                }

                even = !even;
            }
        }

        // 邻接表
        let next1 = next(&edges1, edges1.len() + 1);
        let next2 = next(&edges2, edges2.len() + 1);

        // 将图 2 一分为二
        let (g2_even, g2_odd) = bfs_bisect(&next2, 0);
        // 取较大组的结点数量,
        // 因为总可以控制图 2 的连接结点,
        // 使得图 1 的任意结点到本较大组的结点的距离为偶数
        let c2_max = g2_even.len().max(g2_odd.len());

        let mut ans = vec![];

        // 将图 1 一分为二
        let g1 = bfs_bisect(&next1, 0);
        // 对图 1 的每个结点, 计算其在图 1 中距离为偶数的结点数量,
        // 即其所属分组的结点数量, 加上已计算的图 2 的最大值
        for i in 0..next1.len() {
            let a = if g1.0.contains(&i) {
                g1.0.len()
            } else {
                g1.1.len()
            } + c2_max;
            ans.push(a as i32);
        }

        ans
    }
}
```

### [# 427](https://leetcode.cn/contest/weekly-contest-427/)

@2024.12.08

|      全国排名      | 得分  |      用时      |    同分首位    |    竞赛分数    |
| :------------: | :-: | :----------: | :--------: | :--------: |
| 279/2376 11.8% | 12  | `48:31` (+5) | 85 `12:12` | 1736 (+89) |

#### [T1. 转换数组](https://leetcode.cn/problems/transformed-array/)

直接写。

```rust
impl Solution {
    pub fn construct_transformed_array(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();

        nums.iter()
            .enumerate()
            .map(|(i, &x)| {
                if x > 0 {
                    nums[(i + x as usize) % n]
                } else {
                    nums[(i + n - ((-x as usize) % n)) % n]
                }
            })
            .collect()
    }
}
```

#### [T2. 用点构造面积最大的矩形 I](https://leetcode.cn/problems/maximum-area-rectangle-with-point-constraints-i/)

枚举所有点对，试作为对角顶点，检查是否符合要求。

```rust
impl Solution {
    pub fn max_rectangle_area(points: Vec<Vec<i32>>) -> i32 {
        fn check(p: &[Vec<i32>], i: usize, j: usize) -> i32 {
            let (x1, y1) = (p[i][0], p[i][1]);
            let (x2, y2) = (p[j][0], p[j][1]);
            if x1 == x2 || y1 == y2 || !p.contains(&vec![x1, y2]) || !p.contains(&vec![x2, y1]) {
                return -1;
            }

            let max_x = x1.max(x2);
            let min_x = x1.min(x2);
            let max_y = y1.max(y2);
            let min_y = y1.min(y2);

            let inside = |x: i32, y: i32| -> bool {
                !((x == x2 || x == x1) && (y == y2 || y == y1))
                    && (min_x..=max_x).contains(&x)
                    && (min_y..=max_y).contains(&y)
            };

            for c in p {
                if inside(c[0], c[1]) {
                    return -1;
                }
            }

            (max_x - min_x) * (max_y - min_y)
        }

        let n = points.len();
        let mut ans = -1;

        for i in 0..n {
            for j in i..n {
                ans = ans.max(check(&points, i, j));
            }
        }

        ans
    }
}
```

#### [T3. 长度可被 K 整除的子数组的最大元素和](https://leetcode.cn/problems/maximum-subarray-sum-with-length-divisible-by-k/)

每 `k` 个一组求和得 `sums`，枚举 `0..k` 作为起点，从每个起点开始以 `k` 为步长，将 `sums` 的子序列视为新数组，对该数组求最大非空子数组和，所有和取最大者。

```rust
impl Solution {
    pub fn max_subarray_sum(nums: Vec<i32>, k: i32) -> i64 {
        /// 对 nums 每 k 个一组求和,
        /// 如 nums=[-5,1,2,-3,4], k=2,
        /// 返回 [-4,3,-1,1]
        fn sums(nums: &[i64], k: usize) -> Vec<i64> {
            let mut p = nums.iter().take(k - 1).sum();
            let mut ret = vec![];

            for i in k - 1..nums.len() {
                p += nums[i];
                ret.push(p);
                p -= nums[i - k + 1];
            }

            ret
        }

        /// 从 start 开始, 步长为 k 视为一个新数组,
        /// 返回新数组的最大非空子数组和,
        /// 如 part=[-4,3,-1,1], start=1, k=2,
        /// 新数组为 [3,1], 返回 4
        fn max_sub(part: &[i64], start: usize, k: usize) -> i64 {
            let mut cur = 0;
            let mut ret = i64::MIN;

            for p in part.iter().skip(start).step_by(k) {
                cur += *p;
                ret = ret.max(cur);
                if cur < 0 {
                    cur = 0;
                }
            }

            ret
        }

        // 类型转换
        let k = k as usize;
        let nums: Vec<i64> = nums.iter().map(|&x| x as i64).collect();

        // 每 k 个元素一组求和
        let sums = sums(&nums, k);

        // 0..k 分别作为起点, 步长为 k,
        // 求该组元素的最大非空子数组和
        (0..k).map(|i| max_sub(&sums, i, k)).max().unwrap_or(-1)
    }
}
```

#### [T4. 用点构造面积最大的矩形 II](https://leetcode.cn/problems/maximum-area-rectangle-with-point-constraints-ii/)

```rust
impl Solution {
    pub fn max_rectangle_area(x_coord: Vec<i32>, y_coord: Vec<i32>) -> i64 {
        unimplemented!()
    }
}
```