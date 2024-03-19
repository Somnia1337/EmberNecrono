👈 [[4 第一个程序]]

`[0]` 表示内存单元，其偏移地址为 `0`，如：

```asmatmel
mov ax, [0]
```

表示将一个长为 $2$ 字节的内存单元（字单元）（其偏移地址为 `0`，段地址存储在 DS 中）中的字存放在 AX 中。

`[bx]` 也表示一个内存单元，与 `[0]` 的区别在于其偏移地址为 `bx` 中的数据。

---

用 `(x)` 表示一个寄存器或内存单元中的内容，如：

- `(ax)` <=> AX 中的内容。
- `(21000H) = 0010H` <=> `2000:1000` 处的内容为 `0010H`。
- `(ax) = ((ds)*16 + 2)` <=> `mov ax, [2]`。
- `(sp) = (sp) - 2`，`((ss)*16 + (sp)) = ax` <=> `push ax`。

---

用 `idata` 表示常数，如 `mov ax, [idata]` <=> `mov ax, [1]` ... `mov ax, [n]` ...

### \[BX]

```asmatmel
mov ax, [bx]
```

表示将 `(ds):(bx)` 中的内容存放在 AX 中，即 `(ax) = ((ds)*16 + (bx))`。

### Loop 指令

CPU 执行 `loop` 指令的过程：

1. 执行 `(cx) = (cx) - 1`。
2. 判断 `(cx)` 的大小：
	- 如果 `(cx) = 0`，向下执行。
	- 如果 `(cx) > 0`，转到标号处执行。

用 `loop` 可实现循环的功能，`(cx)` 的初始值决定了循环次数。

计算 $2^{12}$：先让 AX = `2`，然后执行 $11$ 次 `(ax) = (ax) * 2`，也即 `(ax) = (ax) + (ax)`。

```asmatmel
assume cs:code
code segment

	mov ax, 2
	mov cx, 11
s:  add ax, ax
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

标号 `s` 标识了一个地址，该地址处存储了指令 `add ax, ax`。CPU 执行 `loop s` 时，先执行 `cx = cx - 1`、然后判断 `(cx)` 是否为 `0`，如果不为 `0` 则转到 `s` 处执行 `add ax, ax`。由此，控制了从 `s` 处开始到 `loop` 处结束的语句执行 `(cx)` 次。

计算 $123 \times 456$：先让 AX = `0`，然后循环 $123$ 次 `(ax) = (ax) + 456`。

```asmatmel
assume cs:code
code segment

	mov ax, 0
	mov cx, 123
s:  add ax, 456
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

### 在 Debug 中跟踪用 loop 指令实现的循环程序

计算 `FFFF:0006` 单元中的数值 $\times$ $3$，结果存储在 DX 中：

- 一个字节型数据的范围在 $[0, 255]$，乘以 $3$ 后不会超出 DX 字型数据所能容纳的 $65535$。
- 将该数据赋给 AX，用 DX 进行累加（执行 3 次 `(dx) = (dx) + (ax)`。
- 将一个字节型数据赋给一个 $16$ 位寄存器，应该令 `(ah) = 0`、`(al) = FFFF:0006H`。

