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

![[禁用Forticlient自启动.png]]

#### 删除只读文件

假设要删除整个目录 `D:\example`，在终端中使用：

```text
icacls "D:\example" /t /q /c /reset
```

这样就可以删除整个目录了。

#### Windows 状态栏时间显秒

1. 打开“注册表编辑器”。
2. 进入 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`。
3. 右键 `Advanced`，新建一个 DWORD 32 位值，命名为 `ShowSecondsInSystemClock`。
4. 双击新建的值，设置值为 `1`。

#### 语言更新

##### Rust

```text
rustup update
```

##### Go

```text
go install golang.org/dl/go<version>@latest
go<version> download
```

##### Java

安装新版，从 `Windows 设置 - 应用 - 安装的应用` 卸载旧版。

##### Python

下载新版，安装时选择覆盖旧版。