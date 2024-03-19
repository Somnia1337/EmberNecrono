### Hello, Go!

```go
package main // 本文件属于 main 包

import "fmt" // 导入 fmt 包

func main() {
    fmt.Println("Hello, Go!") // 无分号
}
```

Go 的代码通过**包**(package)组织，类似其他语言的库 / 模块，包由单个目录下的若干 `.go` 源代码文件组成，每个源文件开头都有一条 `package` 声明语句。

导入的 `fmt` 包含有格式化输入输出的函数，`Println` 打印以空格间隔的若干值，并在最后打印换行符。

`main` 是一个特殊的包，它定义一个独立可执行的程序，而非一个库。`main()` 是一个特殊的函数，是程序执行的入口。

语句的末尾无需分号，因为编译器将特定符号后的换行符转换为分号。`{` 不能独占一行。

### 命令行参数

```go
// Echo1 prints its command-line arguments.
package main

import (
    "fmt"
    "os"
)

func main() {
    var s, sep string
    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}
```

`i++` 和 `i--` 是语句而非表达式，因此 `j = i++` 非法，`++i` 也非法。

循环语句只有 `for` 一种，形式如：

```go
for initialization; condition; post {
    // statements
}
```

任一部分都可以省略。

`for` 的另一种形式是遍历区间：

```go
for _, arg := range os.Args[1:] {
	s += sep + arg
	sep = " "
}
```

`range` 的语法要求：要处理元素，必须处理索引。

将索引赋值给一个不实际使用的临时变量非法，因为未使用的局部变量无法过编。应使用空标识符 `_`，它可用于任何语法需要变量名、但程序逻辑不需要之处。

变量声明：

```go
s := "" // 只能用于函数内部
var s string // 默认初始化为 ""
var s = "" // 常用于一行声明多个变量
var s string = "" // 用于改换变量的类型
```

`strings` 包的 `Join()`：

```go
strings.Join(os.Args[1:], " ") // 空格连接
```