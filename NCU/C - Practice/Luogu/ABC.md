2022/09/26

[[COCI2006-2007#2] ABC - 洛谷](https://www.luogu.com.cn/problem/P4414)

```c
//解1

#include <stdio.h>

int main()
{
	int a, b, c, x, y, z;
	char m, n, w;
	scanf("%d %d %d", &a, &b, &c);
	getchar();
	getchar();
	scanf("%c%c%c", &m, &n, &w);
	x = a, y = a, z = a;
	if (b > a)
		z = b;
	if (c > z)
		z = c;
	if (b < a)
		x = b;
	if (c < x)
		x = c;
	if ((b > a && b < c) || (b < a && b > c))
		y = b;
	if ((c > a && c < b) || (c > b && c < a))
		y = c;
	if (m == 65 && n == 66)
	{
		printf("%d %d %d", x, y, z);
	}
	else if (m == 65 && n == 67)
	{
		printf("%d %d %d", x, z, y);
	}
	else if (m == 66 && n == 65)
	{
		printf("%d %d %d", y, x, z);
	}
	else if (m == 66 && n == 67)
	{
		printf("%d %d %d", y, z, x);
	}
	else if (m == 67 && n == 65)
	{
		printf("%d %d %d", z, x, y);
	}
	else if (m == 67 && n == 66)
	{
		printf("%d %d %d", z, y, x);
	}

	return 0;
}
```

```c
//解2 by 2022110532fanhongyu

#include<stdio.h>
int main()
{
    int a,b,c,A,B,C,i;
    char M;
    scanf("%d %d %d\n",&a,&b,&c);
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

使用\n规避了换行符的问题