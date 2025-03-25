---
{"dg-publish":true,"tags":["risc-v","体系结构"],"permalink":"/Knowledge point/RISC-V INS/RVV扩展/","dgPassFrontmatter":true}
---

### 什么是SIMD？
这些指令允许您执行的操作是将相同的操作应用于多个元素。我们可以将它与SISD（单指令单数据）进行对比，后者仅在单个元素之间执行操作，它们的差异如下图所示：
![Pasted image 20240628162341.png](/img/user/Knowledge%20point/imgs/Pasted%20image%2020240628162341.png)
具体内容参见[[Knowledge point/RISC-V INS/SMID\|SMID]]

### RVV的特点
RVV实现了可变长度的向量指令集：
* RVV不预设向量长度
* 具体实现可以自主选择提供多长的向量
* 应用程序能够在运行时自主选择需要的长度
可动态调整向量类型和寄存器组(SEW，LMUL)
* RVV可以在编译时动态调整参数以应对不同的数据类型和数据大小。

### RVV寄存器
为了支持矢量计算，RVV需要额外的寄存器组，具体包括：
* 32个矢量寄存器：v0-v31
* 7个非特权寄存器：vtype、vl、vlenb、vstart、vxrm、vxsat以及vcsr
![Pasted image 20240630150104.png](/img/user/Knowledge%20point/imgs/Pasted%20image%2020240630150104.png)
### RVV指令格式
EVV指令可以分为3类：
* 加载与存储指令
* 算术指令
* 配置指令
三类指令中的操作数包含标量操作数（scalar operand）、矢量操作数（vector operand）和掩码操作数（masking operand）。

“The RISC-V Vector ISA is a very clean and optimized set of instructions, with the base ISA numbering around just 300 instructions, far smaller than a typical packed-SIMD alternative.”
尽管RISC-V已经足够简洁，但基本的V扩展仍然有300+条指令。

