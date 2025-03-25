---
{"dg-publish":true,"tags":["C","Bugs"],"permalink":"/Knowledge point/C/Bugs/text variable, no debug info/","dgPassFrontmatter":true}
---

### 问题描述
在Ubuntu下VSCODE中仿真C工程，编译时已经加上-g参数，可以打断点，但只有部分函数可以正常触发断点，且有的函数除法断点后没法单步调试（即单步调试不能进入函数内部）。

### 原因
经排查，注意到<text variable, no debug info>字样，看上去是没有足够的调试信息，可是我已经加入-g参数，遗漏了什么？
将-g修改为-g3后可以正常调试，有关-g和-g3的区别见[[Knowledge point/C/Debug时的C编译选项\|Debug时的C编译选项]]。
-g参数默认等同于-g2，-g3在-g2的基础上增加了更多调试信息，所以合理推测这个问题的原因正是没有足够的调试信息。那么-g3在-g2的基础上具体增加了什么信息？