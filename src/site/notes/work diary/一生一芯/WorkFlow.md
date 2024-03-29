---
{"dg-publish":true,"‌‌‌　　tags":["tools"],"permalink":"/work diary/一生一芯/WorkFlow/","dgPassFrontmatter":true}
---


### verilator
1.编辑源文件
2.编译源文件，例如
	verilator -cc -trace --timing --exe --build cnt_ceil_tb.v sim_main.cpp
3.执行obj目录下的可执行文件
4.gtkwave根目录下的波形文件

### nvboard
1.编辑约束文件，连接对应管脚
2.修改Makefile中的top module name
3.修改main.cpp中的头文件名以及实例化的对象名
4.make run

### yosys-sta
1.修改Makefile：
* `DESIGN` `SDC_FILE` `RTL_FILES`
2.修改SDC文件中的时钟端口名与顶层设计文件的时钟端口名对应
3.make sta
4.在result目录下可以看到：
* `gcd.netlist.v` - Yosys综合的网表文件
* `synth_stat.txt` - Yosys综合的面积报告
* `synth_check.txt` - Yosys综合的检查报告, 用户需仔细阅读并决定是否需要排除相应警告
* `yosys.log` - Yosys综合的完整日志
* `gcd.rpt` - iSTA的时序分析报告, 包含WNS, TNS和时序路径
* `gcd.cap` - iSTA的电容违例报告
* `gcd.fanout` - iSTA的扇出违例报告
* `gcd.trans` - iSTA的转换时间违例报告
* `gcd_hold.skew` - iSTA的hold模式下时钟偏斜报告
* `gcd_setup.skew` - iSTA的setup模式下时钟偏斜报告