---
{"dg-publish":true,"permalink":"/Literature Notes/（2）HAMSA-DI A Low-Power Dual-Issue RISC-V Core Targeting Energy-Efficient Embedded Systems/","dgPassFrontmatter":true}
---

### 摘要
RISC-V架构最近成为一种流行的开源选择，用于设计具有广泛操作规范的通用内核。在本文中，我们提出了HAMSA-DI，一个占勇敢面积小，节能，嵌入式RISC-V核心，具有动态调度，有序，双发射，支持流行的Xpulp扩展。提出的具有成本效益的双问题实现提供了显着的性能提升，并在普通基准下提高了基线低功耗核心的能源效率。其中包括CoreMark得分为3.48 CM/MHz(+22%)和Embench得分为1.3(+13%)，某些基准测试显示的能量比基准CV32E40P核心少22%。所提出的设计是作为16nm测试芯片的一部分制造的，运行在1 GHz, 0.8V电源电压下。硅测量表明，与在单问题核心上编译的代码相比，所提出的核心可以将完全双问题利用的程序的性能提高多达8倍，能源效率提高多达6.5倍。

### WHY？
多年来，处理器这类可编程硬件（cpu）一直由专有指令集（ISA）主导，尤其是用于高性能硬件的X86以及各类嵌入式平台的ARM。而目前RISC-V在嵌入式计算领域变得特别流行，在这个领域，特殊的功能、超低功耗和低成本往往比高性能和操作系统支持更重要。本文提出了一款名叫“HAMSA-DI”的嵌入式RISC-V核，小面积、低功耗并且还有不错的性能。

### HOW？