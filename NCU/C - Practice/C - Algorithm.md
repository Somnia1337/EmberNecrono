## 排序

### 1. 冒泡排序

```c
#include <stdio.h>

int N[10000];

void swap(int* x, int* y)
{
    int t = *x;
    *x = *y;
    *y = t;
}

void bubbleSort(int n, int N[])
{
    int i, v;
    do
    {
        v = 0;
        for (i = 0; i < n - 1; i++)
            if (N[i] > N[i + 1])
                swap(&N[i], &N[i + 1]), v = 1;
    } while (v == 1);
}

int main()
{
    int n;
    int i, j;
    scanf("%d", &n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &N[i]);
    }
    bubbleSort(n, N);
    for (i = 0; i < n; i++)
    {
        printf("%d ", N[i]);
    }
	
    return 0;
}
```

原理：每次遍历数组，如果N[i]>N[i+1]就交换其数值，直到某次遍历时没有相邻的数满足此条件

最简单的排序，时间复杂度O(n^2)，耗时长

### 2. 计数排序

```c
#include <stdio.h>

int N[100], C[1000000] = { 0 };

int main()
{
    int n, max = 0;
    int i, j;
	scanf("%d", &n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &N[i]);
        if (N[i] > max)
        {
            max = N[i];
        }
    }
    for (i = 0; i < n; i++)
    {
        C[N[i]]++;
    }
    for (i = 0; i <= max; i++)
    {
        if (C[i])
        {
            for (j = 0; j < C[i]; j++)
            {
                printf("%d ", i);
            }
        }
    }
	
	return 0;
}
```

原理：创建大小为序列中最大数值的索引数组，遍历数组将数值对应的索引+1，然后根据索引数组依次输出非零值对应的数值

非比较排序，速度快（时间复杂度O(N)），内存占用高，适用于最大值较小且每项为正的情况

## 搜索

### 1. 二分查找

```c
#include <stdio.h>

int N[100];

void swap(int* x, int* y)
{
    int t = *x;
    *x = *y;
    *y = t;
}

void bubbleSort(int n, int N[])
{
    int i, v;
    do
    {
        v = 0;
        for (i = 0; i < n - 1; i++)
            if (N[i] > N[i + 1])
                swap(&N[i], &N[i + 1]), v = 1;
    } while (v == 1);
}

int binarySearch(int N[], int max, int min, int k)
{
    if (max <= min)
        return -1;
    const int mid = (max + min) / 2;
    if (N[mid] == k)
        return mid;
    else if (N[mid] > k)
        binarySearch(N, mid - 1, min, k);
    else if (N[mid] < k)
        binarySearch(N, max, mid + 1, k);
}

int main()
{
    int n, k;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
        scanf("%d", &N[i]);
    scanf("%d", &k);
    bubbleSort(n, N);
    if (binarySearch(N, n, 0, k) >= 0)
        printf("Found");
    else
        printf("Not found");
	
    return 0;
}
```