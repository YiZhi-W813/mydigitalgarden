---
{"dg-publish":true,"tags":["Linux","Bugs"],"permalink":"/knowledge point/Linux/Bugs/Ubuntu22.04频繁掉无线网卡驱动/","dgPassFrontmatter":true}
---

系统环境：Win11+Ubuntu22.04双系统
网卡型号：Intel AX200
问题描述：系统启动后没有wifi标志，进入wifi设置界面显示“no wifi adapter found”，系统启动log中打印了比往常更多的信息，系统关闭时提示”a stop job is running for snap deamon“并因此耗费了更多时间，lspci显示了正确的网卡信息，ifconfig没能打印正确的网络信息，驱动文件正常。
解决方案：禁用Windows 11的快速启动功能，没能实锤具体是什么原因导致了这个问题，但根据Web上的资料大致可以确定快速启动会导致Win+Ubuntu双系统产生诸多问题。