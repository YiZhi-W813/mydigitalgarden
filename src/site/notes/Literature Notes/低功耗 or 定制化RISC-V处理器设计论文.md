---
{"dg-publish":true,"permalink":"/Literature Notes/低功耗 or 定制化RISC-V处理器设计论文/","dgPassFrontmatter":true}
---

# ISA拓展类
### RISC-HD Lightweight_RISC-V Processor for Efficient Hyperdimensional Computing Inference
（RISC-HD：用于高效超维计算推理的轻量级 RISC-V 处理器）
**Published in**: IEEE Internet of Things Journal
**Date of Publication**: 18 July 2022
`简介：`超维（HD）计算是一种轻量级机器学习方法，广泛应用于物联网应用中的分类任务。为了提高超维计算的效率和灵活性而设计了这款增强型的RISC-V内核。所做工作主要是通过扩展 RI5CY （一款开源RISC-V内核后更名为CV32E40P）提出了一种用于 HD 推理的 RISC-V 处理器，称为 RISC-HD，RISC-HD 具有用于 HD 计算的编码和相似性检查阶段的自定义指令。

### RISC-V Galois Field ISA Extension for Non-Binary Error-Correction Codes and Classical and Post-Quantum Cryptography
（适用于非二进制纠错码以及经典和后量子密码学的 RISC-V Galois Field ISA 扩展）
**Published in:** IEEE Transactions on Computers
**Date of Publication:** 12 May 2022
`简介：`由于 5G 新无线电和超 5G 等新通信标准以及量子计算和通信领域的最新进展，出现了将处理器集成到节点的新要求。这些要求旨在提供网络灵活性，以降低运营成本并支持服务和负载平衡的多样性。它们还旨在将新的和经典的算法集成到高效和通用的平台中，执行特定的操作，并以较低的延迟处理任务。此外，一些对于便携式设备至关重要的密码算法（经典和后量子）与纠错码共享相同的算法。例如，高级加密标准 (AES)、椭圆曲线加密、Classic McEliece、Hamming Quasi-Cyclic 和 Reed-Solomon 代码使用 GFð2mÞ 算法。由于该算法是许多算法的基础，因此本文提出了一种通用的 RISC-V Galois field ISA 扩展。 RISC-V 指令集扩展是在 Nexys A7 FPGA 上使用 SweRV-EL2 1.3 实现和验证的。此外，AES、Reed-Solomon 码和 Classic McEliece（后量子密码学）也实现了 5 倍加速，但代价是逻辑利用率提高了 1.27%。

### A RISC-V ISA Extension for Ultra-Low Power IoT Wireless Signal Processing
（用于超低功耗物联网无线信号处理的 RISC-V ISA 扩展）
**Published in:** IEEE Transactions on Computers
**Date of Publication:** 02 March 2021
`简介：`摘要：本文介绍了开源 RISC-V ISA (RV32IM) 的指令集扩展，专用于超低功耗 (ULP) 软件定义的无线物联网收发器。定制指令专为满足正交调制通常所需的 8/16/32 位整数复数运算的需求而定制。所提出的扩展仅占用两个主要操作码，并且大多数指令的设计目标是接近零的能源成本。新架构的指令精确（IA）和周期精确（CA）模型均用于评估六个物联网基带处理测试平台，包括 FSK 解调和 LoRa 前导码检测。仿真结果显示周期数从 19% 提高到 68%。目标 22 nm FDSOI 技术的综合后仿真显示，相对于基准 RV32IM 设计，功耗和面积开销分别低于 1% 和 28%。

# 协处理器
### VPQC: A Domain-Specific Vector Processor for Post-Quantum Cryptography Based on RISC-V Architecture
（VPQC：基于 RISC-V 架构的后量子密码学领域特定矢量处理器）
**Published in:** IEEE Transactions on Circuits and Systems I: Regular Papers
**Date of Publication:** 08 April 2020
`简介：`在5G时代，海量设备需要安全连接到通信网络边缘，而新兴的量子计算机可以轻松破解传统的公钥密码。LBC(Lattice-based cryptography )由于其安全性和效率而成为所有后量子密码学（PQC）中最有前途的方案类型之一。为了满足 5G 高吞吐量和多样化应用场景的要求，我们研究了几种 LBC 候选内核算法的矢量化，从而提出了一种利用可扩展 RISC-V 架构的特定领域矢量处理器 VPQC。

