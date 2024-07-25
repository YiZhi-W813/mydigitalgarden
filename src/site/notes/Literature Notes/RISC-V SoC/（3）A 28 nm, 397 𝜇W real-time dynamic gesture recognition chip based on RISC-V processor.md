---
{"dg-publish":true,"tags":["risc-v","文献阅读","SoC"],"permalink":"/Literature Notes/RISC-V SoC/（3）A 28 nm, 397 𝜇W real-time dynamic gesture recognition chip based on RISC-V processor/","dgPassFrontmatter":true}
---

作者： Jun Han @ 复旦大学
发表在：Microelectronics Journal_2021（中科院三区）

### 简介
一款基于RISC-V处理器的手势识别SoC，仅通过图像处理算法来识别手移动的方向。
性能：输入320\*240的图像，30帧/s，在与目标相距10cm~90cm & 目标速度在10 cm/s∼150 cm/s的情况下达到93%∼99%的精度。

### 动态手势识别（dynamic gesture recognition DGR）算法
提出了一种对环境的识别能力、灵活性和适用性好且复杂度较低的算法：
![QQ_1721633317734.png](/img/user/Literature%20Notes/imgs/QQ_1721633317734.png)
首先逐像素从RGB色域转移到YCrCb色域，对于手来说，像素会落在YCrCb色域中的以下范围内：
![QQ_1721633860910.png](/img/user/Literature%20Notes/imgs/QQ_1721633860910.png)
属于上述范围内的皮肤像素保留，并以此对整个画面做二值化处理。将皮肤像素的数量和预设的阈值相比较，即可判断画面中是否有人手。

提出了两种GR方法，来适应不用的应用场景，一种是inter-frame difference for gesture tracking and recognition (FDTR，帧间差分？)，另一种是finding contours for gesture tracking and recognition (FCTR，寻找轮廓)。
 FDTR适合在复杂背景下且画面中没有其它皮肤色物体的情况，而FCTR is suitable for one-handed movement with no or similar skin color
in the background（这是啥意思？）

FDTR对二值图像进行帧间差分(IFD)运算，得到包含手部运动区域的差分图像。其次，FDTR对差分图像进行中值模糊(MBLUR)运算，滤除光照不均匀引起的椒盐噪声。最后，由于差分图像只包含手的运动区域，质心(CTD)操作直接计算运动区域的质心，用于后续的跟踪和识别过程(TAR)。

FCTR首先对图像进行闭运算(**先膨胀，后腐蚀**)来得到完整的手部轮廓。其次，应用MBLUR，这与FDTR中的MBLUR相同。第三，查找轮廓操作(FINDC)，与OpenCV的FindContours函数相同，查找手的轮廓并获得手的边缘。CTD通过边缘计算手势的质心。FCTR操作只适用于单手或手与用户的脸。对于具有多个皮肤或类皮肤色调的背景，轮廓数超过2，FCTR给出一些反馈来调整背景。

总之两种方法都是要得到手部轮廓的质心，质心的运动轨迹即手的运动轨迹。

### SoC架构
![QQ_1721637099604.png](/img/user/Literature%20Notes/imgs/QQ_1721637099604.png)
RV处理器有4KB的L1I cache以及24KB的L1D cache.
预处理单元PREU接收到来自摄像机的图像时，对图像进行预处理并把处理过后的图像传输给唤醒单元wake-up unit，唤醒单元对预处理后的二值图像进行相加（也就是统计肤色像素的个数）并存入CNT寄存器中，当一个完整的图像输入完成后，与预设的TREG寄存器的值比较，比较的结果决定下一帧是否写入FIFO以及是否唤醒后续的识别进程。
DGR协处理器如图所示：
![QQ_1721640319021.png](/img/user/Literature%20Notes/imgs/QQ_1721640319021.png)
DGR协处理器通过RoCC接口与RV处理器相连
CREG用来存储参数，改变参数可以在不同距离、不同移动速度下提高DGR精度，即通过修改CREG中的参数来使系统适应不同场景。IREG寄存器用来控制协处理器的运行，来自下图所示的扩展指令
![QQ_1721640112180.png](/img/user/Literature%20Notes/imgs/QQ_1721640112180.png)
* RoCC_CONFIG：配置CREG寄存器
* RoCC_INITIAL：初始化协处理器和清楚AHD（accumulated historical displacement）
* RoCC_MBLUR：中值模糊运算（5 × 5的卷积核）
* RoCC_CLOSE：闭运算
* RoCC_FINDC：寻找图像轮廓，算法为八邻域算法（eight-neighborhood search）
* RoCC_CTD：计算质心
* RoCC_TAR：跟踪和识别
* RoCC_PRINT：打印FREG和DREG
DREG和FREG存储来自TAR的输出方向和反馈以供打印。
