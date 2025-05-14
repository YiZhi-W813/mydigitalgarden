---
{"tags":["IC八股"],"dg-publish":true,"permalink":"/QiuZhao/Bagu/APB/","dgPassFrontmatter":true}
---

## APB 总线概述
**APB（Advanced Peripheral Bus）** 是 ARM 公司提出的 AMBA 总线协议中的一个组成部分，主要用于连接**低带宽、低功耗、外围设备**（如 GPIO、UART、SPI、定时器等）。
- APB 总线不用于主系统内高速通信，而是为 **控制寄存器访问**等简洁场景设计。
- APB 为 **片上总线（SoC）设计提供了高效的从设备接口方式**。

## APB 接口信号

|信号|方向|说明|
|---|---|---|
|`PCLK`|输入|时钟信号|
|`PRESETn`|输入|复位信号，低电平有效|
|`PADDR`|输入|地址总线|
|`PWDATA`|输入|写数据|
|`PWRITE`|输入|写使能，高电平表示写操作|
|`PSEL`|输入|从设备选择信号（一个外设对应一个 PSEL）|
|`PENABLE`|输入|表示数据传输阶段|
|`PREADY`|输出|表示从设备准备就绪（可选）|
|`PRDATA`|输出|读数据|
|`PSLVERR`|输出|错误响应（可选）|
## APB 传输协议流程

APB 的传输过程分为两个阶段：
1. **设置阶段（Setup phase）**
2. **访问阶段（Access phase）**

写时序：
	Cycle 1:
	- PSEL   = 1     // 从设备选择
	- PENABLE= 0     // 还没开始传输
	- PWRITE = 1     // 写操作
	- PADDR、PWDATA 设置好
	Cycle 2:
	- PENABLE = 1    // 开始传输
	- 写数据在 PWDATA 上，从设备根据地址写入
	Cycle 3:
	- PSEL/PENABLE = 0  // 停止访问，准备下一个传输

读时序：
	Cycle 1:
	- PSEL = 1，PWRITE = 0
	- PADDR = 读地址
	Cycle 2:
	- PENABLE = 1
	- 从设备将数据放到 PRDATA 总线上
	Cycle 3:
	- PSEL = 0，访问结束
