---
{"dg-publish":true,"permalink":"/knowledge point/tools/Verilator/","dgPassFrontmatter":true}
---

### DPI-C机制
![Pasted image 20240425183448.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240425183448.png)
#### 在sv中定义
import “DPI-C” function read sin(input real r);
此处的input/output均是针对C函数而言的“input/output”
#### 在C中调用
定义同名函数即可，但要注意变量类型之间的对应关系。
#### 需要数组作为参数，怎么办？
可以在sv中定义openarray，也可以是定宽数组，如下是定宽数组：
```verilog
import "DPI-C" function void fib(output bit[31:0] data[20])
```
同样的，在sv中用Verilog定义二维数组：
``` verilog
bit[31:0] data[20]
```
最后在C中调用函数
``` c
void fib(svBitVecVal data[20]) { }
```
