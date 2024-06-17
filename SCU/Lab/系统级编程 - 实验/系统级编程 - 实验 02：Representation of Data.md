@2024.03.07 | Week 02

> 1. Encode two float-point numbers step by step: `56.8`, `-56.8`.
> 2. Follow the instruction (Data_Lab.html) in dlab-handout. Write down your source code and show your screenshot of testing result.

#### 1. 浮点数编码

##### 编码 `56.8`

1. 确定符号位为 `0`。
2. 转换为二进制，整数部分为 `111000`，小数部分为 `110011001100...`。
3. 组合得到编码为 `0|10000100|11001100110011001100110`。

##### 编码 `-56.8`

1. 确定符号位为 `1`。
2. 转换为二进制，整数部分为 `111000`，小数部分为 `110011001100...`。
3. 组合得到编码为 `11000010011001100110011001100110`。

#### 2. 补全位操作代码

```c
int bitAnd(int x, int y) {
    return ~(~x | ~y);
}
```

```c
int bitOr(int x, int y) {
    return ~(~x & ~y);
}
```

```c
int isZero(int x) {
    return !x;
}
```

```c
int minusOne(void) {
    return ~0;
}
```

```c
int tmax(void) {
    return ~(1 << 31);
}
```

```c
int bitXor(int x, int y) {
    int a = ~(x & y);
    int b = ~(~x & ~y);
    return a & b;
}
```

```c
int getByte(int x, int n) {
    return (x >> (n << 3)) & 0xFF;
}
```

```c
int isEqual(int x, int y) {
    return !(x ^ y);
}
```

```c
int negate(int x) {
    return (~x) + 1;
}
```

```c
int isPositive(int x) {
    return !((x >> 31) | !x);
}
```

测试结果：

![[Snipaste_240315_102029.png|600]]