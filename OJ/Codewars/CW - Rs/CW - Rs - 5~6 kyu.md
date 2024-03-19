### 5 kyu

#### [Rot13](https://www.codewars.com/kata/530e15517bc88ac656000716)

@2024.02.27

```rust
fn rot13(message: &str) -> String {
    message.chars()
        .map(|c| {
            match c {
                'a'..='z' => ((((c as u8 - b'a' + 13) % 26) + b'a') as char).to_string(),
                'A'..='Z' => ((((c as u8 - b'A' + 13) % 26) + b'A') as char).to_string(),
                _ => c.to_string(),
            }
        })
        .collect()
}
```

```rust
fn rot13(message: &str) -> String {
    message.chars().map(|c| match c {
        'A'..='M' | 'a'..='m' => char::from(c as u8 + 13),
        'N'..='Z' | 'n'..='z' => char::from(c as u8 - 13),
        _ => c,
    }).collect()
}
```

#### [Maximum subarray sum](https://www.codewars.com/kata/54521e9ec8e60bc4de000d6c)

@2024.02.27

```rust
fn max_sequence(seq: &[i32]) -> i32 {
    let (mut cur, mut ans) = (0, 0);
    for x in seq.iter() {
        if cur + x < 0 {
            ans = ans.max(cur);
            cur = 0;
        } else {
            cur += x;
            ans = ans.max(cur);
        }
    }
    ans
}
```

```rust
fn max_sequence(seq: &[i32]) -> i32 {
    (1..=seq.len()).flat_map(|n| seq.windows(n))
                   .map(|w| w.iter().sum())
                   .max()
                   .unwrap_or(0)
                   .max(0)
}
```

#### [Primes in numbers](https://www.codewars.com/kata/54d512e62a5e54c96200019e)

@2024.02.27

```rust
fn prime_factors(n: i64) -> String {
    let mut ans = String::new();
    let mut num = n.abs();
    let mut factor = 2;
    while factor <= num {
        let mut count = 0;
        while num % factor == 0 {
            num /= factor;
            count += 1;
        }
        if count > 0 {
            ans.push_str(&format!("({}", factor));
            if count > 1 {
                ans.push_str("**");
                ans.push_str(&count.to_string());
            }
            ans.push_str(")");
        }
        factor += 1;
    }
    if ans.is_empty() { ans.push_str("(1)"); }
    ans
}
```

```rust
fn prime_factors(n: i64) -> String {
    let mut num = n;
    let mut prime = vec![];
    let mut fac = 2;
    let mut cnt;
    while num > 1 {
        cnt = 0;
        while num % fac == 0 {
            num /= fac;
            cnt += 1;
        }
        if cnt > 0 {
            prime.push(if cnt > 1 {
                format!("({}**{})", fac, cnt)
            } else {
                format!("({})", fac)
            });
        }
        fac += 1;
    }
    prime.join("")
}
```

#### [Sum of Pairs](https://www.codewars.com/kata/54d81488b981293527000c8f)

@2024.02.27

```rust
fn sum_pairs(ints: &[i8], s: i8) -> Option<(i8, i8)> {
    let mut vis = HashSet::new();
    for &num in ints.iter() {
        let cp = s - num;
        if vis.contains(&cp) { return Some((cp, num)); }
        vis.insert(num);
    }
    None
}
```

#### [Consecutive k-Primes](https://www.codewars.com/kata/573182c405d14db0da00064e)

@2024.02.29

```rust
fn consec_kprimes(k: i32, arr: Vec<i32>) -> i32 {
    let fc = |mut m|
        (2_i32..).map_while(|k|
            if k.pow(2) <= m {
                let mut c = 0;
                while m % k == 0 {
                    (m, c) = (m / k, c + 1);
                }
                Some(c)
            } else { None }
        ).sum::<i32>() + (m > 1) as i32;
    arr.windows(2)
       .filter(|w| fc(w[0]) == k && fc(w[1]) == k)
       .count() as i32
}
```

