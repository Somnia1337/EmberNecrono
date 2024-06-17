大题：

- 位操作
- 栈帧
- 阿姆达尔定律
- 程序优化
- cache 命中率
- 静态链接

#### 防止缓冲区溢出攻击

- 使用安全函数 `fgets()` 和 `strncpy()`
- 对栈加一个随机偏移
- 禁止指令在栈上执行
- 设置并检查“金丝雀”标记

#### GC 算法

| 算法   | 描述                                 | 局限                |
| ---- | ---------------------------------- | ----------------- |
| **标记清扫** | 从 root 开始标记可达结点，<br>清扫无标记的结点       | GC 时暂停执行          |
| **复制**   | 交替使用两个堆，GC 时，<br>将所有可达结点复制到另一个并切换堆 | 空间利用率低            |
| **引用计数** | 维护每个结点的引用计数，<br>计数为 0 时清扫          | 额外计数开销<br>对循环引用无效 |
| **分代式**  | 对年轻代频繁回收，用复制等；<br>对老代降低回收频率，用标记清扫等 |                   |

#### 位操作

```c
// {~, |} => &
int bitAnd(int x, int y)
{
  return ~(~x | ~y);
}

// {~, &} => |
int bitOr(int x, int y)
{
  return ~(~x & ~y);
}

// return x == 0 ? 1 : 0
int isZero(int x)
{
  return !x;
}

// return -1
int minusOne(void)
{
  return ~0;
}

// return ?
int tmax(void)
{
  return ~(1 << 31);
}

// {~, &} => ^
int bitXor(int x, int y)
{
  int a = ~(x & y);
  int b = ~(~x & ~y);
  return a & b;
}

// return nth byte starting from least significant side
int getByte(int x, int n)
{
  return (x >> (n << 3)) & 0xFF;
}

// return x == y ? 1 : 0
int isEqual(int x, int y)
{
  return !(x ^ y);
}

// return -x
int negate(int x)
{
  return (~x) + 1;
}

// return x > 0 ? 1 : 0
int isPositive(int x)
{
  return !((x >> 31) | !x);
}
```

#### 栈帧

```c
int f(int x)
{
	if (x == 1)
	{
		return 1;
	}
	return x + f(x - 1);
}
```

```text
0x40008880  | x = 3
0x4000887C  | 返回地址
0x40008878  | old EBP
---------------------
0x40008874  | x = 2
0x40008870  | 返回地址
0x4000886C  | old EBP
---------------------
0x40008868  | x = 1
```

#### 阿姆达尔定律

$$S = \frac{T_{old}}{T_{new}} = \frac{1}{(1 - \alpha) + \alpha / k}$$

#### 程序优化

- 代码移动
- 减少过程调用
- 减少不需要的内存访问
- 循环展开
- 消除公用子表达式
- 避免临时变量
- 减少内存分配
- 减少重分配，尽量重用

#### cache 命中率

#### 静态链接