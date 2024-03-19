@2024.03.14 | Week 03

卢剑歌 2022141461145

> 1. Study boot.asm source code, write comments for each line of code.
> 2. Change the source code of this assembly program. The new program should show your student id and name on the screen. Show me the source code and the screen shot of testing.

#### 1. 阅读源码并注释

```asmatmel
org 07c00h ; 指定起始地址
mov ax, cs
mov ds, ax
mov es, ax
call DispStr ; 调用代码段 DispStr
jmp $ ; 跳转到“当前地址”
DispStr:
mov ax, BootMessage
mov bp, ax ; bp = ax
mov cx, 16 ; cx = 16
mov ax, 01301h ; ax = 01301h
mov bx, 000ch ; bx = 000ch
mov dl, 0
int 10h ; 中断服务，准备在显示器上显示
ret
BootMessage: db "2022141461145 Lu Jiange"
times 510-($-$$) db 0 ; 填写 510-($-$$) 个 0 到内存中
; 系统引导程序
dw 0xaa55
```

#### 2. 作为虚拟机引导程序显示在屏幕上

用 NASM 制作 `boot.bin` 二进制文件：

![[Snipaste_240314_211411.png]]

最终效果：

![[Snipaste_240314_212227.png|450]]