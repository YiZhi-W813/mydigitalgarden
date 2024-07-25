---
{"dg-publish":true,"tags":["risc-v","文献阅读","SoC"],"permalink":"/Literature Notes/RISC-V SoC/（3）A High-Speed NTT-Based Polynomial Multiplication Accelerator with Vector Extension of RISC-V for Saber Algorithm/","dgPassFrontmatter":true}
---

作者： Jun Han @ 复旦大学
发表在：The 2022 IEEE Asia Pacific Conference on Circuits and Systems_2022

### 简介
提出了一种简洁高效的矢量化 [[knowledge point/Math and Algorithm/数论变换（number-theoretic transform, NTT）\|数论变换（number-theoretic transform, NTT）]] 算法，在此基础上我们设计了一个可配置的矢量 NTT 单元来执行 NTT 和其他算术运算。

### 工作
The contributions of this paper are summarized as follows:
1) 为了使 Saber 能够实现 NTT，我们展示了如何选择合适的prime进行field extension，同时可以方便地进行modular multiplication reduction。
2) 提出了一种高效的矢量 NTT 算法，无需预处理、后处理和位反转操作。vector NTT 单元，旨在实现高吞吐量蝶形运算和算术运算。
3) 设计了一个具有 7 个流水线阶段的协处理器，以实现高速计算。协处理器的功能由 RISC-V ISA 的自定义矢量扩展驱动。
4) 该设计采用 TSMC 28nm CMOS 技术实现，与最先进的 Saber 模乘法器相比，计算时间和 ATP（Area-Time-Product） 分别提高了 5 倍和 3 倍。

### 硬件结构
![QQ_1721786723480.png](/img/user/Literature%20Notes/imgs/QQ_1721786723480.png)
使用Chisel系统由一个通用的RISC-V处理器和一个NTT协处理器组成，协处理器能够执行自定义 RISCV 指令并加速 Saber 的多项式乘法。协处理器具有从 RISC-V 内核接收指令的接口。两个核共享L1 Data Cache。协处理器内部7级流水线，译码和写回各占一个周期，执行阶段占五个周期。
文章对硬件的描述非常简略。
### 扩展指令集
![QQ_1721787286105.png](/img/user/Literature%20Notes/imgs/QQ_1721787286105.png)
基于所提出的自定义指令扩展，Saber 中两个 256 阶多项式的乘法只需 112 条指令即可高效完成。