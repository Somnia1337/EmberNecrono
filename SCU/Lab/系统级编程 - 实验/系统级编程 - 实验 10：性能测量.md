@2024.05.09 | Week 11

### 1. The kernel timer interval

1\.

```text
> ./timestamp -s
This machine's estimated clock rate is: 3177362000 cycles per second
```

2\.

`2508796` 个时钟周期，`0.00079` 秒。

![[Snipaste_240510_154314.png|500]]

3\. 最小 `7,355,072`，最大 `12,715,616`

4\. 共 6 个中断

5\. 有 5 个时钟中断

6\. 有 3 个在切换线程

7\. 有 3 次，均约为 `12,700,000`

8\. 没有

9\. 总时间 `118,100,000` 个周期

10\. 有 `38,100,000` 没有执行

11\. `1` - `2` - `3` - `0`

### 2. VC Profiling

2.1

![[Snipaste_240511_093132.png|400]]

2.2

![[Snipaste_240511_095117.png|400]]

1. 因为 `main()` 中调用了 `10000` 次 `assign()`
2. 有 2 个函数
3. 几乎 100% 的时间都花在 `assign()`
4. 意思是执行整个程序用时为 `0.05` 秒

2.3

Case 1:

```c
void main()
{
    int i;
    int j;
    int max_num = 100000000;
    for (i = 0; i < max_num; i++)
        j = i / 2;
}
```

![[Snipaste_240511_095457.png|400]]

Case 2:

```c
void main()
{
    unsigned int i;
    unsigned int j;
    unsigned int max_num = 100000000;
    for (i = 0; i < max_num; i++)
        j = i / 2;
}
```

![[Snipaste_240511_095554.png|400]]

Case 3:

```c
void main()
{
    int i = 0;
    int j = 0;
    int max_num = 100000000;
    while (i < max_num)
    {
        j = i / 2;
        i = i + 1;
    }
}
```

![[Snipaste_240511_095708.png|400]]

Case 4:

```c
void main()
{
    int i = -1;
    int j = 0;
    int max_num = 100000000;
    while (++i < max_num)
    {
        j = i / 2;
        i = i + 1;
    }
}
```

![[Snipaste_240511_095754.png|400]]

Case 5:

```c
void prt(int i)
{
    int j = i / 2;
}
void main()
{
    int max_num = 10000000;
    for (int i = 0; i < max_num; i++)
        prt(i);
}
```

![[Snipaste_240511_095902.png|400]]

3.1

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void main()
{
    clock_t start, end;
    double cpu_time_used;
	
    start = clock();
	
    float a[250][250], b[250][250], c[250][250];
    int i, j, k;
    for (i = 0; i < 250; i++)
        for (j = 0; j < 250; j++)
            for (k = 0; k < 250; k++)
                // matrix multiplication
                c[i][j] += a[i][k] * b[k][j];
	
    end = clock();
    cpu_time_used = ((double)(end - start)) / CLOCKS_PER_SEC;
    printf("Total time: %d", cpu_time_used);
}
```

![[Snipaste_240511_100804.png|500]]

3.2

![[Snipaste_240511_101010.png|400]]