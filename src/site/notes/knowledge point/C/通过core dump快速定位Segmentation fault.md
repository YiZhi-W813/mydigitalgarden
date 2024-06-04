---
{"dg-publish":true,"tags":["Bugs","C"],"permalink":"/knowledge point/C/通过core dump快速定位Segmentation fault/","dgPassFrontmatter":true}
---

### 开启core dump
参考：https://blog.csdn.net/qq_62783912/article/details/130566836

### 使用gdb调试core dump
* 找到程序崩溃时留下的core文件
* gdb <程序名> <core文件名>
* 通过bt查看调用堆栈信息/p打印变量值/f 0跳转到程序崩溃处

### 我总是找不到生成的coredump文件怎么办
`cat /proc/sys/kernel/core_pattern`
如果终端输出了一些信息，例如一个奇怪的路径什么的，这意味着生成的coredump文件被发送到某个位置了，这不是我们想要的。
因此我们可以`$ sudo gedit core_pattern`将里面的内容修改为`core`，即可正确地在可执行文件处生成coredump文件。

