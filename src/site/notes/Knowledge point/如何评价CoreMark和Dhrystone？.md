---
{"dg-publish":true,"permalink":"/Knowledge point/如何评价CoreMark和Dhrystone？/","dgPassFrontmatter":true}
---

#### CoreMark和Dhrystone不是好的benchmark

**ref：一生一芯学习讲义**

CoreMark和Dhrystone属于合成程序(synthetic program), 意思是用若干个代码片段拼接起来的程序. 例如, CoreMark由链表操作, 矩阵乘法和状态机转移操作这三个代码片段组成; Dhrystone则由一些字符串操作的代码片段组成.

合成程序作为benchmark, 最大的问题是其代表性较弱: CoreMark和Dhrystone能代表什么应用场景呢? 相比于SPEC CPU 2006中的各种真实应用, CoreMark中的代码片段顶多只能算C语言的课后作业; Dhrystone就离应用场景更远了, 其代码非常简单(使用短字符串常量), 甚至在现代编译器的作用下, 循环体中的代码片段很可能被深度优化(是否还记得NEMU中的`pattern_decode()`), 使得评估结果虚高, 无法客观反映系统的性能. [这篇文章](https://www.transputer.net/tn/27/tn27.html)

详细分析了Dhrystone作为benchmark的缺陷.

讽刺的是, 今天不少CPU厂商发布产品时, 仍然采用CoreMark或Dhrystone的评测结果来标识产品的性能, 甚至其中还不乏宣称是面向高性能场景的产品. [体系结构一代宗师, 图灵奖得主David Patterson在介绍Embench时](https://www.sigarch.org/embench-recruiting-for-the-long-overdue-and-deserved-demise-of-dhrystone-as-a-benchmark-for-embedded-computing/), 评价Dhrystone已经过时很久, 应该停止使用. 事实上, Dhrystone的第一版是1984年发布的, 从1988年之后Dhrystone就再也没有得到维护和升级. 和上世纪80年代相比, 今天的计算机领域已经发生了翻天覆地的变化, 应用程序早已更新换代, 编译技术日趋成熟, 硬件算力也大幅提升, 使用40年前的benchmark来评测今天的计算机, 其合理性确实有所欠缺.