在被洛谷的换行符卡WA/TLE无数次以后，我总结出这样的斗争经验...

#### 1. **scanf("%d\n", &x);/scanf("\n");**

格式化输入的时候就考虑换行符

- 适用情况：已知每一行的输入格式

[[COCI2006-2007#2] ABC - 洛谷](https://www.luogu.com.cn/problem/P4414)

```c
#include<stdio.h>
int main()
{
    int a,b,c,A,B,C,i;
    char M;
    scanf("%d %d %d\n",&a,&b,&c);//使用\n规避换行符的问题
    A=a<b?a:b;
    A=A<c?A:c;
    C=a>b?a:b;
    C=C>c?C:c;
    if(a>A&&a<C)
    B=a;
    else if(b>A&&b<C)
    B=b;
    else if(c>A&&c<C)
    B=c;
    for(i=0;i<3;i++)
    {
        scanf("%c",&M);
        if(M=='A')printf("%d ",A);
        if(M=='B')printf("%d ",B);
        if(M=='C')printf("%d ",C);
    }
    return 0;
}
```

[[NOIP2015 普及组] 扫雷游戏 - 洛谷](https://www.luogu.com.cn/problem/P2670)

```c
#include <stdio.h>
#include <math.h>

char C[100][100];

int main()
{
	int n, m;
	int i, j, k, l, c;
	char a;
	scanf("%d %d\n", &n, &m);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
		{
			C[i][j] = getchar();
		}
		scanf("\n");//实在用不明白getchar了，一个scanf解决所有问题，20变100
	}
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
		{
			if (C[i][j] == '*')
			{
				putchar('*');
				continue;
			}
			c = 0;
			for (k = 0; k < n; k++)
			{
				for (l = 0; l < m; l++)
				{
					if (abs(k - i) + abs(l - j) == 1 || (abs(k - i) + abs(l - j) == 2 && k != i && l != j))
					{
						if (C[k][l] == '*')
						{
							c++;
						}
					}
				}
			}
			printf("%d", c);
		}
		putchar('\n');
	}

	return 0;
}
```

#### 2. **while (scanf(“%c", &x) != EOF)**

用关键字EOF结束输入

- 适用情况：未知行数，未知每行格式

[集合求和 - 洛谷](https://www.luogu.com.cn/problem/P2415)

```c
#include <stdio.h>

int main()
{
	long long ans = 0;
	int n = 0, a, i;
	while (scanf("%d", &a) != EOF)//使用EOF关键字
	{
		ans += a;
		n++;
	}
	for (i = 1; i < n; i++)
	{
		ans = 2 * ans;
	}
	printf("%lld", ans);

	return 0;
}
```

行7在VS无法运行，但在洛谷运行通过

#### 3. **if (x == 'a' || x == 1)**

用if判断是否继续输入

- 适用情况：未知输入类型，不适用scanf（只能用char，会很复杂），但已知可能被输入的值数量很少

[语句解析 - 洛谷](https://www.luogu.com.cn/problem/P1597)

```c
#include <stdio.h>

int main()
{
	int N[3] = { 0,0,0 };
	char m, n;
	while (1)
	{
		m = getchar();
		if (m >= 97 && m <= 99 || m >= 48 && m <= 57)//还在输入有效值就继续读入
		{
			getchar();
			getchar();
			n = getchar();
			N[m - 'a'] = (n >= '0' && n <= '9') ? n - '0' : N[n - 'a'];
			getchar();
		}
		else//否则结束读入
		{
			break;
		}
	}
	printf("%d %d %d", N[0], N[1], N[2]);

	return 0;
}
```