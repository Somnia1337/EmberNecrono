@2024.04.18 | Week 08

卢剑歌 2022141461145

`debugmalloc.c` 源文件：

```c
#include <stdlib.h>
#include <string.h>
#include "debugmalloc.h"
#include "dmhelper.h"
#include <stdio.h>

typedef struct
{
	int checkSum;
	int blockSize;
	char *fileName;
	int lineNumber;
	struct FileHeader *nextToAllocate;
	int fence;
} FileHeader;

typedef struct
{
	int fence;
} FileFooter;

int CalculateCheckSum(FileHeader *header);
int isAllocated(FileHeader *header);

static FileHeader guard = {
	.nextToAllocate = NULL};

/* Wrappers for malloc and free */

void *MyMalloc(size_t size, char *fileName, int lineNumber)
{
	void *block = malloc(sizeof(FileHeader) + size + sizeof(FileFooter));
	FileHeader *header = (FileHeader *)block;
	header->blockSize = size;
	header->fileName = fileName;
	header->lineNumber = lineNumber;
	header->nextToAllocate = guard.nextToAllocate;
	guard.nextToAllocate = header;
	header->fence = -1;
	FileFooter *footer = (FileFooter *)(block + sizeof(FileHeader) + size);
	footer->fence = -1;
	header->checkSum = CalculateCheckSum(header);
	return header + 1;
}

void MyFree(void *ptr, char *fileName, int lineNumber)
{
	FileHeader *header = (FileHeader *)(ptr - sizeof(FileHeader));
	FileFooter *footer = (FileFooter *)(ptr + header->blockSize);
	if (!isAllocated(header))
	{
		error(4, fileName, lineNumber);
	}
	if (header->fence != -1)
	{
		errorfl(1, header->fileName, header->lineNumber, fileName, lineNumber);
	}
	if (footer->fence != -1)
	{
		errorfl(2, header->fileName, header->lineNumber, fileName, lineNumber);
	}
	if (CalculateCheckSum(header) != header->checkSum)
	{
		errorfl(3, header->fileName, header->lineNumber, fileName, lineNumber);
	}

	FileHeader *cur = &guard;
	FileHeader *pre;
	while (cur->nextToAllocate != NULL)
	{
		pre = cur;
		cur = cur->nextToAllocate;
		if (cur == header)
		{
			pre->nextToAllocate = cur->nextToAllocate;
			break;
		}
	}
	free(header);
}

int CalculateCheckSum(FileHeader *header)
{
	int checkSum = 0;
	for (char *i = (char *)header + sizeof(int); i < (char *)header + sizeof(FileHeader); i++)
	{
		for (int j = 0; j < 8; j++)
		{
			if (*i & (1 << j))
			{
				checkSum++;
			}
		}
	}
	return checkSum;
}

int isAllocated(FileHeader *header)
{
	int alloc = 0;
	FileHeader *cur = &guard;
	while (cur->nextToAllocate != NULL)
	{
		cur = cur->nextToAllocate;
		if (header->fileName == cur->fileName && header->lineNumber == cur->lineNumber)
		{
			alloc = 1;
			break;
		}
	}
	return alloc;
}

/* returns number of bytes allocated using MyMalloc/MyFree:
	used as a debugging tool to test for memory leaks */
int AllocatedSize()
{
	int total = 0;
	FileHeader *cur = &guard;
	while (cur->nextToAllocate != NULL)
	{
		cur = cur->nextToAllocate;
		total += cur->blockSize;
	}
	return total;
}

/* Optional functions */

/* Prints a list of all allocated blocks with the
	fileName/line number when they were MALLOC'd */
void PrintAllocatedBlocks()
{
	FileHeader *cur = &guard;
	while (cur->nextToAllocate != NULL)
	{
		cur = cur->nextToAllocate;
		printf("fileName:%s, lineNumber:%d\n", cur->fileName, cur->lineNumber);
	}
}

/* Goes through the currently allocated blocks and checks
	to see if they are all valid.
	Returns -1 if it receives an error, 0 if all blocks are
	okay.
*/
int HeapCheck()
{
	int error = 0;
	FileHeader *cur = &guard;
	while (cur->nextToAllocate != NULL)
	{
		cur = cur->nextToAllocate;
		if (CalculateCheckSum(cur) != cur->checkSum || cur->fence != -1 || ((FileFooter *)((char *)cur + sizeof(FileHeader) + cur->blockSize))->fence != -1)
		{
			error = -1;
			break;
		}
	}
	return error;
}
```

`#1` ~ `#8` 测试结果：

![[Snipaste_240421_211635.png]]

![[Snipaste_240421_211659.png]]

![[Snipaste_240421_211723.png]]

![[Snipaste_240421_211751.png]]

![[Snipaste_240421_211809.png]]

![[Snipaste_240421_211837.png]]

![[Snipaste_240421_211857.png]]

![[Snipaste_240421_211916.png]]