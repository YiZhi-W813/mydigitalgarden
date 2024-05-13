---
{"dg-publish":true,"permalink":"/Literature Notes/从零开始设计一个GPU/","dgPassFrontmatter":true}
---

### 参考📕
* https://mp.weixin.qq.com/s/YdRdWcXDC5HTIG0OTOpHzQ
* https://twitter.com/MajmudarAdam/status/1783304260303855774
* https://github.com/adam-maj/tiny-gpu?tab=readme-ov-file#kernels

### 概述
一位工程师在Twitter上分享了自己DIY GPU的全流程。
“如果您想了解 CPU 从架构到控制信号的整个工作原理，网络上有许多资源可供参考，而GPU却与之不同，由于 GPU 市场竞争如此激烈，所有现代架构的底层技术细节仍然是专有的。虽然有很多资源可以学习 GPU 编程，但几乎没有任何资源可以学习 GPU 在硬件级别的工作原理。
最好的选择是浏览[Miaow](https://github.com/VerticalResearchGroup/miaow)和[VeriGPU](https://github.com/hughperkins/VeriGPU/tree/main)等开源 GPU 实现，并尝试弄清楚发生了什么。这是具有挑战性的，因为这些项目的目标是功能完整和实用，因此它们非常复杂。
这就是我建设`tiny-gpu`的原因。”
**tiny-gpu**是一个最小的 GPU 实现，经过优化，可以完整地学习 GPU 的工作原理。