---
{"dg-publish":true,"permalink":"/knowledge point/GPU体系结构/thread，block，grid的概念与关联/","dgPassFrontmatter":true}
---

- **thread:** 一个CUDA的并行程序会被以许多个thread来执行。
- **block:** 数个thread会被群组成一个block，同一个block中的thread可以同步，也可以通过shared memory进行通信。这里为什么要有一个中间的层次block呢？这是因为CUDA通过这个概念，提供了细粒度的通信手段，因为block是加载在SM上运行的，所以可以利用SM提供的shared memory和__syncthreads()功能实现线程同步和通信，这带来了很多好处。而block之间，除了结束kernel之外是无法同步的，一般也不保证运行先后顺序，这是因为CUDA程序要保证在不同规模（不同SM数量）的GPU上都可以运行，必须具备规模的可扩展性，因此block之间不能有依赖。
- **grid:** 多个block则会再构成grid。
![Pasted image 20240518095838.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240518095838.png)