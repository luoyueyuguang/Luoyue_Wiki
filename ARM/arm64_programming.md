*reference:《ARM64体系结构编程与实践》奔跑吧Linux社区*
*reference: https://github.com/runninglinuxkernel/arm64_programming_practice*
***
*一些概念*
- 处理机(Processing Element,PE):在 ARM 公司的官方技术手册中提到的一个概念,
把处理器处理事务的过程抽象为处理机。
- 执行状态(execution state):处理器运行时的环境,包括寄存器的位宽、支持的指
令集、异常模型、内存管理以及编程模型等。ARMv8 体系结构定义了两个执行
状态。
-  AArch64:64 位的执行状态。
	- 提供 31 个 64 位的通用寄存器。
	- 提供 64 位的程序计数(Program Counter,PC)指针寄存器、栈指针(Stack
		Pointer,SP)寄存器以及异常链接寄存器(Exception Link Register,ELR)。
		提供 A64 指令集。
	- 定义 ARMv8 异常模型,支持 4 个异常等级,即 EL0~EL3。
	- 提供 64 位的内存模型。
	-   定义一组处理器状态(PSTATE)用来保存 PE 的状态。
 - AArch32:32 位的执行状态。
	- 提供 13 个 32 位的通用寄存器,再加上 PC 指针寄存器、SP 寄存器、链接寄
		存器(Link Register,LR).
	- 支持两套指令集,分别是 A32 和 T32(Thumb 指令集)指令集。
    - 支持 ARMv7-A 异常模型,基于 PE 模式并映射到 ARMv8 的异常模型中。
	-  定义一组 PSTATE 用来保存 PE 的状态。
- ARMv8 指令集:ARMv8 体系结构根据不同的执行状态提供不同指令集的支持。
	- A64 指令集:运行在 AArch64 状态下,提供 64 位指令集支持。
	- A32 指令集:运行在 AArch32 状态下,提供 32 位指令集支持。
	- T32 指令集:运行在 AArch32 状态下,提供 16 位和 32 位指令集支持。
- 系统寄存器命名:在 AArch64 状态下,很多系统寄存器会根据不同的异常等级提供不
同的变种寄存器。系统寄存器的使用方法如下。
    ```<register_name>_Elx //最后一个字母 x 可以表示 0、1、2、3```
***
特权等级
- EL0:用户特权,用于运行普通用户程序
- EL1:系统特权,通常用于操作系统内核。如果系统使能了虚拟化扩展,运行虚拟机
操作系统内核
- EL2:运行虚拟化扩展的虚拟机监控器(hypervisor)
- EL3:运行安全世界中的安全监控器(secure monitor)
***
