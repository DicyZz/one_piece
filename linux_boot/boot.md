![image-20201215171930922](/home/jian/Work/tools/linux_boot/boot/image-20201215171930922.png)

![image-20201215171957244](/home/jian/Work/tools/linux_boot/boot/image-20201215171957244.png)



![image-20201215172148939](/home/jian/Work/tools/linux_boot/boot/image-20201215172148939.png)



![image-20201215182922068](/home/jian/Work/tools/linux_boot/boot/image-20201215182922068.png)


* 在多核系统中，使非原磁芯到睡眠见引导SMP系统上 第18-17页。
* 初始化异常向量。
* 初始化内存系统，包括MMU。
* 初始化核心模式堆栈和寄存器。
* 初始化所有关键的I / O设备。
* 执行NEON或VFP的任何必要的初始化。
* 启用中断。
* 更改核心模式或状态。
* 处理安全世界所需的任何设置（请参阅第21章））。 *
* 调用main（）应用程序。

QUESTION
mcr p15, 0, r0, c7, c10, 4 @ DSB，数据同步屏障，内存和缓存，tlb等维护操作完成才进行后面指令的操作
mcr p15, 0, r0, c7, c5, 4 @ ISB，指令同步屏障，清空流水线










