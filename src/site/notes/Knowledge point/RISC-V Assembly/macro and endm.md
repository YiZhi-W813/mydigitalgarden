---
{"dg-publish":true,"tags":["C_assembly"],"permalink":"/Knowledge point/RISC-V Assembly/macro and endm/","dgPassFrontmatter":true}
---


.macro和.endm

.macro和.emdm伪操作用于将一串汇编代码定义成为一个宏。

“.macro name arg1 [, argn]”用于定义名为name的宏，并且可以传入若干由分号分隔的参数。

“.endm”用于结束宏定义