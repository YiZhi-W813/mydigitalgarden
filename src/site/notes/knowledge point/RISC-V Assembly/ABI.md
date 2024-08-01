---
{"dg-publish":true,"tags":["C_assembly","risc-v"],"permalink":"/knowledge point/RISC-V Assembly/ABI/","dgPassFrontmatter":true}
---

ABI的全称是Application Binary Interface，是二进制级别的协议，它指导着编译器如何生成代码和二进制程序，同样也指导着用户如何写汇编代码。它主要包含函数调用约定（calling convention）、数据的对齐方式等内容。

其中对代码密度影响最大的就是函数调用约定，它规定了堆栈寄存器、链接寄存器、哪些寄存器寄存器需要在函数头尾保存和恢复、哪些寄存器可以作为参数寄存器等，还有一些特殊用途的寄存器。大部分特殊寄存器都是会被高频使用的，配合指令集设计可以降低代码密度；需要保存和恢复的寄存器个数同样也会影响代码密度。