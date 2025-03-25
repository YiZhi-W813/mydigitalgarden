---
{"dg-publish":true,"tags":["DigitalCircuit","SoC"],"permalink":"/Knowledge point/SoC Design/Bus/AXI vs. AHB/","dgPassFrontmatter":true}
---

### [AXI Vs AHB. Difference Between AXI and AHB (vlsiip.com)](http://www.vlsiip.com/amba/axi_vs_ahb.html)
AHB 可能被使用在一个完全同步的系统中，例如用于小型 SoC，即物联网 SoC、音频 SoC。AHB 是通常用于没有高通量要求的系统， 或者在运行频率相对较低，例如<150 MHz。  
但是，如果 SoC 很大，具有多个 clock domains，或者如果子系统很大，并且具有多个 clock domains，并且需要吞吐量和带宽较高，时钟频率更高的频率，例如 200+MHz，那么 AXI 是正确的选择。