#### 任务

1. 编程，向内存 `0:200` ~ `0:23F` 依次传送数据 `0` ~ `63`(`3FH`)。

```asmatmel
assume cs:code
code segment

	mov ax, 0
	mov ds, ax
	mov bx, 200H
	mov al, 0
	mov cx, 64
	
s:  mov [bx], al
	inc bx
	inc al
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

![[Snipaste_240317_145348.png|500]]

![[Snipaste_240317_150247.png|500]]

用 G 命令 `g 0014` 直接执行完所有循环，程序结束后用 D 命令 `d 0:0200` 查看内存，发现成功写入 `0` ~ `3FH`。

2. 重复 `1.`，但除最后两条返回语句外只能使用 7 条语句。

```asmatmel
assume cs:code
code segment

	mov ax, 20H
	mov ds, ax
	mov bx, 0
	mov cx, 64
	
s:  mov [bx], bx
	inc bx
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

将目标地址重新表示为 `0020:0` ~ `0020:3F`，从而可以仅使用 BX 同时表示数据和偏移地址 `0` ~ `3FH`。

![[Snipaste_240317_152528.png|500]]

![[Snipaste_240317_152611.png|500]]

3. 下面的程序的功能是将 `mov ax, 4C00H` 之前的指令复制到内存 `0:200` 处，补全程序。上机调试，跟踪运行结果。

```asmatmel
assume cs:code
code segment

	mov ax, cs  ; todo!
	mov ds, ax
	mov ax, 0020H
	mov es, ax
	mov bx, 0
	mov cx, 17H ; todo!
	
s:  mov al, [bx]
	mov es:[bx], al
	inc bx
	loop s
	
	mov ax, 4C00H
	int 21H

code ends
end
```

![[Snipaste_240317_153702.png|500]]

![[Snipaste_240317_153855.png|500]]