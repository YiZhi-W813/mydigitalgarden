---
{"dg-publish":true,"tags":["Bugs","C"],"permalink":"/knowledge point/C/通过core dump快速定位Segmentation fault/","dgPassFrontmatter":true}
---

### 开启core dump
参考：https://blog.csdn.net/qq_62783912/article/details/130566836

### 使用gdb调试core dump
* 找到程序崩溃时留下的core文件
* gdb <程序名> <core文件名>
* 通过bt查看调用堆栈信息/p打印变量值/f 0跳转到程序崩溃处