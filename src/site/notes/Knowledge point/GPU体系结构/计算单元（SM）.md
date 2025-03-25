---
{"dg-publish":true,"tags":["GPU体系结构"],"permalink":"/Knowledge point/GPU体系结构/计算单元（SM）/","dgPassFrontmatter":true}
---

计算单元（Streaming Multiprocessor）：执行计算的。每一个SM都有自己的控制单元（Control Unit），寄存器（Register），缓存（Cache），指令流水线（execution pipelines）。

如下图是一个SM：
![Pasted image 20240513144508.png](/img/user/Knowledge%20point/imgs/Pasted%20image%2020240513144508.png)
其中： 
1） Instruction Cache：指令缓存；  
2） Warp Scheduler：线程束调度器；  
3） Dispatch Unit：指令分发器，根据Warp Scheduler的调度向核心发送指令；  
4） Register File：寄存器；  
5） Core：计算核心，负责浮点数和整数的计算；  
6） DP Unit：双精度浮点数计算单元；  
7） SFU：Special Function Units，特殊函数计算单元；  
8） LD/ST：访存单元；  
9） L1 Cache：一级缓存；  
10） Shared Memcoy：共享内存。

在GP100里，每一个SM有两个SM Processing Block（SMP），里边的绿色的就是CUDA Core，CUDA core也叫Streaming Processor（SP），这俩是一个意思。每一个SM有自己的指令缓存，L1缓存，共享内存。而每一个SMP有自己的Warp Scheduler、Register File等。要注意的是CUDA Core是Single Precision的，也就是计算float单精度的。双精度Double Precision是那个黄色的模块。所以一个SM里边由32个DP Unit，由64个CUDA Core，所以单精度双精度单元数量比是2:1。LD/ST 是load store unit，用来内存操作的。SFU是Special function unit，用来做cuda的intrinsic function的，类似于__cos()这种。