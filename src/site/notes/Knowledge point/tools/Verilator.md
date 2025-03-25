---
{"dg-publish":true,"permalink":"/Knowledge point/tools/Verilator/","dgPassFrontmatter":true}
---

### DPI-C机制
![Pasted image 20240425183448.png](/img/user/Knowledge%20point/imgs/Pasted%20image%2020240425183448.png)
#### 在sv中定义
import “DPI-C” function read sin(input real r);
此处的input/output均是针对C函数而言的“input/output”
#### 在C中调用
定义同名函数即可，但要注意变量类型之间的对应关系。
#### 需要数组作为参数，怎么办？
可以在sv中定义如下的定宽数组：
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
也可以使用OpenArray，在sv中定义数组:
``` verilog
import "DPI-C" function void dpic_regs_read(input logic [31:0] rf[]);

reg [DATA_WIDTH-1:0] rf [2**ADDR_WIDTH-1:0];

always @(posedge clk) begin
	dpic_regs_read(rf);
end
```
在C中定义：
``` C
extern "C" void dpic_regs_read(const svOpenArrayHandle regs) {
temp = (word_t*)(((VerilatedDpiOpenVar*)regs)->datap());
}
```
DPI-C的资料不好找，没有找到关于“VerilatedDpiOpenVar*”的具体说明，包括Verilator官方手册也只有一个比较短的章节描述了有关DPI-C的内容，但经过测试如上的方法是可用的，实现了将一个32bit的cpu寄存器堆的数据通过DPI-C从sv传输到C中。