```asmatmel
assume cs:code
code segment

	mov ax, 0FFFFH ; FFFFH 必须加前导 0
	mov ds, ax
	mov bx, 6
	mov al, [bx]
	mov ah, 0
	mov dx, 0
	mov cx, 3
s:  add dx, ax
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

大于 `9FFFH` 的 $16$ 进制数据都以字母开头，但汇编源程序中的数据必须以数字开头，因此需加前导 `0`。

用 Debug 逐句执行：

![[Snipaste_240316_084331.png|500]]

用 G 命令让程序直接执行到指定位置：

```text
-g 0016
```

会导致程序从头直接执行到 `CS:0016` 处，在本程序中即为执行完全部循环：

![[Snipaste_240316_084506.png|500]]

### Debug 和汇编编译器 masm 对指令的不同处理

```asmatmel
mov ax, [0]
```

这条 Debug 指令表示 `(ax) = ((ds)*16 + 0)`，但在汇编源程序中，编译器 masm 将其作为 `(ax) = 0` 处理。即：Debug 将 `[idata]` 解释为内存单元 `ds:idata`，而 masm 将其解释为值 `idata`。

为了在源程序中将 `mov ax, [0]` 实现为等同于 Debug 的作用，可以用 BX 作为中转：先执行 `(bx) = 0`，然后执行 `(ax) = [bx]`。

```asmatmel
mov ax, 2000H
mov ds, ax
mov bx, 0
mov ax, [bx]
```

其实也可以用 `[]` 给出，不过要在前部显式地添加段寄存器：

```asmatmel
mov ax, 2000H
mov ds, ax
mov ax, ds:[0]
```

在汇编源程序中：

- `mov ax, [0]` <=> `(ax) = 0`
- `mov ax, ds:[0]` <=> `(ax) = ((ds)*16 + 0)`
- `mov ax, [bx]` <=> `(ax) = ((ds)*16 + (bx))`
- `mov ax, ds:[bx]` 等效于 `mov ax, [bx]`，即：如果 `[]` 内填寄存器，则可以省略前部的段寄存器

### loop 和 \[BX] 的联合应用

计算 `FFFF:0` 到 `FFFF:BH` 中数据的和，存储在 DX 中：

- 如果用 DX 存储，则相加时出现类型不匹配。
- 如果用 DL 存储，则结果可能溢出。

解决方法是用一个 $16$ 位寄存器作为中介，将内存中的字节数据赋给 $16$ 位寄存器 AX，再执行 `(dx) = (dx) + (ax)`。

```asmatmel
assume cs:code
code segment

	mov ax, 0FFFFH
	mov ds, ax
	mov dx, 0
	
	mov al, ds:[0]
	mov ah, 0
	add dx, ax
	
	mov al, ds:[1]
	mov ah, 0
	add dx, ax
	
	; ...
	
	mov al, ds:[0BH]
	mov ah, 0
	add dx, ax
	
	mov ax, 4C00H
	int 21H

code ends
end
```

有大量重复的相似片段：

```asmatmel
mov al, ds:[XH]
mov ah, 0
add dx, ax
```

用 loop 改进：

```asmatmel
assume cs:code
code segment

	mov ax, 0FFFFH
	mov ds, ax
	mov bx, 0      ; 初始化 ds:bx 指向 FFFF:0
	mov dx, 0      ; 初始化累加寄存器 (dx)=0
	mov cx, 12     ; 初始化循环计数寄存器 (cx)=12
	
s:  mov al, [bx]
	mov ah, 0
	add dx, ax     ; 间接向 (dx) 加上 ((ds)*16 + (bx))
	inc bx         ; ds:bx 指向下个数据单元
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

### 段前缀

指令 `mov ax, [bx]` 中，偏移地址为 `(bx)`，段地址默认为 `(ds)`，而段地址也可以指定从其他寄存器获取：

```asmatmel
mov ax, ds:[bx]
mov ax, ss:[0]
```

`ds:`、`cs:`、`ss:` 等称为 **段前缀**。

### 一段安全的空间

在确定一段内存空间没有存放重要的数据或代码之前，不能随意覆写其中的内容，否则可能导致系统崩溃。

在 OS 环境中，OS 负责管理系统资源，应使用 OS 给程序分配的空间以确保安全。

但使用汇编的一个理由就是获取细粒度的底层控制，不应理会 OS。

一般情况下，没有其他程序使用 `0:0200` ~ `0:02FF` 这段空间，因此可以安全地写入。

### 段前缀的使用

将 `FFFF:0` ~ `FFFF:B` 中的数据复制到 `0:0200` ~ `0:020B`：

目标内存段可以被 `0020:0` ~ `0020:B` 等价描述，以与源内存段使用相同的偏移地址访问。

初始化 `x = 0`，循环 `BH` 次：

1. 将 `FFFF:X` 单元的数据送入 `0020:X`（需要寄存器中转）。
2. `x = x + 1`。

```asmatmel
assume cs:code
code segment

	mov bx, 0      ; 偏移地址从 0 开始
	mov cx, 12     ; 循环 12 次
	
s:  mov ax, 0FFFFH
	mov ds, ax     ; (ds) = 0FFFFH
	mov dl, [bx]   ; 将 FFFF:bx 中的数据送入 DL
	mov ax, 0020H
	mov ds, ax     ; (ds) = 0020H
	mov [bx], dl   ; 将 DL 中的数据送入 0020:bx
	inc bx         ; bx = bx + 1
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

每次循环中两次设置 DS，可以使用两个段寄存器分别存储两边的段地址进行改进：

```asmatmel
assume cs:code
code segment

	mov ax, 0FFFFH
	mov ds, ax
	mov ax, 0020H
	mov es, ax      ; 用 ES 存储目标内存段的段地址
	
	mov bx, 0
	mov cx, 12
	
s:  mov dl, [bx]
	mov es:[bx], dl ; 使用段前缀显式指定段地址
	inc bx
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

👉 