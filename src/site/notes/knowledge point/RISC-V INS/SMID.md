---
{"dg-publish":true,"permalink":"/knowledge point/RISC-V INS/SMID/","dgPassFrontmatter":true}
---

**这个概念分在这个目录下有些不合适，但我暂时还没想到它应该归类到哪儿**

SIMD(**Single Instruction Multiple Data**)即单指令流多数据流，是一种采用一个控制器来控制多个处理器，同时对一组数据（又称“数据向量”）中的每一个分别执行相同的操作从而实现空间上的并行性的技术。简单来说就是一个指令能够同时处理多个数据。
这种技术不止在CPU中使用，在GPU的ALU单元内，一条指令可以处理多维向量（一般是4D）的数据。比如，有以下shader指令：
```
float4 c = a + b; // a, b都是float4类型
```
对于没有SIMD的处理单元，需要4条指令将4个float数值相加，汇编伪代码如下：
```assembly
ADD c.x, a.x, b.x
ADD c.y, a.y, b.y
ADD c.z, a.z, b.z
ADD c.w, a.w, b.w
```
但有了SIMD技术，只需一条指令即可处理完：
```assembly
SIMD_ADD c, a, b
```
![Pasted image 20240513133047.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240513133047.png)

