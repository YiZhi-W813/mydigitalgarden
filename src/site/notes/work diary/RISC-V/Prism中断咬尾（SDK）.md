---
{"dg-publish":true,"permalink":"/work diary/RISC-V/Prism中断咬尾（SDK）/","dgPassFrontmatter":true}
---

# 参考资料📚
http://bbs.eeworld.com.cn/thread-1162456-1-1.html （论坛、博客、专栏）
https://zhuanlan.zhihu.com/p/588075416 （论坛、博客、专栏）
GD32VF103 SDK （源代码）
![Pasted image 20231218115302.png](/img/user/work%20diary/imgs/Pasted%20image%2020231218115302.png)
![Pasted image 20231218115546.png](/img/user/work%20diary/imgs/Pasted%20image%2020231218115546.png)
![Pasted image 20231218115642.png](/img/user/work%20diary/imgs/Pasted%20image%2020231218115642.png)

# Prism与中断相关的软件流程分析
## 启动程序（crt0.S）
1.关闭全局使能
2.调用函数--call init（在handler.c）

## 中断处理（handler.c）
1.mstatus写入0x08,即打开全局使能；mtvec写入trap_entry的地址，即中断入口。
2.定义一个中断公共入口函数，分辨trap类型，跳入不同trap的处理函数。

## entry.S
1.包含上下文入栈/出栈两个函数。
2.处理trap，首先入栈上下文，然后call trap的公共入口（即handler.c中的函数）。

# 修改SDK以支持中断咬尾
## crt0.S
目前不必修改

## handler.c
1.将原来的trap handler（trap指的是狭义异常与中断），拆分为异常 handler和中断handler。
2.init函数：向mtvec、mtvt、mtvt2这三个csr寄存器中写入异常处理公共入口、中断向量表、中断处理公共入口的基地址。
3.中断向量表：一个由jal指令构成的指令块儿，每一条jal指令对应一个中断，跳向该中断对应的ISR。

## entry.S
1.上下文入栈/出栈函数，没有修改。
2.异常handler：没有修改，注意call异常的公共入口即可。
3.中断handler：入栈、特殊指令、出栈。