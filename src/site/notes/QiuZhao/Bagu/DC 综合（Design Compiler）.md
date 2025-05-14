---
{"tags":["IC八股"],"dg-publish":true,"permalink":"/QiuZhao/Bagu/DC 综合（Design Compiler）/","dgPassFrontmatter":true}
---

## 一、什么是 DC 综合？
**Design Compiler（DC）** 是 Synopsys 提供的主流逻辑综合工具，功能是将 RTL（Verilog/VHDL）代码转换为门级网表（gate-level netlist），满足时序、面积、功耗等约束。

## 二、DC 综合流程简图
1. **读取 RTL 设计（.v/.sv）**
2. **读取库文件（Library）**
3. **读取时序约束（SDC）**
4. **综合 RTL 到门级网表（compile_ultra）**
5. **检查设计质量（面积、时序、功耗等）**
6. **输出综合结果（write -format verilog）**

## 三、DC 涉及的主要 Library 类型 ✅

|库名|作用|后缀|
|---|---|---|
|**Technology Library**|提供标准单元延时、电压、温度等信息|`.lib` / `.db`|
|**Data Library**|含标准单元的结构与属性描述（通常与 tech lib 合一）|`.lib` / `.db`|
|**Symbol Library**|图形展示用，非功能性|`.sdb`|
|**Milkyway Library**|布局库，供物理综合/布局布线使用|二进制格式|
