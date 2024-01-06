---
{"dg-publish":true,"tags":["risc-v","体系结构"],"permalink":"/work diary/RISC-V/Prism的中断咬尾（Bumblebee中断咬尾实现逻辑）/","dgPassFrontmatter":true}
---

# 参考材料📚
1.用于RISC-V架构的中断系统（专利）
2.Bumblebee内核指令架构手册（技术文档）

# Bumblebee非矢量中断处理流程和jalmnxti寄存器
![Pasted image 20231211200844.png|600](/img/user/work%20diary/imgs/Pasted%20image%2020231211200844.png)
![Pasted image 20231211200945.png|600](/img/user/work%20diary/imgs/Pasted%20image%2020231211200945.png)


![Pasted image 20231211201139.png|600](/img/user/work%20diary/imgs/Pasted%20image%2020231211201139.png)


`csrrw ra，CSR_JALMNXTI，ra`指令的编程模型如下

![Pasted image 20231216115833.png](/img/user/work%20diary/imgs/Pasted%20image%2020231216115833.png)
首先保存上下文，包含部分通用寄存器和mcause等csr寄存器。

并且当中断处理函数执行完成后，不使用mret指令返回，而是以被调用的函数的形式返回（即返回到ra寄存器保存的地址），那么相当于又会回到该特殊指令执行，判断是否还需要处理中断。
## 除了jalmnxti，bumblebee还有mtvt和mtvt2寄存器
mtvec：没有变化，存储着广义异常的入口地址。
mtvt：mtvt 寄存器用于保存 ECLIC 中断向量表的基地址
mtvt2：如果配置 CSR 寄存器 mtvt2 的最低位为1，则所有非向量中断共享的入口地址由 CSR 寄存器 mtvt2 的值（忽略最低 2 位的值）指定。为了让中断以尽可能快的速度被响应和处理，推荐将 CSR 寄存器 mtvt2 的最低位设置为 1，即，由 mtvt2指定一个独立的入口地址供所有非向量中断专用，和异常的入口地址（由 mtvec 的值指定）彻底分开。
总之，中断咬尾时，mtvt中保存着中断向量表的地址，mtvt2中保存非向量中断的公共入口。

# 方案
首先处理器工作在非矢量模式下，但是软件仍然使用一个中断向量表，中断ISR不保存上下文。当执行csrrw ra，CSR_JALMNXTI，ra指令时，查询pending信号以及对应的中断ID号，硬件跳转到对应的ISR，并且向ra寄存器写入这条指令的PC。ISR结束时以执行**函数返回**的指令，此时硬件会返回到ra寄存器保存的地址，也就是返回该指令，再次查询对应的中断，直到所有当前正在等待的中断全部完成。
关于指令本身的实现细节：
1.首先查询plic给出的中断号是否为0（不确定，也许可以看有没pending），如果不是0，那么打开中断使能MIE。
2.实现jal指令的功能，跳转到mvtec+4id，写ra寄存器为当前pc值。
3.修改几个中断相关的csr寄存器，这是以调用函数的形式触发了中断，因此要自行修改csr。（关注interrupt_ctrl模块中的trap_index信号）

# 备忘
1.Prism的CSR寄存器module中包含trap_entry_en和trap_exit_en信号，有没有必要改？主动进入中断是否要修改这两个信号？