### RISE: RISC-V SoC for En/Decryption Acceleration on the Edge for Homomorphic Encryption
（RISE：用于同态加密边缘加密/解密加速的 RISC-V SoC）
**Published in:** IEEE Transactions on Very Large Scale Integration (VLSI) Systems
**Date of Publication:** 17 July 2023
`简介：`如今，边缘设备通常连接到云以使用其存储和计算功能。这导致了有关用户数据的安全和隐私问题。同态加密 (HE) 是解决数据隐私问题的一种有前景的解决方案，因为它允许对加密数据进行任意复杂的计算，而无需解密。
为了加密数据，边缘设备需要执行编码、误差采样和加密。这三个操作共同构成了“消息到密文”的转换操作。类似地，为了解密从云端接收的数据，边缘设备需要执行解密和解码。这两个操作共同构成了“密文到消息”的转换操作。这些边缘端操作会产生巨大的内存消耗（大约几MB）和计算开销。作者分析认为在边缘侧转换操作中，正是加密和解密操作成为了瓶颈。
为了解决这些问题，设计了这款SoC，包含BlackParrot（一款开源 RISC-V 多核处理器）以及执行错误采样、加密和解密的加速器。


# 低功耗高性能处理器核设计
### A High-Performance Core Micro-Architecture Based on RISC-V ISA for Low Power Applications
（基于 RISC-V ISA 的低功耗应用高性能核心微架构）
**Published in:** IEEE Transactions on Circuits and Systems II: Express Briefs
**Date of Publication**: 08 December 2020
`简介：`:设计了一款具有极低功耗的高性能的新型处理器微架构。微架构基于RISC-V指令集架构（ISA）。该内核在 Xilinx Virtex-7 FPGA 板上实现和验证，资源需求为 7617 个 LUT 和 2319 个 FF。该内核的 D​​hrystone 基准测试得分为 1.71 DMIPS/MHz，高于 ARM Cortex-M3（1.50 DMIPS/MHz）和 ARM Cortex-M4（1.52 DMIPS/MHz）。 Coremark 基准测试也在该内核上进行了测试，每 MHz 的 Coremark 为 4.13。总之就是该内核的功耗和性能优于许多现有的商业和开源内核。

### HAMSA-DI: A Low-Power Dual-Issue RISC-V Core Targeting Energy-Efficient Embedded Systems
（HAMSA-DI：面向高能效嵌入式系统的低功耗双核 RISC-V 内核）
**Published in:** IEEE Transactions on Circuits and Systems I: Regular Papers
**Date of Publication:** 23 October 2023
摘要：设计了HAMSA-DI，这是一款占用空间小、低功耗的嵌入式 RISC-V 内核，具有动态调度、有序、双发射流水线，支持流行的 Xpulp 扩展。所提出的经济高效的双问题实施方案与通用基准下的基准低功耗核心相比，显着提高了性能并提高了能效。其中包括 3.48 CM/MHz (+22%) 的 CoreMark 分数和 1.3 (+13%) 的 Embench 分数，某些基准测试显示比基准 CV32E40P 内核能耗低多达 22%。所提出的设计是作为 16 nm 测试芯片的一部分制造的，运行频率为 1 GHz，电源电压为 0.8V。芯片测量表明，与单问题核心上的编译代码相比，所提出的核心对于完全双问题利用运行的程序来说，性能可提高多达 8 倍，能效提高多达 6.5 倍。

# 基于RISC-V核设计的SoC
### Arnold: An eFPGA-Augmented RISC-V SoC for Flexible and Low-Power IoT End Nodes
**Published in:** IEEE Transactions on Very Large Scale Integration (VLSI) Systems
**Date of Publication:** 04 March 2021
（Arnold：用于灵活、低功耗物联网终端节点的 eFPGA 增强型 RISC-V SoC）
`简介：`广泛的物联网 (IoT) 应用需要强大、节能且灵活的终端节点来从多个来源获取数据，通过近传感器数据分析算法处理和提取感测数据，并进行无线传输。这项工作展示了 Arnold：采用 22 nm Globalfoundries GF22FDX (GF22FDX) 技术制造的 0.5 至 0.8 V、46.83 µW/MHz、600 MOPS 完全可编程 RISC-V 微控制器单元 (MCU)，并配有状态-最先进的 (SoA) 微控制器到嵌入式现场可编程门阵列 (eFPGA)。我们展示了片上系统 (SoC) 的灵活性，可应对许多新兴物联网应用的挑战，例如将传感器和加速器与非标准接口连接、对从外设流式传输的数据执行动态预处理任务，以及加速近传感器分析、加密和机器学习任务。


