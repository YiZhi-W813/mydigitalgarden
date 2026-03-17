---
{"dg-publish":true,"tags":["体系结构","risc-v","SoC"],"permalink":"/work diary/RISC-V/Sjoy超标量RISC-V处理器SoC/","dgPassFrontmatter":true}
---

# 架构图
![Pasted image 20250605200613.png](/img/user/Pasted%20image%2020250605200613.png)
# 总线
|    预定义地址空间    |  总线  |           地址范围            |   外设   |
| :-----------: | :--: | :-----------------------: | :----: |
|     SRAM      | AXI4 | 0x8000 0000 - 0x8001 FFFF | I-SRAM |
|     SRAM      | AXI4 | 0x8002 0000 - 0x8002 FFFF | D-SRAM |
| APB Subsystem | AXI4 | 0x4000 0000 - 0x4000_0FFF | uart0  |
| APB Subsystem | AXI4 | 0x4000 1000 - 0x4000_1FFF |  gpio  |
| APB Subsystem | AXI4 | 0x4000 2000 - 0x4000_2FFF | timer  |
|    **总计**     |      |            8h             |        |
