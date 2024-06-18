---
{"dg-publish":true,"permalink":"/work diary/一生一芯/RTFSC--NEMU的ECALL流程/","dgPassFrontmatter":true}
---

### 以am_test中的yield test为例
首先CTE(simple_trap)，即调用cte_init函数进行初始化
![Pasted image 20240618130907.png](/img/user/work%20diary/imgs/Pasted%20image%2020240618130907.png)
这个函数通过函数指针把simple_trap函数注册成user_handler，并将__am_asm_trap的地址写入mtvec。

yield test的主函数会调用yield()，其中包含ECALL指令。ECALL指令被执行时会调用isa_raise_intr()，该函数会按照指令集手册中的规定修改mepc等csr寄存器并返回mtvec，随后ECALL指令会使得系统跳入mtvec中的地址，即前文说的__am_asm_trap。

该__am_asm_trap函数是一段汇编函数，它负责保存、恢复上下文以及跳入__am_irq_handle函数，在该函数内部调用cte_init函数中注册的user_handler，从而实现用户想要在ECALL时进行的操作。