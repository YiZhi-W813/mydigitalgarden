---
{"dg-publish":true,"permalink":"/Literature Notes/从零开始设计一个GPU/","dgPassFrontmatter":true}
---

### 参考📕
* https://mp.weixin.qq.com/s/YdRdWcXDC5HTIG0OTOpHzQ
* https://twitter.com/MajmudarAdam/status/1783304260303855774
* https://github.com/adam-maj/tiny-gpu?tab=readme-ov-file#kernels
* https://mp.weixin.qq.com/s/gDWQGs4MyVWqsmONdEqvpQ
### 概述
一位工程师在Twitter上分享了自己DIY GPU的全流程。
“如果您想了解 CPU 从架构到控制信号的整个工作原理，网络上有许多资源可供参考，而GPU却与之不同，由于 GPU 市场竞争如此激烈，所有现代架构的底层技术细节仍然是专有的。虽然有很多资源可以学习 GPU 编程，但几乎没有任何资源可以学习 GPU 在硬件级别的工作原理。
最好的选择是浏览[Miaow](https://github.com/VerticalResearchGroup/miaow)和[VeriGPU](https://github.com/hughperkins/VeriGPU/tree/main)等开源 GPU 实现，并尝试弄清楚发生了什么。这是具有挑战性的，因为这些项目的目标是功能完整和实用，因此它们非常复杂。
这就是我建设`tiny-gpu`的原因。”
**tiny-gpu**是一个最小的 GPU 实现，经过优化，可以完整地学习 GPU 的工作原理。

### 设计步骤
#### 步骤 1 ：学习 GPU 架构的基础知识
作者主要提到了以下基础知识：
- 全局内存（Global Memory）：存储数据和访问它的程序的外部内存是 GPU 编程的巨大瓶颈和限制，[[Knowledge point/GPU体系结构/全局内存\|全局内存]]。
- 计算单元（Streaming Multiprocessor，SM）：在不同线程中并行执行内核代码的主要计算单元，[[Knowledge point/GPU体系结构/计算单元（SM）\|计算单元（SM）]]。
- 分层缓存：缓存可最大限度地减少全局内存访问。
- 内存控制器：处理对全局内存的限制请求（不知道是不是翻译的问题，这究竟是个什么东西？）。
- 调度程序：GPU的主要控制单元，将线程分配给可用资源执行。

#### 步骤2：创建我自己的 GPU 架构
作者的目标是创建一个最小的 GPU，以突出 GPU 的核心概念并消除不必要的复杂性，以便其他人可以更轻松地了解 GPU。
作者想强调 GPU 在通用并行计算 (GPGPU) 和 ML 方面的更广泛用例，因此我决定专注于核心功能而不是图形特定硬件。
综合以上考虑，作者的GPU架构如下图所示：
![Pasted image 20240513162817.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240513162817.png)
* Device Control Register：设备控制寄存器通常存储指定内核应如何在 GPU 上执行的元数据。在这种情况下，设备控制寄存器仅存储thread_count为活动内核启动的线程总数。
* Dispatcher：一旦内核启动，调度程序就是实际管理将线程分配到不同计算核心的单元。调度程序将线程组织成可以在单个核心上并行执行的组（称为块），并将这些块发送出去以供可用核心处理。一旦处理完所有块，调度程序就会报告内核执行已完成。
![Pasted image 20240513162926.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240513162926.png)
* Scheduler：每个核心都有一个调度程序来管理线程的执行。tiny-gpu 调度程序在拾取新块之前执行单个块的指令直至完成，并且它同步且顺序地执行所有线程的指令。在更高级的调度程序中，使用诸如流水线之类的技术来流式执行多个指令和后续指令，以便在前面的指令完全完成之前最大化资源利用率。此外，warp 调度可用于并行执行块内的多批线程。调度程序必须解决的主要约束是与从全局内存加载和存储数据相关的延迟。虽然大多数指令可以同步执行，但这些加载存储操作是异步的，这意味着指令执行的其余部分必须围绕这些漫长的等待时间构建。
* Fetcher：从程序存储器中异步获取当前程序计数器处的指令（实际上大多数应该是在执行单个块后从缓存中获取）。
* Decoder：将获取的指令解码为线程执行的控制信号。
* Registers：每个寄存器由 4 bit，因此共有 16 个寄存器。前 13 个寄存器 R0 - R12 是支持读/写的空闲寄存器。最后 3 个寄存器是特殊的只读寄存器，用于提供对 SIMD 至关重要的%blockIdx、%blockDim 和 %threadIdx。每个线程都有它自己的专用寄存器文件集。寄存器文件保存每个线程正在执行计算的数据，从而实现同一指令多数据 (SIMD) 模式。重要的是，每个寄存器文件包含一些只读寄存器，保存有关本地执行的当前块和线程的数据，使内核能够根据本地线程 ID 使用不同的数据执行。
#### 步骤三：为我的 GPU 编写自定义汇编语言
作者受 LC4 ISA 的启发制作了自己的小型 11 指令 ISA，以允许我编写一些简单的矩阵数学内核作为概念证明。
![Pasted image 20240513164852.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240513164852.png)
- `BRnzp` - 如果 NZP 寄存器与指令中的 nzp 条件匹配，则跳转到另一行程序存储器的分支指令。
- `CMP` - 比较两个寄存器的值，并将结果存储在 NZP 寄存器中，以用于以后的 BRnzp 指令。
- `ADD`, `SUB`, `MUL`, `DIV` - 启用张量数学的基本算术运算.
- `LDR` - 从全局内存加载数据。
- `STR` - 将数据存储到全局内存中。
- `CONST` - 将常量值加载到寄存器中。
- `RET` - 表示当前线程已达到执行结束的信号。

#### 步骤4 ：使用我的 ISA 编写矩阵数学内核
作者使用这套 ISA 编写了一个矩阵加法和矩阵乘法内核作为概念证明，以演示使用 GPU 进行 SIMD 编程和执行。
矩阵加法：此矩阵加Kernel核通过在单独的线程中执行 8 个元素的逐向加法来添加两个 1 x 8 矩阵。
![Pasted image 20240515154925.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240515154925.png)
矩阵乘法：矩阵乘法核将两个 2x2 矩阵相乘。它对相关行和列的点积执行元素计算，并使用 CMP 和 BRnzp 指令来演示线程内的分支。
![Pasted image 20240515155015.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240515155015.png)
#### 步骤5 ：在 Verilog 中构建我的 GPU 并运行我的内核
![Pasted image 20240515151854.png](/img/user/Literature%20Notes/imgs/Pasted%20image%2020240515151854.png)
这类似于标准的 CPU 架构，并且在功能上也非常相似。主要区别在于 %blockIdx、%blockDim 和 %threadIdx 值位于每个线程的只读寄存器中，从而启用 SIMD 功能。

#### 步骤6 ：将我的设计转换为完整的芯片布局
作者使用OpenLane EDA在Skywater 130nm 工艺节点上完成了芯片设计，并通过Tiny Tapeout项目进行流片。

### 高级功能
#### 多层缓存和共享内存（Multi-layered Cache & Shared Memory）
在现代 GPU 中，使用多个不同级别的缓存来最大程度地减少需要从全局内存访问的数据量。tiny-gpu 在请求内存的单个计算单元和存储最近缓存数据的内存控制器之间仅实现一个缓存层。
通过实现多层缓存，可以将经常访问的数据更本地地缓存到使用位置（在单个计算核心中有一些缓存），从而最大限度地减少此数据的加载时间。
使用不同的缓存算法来最大化缓存命中率 - 这是一个关键维度，可以改进以优化内存访问。
此外，GPU 通常使用同一块内线程的共享内存来访问可用于与其他线程共享结果的单个内存空间。

#### 内存合并（ Memory Coalescing）
GPU 使用的另一个关键内存优化是内存合并。并行运行的多个线程通常需要访问内存中的顺序地址（例如，一组线程访问矩阵中的相邻元素） - 但这些内存请求中的每一个都是单独放入的。
内存合并用于分析排队的内存请求，并将相邻请求合并到单个事务中，从而最大限度地减少寻址和同时发出所有请求所花费的时间。

#### 流水线（Pipelining）
在 tiny-gpu 的控制流中，内核等待在一组线程上执行一条指令，然后再开始执行下一条指令。
现代 GPU 使用流水线来一次流式传输多个顺序指令的执行，同时确保相互依赖的指令仍按顺序执行。
这有助于最大限度地提高内核内的资源利用率，因为资源在等待时不会处于空闲状态（例如：在异步内存请求期间）。

#### 翘曲调度（Warp Scheduling）
另一种用于最大限度地提高课程资源利用率的策略是翘曲调度。这种方法涉及将块分解为可以一起执行的单个批次的 thead。
通过在另一个经线等待时执行来自一个经线的指令，可以在单个内核上同时执行多个经线。这类似于流水线，但处理来自不同线程的指令。

#### 分支发散（Branch Divergence）
假设单个批处理中的所有线程在每条指令后最终都在同一台 PC 上结束，这意味着线程可以在其整个生命周期内并行执行。
实际上，单个线程可能会彼此发散，并根据其数据分支到不同的行。对于不同的PC，这些线程需要拆分为单独的执行行，这需要管理发散的线程并注意线程何时再次收敛。

#### 同步和障碍（Synchronization & Barriers）
现代 GPU 的另一个核心功能是能够设置屏障，以便块中的线程组可以同步并等待，直到同一块中的所有其他线程都达到某个点后再继续执行。
这对于线程需要相互交换共享数据的情况非常有用，以便它们可以确保数据已得到完全处理。