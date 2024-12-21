### Windows

#### 获取文件的哈希值（MD5 算法）

在终端中使用：

```
Get-FileHash <D:\example(不加双引号)> -Algorithm md5 | Format-List
```

#### 删除 Win11 22H2 文件资源管理器的“主文件夹”

1. 打开“注册表编辑器”。
2. 进入 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace`。如果开启了多标签，则会有另一个 `NameSpace_xxxxx`。
3. 删除 `{f874310e-b6b7-47dc-bc84-b9e6b38f5903}` 文件夹。

#### 禁用 Forticlient 自启动

![[禁用 Forticlient 自启动.png]]

#### 删除只读文件

假设要删除整个目录 `D:/example`：

```sh
icacls "D:\example" /t /q /c /reset
```

#### Windows 状态栏时间显秒

1. 打开“注册表编辑器”。
2. 进入 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`。
3. 右键 `Advanced`，新建一个 DWORD 32 位值，命名为 `ShowSecondsInSystemClock`。
4. 双击新建的值，设置值为 `1`。

#### Powershell 运行 `.exe`

运行当前目录下的 `abc.exe`：

```sh
./abc
```

#### Markdown 导出 PDF 强制另起一页

在需要强制另起一页的地方插入：

```html
<div style="page-break-after: always"></div>
```

#### Python 虚拟环境

```text
venv\Scripts\activate
uvicorn app.main:app --reload
```

#### 禁用 Administrator 账户登陆选项

```sh
net user administrator /active:no
```

#### 自修复命令

检查并修复损坏的系统文件：

```
sfc /scannow
```

修复系统映像和 Windows 组件存储：

```
DISM /Online /Cleanup-Image /CheckHealth
```

```
DISM /Online /Cleanup-Image /ScanHealth
```

```
DISM /Online /Cleanup-Image /RestoreHealth
```

扫描硬盘上的文件系统错误和坏扇区，并尝试修复它们：

```
chkdsk C: /f /r
```

- `C:` 表示要检查的磁盘分区，可以根据需要更改。
- `/f` 选项用于修复磁盘上的错误。
- `/r` 选项用于查找坏扇区并恢复可读信息。

检查系统内存问题：

1. 按 **Win + R** 打开运行窗口，输入 `mdsched.exe`，按回车。
2. 选择“立即重新启动并检查问题”。
3. 系统将重新启动，并运行内存诊断程序。完成后，系统会在登录后显示诊断结果。

修复与系统引导相关的问题，如 Windows 无法正常启动：

```
bootrec /fixmbr
```

```
bootrec /fixboot
```

```
bootrec /rebuildbcd
```

### Cargo / Rustc

#### 以汇编指令查看编译结果

```sh
rustc -O --emit=asm src/main.rs
```

### Git / GitHub

#### 初始化

```sh
git init
git remote add origin https://github.com/Somnia1337/xxx.git
git branch -m master main
git pull origin main --rebase
git push -u origin main
```

### 更新日志

- feat (Feature) 新功能的引入
- fix (Fix) 缺陷修复
- docs (Documentation) 文档相关的变更，比如 README 的更新
- style (Style) 不影响代码逻辑的变更，比如格式化、空格、漏掉的分号等
- refactor (Refactor) 代码重构，即既不修复缺陷也不增加新功能的代码变更
- perf (Performance) 提高性能的代码变更
- test (Tests) 添加或修改测试代码
- chore (Chore) 构建过程或辅助工具的变更，不影响源代码或测试文件的其他变更
- build (Build) 影响构建系统或外部依赖的变更，比如 Gradle、Maven、npm 等
- ci (Continuous Integration) 持续集成相关的变更，比如 Travis、Circle、Jenkins 配置文件的更新

### Misc

#### HTML 文字样式

```html
<p style="text-align: center; font-size: 24px; color: #027dfd">text</p>
```

### 语言更新

#### Rust

```sh
rustup update
```

#### Go

```sh
go install golang.org/dl/go<version>@latest
go<version> download
```

#### Java

安装新版，从 `Windows 设置 - 应用 - 安装的应用` 卸载旧版。

#### Python

下载新版，安装时选择覆盖旧版。