## Overview

[参考尚硅谷Hadoop教程](https://www.bilibili.com/video/BV1Qp4y1n7EN)

[hadoop docs](https://hadoop.apache.org/docs/)

**大数据：**

> **大数据（big data）**，是指无法在一定时间范围内用常规软件工具进行**采集**、**存储**、**分析处理**的数据集合，是需要新处理模式才能具有更强的决策力、洞察发现力和流程优化能力的**海量、高增长率和多样化**的信息资产。
>
> 大数据的 5V 特点：Volume（大量）、Velocity（高速）、Variety（多样）、Value（低价值密度）、Veracity（真实性）。
>

**大数据部门组织结构：**

<img src="D:/OneDrive/_mine/docsify/_img/image-20210511173318523.png" alt="大数据部门组织架构" style="zoom:80%;" />

---

## 概述

> Hadoop 是 Apache 开发的用于解决海量数据**存储**与分析**计算**问题的**分布式系统基础架构**。
>
> 特点：高可靠、高扩展、高效、高容错。

---

### Hadoop 组成

 3.x 在组成上没有变化。2.x 加入了 YARN 将计算和调度分离开来。

<img src="D:/OneDrive/_mine/docsify/_img/image-20210511173900249.png" alt="1.x、2.x 区别" style="zoom: 67%;" />

#### HDFS

> Hadoop Distribute File System，一个 HDFS 集群由一个 NameNode（管理文件系统所有元数据），多个 DataNode（管理数据的存储）组成。

HDFS 中，一个文件被分为许多存在一系列 DataNode 上的块（block）。NameNode 负责管理 block 到 DataNode 的映射，同时负责下达例如打开、关闭、重命名等文件元数据（Metadata）相关的操作指令。DataNode 根据来自 NameNode 或 Client 的指令，负责 block 的创建、销毁、复制等操作。

<img src="D:/OneDrive/_mine/docsify/_img/hdfsarchitecture.png" alt="HDFS Architecture" style="zoom:67%;" />

---

#### MapReduce

> Hadoop MapReduce 是一个软件框架，用于编写可靠的并行处理数以千计的节点上海量数据的程序。

**一个 MapReduce job 分为两步：**

- map task：将输入的数据集划分为多个独立的 chunk。

- reduce tasks：map 的结果经排序处理，作为 reduce 的输入，进行计算。

这些任务产生的输入和输出都会被存在文件系统中（即**基于磁盘计算**），框架管理着这些任务的执行顺序，并确保执行失败后重新执行。

通常情况下，计算节点和存储节点是对应的，在哪里存就在哪里算，这样框架可以有效地调度任务，节省了集群的带宽。

---

#### YARN

> Yet Another Resource Negotiator，出现的目的是将资源管理和任务调度分开。

![MapReduce NextGen Architecture](D:/OneDrive/_mine/docsify/_img/yarn_architecture.gif)

**组成：**

- ResourceManager（RM）：大管家，拥有绝对的资源（CPU、memory、disk、network）分配权限，包含 Scheduler 和 ApplicationManager 两个模块。
  - Scheduler：负责根据容量、队列等约束条件，分配资源给正在运行的程序。它根据程序对资源的申请进行调度，只是一个单纯的调度程序，不具备监管和追踪的功能，无法对失败的任务进行重做。调度器采用插件式策略，如可选择CapacityScheduler、 FairScheduler等。
  - ApplicationManager （AM）：负责接收客户端提交的作业，与特定的 ApplicationMaster 协商分配第一个容器，并负责失败重做。
- NodeManager（NM）：管理每个节点 Container 的资源分配，向 RM 汇报资源使用状态。
  - Container：封装执行任务所需资源的抽象容器。
- ApplicationMaster：与 RM 进行协商请求合适的资源，并负责执行、监管任务。



---

### Hadoop 生态

![HadoopEcosystem](D:/OneDrive/_mine/docsify/_img/image-20210512171150173.png)

----

## 环境搭建







----

## 集群配置

