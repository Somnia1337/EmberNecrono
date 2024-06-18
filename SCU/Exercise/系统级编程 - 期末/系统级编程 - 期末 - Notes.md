### 选择题

When executing a function `callee()`, which of the following are true regarding the value of the frame pointer?

- It marks the top of the stack frame of the function that invoked `callee()`. 
- It marks the bottom of the stack frame of `callee()`.
- It is the top of the stack.

**I and II only**

The C expression `a -> b` is equivalent to **`(*a).b`**.

In C, when a struct is freed, **no pointers within the struct are freed automatically**.

In C, `calloc()` differs from `malloc()` in that `calloc()` **sets the contents of the block to zero before returning**.

A static variable by default gets initialized to **`0`**.

Mark-and-sweep garbage collectors are called conservative if **they treat everything that looks like a pointer as a pointer**.

Which of the following statements are true?

- The worst-fit memory allocation algorithm is slower than the best-fit algorithm (on average).
- Deallocation using boundary tags is fast only when the list of free blocks is ordered according to increasing memory addresses.

**none of them**

What is TSC? **a timer mechanism of x86 platform, which is the shortname of time stamp counter**

Wall time measures **the total duration of a program's execution**.

CPU time measures **the time spent by a program executing program instructions**.

On profiling, which is/are WRONG?

- gprof is the profilling tool on linux platform
- it can be used to estimate where time is spent in the program
- it can incorporate instrumentation code to determine how much time the different parts of the program require

**none**

Which of the following manages the transfer of data between the CPU registers and the cache? **compiler**

Which of the following manages the transfer of data between the cache and main memory? **operating system**

Which of the following caches (all having the same size) will have the least number of conflict misses? **a fully associative cache**

At what time can linking happen?

- compile time
- load time
- run time

**I, II and III**

`.data` 段存已初始化的 静态变量 / 全局变量，`.bss` 存未初始化的 ~。

Which file format is used for executable object file?

- pe
- coff
- elf
- a.out

**all**

Where the field, which describes whether relocatable object file is using little endian or big endian, locates? **elf headers**

Which section is used for resolution?

- elf header
- section header tables
- `.symtab`
- `.rel.text` and `.rel.data`

**III only**

Which section is used for relocation?

- elf header
- section header tables
- `.symtab`
- `.rel.text` and `.rel.data`

**IV only**

What is right about trap?

- it is a kind of exception
- it can be used to implement system call
- it can be used to implement hard disk interrupt

**I and II only**

In ia32 or x86, the exception includes

- interrupt
- trap
- fault
- abort

**all**

In 32 or x86, which exception is used to implement demand paging

- interrupt
- trap
- fault
- abort

**III**

About process, which one is right?

- by using process, we are presented the illusion that our program appears to have exclusive use of both the processor and the memory
- process is a running program
- process is possible by the help of exception

**I, II and III**

### 解答题

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