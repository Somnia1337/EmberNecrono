@2024.05.16 | Week 12

### Practice 1. Optimizing Analysis

用 CB Profiler 比较程序的性能：

优化前：

![[Snipaste_240517_092437.png|500]]

优化后：

![[Snipaste_240517_092508.png|500]]

用 DiffChecker 比较两个源文件：

![[Snipaste_240517_093118.png|600]]

优化：用宏比较两字符串。

![[Snipaste_240517_093256.png|600]]

优化：如果 `words` 中存在相同字符串，就无需再分配空间了。

### Practice 2: Optimize Program Performance

引入了数百个头文件，仍然无法构建程序。

观察程序，`substitude()` 调用次数较多：

```c
void substitute(CString *data, CString *pattern, CString *replacement)
{
    int loc;
    // find every occurrence of pattern:
    for (loc = data->Find(*pattern, 0); loc >= 0;
         loc = data->Find(*pattern, 0))
    {
        // delete the pattern string from loc:
        data->Delete(loc, pattern->GetLength());
        // insert each character of the replacement string:
        for (int i = 0; i < replacement->GetLength(); i++)
        {
            data->Insert(loc + i, (*replacement)[i]);
        }
    }
}
```

可以进行的优化：

- 用变量存储 `pattern->GetLength()` 和 `replacement->GetLength()`，避免重复调用。
- 更新 `loc` 时每次都从 `0` 位置重新检查，可以改为从 `loc - pattern->GetLength()` 处开始检查（因为此前一定没有匹配）。

优化后：

```c
void substitute(CString *data, CString *pattern, CString *replacement)
{
    int loc;
    int patternLength = pattern->GetLength();
    int replacementLength = replacement->GetLength();
    // find every occurrence of pattern:
    for (loc = data->Find(*pattern, 0); loc >= 0;
         loc = data->Find(*pattern, loc - patternLength))
    {
        // delete the pattern string from loc:
        data->Delete(loc, patternLength);
        // insert each character of the replacement string:
        for (int i = 0; i < replacementLength; i++)
        {
            data->Insert(loc + i, (*replacement)[i]);
        }
    }
}
```