---
{"dg-publish":true,"tags":["数学与算法"],"permalink":"/knowledge point/Math and Algorithm/LU分解的高斯消元法求解线性方程组/","dgPassFrontmatter":true}
---

### 要解决的问题
已知`Ax = b`求解x

### 求解思路
设`A = LU`，通过LU分解，可以通过已知的A分解出**确定**的L阵和U阵（具体分解方法见下一节）。
那么我们得到`LUx = b`，即
$$
\left\{\begin{aligned} Ly=b    （1）\\Ux=y    （2） \end{aligned}\right.
$$
在（1）中，L阵由LU分解得到，b已知，所以可以解出y。
在（2）中，U阵由LU分解得到，y已知，所以解出x也是手拿把掐。

### 如何通过LU分解得到L阵和U阵
最简单的方法是高斯消元法：
![Pasted image 20240505104645.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240505104645.png)
易于理解的是$L_2L_1L_0=L^{-1}$，消元的结果就是U。
算法：
![Pasted image 20240505104738.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240505104738.png)
因为有三重嵌套的循环，所有LU分解的复杂度是 𝑂(𝑛3) ，其中 加法操作∼${1 \over 3}𝑛^3$，乘法操作 ∼${1 \over 3}𝑛^3$，一共∼${2 \over 3}𝑛^3$。
总体来看，使用LU分解求解线性系统的计算量：
- 第一步：LU分解：∼${2 \over 3}𝑛^3$ （计算量最大的部分）
- 第二步：求解 𝐿𝑦=𝑏： ∼$𝑛^2$
- 第三步：求解 𝑈𝑥=𝑦： ∼$𝑛^2$