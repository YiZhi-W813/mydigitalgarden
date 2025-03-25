---
{"dg-publish":true,"tags":["C","C_assembly"],"permalink":"/Knowledge point/RISC-V Assembly/ELF文件的段/","dgPassFrontmatter":true}
---

### ELF文件常见的段
.text段中放置了已编译程序的指令代码段，.rodata段中放置了只读数据，比如常数const，.data段中放置了已初始化的C程序全局变量和静态局部变量，.bss段中放置了未初始化的C程序全局变量和静态局部变量，.debug段中放置了调试符号表，此段信息用于帮助调试器进行调试，除了上述常见的段外，ELF文件还包含很多其它类型的段。需要注意的是C程序的普通局部变量在程序运行时被并保存在堆栈中，因此既不会出现在.data中也不会出现在.bss中，但如果该变量被初始化为0，则也有可能会被放到bss段中。