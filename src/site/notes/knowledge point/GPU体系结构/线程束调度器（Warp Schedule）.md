---
{"dg-publish":true,"tags":["GPU体系结构"],"permalink":"/knowledge point/GPU体系结构/线程束调度器（Warp Schedule）/","dgPassFrontmatter":true}
---

在cuda中，每32个线程会被『捆』成一束(线程束)，英文是`warp`, 一个warp执行一个指令，换句话说32个线程每次都是执行相同的指令。指令发射任务是由warp scheduler来完成的，这个线程集合被“编织在一起”并且“步调一致”的形式执行。在程序中的每一行，线程束中的每个线程都将在不同数据上执行相同的命令。