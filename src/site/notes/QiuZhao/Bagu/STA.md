---
{"tags":["IC八股"],"dg-publish":true,"permalink":"/QiuZhao/Bagu/STA/","dgPassFrontmatter":true}
---

# STA（Static Timing Analysis，静态时序分析）
## 概念
**静态时序分析（Static Timing Analysis, STA）** 是在不依赖输入矢量的前提下，分析芯片电路在给定时钟频率下是否能够正确工作的分析方法。它主要通过分析电路中每一个路径的 **最大延迟（setup check）** 和 **最小延迟（hold check）** 来验证设计是否满足时序要求。

## 三大关键要素
### 1. P（Process）工艺角
表示制造工艺的波动，典型有：
- **TT（Typical-Typical）**：典型工艺角
- **FF（Fast-Fast）**：NMOS 和 PMOS 都变快
- **SS（Slow-Slow）**：NMOS 和 PMOS 都变慢
- 其他角如 SF、FS 也有针对混合快慢情况

### 2. V（Voltage）电压
芯片供电电压的波动，例如：
- Nominal：正常电压（如1.0V）
- High：电压上升
- Low：电压下降

### 3. T（Temperature）温度
高温通常会使晶体管变慢，例如：
- 0°C、25°C、125°C 等常用温度点
这三者构成了 STA 中的 **Operating Condition（工作条件）**，用于模拟最坏与最好情形。

## 核心概念

### 1. 时序路径类型
- **时钟路径**：传输时钟信号的路径
- **数据路径**：传输数据的路径（从一个触发器到另一个触发器）
- **组合逻辑路径**：由门电路构成的数据路径

### 2. 时序检查类型
- **Setup Time Check**（建立时间检查）：检查数据是否能在时钟上升沿到来前稳定。
- **Hold Time Check**（保持时间检查）：检查数据是否在时钟沿后维持稳定时间。

### 3. 延时计算
- **Cell Delay（单元延时）**：逻辑门内部延时，依赖 PVT 和负载
- **Net Delay（布线延时）**：信号在连线上传播的延时