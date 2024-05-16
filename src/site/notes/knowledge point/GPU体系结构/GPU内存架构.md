---
{"dg-publish":true,"tags":["GPU体系结构"],"permalink":"/knowledge point/GPU体系结构/GPU内存架构/","dgPassFrontmatter":true}
---

GPU内存架构如下图所示：
![Pasted image 20240513144717.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240513144717.png)
1.L1 Cache：Pascal架构上，L1 Cache和Texture已经合为一体（Unified L1/Texture Cache），作为一个连续缓存供给warp使用。
2.L2 Cache：用来做Global Memory的缓存，容量大，给整个GPU使用。