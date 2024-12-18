---
title: 2024秋冬季开源操作系统训练营第二阶段总结-hbuxiaofei
date: 2024-11-07 22:19:43
tags:
---

第二阶段开始从零开始构建操作系统的各个模块，不断完善操作系统核心功能，这一阶段，我首先从裸机开始实现了一个简化的内核启动程序，学习到了从了裸机开发OS的基本思路，后面随着每章的习题练习，不断学习了进程管理、内存管理、文件系统、并发等操作系统必备的各个子系统的基本原理。

第一个实验是实现`sys_task_info`系统调用，这个系统调用用于获取任务的一些基本信息，包含任务状态、使用的系统调用次数以及任务运行的时间，通过这个系统调用的实现我学习到了任务的基本实现原理及任务调度的基本原理。

第二个实验是实现 mmap 和 munmap 两个系统调用，首先将传入的参数转换成VirtAddr结构, mmap中， 获取当前进程task， 通过调用 task 中 memory_set.insert_framed_area 进行映射并插入到 memory_set 中； munmap中， 同样 获取当前进程的task， 然后获取task的memory_set检查内存区域是否合法， 最后在调用 memory_set.umap接触映射。这个实现我学习到了内存管理实现的基本原理。

第三个实验是实现 spawn 系统调用和stride 调度算法。spawn 实现了fork+exec，根据进程名解析elf，使用TaskControlBlock::new创建进程，加入到任务队列中，即完成了进程的创建及执行。stride 实现了任务根据优先级进行简单调度算法，通过优先级计算pass，P.pass = BigStride / P.priority  进程每次调度进行累加，每次调度根据stride，找到最小的进行调度。通过这个实验也算是自己实现了一个任务调度算法，感觉还是比较神奇的。

第四个实验是实现了三个系统调用 sys_linkat、sys_unlinkat、sys_stat，这三个系统调用都是和文件相关的，我实现的比较粗暴，link实现根据源文件，创建DirEntry跟原来的文件名不同，其他相同，unlink根据文件名删除Direntry， stat实现为每一个File示例实现stat函数，或许在细致实现一下可以增加一些字段来进行必要的记录，以避免每次都遍历root node。这个实验算是文件系统的小时牛刀，对于理解文件系统的原理十分有帮助。

第五个实验实现了是否开启死锁检查系统调用enable_deadlock_detect及死锁检查的算法。enable_deadlock_detect实现是为每个进程增加一个标记enable_dld来控制是否开启死锁检测，死锁检测算法是按照题目中给出的算法描述严格实现的，其实实际上可以简化很多，例如如果将锁比作临界资源，一个进程一次获取锁只相当于获取了数量为1的资源，是不可能同时占有多于1个 临界资源的。

在逐步构建和完善操作系统的过程中，我深入理解操作系统的内部机制，并逐步实现从基础功能到复杂功能的过渡。






