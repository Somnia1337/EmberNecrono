👈 [[9 转移指令的原理]]

### `ret` 和 `retf`

`ret` 修改 IP 实现近转移，相当于：

```asmatmel
pop ip
```

`retf` 修改 CS 和 IP 实现远转移，相当于：

```asmatmel
pop ip
pop cs
```

### `call` 指令

`call` 实现了 $16$ 位的远转移，相当于：

```text
压入 IP / (CS 和 IP)
转移
```

### 依据位移进行转移的 `call` 指令

`call {tag}` 相当于：

```asmatmel
push ip
jmp near ptr {tag}
```

### 转移的目的地址在指令中的 `call` 指令

`call far ptr {tag}` 相当于：

```asmatmel
push cs ; 先压 cs, 再压 ip,
push ip ; 和 `ret` 顺序相反
jmp far ptr {tag}
```

### 转移地址在寄存器中的 `call` 指令

`call {16 位 reg}` 相当于：

```asmatmel
push ip
jmp {16 位 reg}
```

### 转移地址在内存中的 `call` 指令

有 `word` / `dword` 两种。

`call word ptr {内存单元地址}` 相当于：

```asmatmel
push ip
jmp word ptr 内存单元地址
```

`call dword ptr {内存单元地址}` 相当于：

```asmatmel
push cs
push ip
jmp dword ptr {内存单元地址}
```

### `call` 和 `ret` 的配合使用

由 `call` 指令转到执行子程序之前，**其后面的指令** 的地址被压栈，在子程序中执行 `ret` 指令将导致程序跳转到 `call` 后面的指令，实现了子程序的机制。

### `mul` 指令

`mul` 实现乘法：

- 乘数：都是 $8$ 位 / 都是 $16$ 位。
	- 如果是 $8$ 位，一个默认从 AL 取，另一个从 $8$ 位 reg 或内存中取。
	- 如果是 $16$ 位，一个默认从 AX 取，另一个从 $16$ 位 reg 或内存中取。
- 结果：默认放在 AX($8$ 位乘法) / 高位放在 DX、低位放在 AX($16$ 位乘法)。

计算 $100 \times 10$：

```asmatmel
mov al, 100
mov bl, 10
mul bl
```

执行后 AX = `03E8H`(`1000`)。

计算 $100 \times 10000$：

```asmatmel
mov ax, 100
mov bx, 10000
mul bx
```

执行后 DX = `000FH`，AX = `4240H`(`F4240H` = `1000000`)。

### 模块化程序设计

### 参数和结果传递的问题

设计子程序，计算 $N^3$：

```asmatmel
cube:   mov ax, bx
		mul bx
		mul bx
		ret
```

计算 `data` 中第一组数据的 $^3$，保存在第二组单元中：

```asmatmel
assume cs:code

data segment
	dw 1, 2, 3, 4, 5, 6, 7, 8
	dd 0, 0, 0, 0, 0, 0, 0, 0
data ends

code segment
start:  mov ax, data
		mov ds, ax
		mov si, 0
		mov di, 16
		
		mov cx, 8
s:      mov bx, [si]
		call cube
		mov [di], ax
		mov [di].2, ax
		add si, 2
		add di, 4
		loop s
		
		mov ax, 4C00H
		int 21H
		
cube:   mov ax, bx
		mul bx
		mul bx
		ret
code ends
end start
```

👉 