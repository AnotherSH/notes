# Assembly
[NASM](https://www.nasm.us/)
[MASM](http://www.masm32.com/) TASM
## 8086 16位寄存器
* 4个16位<b>通用</b>寄存器：AX,BX,CX,DX（AX可分成AH和AL两个8位寄存器）
* 2个16位<b>指针</b>寄存器：SI，DI（不可分解成8位寄存器）
* 2个16位BP、SP寄存器：用来指向机器语言堆栈里的数据。（BP：基址寄存器，SP：堆栈指针寄存器）
* 4个16位<b>段</b>寄存器：CS——代码段，DS——数据段，SS——堆栈段，ES——附加段（暂时段寄存器）
* IP（<b>指令</b>寄存器）一般与CS寄存器一起使用来追踪CPU下条执行指令。一般当一条指令执行时，IP会提前指向内存里的下一条指令。
* FLAGS寄存器：存储前面指令执行结果的重要信息。
## 80386 32位寄存器
* EAX,EBX,ECX,EDX,ESI,EDI
* AX为EAX的低16位（无访问高16位的方法）
* EBP，ESP，EFLAGS，EIP
* 2个新的附加<b>段</b>寄存器：FS，GS
## DEBUG的使用
- R命令：查看、改变CPU寄存器的内容
    - R - 查看寄存器内容<br>
    - R 寄存器名 - 改变指定寄存器内容
- D命令：查看内存中的内容
  - D - 列出预设地址内存处的128个字节的内容
  - D 段地址:偏移地址 - 列出内存中指定地址处的内容
  - D 段地址:偏移地址 结尾偏移地址 - 列出内存中指定地址范围内的内容
- E命令：改变内存中的内容
  - E 段地址:偏移地址 数据1 数据2 ...
  - E 段地址:偏移地址
    - 逐个询问式修改
    - 空格 - 接受，继续
    - 回车 - 结束
- U命令：将内存中的机器指令翻译成汇编指令
  - U 地址 - 查看代码
- A命令：以汇编指令的格式在内存中写入机器指令
  - A 段地址:偏移地址
- T命令：执行CS:IP处的机器指令
- G命令：执行所有命令
- Q命令： 退出DEBUG
## Assembly语法
- 程序中的三种伪指令
  - 段定义
    - 一个汇编程序是由多个段组成的，这些段被用来存放代码、数据或当做栈空间来使用
    - 一个有意义的汇编程序中至少要有一个段，这个段用来存放代码。
    - 定义程序中的段：每个段都需要有段名
      - 段名 segment - 段的开始
      - ...
      - 段名 ends - 段的结束
  - end
    - 汇编程序的结束标记。若程序结尾处不加end，编译器在编译程序时，无法知道程序在何处结束。
  - assume（假设）
    - 含义是假设某一段寄存器和程序中的某一个用segment...end定义的段相关联。
    - assume cs:codesg指CS寄存器与codesg关联定义的codesg当做程序的代码段使用。
  - 段内存放数据
    - dw - define word,定义字型数据
    - db - 定义一个字节
    - dd - 定义一个双字
  - 定义一个标号指示代码开始的位置
    - 代码结束则需要添加 - end 标号
    - end的作用
      - 通知编译器程序结束
      - 通知编译器程序的入口在什么地方
    - 如下代码，程序加载后，CS:IP指向要执行的第一条指令在start处
  ``` assembly
  assume cs:code
  code segment
    dw 0123H,0456H,0789H,0abcH,0defH,0fedH,0cbaH,0987H
    start: mov bx,0
           mov ax,0
           mov cx,8

        s: add ax,cs:[bx]
           add bx,2
           loop s

           mov ax,4c00h
           int 21h
    code ends
    end start
  ``` 
  - 分号为代码注释
``` assembly
   assume cs:code,ds:data,ss:stack
   data segment
    dw 0123H,0456H,0789H,0abcH,0defH
   data ends
   stack segment
    dw 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
   stack ends
   code segment
    mov ax,stack
    mov ss,ax
    mov sp,20h
    mov ax,data
    mov ds,ax
    mov bx,0
    mov cx,8
    s:push [bx]
    add bx,2
    loop s
    
    mov ax,4c00h
    int 21h
   code ends
   end
```
- mov ax,bx
- add ax,bx
- inc ax - 加1
- jmp 段地址:偏移地址
- push ax
  - SP=SP-2;
  - 将ax中的内容送入SS:SP指向的内存单元处，SS:SP此时指向新栈顶
- pop ax
  - 将SS:SP指向的内存单元处的数据送入ax中
  - SP=SP+2,SS:SP指向当前栈下面的单元，以当前栈顶下面的单元为新的栈顶
- loop 标号 - 循环
  - (cx)=(cx)-1
  - 判断cx中的值
    - 不为零则转至标号处执行程序
    - 如果为0则向下执行
- [...] - (汇编语法规定)表示一个内存单元
  - 段前缀:[...]
- (...) - (为学习方便做出的约定)表示一个内存单元或寄存器中的内容
## 内存寻址方式