#### [Buddy Pairs](https://www.codewars.com/kata/59ccf051dcc4050f7800008f)

@2024.02.29

```rust
fn buddy(start: i64, limit: i64) -> Option<(i64, i64)> {
    for x in start..=limit {
        let candi = s(x) - 1;
        if candi <= x { continue; }
        if s(candi) == x + 1 { return Some((x, candi)); }
    }
    None
}

fn s(x: i64) -> i64 {
    let (sqrt, mut ret) = ((x as f64).sqrt() as i64, 1);
    if sqrt * sqrt == x {
        ret += sqrt;
        for fac in 2..sqrt {
            if x % fac == 0 { ret += fac + x / fac; }
        }
    } else {
        for fac in 2..=sqrt {
            if x % fac == 0 { ret += fac + x / fac; }
        }
    }
    ret
}
```

```rust
fn buddy(start: i64, limit: i64) -> Option<(i64, i64)> {
    for x in start..=limit {
        let candi = sum_divisors(x) - 1;
        if candi > x && sum_divisors(candi) == x + 1 {
            return Some((x, candi));
        }
    }
    None
}

fn sum_divisors(n: i64) -> i64 {
    (2..).take_while(|x| x * x <= n)
         .filter(|x| n % x == 0)
         .map(|x| if n / x == x { x } else { x + n / x })
         .sum::<i64>() + 1
}
```

#### [Meeting](https://www.codewars.com/kata/59df2f8f08c6cec835000012)

@2024.03.01

```rust
fn meeting(s: &str) -> String {
    let mut names =
        s.to_uppercase()
         .split(';')
         .map(|p| {
             let split =
                 &p.split(':')
                   .collect::<Vec<&str>>();
             format!("({}, {})", split[1], split[0])
         }).collect::<Vec<_>>();
    names.sort();
    names.join("")
}
```

```rust
fn meeting(s: &str) -> String {
    s.to_uppercase()
     .split(';')
     .flat_map(|person| person.split(':'))
     .tuples()
     .map(|(first, last)| format!("({}, {})", last, first))
     .sorted()
     .collect()
}
```

### 6 kyu

#### [Create Phone Number](https://www.codewars.com/kata/525f50e3b73515a6db000b83)

@2024.02.01

```rust
fn create_phone_number(numbers: &[u8]) -> String {
    let mut ans = String::from("");
    for x in numbers {
        ans.push(('0' as u8 + x) as char);
    }
    ans.insert(0, '(');
    ans.insert(4, ')');
    ans.insert(5, ' ');
    ans.insert(9, '-');
    ans
}
```

```rust
fn create_phone_number(numbers: &[u8]) -> String {
    let s: String = numbers.into_iter().map(|d| d.to_string()).collect();
    format!("({}) {}-{}", &s[..3], &s[3..6], &s[6..])
}
```

#### [Find the odd int](https://www.codewars.com/kata/54da5a58ea159efa38000836)

@2024.02.02

```rust
use std::collections::HashMap;

fn find_odd(arr: &[i32]) -> i32 {
    let mut count = HashMap::new();
    for x in arr {
        let cnt = count.entry(x).or_insert(0);
        *cnt += 1;
    }
    for x in arr {
        if count[x] & 1 == 1 {
            return *x;
        }
    }
    -1
}
```

```rust
fn find_odd(arr: &[i32]) -> i32 {
    let mut xor = 0;
    for &x in arr.iter() {
        xor ^= x;
    }
    xor
}
```

```rust
fn find_odd(arr: &[i32]) -> i32 {
    arr.iter().fold(0_i32, |x, v| x ^ v)
}
```

#### [Who likes it?](https://www.codewars.com/kata/5266876b8f4bf2da9b000362)

@2024.02.02

```rust
fn likes(names: &[&str]) -> String {
    match names.len() {
        0 => "no one likes this".to_string(),
        1 => format!("{} likes this", names[0]),
        2 => format!("{} and {} like this", names[0], names[1]),
        3 => format!("{}, {} and {} like this", names[0], names[1], names[2]),
        _ => format!(
            "{}, {} and {} others like this",
            names[0],
            names[1],
            names.len() - 2,
        ),
    }
}
```

