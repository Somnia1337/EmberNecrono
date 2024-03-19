### 字符串

```go
// int 转 string
s := strconv.Itoa(num)
```

```go
// 库函数
list := strings.Split(s, " ")
```

### `if`

```go
if i, ok := idx[target-x]; ok {} // ok
```

### 哈希表

```go
mp := make(map[int]int)
```

```go
pairs := map[byte]byte{
	')': '(',
	']': '[',
	'}': '{',
}
```

```go
!mp[key]
```

### 栈

```go
stk := []byte{}
stk = append(stk, s[i]) // 追加尾元素
stk = stk[:len(stk)-1] // 弹出尾元素
```

### 自定义排序

```go
sort.Slice(arr, func(i, j int) bool {
	return arr[i][0] < arr[j][0]
})
```