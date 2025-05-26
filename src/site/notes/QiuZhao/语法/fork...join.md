---
{"tags":["IC八股"],"dg-publish":true,"permalink":"/QiuZhao/语法/fork...join/","dgPassFrontmatter":true}
---

## `fork ... join` 家族
fork     
	// 线程1     
	// 线程2    
	... 
join          // 等所有线程都执行完，才往下执行（阻塞）
join_any     // 任意一个线程执行完就往下走（阻塞） 
join_none     // 立刻往下执行，不等任何线程（非阻塞）`