```rust
fn likes(names: &[&str]) -> String {
    match names {
        [] => format!("no one likes this"),
        [a] => format!("{} likes this", a),
        [a, b] => format!("{} and {} like this", a, b),
        [a, b, c] => format!("{}, {} and {} like this", a, b, c),
        [a, b, rest @ ..] => format!("{}, {} and {} others like this", a, b, rest.len()),
    }
}
```

#### [Array.diff](https://www.codewars.com/kata/523f5d21c841566fde000009)

@2024.02.26

```rust
fn array_diff<T: PartialEq>(a: Vec<T>, b: Vec<T>) -> Vec<T> {
    let mut ans = vec![];
    for x in a {
        if !b.contains(&x) { ans.push(x); }
    }
    ans
}
```

```rust
fn array_diff<T: PartialEq>(a: Vec<T>, b: Vec<T>) -> Vec<T> {
    a.into_iter().filter(|x| !b.contains(x)).collect()
}
```

#### [Counting Duplicates](https://www.codewars.com/kata/54bf1c2cd5b56cc47f0007a1)

@2024.02.26

```rust
fn count_duplicates(text: &str) -> u32 {
    let mut char_count = HashMap::new();
    for c in text.to_lowercase().chars() {
        *char_count.entry(c).or_insert(0) += 1;
    }
    char_count.iter().filter(|&c| *c.1 >= 2).count() as u32
}
```

```rust
fn count_duplicates(text: &str) -> u32 {
    text.to_lowercase()
        .chars()
        .counts()
        .values()
        .filter(|&&cnt| cnt >= 2)
        .count() as u32
}
```

#### [Duplicate Encoder](https://www.codewars.com/kata/54b42f9314d9229fd6000d9c)

@2024.02.26

```rust
fn duplicate_encode(word: &str) -> String {
    let word = word.to_lowercase();
    let mut char_count = HashMap::new();
    for c in word.chars() {
        *char_count.entry(c).or_insert(0) += 1;
    }
    let mut ans = String::new();
    for c in word.chars() {
        if *char_count.get(&c).unwrap() >= 2 { ans.push(')'); } else { ans.push('('); }
    }
    ans
}
```

#### [Take a Ten Minutes Walk](https://www.codewars.com/kata/54da539698b8a2ad76000228)

@2024.02.27

```rust
fn is_valid_walk(walk: &[char]) -> bool {
    if walk.len() != 10 { return false; }
    let dirs = [0, 1, 0, -1, 0];
    let key = "nesw";
    let (mut x, mut y) = (0, 0);
    for &d in walk.iter() {
        let idx = key.find(d).unwrap();
        x += dirs[idx];
        y += dirs[idx + 1];
    }
    x == 0 && y == 0
}
```

```rust
fn is_valid_walk(walk: &[char]) -> bool {
    if walk.len() != 10 { return false; }
    let (mut x, mut y) = (0, 0);
    for &d in walk.iter() {
        match d {
            'n' => x += 1,
            'e' => y += 1,
            's' => x -= 1,
            'w' => y -= 1,
            _ => {}
        }
    }
    x == 0 && y == 0
}
```

#### [Replace With Alphabet Position](https://www.codewars.com/kata/546f922b54af40e1e90001da)

@2024.02.27

```rust
fn alphabet_position(text: &str) -> String {
    let alp = "abcdefghijklmnopqrstuvwxyz";
    let text = text.to_lowercase();
    let mut ans = String::new();
    for c in text.chars() {
        match alp.find(c) {
            None => {}
            Some(idx) => ans.push_str(format!("{} ", idx + 1).as_str()),
        }
    }
    if !ans.is_empty() { ans.remove(ans.len() - 1); }
    ans
}
```

