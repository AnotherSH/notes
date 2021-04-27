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
## Assembly和指令操作
* 一条汇编指令通常格式：mnemonic(助记符) operand(s)(操作数)