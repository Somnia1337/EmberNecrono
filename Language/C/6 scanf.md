### scanf

```c
#include <stdio.h>

int main()
{
  char c;
  short s;
  int n;
  long l;
  float f;
  double d;
  //定义了6个不同类型的变量，但没有初始化
  
  scanf("%hhd %hd %d %ld %f %lf",&c,&s,&n,&l,&f,&d);
  //运行后键入6个值+[Enter]，scanf函数根据转换规范将输入的字符转换为二进制数据，&（and符号）表示将转换后的值存储到变量中
  //特别地，此处的各转换规范之间用空格隔开，那么用户输入时也需用空格隔开；若改为逗号隔开，那么用户输入时也需用逗号隔开
  
  printf("%d %d %d %d %f %f\n",c,s,n,l,f,d);
  
  return 0;
}
```

第一个参数为字符串、其内容为转换规范，后续参数为数据存放位置。

存储位置为基本变量，变量名前需要加&；==存储位置为字符串，变量名前不需要加&==。

### 转换规范

![[6-1.png|450]]

==转换规范c==对应的数据类型为==不是char而是字符对应的ASCII==。

### 字符与字符串的读入

```c
#include <stdio.h>

int main()
{
  char c;
  scanf("%c",&c); //运行后键入A，字符A将被输入变量c
  printf("%d %c\n",c,c); //打印：65 A
  
  return 0;
}
```

```c
#include <stdio.h>

int main()
{
  char c;
  scanf("%hhd",&c); //运行后键入65，数值65将被输入变量c
  //如果此处错误地使用%c，键入65，则第一个字符（此处为6）的ASCII将被输入变量c（原因见上图）
  printf("%d %c\n",c,c); //打印：65 A
  
  return 0;
}
```

```c
#include <stdio.h>

int main()
{
  char str[10];
  scanf("%s",str);
  printf("%s",str);
  
  return 0;
}
```