```rust
fn alphabet_position(text: &str) -> String {
    text.to_lowercase()
        .chars()
        .filter(|&c| c >= 'a' && c <= 'z')
        .map(|c| (c as u32 - 96).to_string())
        .collect::<Vec<_>>()
        .join(" ")
}
```

#### [Persistent Bugger.](https://www.codewars.com/kata/55bf01e5a717a0d57e0000ec)

@2024.02.27

```rust
fn persistence(num: u64) -> u64 {
    let (mut num, mut ans) = (num, 0);
    while num > 9 {
        num = multiply(num);
        ans += 1;
    }
    ans
}

fn multiply(num: u64) -> u64 {
    let (mut num, mut ret) = (num, 1);
    while num >= 1 {
        ret *= num % 10;
        num /= 10;
    }
    ret
}
```

#### [Your order, please](https://www.codewars.com/kata/55c45be3b2079eccff00010f)

@2024.02.27

```rust
fn order(sentence: &str) -> String {
    let mut words: Vec<_> = sentence.split_whitespace().map(String::from).collect();
    words.sort_by_key(|s| s.chars().find(|c| c.is_digit(10)).unwrap());
    words.join(" ")
}
```

#### [Tribonacci Sequence](https://www.codewars.com/kata/556deca17c58da83c00002db)

@2024.02.27

```rust
fn tribonacci(signature: &[f64; 3], n: usize) -> Vec<f64> {
    if n < 3 {
        let mut ans = vec![];
        for i in 0..n {
            ans.push(signature[i]);
        }
        return ans;
    }
    let mut ans = vec![signature[0], signature[1], signature[2]];
    for i in 3..n {
        ans.push(ans[i - 3] + ans[i - 2] + ans[i - 1]);
    }
    ans
}
```

```rust
fn tribonacci(signature: &[f64], n: usize) -> Vec<f64> {
    let mut ans = Vec::from(signature);
    ans.resize(n, 0.0); // 同时处理了 n<3 的情况
    for i in 3..n {
        ans[i] = ans[i - 1] + ans[i - 2] + ans[i - 3];
    }
    ans
}
```

#### [Unique In Order](https://www.codewars.com/kata/54e6533c92449cc251001667)

@2024.02.28

```rust
fn unique_in_order<T>(sequence: T) -> Vec<T::Item>
    where
        T: IntoIterator,
        T::Item: PartialEq + Clone,
{
    let mut ans = Vec::new();
    let mut iter = sequence.into_iter();
    let mut pre = None;
    while let Some(t) = iter.next() {
        if pre != Some(t.clone()) {
            ans.push(t.clone());
            pre = Some(t);
        }
    }
    ans
}
```

```rust
fn unique_in_order<T>(sequence: T) -> Vec<T::Item>
    where
        T: IntoIterator,
        T::Item: PartialEq + Clone,
{
    sequence.into_iter().dedup().collect()
}
```

#### [Playing with digits](https://www.codewars.com/kata/5552101f47fc5178b1000050)

@2024.02.28

```rust
fn dig_pow(n: i64, p: i32) -> i64 {
    let l = n.to_string().len();
    let mut p = p + l as i32 - 1;
    let mut sum = 0;
    let (hold, mut n) = (n, n);
    while n > 0 {
        sum += (n % 10).pow(p as u32);
        n /= 10;
        p -= 1;
    }
    if sum % hold == 0 { sum / hold } else { -1 }
}
```

```rust
fn dig_pow(n: i64, p: i32) -> i64 {
    let r: i64 =
        n.to_string()
         .chars()
         .map(|c| (c as u8 - b'0') as i64)
         .enumerate()
         .map(|(i, d)| i64::pow(d, p as u32 + i as u32))
         .sum();
    match r % n {
        0 => r / n,
        _ => -1,
    }
}
```

#### [Detect Pangram](https://www.codewars.com/kata/545cedaa9943f7fe7b000048)

@2024.02.28

```rust
fn is_pangram(s: &str) -> bool {
    let mut vis = [false; 26];
    for c in s.to_lowercase().chars() {
        if c.is_ascii_lowercase() {
            vis[(c as u8 - b'a') as usize] = true;
        }
    }
    vis.iter().all(|&v| v)
}
```

