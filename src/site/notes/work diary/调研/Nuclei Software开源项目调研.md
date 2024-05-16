---
{"dg-publish":true,"tags":["调研"],"permalink":"/work diary/调研/Nuclei Software开源项目调研/","dgPassFrontmatter":true}
---

# Nuclei Software

本组织主要提供芯来科技基础软件平台相关的代码，主要包含：

nuclei-sdk

芯来科技RISC-V处理器嵌入式软件SDK平台Nuclei SDK，基于 NMSIS 构建，用户可以访问 NMSIS 提供的所有 API 以及 Nuclei SDK 提供的 API，主要用于板载外设访问，如 GPIO、UART、SPI 和 I2C 等。只支持2023.10版本以后的Toolchain或Nuclei Studio。

包含大量的示例代码，例如通过 FreeRTOS、UCOSII 和 RT-Thread 提供实时操作系统服务、为嵌入式系统软件初学者和资源有限的用例提供裸机服务等等。

nuclei-linux-sdk

芯来科技RISC-V处理器Linux软件平台Nuclei Linux SDK，没有找到用法的说明，看上去像是一个用来为Nuclei的SoC构建Linux系统的仓库。

## Nuclei Board Labs

这里面包含多个可以在Nuclei指定的开发板上直接运行的实验例程，是Nuclei 大学课程的一部分，通过Nuclei SDK开发。

## Nuclei MCU Software Interface Standard (NMSIS)

NMSIS是一个独立于供应商的硬件抽象层，适用于基于 Nuclei 处理器的微控制器。NMSIS 定义了通用工具接口，为处理器和外设提供了简单的软件接口，能够跨开发工具和微控制器进行开发，简化了软件重用，缩短了微控制器开发人员的学习曲线，并缩短了新设备的上市时间，（感觉有点像HAL库之于STM32）。

## Nuclei RISC-V Toolchain

Nuclei这套工具链是由risc-v基金会的官方工具链修改而来的，官方工具链存储库位于 https://github.com/riscv-collab/riscv-gnu-toolchain.git。Nuclei 维护的工具链 repo 位于 https://github.com/riscv-mcu/riscv-gnu-toolchain，分支为 nuclei/2024，其中包含的工具版本有：gcc13、binutils2.40、gdb13.2，并且还合并了来自其上游的一些重要补丁，以及对 Nuclei 自定义扩展和流水线等的额外支持。

如果要为Prism构建一个包含库函数、示例、实时操作系统服务等内容的软件生态的话，可以参考Nuclei的这些开源项目的源码，主要是NMSIS（库函数）以及nuclei-sdk（基于库函数的各种应用），但是考虑到硬件实现有不同，直接拿来用是不行的，要花一些时间进行修改。

# Nuclei 开源硬件

有关Nuclei硬件相关的开源项目目前就找到蜂鸟E203。官网上还可以找到一些架构的说文档，例如Bumblebee内核、Nuclei_N级别指令架构手册（不是最新的），但没有开源的硬件可以参考。

# RISC-V GCC and ARM GCC

GNU编译器集合(GNU Compiler Collection GCC)是大多数嵌入式系统的传统编译器，因为它作为GNU/Linux的官方编译器，在后端支持许多不同的指令集体系结构(ISA)，GCC也是第一个支持RISC-VISA的编译器。

GNU ARM GCC是GCC git库（https://gcc.gnu.org/git/?p=gcc.git;a=summary）中的ARM供应商分支。所谓的Arm GNU Toolchain是由社区支持的用于基于Arm CPU的预构建GNU编译器工具链。

RISC-V官方的GNU RISC-V GCC在仓库https://github.com/riscv-collab/riscv-gcc。RISC-V GCC是RISC-V Toolchain的一个组成部分。

根据经验ARM GCC要比RISC-V GCC性能更好，例如下图是RISC-V官方网站在2019发的一个PPT中的图：

想说明的是对于同一个软件，在不同的优化条件下ARM GCC编译出程序的CodeSize都要比RISC-V GCC的小。但是没有再找到别的书面的材料说明具体有哪些不同。

Prism仓库里现在用的是xpack版本的risc-v gcc 工具链，指的是GNU GCC的Windows平台发行版，与上游 GNU 版本相比，没有功能变化。我觉得工具链好像没有必要很关注，感受不明显，如果要用Nuclei的SoC的话，由于他们的SoC有自定义扩展等内容，所以需要使用他们的工具链。

## 参考

除了标出来的链接以外，参考了Nuclei官方的documents、github的readme、Nuclei的工具手册仓库https://github.com/Nuclei-Software/nuclei-tool-guide/tree/master以及RISC-V
官方的ppt（比较早之前了）：https://riscv.org/wp-content/uploads/2019/12/12.10-14.00a-Software-PPA-Metrics-Results-from-Real-world-MCU-Security-Applications.pdf