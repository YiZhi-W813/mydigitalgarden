---
{"dg-publish":true,"tags":["C","Bugs"],"permalink":"/knowledge point/C/Bugs/Segmentation fault (core dumped)/","dgPassFrontmatter":true}
---

调试C程序时经常容易遇到段错误，大多数情况下是因为使用了未初始化的指针/野指针，传错参数导致错误的指针解引用等等，所以大多数情况下问题都比较容易被定位。

但是这次遇到了奇怪的情况：
在nemu中函数“disassemble”很好地工作，但是在npc中调用同样的函数，传入同样类型的参数却触发了段错误。

首先看nemu调用该函数时（正确运行）
![Pasted image 20240515131319.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240515131319.png)
调用disassemble时传入的参数是(uint8_t \*)&iringbuf[i].inst，这个地址的具体值为0x55555557a6e4，对应参数“code”，但奇怪的是进入disassemble函数后，看到code的值为0x00007fffffffe11c，目前还没能理解为什么会有这样的变化，code指向的值即inst是正确的。

然后是npc调用该函数时
![Pasted image 20240515133619.png](/img/user/knowledge%20point/imgs/Pasted%20image%2020240515133619.png)
与nemu不同，npc中传入的iringbuf的地址和函数内code是一致的，相比之下我觉得npc的这种情况才是正常的，可事实上npc会在这里遇到段错误。

有关Segmentation fault问题的快速定位方法，参见[[knowledge point/C/Bugs/Segmentation fault (core dumped)\|Segmentation fault (core dumped)]]。

### 结论
de了两天bug终于破案了，这套反汇编库函数中有一个init函数，在nemu中这个init函数会在monitor初始化时被调用一次，所以nemu功能正常。但npc中并没有进行过init，因此导致该问题。前面的判断并没有错，传入disassemble函数的参数全都是正确的，问题不是由该函数导致，只是由于没有init而在该函数处触发。

### 有关该问题的其它记录