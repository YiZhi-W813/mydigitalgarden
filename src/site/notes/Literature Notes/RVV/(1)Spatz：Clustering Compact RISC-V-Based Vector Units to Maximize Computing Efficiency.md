---
{"dg-publish":true,"tags":["文献阅读","risc-v"],"permalink":"/Literature Notes/RVV/(1)Spatz：Clustering Compact RISC-V-Based Vector Units to Maximize Computing Efficiency/","dgPassFrontmatter":true}
---

这篇文章更多地在写使用由Spartz+Snitch构成的CC搭建计算集群的事情。只读了有关Spatz的相关部分。
### 微架构
Spatz是基于RISCV向量扩展(RVV) 1.0版本的紧凑的参数化的VPU。Spatz的微架构(一个带有fpu的Spatz实例)及其与共享l1集群的集成如图所示。Snitch和Spatz形成了一个Core Complex(CC)，集成在一个共享l1集群中，有16个SRAM银行，每个8 kb。本节描述Spatz的体系结构，重点介绍其主要组件。
![QQ_1722390990288.png](/img/user/Literature%20Notes/imgs/QQ_1722390990288.png)

Spatz实现了RVV ISA, version 1.0[20]。本文中作者的目标是Zve64d子集，专为具有8,16,32和64位整数和浮点支持的嵌入式向量机而设计。
此外，作者他提到在另一篇Spartz文章中提供了一个针对Zve32x RVV子集的Spatz配置，支持8位、16位和32位整数操作。Spatz与处理器无关，并通过core - v的X-Interface加速器接口与标量核心通信。因此，Spatz可以与任何与X-Interface兼容的内核一起使用。Snitch的小占用空间特别适合于大多数计算发生在矢量单元中的执行范例。

总体看下来内容和另一篇文章中对Spartz的描述没有太大区别。