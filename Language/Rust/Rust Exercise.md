```rust
// 1 反转字符串
assert_eq!(solve1(String::from("hello")), String::from("olleh"));

fn solve1(s: String) -> String {
    s.chars().rev().collect()
}
```

```rust
// 2 斐波那契数列
assert_eq!(solve2(10), 55);

fn solve2(n: usize) -> usize {
    if n <= 2 {
        return 1;
    }
    let (mut pre, mut cur) = (1, 1);
    for _ in 3..=n {
        let next = pre + cur;
        pre = cur;
        cur = next;
    }
    cur
}
```

```rust
// 3 最大值与最小值
assert_eq!(solve3(vec![4, -3, 9, 0, -6]), (9, -6));

fn solve3(arr: Vec<i32>) -> (i32, i32) {
    let max = arr.iter().max().unwrap_or(&i32::MIN).clone();
    let min = arr.iter().min().unwrap_or(&i32::MAX).clone();
    (max, min)
}
```

```rust
// 4 判定回文
assert!(solve4(String::from("abccba")));
assert!(solve4(String::from("abcdcba")));
assert!(!solve4(String::from("abcd")));

fn solve4(s: String) -> bool {
    s.chars().eq(s.chars().rev())
}
```

```rust
// 5 合并排序数组
assert_eq!(solve5(vec![1, 4, 6], vec![2, 3, 5]), vec![1, 2, 3, 4, 5, 6]);

fn solve5(v1: Vec<i32>, v2: Vec<i32>) -> Vec<i32> {
    let mut i1 = 0;
    let mut i2 = 0;
    let mut res = vec![];
	
    while i1 < v1.len() && i2 < v2.len() {
        if v1[i1] <= v2[i2] {
            res.push(v1[i1]);
            i1 += 1;
        } else {
            res.push(v2[i2]);
            i2 += 1;
        }
    }
	
    res.extend_from_slice(&v1[i1..]);
    res.extend_from_slice(&v2[i2..]);
    res
}
```

```rust
// 6 计算质数
assert_eq!(
    solve6(100),
    vec![
        2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83,
        89, 97
    ]
);

fn solve6(n: usize) -> Vec<usize> {
    (2..=n)
        .filter(|&x| (2..).take_while(|i| i * i <= x).all(|i| x % i != 0))
        .collect()
}
```

```rust
// 7 字符串压缩
assert_eq!(
    solve7(String::from("aabcccccaaa")),
    String::from("a2b1c5a3")
);

fn solve7(s: String) -> String {
    let mut res = String::new();
    let mut chars = s.chars().peekable();
	
    while let Some(c) = chars.next() {
        let mut count = 1;
        while chars.peek() == Some(&c) {
            count += 1;
            chars.next();
        }
        res.push(c);
        res.push_str(&count.to_string());
    }
	
    res
}
```

```rust
// 8 两数之和
let arr = vec![4, -2, 9, 6, -7, 3, 0];
assert_eq!(solve8(&arr, 9), Some((3, 5)));
assert_eq!(solve8(&arr, -5), None);

fn solve8(arr: &Vec<i32>, tar: i32) -> Option<(usize, usize)> {
    use std::collections::HashMap;
	
    let mut pos = HashMap::new();
    for (i, &x) in arr.iter().enumerate() {
        if let Some(&j) = pos.get(&(tar - x)) {
            return Some((j, i));
        }
        pos.insert(x, i);
    }
	
    None
}
```

```rust
// 11 有效的括号
assert!(solve11(String::from("()")));
assert!(solve11(String::from("()[]{}")));
assert!(!solve11(String::from("(]")));
assert!(!solve11(String::from("([)]")));
assert!(solve11(String::from("{[]}")));

fn solve11(s: String) -> bool {
    use std::collections::HashMap;
	
    let mut pairs = HashMap::new();
    pairs.insert(')', '(');
    pairs.insert(']', '[');
    pairs.insert('}', '{');
	
    let mut stk = Vec::new();
    for c in s.chars() {
        match c {
            '(' | '[' | '{' => stk.push(c),
            _ => {
                if let Some(&p) = pairs.get(&c) {
                    if stk.pop() != Some(p) {
                        return false;
                    }
                }
            }
        }
    }
    
    stk.is_empty()
}
```