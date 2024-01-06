---
{"dg-publish":true,"tags":["体系结构","risc-v"],"permalink":"/work diary/RISC-V/Prism的中断咬尾（Prism的RTL代码修改）/","dgPassFrontmatter":true}
---

# 参考资料📚
Bumblebee内核指令架构手册（文档）

# 目标🎯
在Prism上实现[[work diary/RISC-V/Prism的中断咬尾（Bumblebee中断咬尾实现逻辑）#mnxti寄存器和jalmnxti寄存器\|jalmnxti寄存器]]，从而实现[[work diary/RISC-V/Prism的中断咬尾（Bumblebee中断咬尾实现逻辑）#mnxti寄存器和jalmnxti寄存器\|csrrw ra,jalmnxti,ra]]这条特殊指令的功能，从而实现中断咬尾。

## 指令的执行逻辑
1.由于中断1到来，使得系统进入非矢量中断模式的公共中断入口，此时MIE已经被关闭，直到cpu cliam中断1后，PLIC不再输出中断请求。直到新的中断到来，PLIC将再次仲裁出一个当前最高优先级的中断。
2.然后系统执行公共中断入口中的特殊指令，该指令检测到目前有中断pending，那么这条指令将把这条指令的pc保存到ra寄存器中去，并且硬件跳入mtvt+4×id的地址，该地址的指令是中断向量表的一部分，是一条jal指令，将会跳转到当前正在pending的中断的对应中断服务函数中去（感觉很接近矢量中断了），并且是`jump而不link`，这点很重要，因为特殊指令已经把自己的pc保存到ra了，我们希望中断服务函数执行完毕后能够再返回到特殊指令处，因此中断向量表中的jal指令不能link，否则会覆盖ra，导致中断服务函数执行完毕后无法正确返回。
3.中断服务函数执行完毕后，像普通的函数一样通过ret指令跳回ra寄存器保存的地址，即特殊指令处，再次重复上述过程直到所有待处理中断执行完毕。

# 修改内容
## PLIC模块
引出id和prio两组信号，使得PLIC目前在给core输出中断信号的同时，还会给出中断对应的id和优先级信息。

## CSR寄存器
增加CSR寄存器jalmnxti，参考Bumblebee该寄存器地址为0x7ed，SDK中加入相关宏定义。jalmnxti寄存器除了执行一般的csr寄存器的功能外，还要保存着译码模块的pc，那么当特殊指令进入ex模块时，就能读取到自己的pc并写入ra寄存器中。
增加一个CSR寄存器mtvt，指示中断向量表的地址。
此外还增加一个CSR寄存器mtvt2，这是因为mtvt已经被用来保存中断公共入口的地址了，那么非向量模式中断的中断向量表没有地方保存了，因此参考bumblebee的架构，加入一个csr寄存器。
![Pasted image 20231214153603.png](/img/user/work%20diary/imgs/Pasted%20image%2020231214153603.png)

## interrupt_ctrl模块
将原来的trap_entry_en信号（广义异常，包含狭义异常和中断），拆分为狭义异常和中断两个信号。

## IF模块
根据狭义异常/中断跳入不同的地址，即mtvec和mtvt2这两个CSR寄存器中保存的内容。

## ID模块
csrrw ra, CSR_JALMNXTI, ra指令编码：
csr    rs1    固定值    rd    固定值
0x7ed    ra    001    ra    1110011
0x7ed    00001    001    00001    1110011
即0x7ed090f3
当指令为0x7ed090f3时，译码模块给出信号dec_jalmntxi，跳转地址计算中的立即数2变为0。
将dec_jalmntxi信号加入跳转指令译码总线。

## ID_branch模块
收到dec_jalmntxi信号时，跳转地址计算中的立即数1（在id_branch模块中）变为`mtvt2 + id×4`(待定)，与jal指令一样，预测其一定会跳转。

## EX模块
将该特殊指令和jal指令一样视为一定会跳转的指令，避免被插入空泡。

## 实现JAL功能
跳转条件：首先必须有正在pending的中断，保存ra为当前PC的同时，跳转到int_id×4的位置（即中断向量表对应的jal指令，该jal指令jump但不link）。
![Pasted image 20231213151335.png](/img/user/work%20diary/imgs/Pasted%20image%2020231213151335.png)

## 备忘
1.在进入公共中断入口后，pending就会被清楚吗？还是要软件清除？
2.jalmntxi寄存器保存id阶段的pc，所以特殊指令在ex模块时访问到的就是特殊指令的pc（延迟了一个时钟），前提是csrrw指令读操作数是在ex阶段进行的，问题在于真的是这样吗？