```rust
fn is_pangram(s: &str) -> bool {
    s.to_lowercase()
     .chars()
     .filter(|c| c.is_alphabetic())
     .collect::<HashSet<char>>()
     .len() == 26
}
```

```rust
fn is_pangram(s: &str) -> bool {
    ('a'..='z').all(|c| s.to_lowercase().contains(c))
}
```

#### [Equal Sides Of An Array](https://www.codewars.com/kata/5679aa472b8f57fb8c000047)

@2024.02.28

```rust
fn find_even_index(arr: &[i32]) -> Option<usize> {
    let n = arr.len();
    let mut pre = vec![0; n + 2];
    for i in 1..=n {
        pre[i] = pre[i - 1] + arr[i - 1];
    }
    for i in 1..n {
        if pre[i - 1] == pre[n] - pre[i] { return Some(i - 1); }
    }
    if pre[n - 1] == 0 { Some(n - 1) } else { None }
}
```

```rust
fn find_even_index(arr: &[i32]) -> Option<usize> {
    let (mut l, mut r) = (0, arr.iter().sum::<i32>());
    for (i, x) in arr.iter().enumerate() {
        r -= x;
        if r == l { return Some(i); }
        l += x;
    }
    None
}
```

```rust
fn find_even_index(a: &[i32]) -> Option<usize> {
    (0..a.len()).position(|i| a[..i]
        .iter()
        .sum::<i32>() == a[i + 1..]
        .iter()
        .sum::<i32>())
}
```

#### [Sort the odd](https://www.codewars.com/kata/578aa45ee9fd15ff4600090d)

@2024.02.28

```rust
fn sort_array(arr: &[i32]) -> Vec<i32> {
    let mut odds: Vec<_> =
        arr.iter()
           .filter(|&x| x % 2 == 1)
           .copied()
           .collect();
    let mut ans = arr.to_vec();
    odds.sort();
    for num in ans.iter_mut().filter(|&&mut x| x % 2 == 1) {
        *num = odds.remove(0);
    }
    ans
}
```

```rust
fn sort_array(arr: &[i32]) -> Vec<i32> {
    let mut os =
        arr.iter()
           .filter(|&x| x % 2 == 1)
           .sorted();
    arr.iter()
       .map(|x|
           if x % 2 == 1 {
               os.next().unwrap()
           } else {
               x
           })
       .cloned()
       .collect()
}
```

```rust
fn sort_array(arr: &[i32]) -> Vec<i32> {
    let mut odd =
        arr.iter()
           .filter(|&x| x % 2 == 1)
           .collect::<Vec<_>>();
    odd.sort_by(|a, b| b.cmp(&a));
    arr.iter()
       .map(|&x| if x & 1 == 1 { *odd.pop().unwrap() } else { x })
       .collect()
}
```

  
#### [Highest Scoring Word](https://www.codewars.com/kata/57eb8fcdf670e99d9b000272)

@2024.02.29

```rust
fn high(input: &str) -> &str {
    input.split_whitespace()
         .rev()
         .max_by_key(|s|
             s.chars()
              .map(|c| c as u16 - 96)
              .sum::<u16>()
         ).unwrap_or("")
}
```

#### [Delete occurrences of an element if it occurs more than n times](https://www.codewars.com/kata/554ca54ffa7d91b236000023)

@2024.02.29

```rust
fn delete_nth(lst: &[u8], n: usize) -> Vec<u8> {
    let mut count = HashMap::new();
    let mut ans = vec![];
    for &x in lst.iter() {
        *count.entry(x).or_insert(0) += 1;
        if *count.get(&x).unwrap() <= n as i32 { ans.push(x); }
    }
    ans
}
```

#### [Break camelCase](https://www.codewars.com/kata/5208f99aee097e6552000148)

@2024.02.29

