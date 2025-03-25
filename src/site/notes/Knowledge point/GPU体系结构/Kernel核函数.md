---
{"dg-publish":true,"permalink":"/Knowledge point/GPU体系结构/Kernel核函数/","dgPassFrontmatter":true}
---

CUDA是Nvidia提供的编程接口，用于为其GPU编写程序。在CUDA中，你可以用类似于C/C++函数的形式来表达你想要在GPU上运行的计算，这个函数被称为内核。内核对作为函数参数提供给它的数字向量进行并行操作。一个简单的例子是执行向量加法的内核，即，一个内核，它将两个数字向量作为输入，将它们逐元素相加，并将结果写入第三个向量。