---
{"dg-publish":true,"permalink":"/knowledge point/SoC Design/Bus/AHB(Advanced High-performance Bus)/","dgPassFrontmatter":true}
---

### 特点
高速总线、高性能
流水线操作
可支持多个总线主设备（最多16个） 
支持burst传输
总线带宽：8、16、32、64、128bits
上升沿触发操作
对于一个新设计建议使用AHB

### 信号
HCLK - 总线时钟
HRESETn - 总线复位，低电平有效
HADDR[31:0] - 32位系统地址总线
HWDATA[31:0] - 写数据总线，从主设备写到从设备
HRDATA[31:0] - 读数据总线，从从设备读到主设备
HTRANS[1:0] - 指出当前传输的类型(NONSEQ、SEQ、IDLE、BUSY)
HSIZE[2:0] - 指出当前传输的大小 • HBURST[2:0] - 指出传输的burst类型
HRESP[1:0] - 从设备发给主设备的总线传输状态(OKAY、ERROR、RETRY、SPLIT)
HREADY - 高电平：从设备指出传输结束 - 低电平：从设备需延长传输周期