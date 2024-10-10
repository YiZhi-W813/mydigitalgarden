---
{"dg-publish":true,"tags":["DigitalCircuit","risc-v","SoC"],"permalink":"/knowledge point/SoC Design/Spatz_Snitch工程信号解析/","dgPassFrontmatter":true}
---

### 信号类型
##### data_t
一个数据位宽的信号，例如一个DataWidth位宽的信号。
##### addr_t
一个地址位宽的信号。
##### acc_addr_e
虽然有[31:0]的位宽，但实际上用到的就最低为，分别选择0：Spatz或1：DMA_SS。
##### acc_rsp_t
包含一个[5:0]位宽的id，一个bit位宽的error，一个data_t型的data。
##### acc_issue_req_t
包含一个acc_addr_e型的addr，一个[5:0]位宽的id，一个[31:0]位宽的data_op，两个data_t型的data_arga和data_argb，一个addr_t类型的data_argc
##### acc_issue_rsp_t
包含accept、writeback、loadstore、exceptio、isfloat共五个1bit的信号
##### amo_op_e
一个4bit宽的enum类型的信号，标志了是哪种原子操作。
##### spatz_mem_req_t 
在SpatzCC中被赋值为了tcdm_req_chan_t，包含addr_t类型的addr，1bit宽的write，amo_op_e类型的amo，data_t类型的data，strb_t类型的strb和user_t类型的user。
##### spatz_mem_rsp_t
在SpatzCC中被赋值为了tcdm_rsp_chan_t，只有一个data_t类型的data

##### tcdm_req_t
在tcdm_interface的typedef中被赋值为了tcdm_req_chan_t
##### dreq_t
在Spatz_cluster中被赋值为了reqrsp_req_t，包含一个req_chan_t类型的q，一个1bit宽的q_valid以及一个1bit宽的p_ready，其中reqrsp_req_t与tcdm_req_chan_t略有不同，前者没有user_t类型的user，替换为了size_t类型的size，用来访问DTCM
##### drsp_t
在Spatz_cluster中被赋值为了reqrsp_rsp_t，包含一个rsp_chan_t类型的p，一个1bit宽的p_valid以及一个1bit宽的q_ready，其中reqrsp_rsp_t与tcdm_rsp_chan_t略有不同，前者比后者多了一个1bit宽的error信号，用来访问DTCM
### Snitch
##### acc_qreq_o 
提供给加速器的请求信号包，acc_issue_req_t型，
##### acc_qrsp_i
加速器反馈的信号包，acc_issue_rsp_t型，用在control模块中判断stall、访存计数等
##### acc_qvalid_o
提供给加速器的1bit的valid握手信号
##### acc_qready_i
来自加速器的1bit的ready握手信号
##### acc_prsp_i
来自加速器的反馈信号包，acc_rsp_t型，在译码中使用，译出写回的寄存器id、data。
##### acc_pvalid_i
用来退休指令
##### acc_pready_o
acc_pvalid_i拉高时该信号就会紧跟着拉高（组合逻辑）
##### acc_mem_finished_i
2bit位宽信号，指示加速器完成了一次内存操作
##### acc_mem_str_finished_i
2bit位宽信号，指示加速器完成了一次内存操作，且是store操作
### Spatz
##### issue_valid_i
来自主核的1bit的valid握手信号
##### issue_ready_o
输出给主核的1bit的ready握手信号
##### issue_req_i
来自主核的请求信号包，spatz_issue_req_t型
##### issue_rsp_o
反馈给主核的信号包，spatz_issue_rsp_t型
##### rsp_valid_o
输出给主核的1bit的valid握手信号
##### rsp_ready_i
来自主核的1bit的ready握手信号
##### rsp_o      
回馈给主核的信号包，spatz_rsp_t型，实际上被赋值为了acc_rsp_t型
##### spatz_mem_req_o
对TCDM的访存请求信号包，spatz_mem_req_t类型。
##### spatz_mem_req_valid_o
对TCDM的访存的1bit宽valid握手信号
##### spatz_mem_req_ready_i
TCDM反馈的1bit宽ready握手信号
##### spatz_mem_rsp_i
TCDM反馈的信号包，但实际上只有NrMemPorts * DataWidth宽的data。
##### spatz_mem_rsp_valid_i
TCDM反馈的1bit宽数据有效握手信号
### Interface
##### rearsp interface
snitch load/store时会用到该接口，包含请求通道q（request）和反馈通道p（respond）

The `reqrsp_interface` (request and response) is a custom interface based on common principles found in other interconnects such as AXI or TileLink. It has only two channels (request and response) which are handshaked according to the AMBA standard:

- The initiator asserts `valid`. The assertion of `valid` must not depend on `ready`.
- Once `valid` has been asserted all data must remain stable.
- The receiver asserts `ready` whenever it is ready to receive the transaction.
- When both `valid` and `ready` are high the transaction is successful.

### 一条向量指令如何被执行
译码模块首先译出这是一条需要被卸载到spatz的指令，将给acc_qreq_o赋值：.addr = SPATZ; .id = rd; .data_op = inst_data_i; .data_arga = {{32{opa[31]}}, opa}; .data_argb = {{32{opb[31]}}, opb}; .data_argc = ls_paddr; 还要判断acc_qvalid_o是否为1，valid_instr必须为1，此外对于浮点ls指令还需额外判断trans_ready，对于vectorls指令还需要判断!acc_mem_stall。

如果握手成功，acc_qreq_o就会被协处理器的接收，协处理器的经过译码后，会返还一个acc_qrsp_i信号包，其中的信号指示了该向量指令是否属于浮点指令、ls指令等，主核需要根据这些信息判断是否需要stall。同时sptz也会开始处理这条向量指令，如果LSU需要访问TCDM，Spatz会通过一组专用的TCDM接口进行访问，这组接口是用在多个bank的组织形式的内存的，除了Spatz的LSU所需的n个TCDM接口外，还有一个额外的供FPU Sequencer取数用的访存接口，该接口通过一个mux与snitch复用访存的数据通路。

向量指令退休时，首先协处理器会发一个握手信号acc_pvalid_i，并发一个acc_prsp_i数据包，数据包中的.id即要写回的寄存器地址，.data即要写回的数据。如果能够退休该指令，处理器需要将握手信号acc_pready_o置1。退休时计分板也要相应调整。