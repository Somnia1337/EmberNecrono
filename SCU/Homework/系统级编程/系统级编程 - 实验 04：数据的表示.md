@2024.03.21 | Week 04

#### 1. ArrayStorage.cpp

> 阅读源代码，运行并解释输出。

```cpp
#include <stdio.h>
void Initialize(char * a, char * b) {
	a[0] = 'T'; a[1] = 'h'; a[2] = 'i';
	a[3] = 's'; a[4] = ' '; a[5] = 'i'; a[6] = 's';
	a[7] = ' '; a[8] = 'A'; a[9] = '\0';
	b = a;
	b[8] = 'B';
}

#define ARRAY_SIZE 10

char a[ARRAY_SIZE];
char b[ARRAY_SIZE];
int main(int argc, char * argv[]) {
	Initialize(a, b);
	printf("%s\n%s\n", a, b);
	return 0;
}
```

输出（第 2 行为空）：

```text
This is B

```

解释：一句话就是“形参与实参分离”。

首先，在 `main()` 中的 `a`、`b` 不相等。

传入 `Initialize()`，先将 `a` 处的 `char[]` 赋为 `"This is A"`，然后赋值 `b = a`，从而将内存中 **实际的** `a` 处修改为 `"This is B"`，但 **实际的** `b` 处没有内容。

函数返回，在 `main()` 中的 `a`、`b` **仍然** 不相等，此时打印出 **内存的实际内容**，也就是 `a` 处的 **`"This is B"`**，以及 `b` 处的 **空串**。

#### 2. strchr.cpp

> 阅读源代码，列出有关字符串的函数并解释它们，然后解释程序的算法。

```cpp
#include <stdio.h>
#include <string.h>

#define MAXLINE_LEN 80

int main(int argc, char *argv[]) {
    char Buffer[MAXLINE_LEN];
    char *space;
    char *word;
	
    printf("Input> ");
    do {
        if (fgets(Buffer, MAXLINE_LEN, stdin) == NULL) break;
        word = Buffer;
        while (space = strchr(word, ' ')) {
            *space = '\0';
            printf("%s\n", word);
            word = ++space;
            while (*word == ' ') word++;
        }
        if (strchr(word, '\n')) {
            printf("%sInput> ", word);
        }
        else {
            printf("%s\n", word);
        }
    } while (strncmp(Buffer, "exit", 4));
    return 0;
}
```

有关字符串的函数：

- `char *fgets(char *str, int n, FILE *stream)`：从指定的文件流 `stream` 读取一行，并将其存储在 `str` 指向的字符数组中，直到 `n - 1` 个字符或者遇到换行符为止。
- `char *strchr(const char *str, int c)`：在字符串 `str` 中查找第一次出现字符 `c` 的位置，并返回该字符在字符串中的指针。
- `int strncmp(const char *str1, const char *str2, size_t n)`：比较两个字符串的前 `n` 个字符是否相等：
	- 如果相等，返回 `0`。
	- 如果 `str1 > str2`，返回正数。
	- 如果 `str1 < str2`，返回负数。

程序的算法：通过 `printf("Input> ")` 提示用户不断输入，将用户输入的每一行都根据空格拆分成若干单词、逐行输出每个单词，当用户输入 `"exit"` 时停止。

具体解析见注释：

```cpp
do {
	if (fgets(Buffer, MAXLINE_LEN, stdin) == NULL) break;
	word = Buffer; // word 指向输入缓存的首部
	while (space = strchr(word, ' ')) { // 当 word 中存在空格时
		*space = '\0'; // 将首个空格替换为字符串结尾字符 \0,
					   // 从而分割出一个单词
		printf("%s\n", word); // 打印这个单词
		word = ++space; // 将 word 直接跳到空格的下一个字符处
		while (*word == ' ') word++; // 跳过本单词后的连续空格
	}
	// 最后, 打印最后一个单词
	// 如果本身已经含有换行符 \n, 就不再添加换行符
	if (strchr(word, '\n')) {
		printf("%sInput> ", word);
	}
	// 否则, 换一行
	else {
		printf("%s\n", word);
	}
} while (strncmp(Buffer, "exit", 4)); // 用户输入 exit 时停止
```