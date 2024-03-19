### 7 kyu

#### [Regex validate PIN code](https://www.codewars.com/kata/55f8a9c06c018a0d6e000132)

@2024.02.01

```rust
fn validate_pin(pin: &str) -> bool {
    let n = pin.len();
    if !(n == 4 || n == 6) {
        return false;
    }
    for x in pin.chars() {
        if !(x >= '0' && x <= '9') {
            return false;
        }
    }
    true
}
```

```rust
fn validate_pin(pin: &str) -> bool {
    let n = pin.len();
    (n == 4 || n == 6) && pin.chars().all(|d| d.is_digit(10))
}
```

#### [Vowel Count](https://www.codewars.com/kata/54ff3102c1bad923760001f3)

@2024.02.01

```rust
fn get_count(string: &str) -> usize {
    let mut ans: usize = 0;
    for x in string.chars() {
        if x == 'a' || x == 'e' || x == 'i' || x == 'o' || x == 'u' {
            ans += 1;
        }
    }
    ans
}
```

```rust
fn get_count(string: &str) -> usize {
    string.matches(|x| "aeiou".contains(x)).count()
}
```

#### [Disemvowel Trolls](https://www.codewars.com/kata/52fba66badcd10859f00097e)

@2024.02.01

```rust
fn disemvowel(s: &str) -> String {
    s.matches(|x| !"AEIOUaeiou".contains(x))
     .map(|x| x.to_string())
     .collect()
}
```

```rust
fn disemvowel(s: &str) -> String {
    let mut ans = s.to_string();
    ans.retain(|x| !"AEIOUaeiou".contains(x));
    ans
}
```

```rust
fn disemvowel(s: &str) -> String {
    s.replace(['A', 'E', 'I', 'O', 'U', 'a', 'e', 'i', 'o', 'u'], "")
}
```

#### [Square Every Digit](https://www.codewars.com/kata/546e2562b03326a88e000020)

@2024.02.02

```rust
fn square_digits(num: u64) -> u64 {
    if num < 10 {
        num * num
    } else {
        let d = num % 10;
        if d < 4 {
            square_digits(num / 10) * 10 + d * d
        } else {
            square_digits(num / 10) * 100 + d * d
        }
    }
}
```

```rust
fn square_digits(num: u64) -> u64 {
    num.to_string()
       .chars()
       .map(|i| i.to_digit(10).expect("char isnt digit").pow(2).to_string())
       .collect::<String>()
       .parse()
       .expect("result isnt u64 parsable")
}
```

#### [Highest and Lowest](https://www.codewars.com/kata/554b4ac871d6813a03000035)

@2024.02.26

```rust
fn high_and_low(numbers: &str) -> String {
    let nums: Vec<i32> = numbers.split_whitespace().map(|s| s.parse().unwrap()).collect();
    let mut nums = nums.into_iter().map(|n| n).collect::<Vec<i32>>();
    nums.sort_unstable();
    format!("{} {}", nums.last().unwrap(), nums.first().unwrap())
}
```

```rust
fn high_and_low(numbers: &str) -> String {
    use std::cmp;
    let f = |(max, min), x| (cmp::max(max, x), cmp::min(min, x));
    
    let ans =
        numbers.split_whitespace()
               .map(|x| x.parse::<i32>().unwrap())
               .fold((i32::MIN, i32::MAX), f);
    format!("{} {}", ans.0, ans.1)
}
```

#### [Descending Order](https://www.codewars.com/kata/5467e4d82edf8bbf40000155)

@2024.02.26

```rust
fn descending_order(x: u64) -> u64 {
    let mut chs: Vec<char> = x.to_string().chars().collect();
    chs.sort_unstable_by(|a, b| b.cmp(a));
    chs.iter().collect::<String>().parse().unwrap_or(0)
}
```

```rust
fn descending_order(x: u64) -> u64 {
    x.to_string()
     .chars()
     .sorted()
     .rev()
     .collect::<String>()
     .parse::<u64>()
     .unwrap()
}
```

#### [Get the Middle Character](https://www.codewars.com/kata/56747fd5cb988479af000028)

@2024.02.26

```rust
fn get_middle(s: &str) -> &str {
    &s[(s.len() - 1) / 2..s.len() / 2 + 1]
}
```

### 8 kyu

#### [Even or Odd](https://www.codewars.com/kata/53da3dbb4a5168369a0000fe)

@2024.02.01

```rust
fn even_or_odd(number: i32) -> &'static str {
    if number % 2 == 0 {
        "Even"
    } else {
        "Odd"
    }
}
```

```rust
fn even_or_odd(number: i32) -> &'static str {
    match number % 2 {
        0 => "Even",
        _ => "Odd",
    }
}
```

#### [Multiply](https://www.codewars.com/kata/50654ddff44f800200000004)

@2024.02.02

```rust
fn multiply(a: i32, b: i32) -> i32 {
    a * b
}
```

#### [Return Negative](https://www.codewars.com/kata/55685cd7ad70877c23000102)

@2024.02.26

```rust
fn make_negative(n: i32) -> i32 {
    if n >= 0 { -n } else { n }
}
```

```rust
fn make_negative(n: i32) -> i32 {
    -n.abs()
}
```

#### [Sum of positive](https://www.codewars.com/kata/5715eaedb436cf5606000381)

@2024.02.26

```rust
fn positive_sum(slice: &[i32]) -> i32 {
    slice.iter().filter(|&&x| x > 0).sum()
}
```