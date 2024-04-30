---
{"dg-publish":true,"permalink":"/work diary/一生一芯/RTFSC--通过NUME_STATE控制NEMU/","dgPassFrontmatter":true}
---

在cpu每次调用cpu_exec时，即每执行一条指令，都会对nemu_state进行判断，判断分为两部分，首先在指令真正被执行前，即execute(n)函数执行前，先判断nemu_state是否为NEMU_END或者NEMU_ABORT，如果是这两个状态，那么指令不必继续执行。
![Pasted image 20240424184936.png](/img/user/work%20diary/imgs/Pasted%20image%2020240424184936.png)
另一部分是在指令完成后也要进行一次判断，如果nume_state进入了终止的两种状态，意味着程序已经执行结束，需要做一次判断是否"HIT GOOD TRAP","HIT GOOD TRAP"的条件是nemu_state.halt_ret为0。

NEMU_ABORT用在取到非法指令后终止整个nemu的运行。
NEMU_QUIT用在需要退出nemu的地方，npc是怎样实现的？
还差一个state.c中的is_exit_status_bad没有实现，它有什么用？
取到非法指令时会给nemu.halt_ret传入一个-1，此时为"HIT BAD TRAP"。
但是似乎只有这种情况会修改halt_ret,这个实际使用起来不符，这是因为NEMUTRAP也被define为了set……，这个NEMUTRAP会在执行ebreak指令时被调用，此时会将R(10)也就是a0寄存器的值写入nemu.halt_ret，换而言之，只要执行到ebreak指令是a0寄存器的值为0即是"HIT GOOD TRAP"，也许是测试程序中，如果正确执行的话，就会在ebreak指令执行前清零a0？这点不确定，可以问问助教。
需要把hostcall.c中的invalid_inst函数插入到id模块译码时的default中，作为非法指令/未注册指令。
通过dpic把regs中的值读出来，最好是并行读出来，方便后面写别的函数的代码。
再添加一些指令。。。
ALU做运算的时候，是引一个有符号数信号，还是有符号操作和无符号操作分开写？综合出来的结果应该差别不大。
添加了SLT指令，添加了一根信号线指示ALU操作是不是有符号操作。遇到一个warning指示比较后只有bit的值，但是要赋值到32bit的信号，应该不会有问题。
$(signed())这种语法，verilator可能会没法编译。
有符号数的算数右移>>>，左侧扩位符号位，如右移n位，则左侧增加n个符号位，右侧删除n位，即进行除n运算