---
{"dg-publish":true,"permalink":"/knowledge point/SoC Design/Bus/APB(Advanced Peripheral Bus)/","dgPassFrontmatter":true}
---

### 特点
低速总线、低功耗
接口简单
在Bridge中锁存地址信号和控制信号
适用于多种外设
上升沿触发

### 信号
PCLK
PRESETn
PADDR[31:0]
PSELx
PENABLE
PWRITE
PRDATA -最多32bit宽
PWDATA -最多32bit宽