---
{"dg-publish":true,"tags":["C"],"permalink":"/knowledge point/C/Debug时的C编译选项/","dgPassFrontmatter":true}
---

### 参考
[https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html

### -g 
Produce debugging information in the operating system’s native format (stabs, COFF, XCOFF, or DWARF). GDB can work with this debugging information.

On most systems that use stabs format, -g enables use of extra debugging information that only GDB can use; this extra information makes debugging work better in GDB but probably makes other debuggers crash or refuse to read the program. If you want to control for certain whether to generate the extra information, use -gvms (see below).

以操作系统的本机格式（stabs、COFF、XCOFF或DWARF）生成调试信息。GDB可以使用这些调试信息。 在大多数使用stabs格式的系统上，-g允许使用只有GDB可以使用的额外调试信息;这些额外信息使调试在GDB中工作得更好，但可能会使其他调试器崩溃或拒绝读取程序。如果你想控制是否生成额外的信息，可以使用-gvms（见下文）。

### -g -g0 -g1 -g2 -g3的区别与关联
Request debugging information and also use level to specify how much information. The default level is 2.  
请求调试信息，并使用级别来指定如何 很多信息。 默认级别为2。

Level 0 produces no debug information at all. Thus, -g0 negates -g.  
级别0根本不产生调试信息。 因此，-g 0否定 -g.

Level 1 produces minimal information, enough for making backtraces in parts of the program that you don’t plan to debug. This includes descriptions of functions and external variables, and line number tables, but no information about local variables.  
级别1产生最少的信息，足以在 你不打算调试的程序部分。 这包括 函数和外部变量的描述，以及行号 表，但没有关于局部变量的信息。

Level 3 includes extra information, such as all the macro definitions present in the program. Some debuggers support macro expansion when you use -g3.  
级别3包括额外的信息，如所有宏定义 出现在节目中。 某些调试器支持宏扩展， 你用-g3。
