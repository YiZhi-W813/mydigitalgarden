---
{"dg-publish":true,"tags":["risc-v","向量处理器"],"permalink":"/Knowledge point/RISC-V INS/RISC-V Vector扩展对比ARM SIMD指令集/","dgPassFrontmatter":true}
---

# 传统的SIMD面临挑战

使用特定矢量长度（VLS）[SIMD](https://en.wikipedia.org/wiki/SIMD)指令集时，主要问题是选择正确的矢量寄存器大小。当然，在数据级并行度和硬件成本之间需要权衡。由于摩尔定律，矢量寄存器大小可以随着时间的推移而增加，而不会使CPU芯片更加昂贵。此外，一些用户对具有更宽矢量寄存器的强大CPU感兴趣，而普通用户则对平均大小的寄存器感兴趣。因此，没有一个正确的向量寄存器大小。例如，在x86中，答案是提供一个接一个的VLSISA，例如[MMX](https://en.wikipedia.org/wiki/MMX_(instruction_set))（64位）、[SSE](https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions)（128位）、[AVX](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions)（256位）和[AVX512](https://en.wikipedia.org/wiki/AVX-512)（512位）。

为了满足[向后兼容性](https://en.wikipedia.org/wiki/Backward_compatibility)，添加新VLS ISA的每个CPU还必须支持所有现有CPU。这会导致操作码空间的浪费，并增加CPU指令解码器的复杂性。当然，这也增加了程序员的复杂性，因为程序员会记住（或一直查找）所有VLS ISA之间的语法和功能差异。

这意味着，虽然为较小的向量寄存器编写的VLS代码在较新的CPU上运行，但它不能使用较宽的向量寄存器。因此，必须一次又一次地重新实现现有代码，以利用新的VLS ISA。同样，为高端CPU编写的代码不能在中端CPU上运行（因为它需要具有更宽矢量寄存器的VLS-ISA）。因此，要么必须针对一些较旧的（希望广泛可用）VSL-ISA，要么必须为不同的VSL-ISA提供多个实现。

解决所有这些问题的解决方案是设计一个可变长度的向量指令集。这样一来，指令就与具体CPU实现的向量寄存器大小无关。因此，二进制代码可以在低端、中端和高端CPU之间移植，并自动在较新的CPU中使用更宽的寄存器。

# 2 ARM中的SIMD

## 2.1 ARM早期的SIMD指令集

ARMv6架构引入了SIMD指令集的一小部分，对打包到标准32位通用寄存器中的多个16位或8位值进行操作。这允许某些操作以两倍或四倍的速度执行，而无需实现额外的计算单元。这些指令的助记符通过将8或16附加到基本形式来识别，以指示操作的数据值的大小。

## 2.2 NEON（传统的SIMD）

ARM7架构引入了高级SIMD扩展（advanced Single Instruction Multiple Data architecture extension）作为ARMV7-A和ARMV7-R系列处理器的可选扩展。

NEON就是一种基于SIMD思想的ARM技术，相比于ARMv6或之前的架构，NEON结合了64-bit和128-bit的SIMD指令集，提供128-bit宽的SIMD运算。

## 2.3 SVE(Scalable Vector Extension)

SVE，可伸缩向量指令，是ARMv8-A架构的一个扩展，它是针对高性能计算(HPC)和机器学习等领域开发的一套全新的矢量指令集，它是下一代SIMD指令集实现，而不是NEON指令集的简单扩展。SVE指令集中有很多概念与NEON指令集类似，例如矢量、通道、数据元素等。SVE指令集也提出了一个全新的概念：可变矢量长度编程模型(VectorLength Agnostic,VLA)。

传统的SIMD指令集采用固定大小的向量寄存器，例如NEON指令集采用固定的128位长度的矢量寄存器。而支持VLA编程模型的SVE指令集则支持可变长度的矢量寄存器。这样允许芯片设计者根据负载和成本来选择一个合适的失量长度。SVE指令集的矢量寄存器的长度最少支持128位，最大可以支持2048位，以128位为增量。SVE设计确保同一个应用程序可以在支持不同矢量长度的SVE指令机器上运行，而不需要重新编译代码，这是VLA编程模型的精髓。

## 2.4 SME(Scalable Matrix Extension)

ARMV9引入了SME(Scalable Matix Extension)，SME建立在SVE2的基础之上，新增了高效处理矩阵的能力,旨在加速AI和机器学习应用，特别是对于大型生成式AI模型。SME通过增强矩阵操作，提供了更高的灵活性和效率，以应对日益复杂的AI需求，确保了Arm架构在AI领域的持续竞争力和创新能力。

指令集的目的和功能看上去与玄铁提出的Matrix扩展类似。

## 2.5 MVE(M-profile Vector Extension)

MVE是针对ARM Corex-M系列处理器的向量处理扩展，旨在提高能效和处理效率，适合用于微控制器和嵌入式系统。它提供对各种SIMD操作的支持。Arm Helium技术Arm Cortex-M处理器系列的M系列向量扩充方案(MVE)。Helium为Armv8.1-M架构的延伸，可协助机器学习(ML)和数字信号处理(DSP)应用大幅提升效能。

MVE有两种变体，ME-I和MVE-F。MVE-I仅支持整数矢量，MVE-F支持浮点数矢量。在处理器核心中包含VE-F还要求处理器支持MVSE-I和浮点扩展。

# 3 RISC-V的SIMD vs. ARM的SIMD

Flynn分类法是计算机体系结构中的经典分类方法。根据Flynn的分类法，向量处理也属于SIMD类，如前文所述，向量处理与传统的SIMD最大的区别在于向量长度是否固定。

ARM中既有传统的SIMD指令集（NEON），又有向量处理指令集（SVE、MVE）。而RISC-V最新的指令集手册中把P扩展（传统的SIMD）砍掉了，只剩下了向量扩展Vector。

传统的SIMD（x86的AVX、arm的Neon）指令集是长度固定的，所以软件和硬件是捆绑的，导致了一个严重的问题：软件的可移植性差，以ARM SVE和RISC-V的RVV为首的新的与长度无关向量指令集架构（ISA）解决了该问题。可变长度的矢量指令集是ARM和RISC-V的共同发展方向，所以只能说RVV相比ARM的传统的SIMD（NEON）技术相比有软件移植性好这个巨大优势，但是ARM新的SIMD指令集和RVV总体发展思路和定义是非常相似的，是一个路数，所以我觉得不能说RVV就比ARM的SIMD（新的）更好。