```rust
fn solution(s: &str) -> String {
    let mut ans = String::new();
    for c in s.chars() {
        if c.is_uppercase() { ans.push(' '); }
        ans.push(c);
    }
    ans
}
```

#### [Write Number in Expanded Form](https://www.codewars.com/kata/5842df8ccbd22792a4000245)

@2024.02.29

```rust
fn expanded_form(n: u64) -> String {
    if n == 0 { return String::from("0"); }
    let mut n = n;
    let mut arr = vec![];
    let mut d = 1;
    while n > 0 {
        if n % 10 > 0 {
            arr.push(n % 10 * d);
        }
        d *= 10;
        n /= 10;
    }
    if arr.len() == 1 { return format!("{}", arr[0]); }
    let mut ans = String::new();
    for (i, &x) in arr.iter().rev().enumerate() {
        ans += format!("{}", x).as_str();
        if i < arr.len() - 1 { ans.push_str(" + "); }
    }
    ans
}
```

```rust
fn expanded_form(n: u64) -> String {
    n.to_string()
     .chars()
     .rev()
     .zip(0..)
     .filter(|&(c, _)| c > '0')
     .map(|(c, p)| format!("{}{}", c, "0".repeat(p)))
     .collect::<Vec<_>>()
     .into_iter()
     .rev()
     .collect::<Vec<_>>()
     .join(" + ")
}
```

#### [Count characters in your string](https://www.codewars.com/kata/52efefcbcdf57161d4000091)

@2024.02.29

```rust
fn count(input: &str) -> HashMap<char, i32> {
    let mut ans = HashMap::new();
    for c in input.chars() {
        *ans.entry(c).or_insert(0) += 1;
    }
    ans
}
```

#### [Mexican Wave](https://www.codewars.com/kata/58f5c63f1e26ecda7e000029)

@2024.02.29

```rust
fn wave(s: &str) -> Vec<String> {
    let mut ans = vec![];
    for (idx, ch) in s.chars().enumerate() {
        if !ch.is_alphabetic() { continue; }
        let mut cur = String::new();
        for (i, c) in s.chars().enumerate() {
            cur.push(if i == idx && c.is_alphabetic() { (c as u8 - 32) as char } else { c });
        }
        ans.push(cur);
    }
    ans
}
```

```rust
fn wave(s: &str) -> Vec<String> {
    s.char_indices()
     .map(|(i, c)|
         s[..i].to_string() + &c.to_uppercase().to_string() + &s[i + 1..]
     ).filter(|wave| wave != s)
     .collect()
}
```

#### [Cafeteria](https://www.codewars.com/kata/59f6a855bee845d3cd000046)

@2024.03.01

```rust
mod preloaded;

use preloaded::{Coffee, Milk, Sugar};

struct CoffeeBuilder {
    sort: String,
    milk: Vec<Milk>,
    sugar: Vec<Sugar>,
}

impl CoffeeBuilder {
    fn new() -> CoffeeBuilder {
        CoffeeBuilder {
            sort: String::new(),
            milk: vec![],
            sugar: vec![],
        }
    }
    
    fn set_black_coffee(mut self) -> CoffeeBuilder {
        self.sort = String::from("Black");
        self
    }
    
    fn set_cubano_coffee(mut self) -> CoffeeBuilder {
        self.sort = String::from("Cubano");
        self.sugar.push(Sugar { sort: "Brown".to_string() });
        self
    }
    
    fn set_antoccino_coffee(mut self) -> CoffeeBuilder {
        self.sort = String::from("Americano");
        self.milk.push(Milk { fat: 0.5 });
        self
    }
    
    fn with_milk(mut self, fat: f32) -> CoffeeBuilder {
        self.milk.push(Milk {
            fat,
        });
        self
    }
    
    fn with_sugar(mut self, sort: String) -> CoffeeBuilder {
        self.sugar.push(Sugar {
            sort,
        });
        self
    }
    
    fn build(self) -> Coffee {
        Coffee {
            sort: self.sort,
            milk: self.milk,
            sugar: self.sugar,
        }
    }
}
```