# Towards a fully disaggregated and programmable data center
  通往一种完全分解和可编程的数据中心

## Abstract
   摘要
   
2 trends in data centers:
1)application more fine-grained
2)data-center hardware heterogeneous and customized to diffierent computing needs
Because of the 2 trends and better managability, Serval major data center is becoming disaggregated architecture where hardware resources are organized as individual and network-conncted resources pool.
But, today data centers still server-centric, rely on traditional CPU-based servers.

数据中心的2个趋势：
1）应用程序更细化
2）数据中心硬件异构，并且为不同的计算定制。
由于这两个趋势和便于管理，一些主要数据中心正在转向硬件资源被组织成独立的、网络连接的资源池的分散架构
但是，目前数据中心仍然服务器中心化，依赖传统的基于CPU的服务器。

Our work: further explore probability of building a fully disaggregated data center.

本文工作：进一步探索构建一个完全分解的数据中心的可能性。

Feature: Each type of resource disaggregatability.

特征：每一种类的资源可分解

Other work :Discuss the requirements and implements in making each disaggregated devices programmable.
其他工作：探讨使每个分解设备可编程的要求和影响，并问数据中心设计者提供了指导原则和初步解决方案。

Specifical achievement: Decompose 4 main problems to 4 sub-problems and provide each solutions.
具体实现：将主要问题分解为四个子问题并提出各自的解决方案。

Top layer: explore 2 types of abstractions, propose a disaggregate-native design method.
顶层：探索两种类型的抽象，提出了一种以分解为本的设计方法。

Bottom layer: describe the hardware and key feature in building disaggregated devices and network infrastructure to connect them.
底层：描述了构建分解设备所需的硬件和关键特性，以及连接这些设备的网络基础设施。

To connect top and bottom, propose a static-time unit, which through a disaggregate-native intermediate representation to compile different user application to heterogenous disaggregated devices.
为连接底层和顶层，提出了一个静态时间组件，通过一种以分解为本的中间表示将不同用户程序编译到异构分解设备中。

Propose a runtime system to manage hardware resources and schedule compiler generated execution units.
提出了一种运行时系统，用于管理硬件资源和调度编译器生成的执行单元

Works:
工作：
1.explore the possibility of building a fully disaggregated data center
  探索构建完全分解的数据中心的可能性
2.explore the requirements and implications of making disaggregated data centers programmable
  探索使分解的数据中心可编程性的需求和内涵
Present guidelines and initial solutions, to navigate design trade-offs
提出纲要和初始解决解，引导设计权衡
Decompose problem to 4 sub-p.
分解问题为四个子问题
Top layer: 2 abstractions, propose a disaggregation-native design methodology
顶层：两个抽象，提出一个自然分解设计技术
Bottom layer: describe hardware and key features, to build disaggregated devices and networking infrastructure to connect
底层：描述硬件和关键特征，来构建分解设备和网络基础设施来连接
To bridge the 2: static-time component and run-time system.
为了连接两层：静态时间构成和运行时系统


## 1 Introduction

Drastic development of hardware and software in data center:1. application becomes fine-grained: Driven by microservices software model and the cloud serverless computing paradigm, easier to scale and utilize hardware resources in data center efficently.

数据中心的软件和硬件大发展：1.应用程序细粒化：由微服务软件模型和云无服务计算范式驱动，更容易扩展和高效应用数据中心硬件资源。


2.Data center hardware infrastructure become more heterogeneous and progeammable
Cuz CPU meet its limitation, acceletators be deployed widely in major data centers.
Today's accelerators fully or partly programmable
Network resources: heterogeneous and programmable

2.数据中心的硬件基础设施变得更加异构化和可编程化
由于CPU受到局限，加速器正在主要数据中心大规模部署。如今的许多加速器时完全可编程或者部分可编程。除了计算资源外，网络资源也变得更加异构化和可编程。


However, data center architecture keep remained: severs connect by data-center-wide network. But the fine-grained and variety makes data center based on servers difficult to deploy and manage. Therefore, proposing a new data center architecture named resource disaggregation.
但是，数据中心的架构在过去几十年内保持不变：即服务器通过数据中心范围的网络连接。然而，当今软硬件的细粒度和多样性使得在基于服务器的数据中心中部署和管理困难。因此，移除了一种称为资源分解[1]的新数据中心架构。

4.Three questions in today's disaggregation solutions:
1)What is a generic disaggregation design which fit heterogeneous types resources
2)How to efficiently connect disaggregated devices?
3)How to best map apps to a disaggregated hardware platform?

尽管分解方案取得了成功，但是仍然有三个开放性问题：1.如何分解加速器、网络设备等其他类型的资源？2.如何有效连接分解的设备？ 3，如何最佳地将应用程序映射到分解的硬件平台？


If above problem be solved, evolve DC into fully disaggregated and programmable.
This paper devide general goal to 4 sub prob(in figure 1)

如果这些挑战得到解决，我们可以将数据中心发展为完全分解和可编程的。本文提供了一些指导原则，提出了建立这样一种数据中心的潜在方案，并将总体目标分解为四个子问题，如图1所示。


1)we need to decide how users can program and run apps
 on a FDP-DC
 2types abstraction, FDP-DC can choose 1-2
 1)backward-compatible, assume run on a VM or serverless model
 2)expose the underlying disaggregated and programmable nature to apps

第一个子问题：FDP-DC的抽象，即决定用户如何在FDP-DC上编程和运行他们的应用程序。设想了两种类型的抽象，可以选择其中1-2种。第一种：向下兼容的，分解或可编程硬件无意识的虚拟机或无服务器模型。第二种：一定程度上向应用程序暴露了FDP-DC的底层分解和可编程特性。


2nd problem:how to map apps to FDP to the FDP-DC hardware and sys infrasturcture
Compare to traditional server setting:
Similarity: FDP-DC needs compiler
Difference: apps and hardware heterogeneous in FDP-DC
New propose to easily manage heterogeneity: Intermediate Representation(IR), which's center around disaggregared execution units'(that schedule and execution) or codelets' concept
In each codelet, encapsulate various execution features or hints.
Use MLIR[Q2] to decompose program to smaller codelets.
To dictate the execution order of codelets: use a companion DAG
Compiler further add features to DAG edges.

第二个问题：如何将应用程序映射到FDP-DC的硬件和系统基础设施上。为了轻松管理FDP中应用程序和硬件的异构性，提出使用中间表示（Intermidate representaion, IR）作为中间层。IR是调度和执行的单元。除了代码和数据，我们在每个代码块中封装了各种执行特性和提示。最后，提出使用MLIR[39]将程序分解为多个较小的代码块和一个伴随的DAG，指示代码块的执行顺序。

The 3rd sub-problem: Develop infrastructure in FDP-DC. Proposing guideline and 3 key features: network connect, hardware abstraction, multi-user isolation/security. Devices also have ability to provide programmable and configurable.
第三个问题：在FDP-DC中构建硬件基础设施。提供了指导方针，确定了三个关键特性：网络连接性、硬件虚拟化、多用户隔离/安全性。设备也可以提供一定的可编程性或可配置性。
We propose a network design to connect disaggregated devices: imaging a reconfigurable network topology architecture, using circuits switch and/or packet switch with cut-through forwarding. Above that, we propose a programmable network infrastructure that be composed by 
我们提出了一个用于连接分解设备的网络设计：设想了一个可重配置的网络拓扑结构，借助电路交换和/或具有直通转发功能的分组交换来实现。在这之上，提出了一种由可编程交换机和多宿主SmartNICs组成的可编程网络基础设施，应用程序代码块和提供者管理任务可以卸载到这些设备上。我们进一步将这些多宿主smartNICs集中到一个池中，有效地分解了网络功能。


在应用程序、编译器和硬件基础设施到位的情况下，最后缺失的部分是FDP-DC操作系统。FDP-DC操作系统将监督所有资源池和网络系统，并将代码块通过DAG映射（调度）到底层基础设施。在调度时，它将使用编译器提供的提示。

 To illstrate how 4 componets work, now discuss a data processing system server2server development and execution's process

为了说明上述四个组件如何协同工作，我们简要讨论一个数据处理系统的端到端开发和执行流程。首先，系统开发者可以为每个操作符天价注释，指明它需要什么资源，是否打算在加速器上运行等。而且，他们还会将处理流程捕获为操作符的DAG。然后，我们的编译器将基于操作符的DAG生成代码块和它的DAG。在这个过程中，我们的编译器会决定代码块的范围并为异构设备编译每个代码块为多个二进制文件。在运行时，我们的操作系统将根据资源可用性调度代码块DAG，并动态选择硬件设备来运行代码块。操作系统还将使用DAG边信息来决定工作负载的网络拓扑和网络策略。根据网络拓扑，操作系统可能决定在可编程网络设备上执行某些代码块。最后网络设备将在一个虚拟化和隔离的环境中执行每个代码块。


Potential route: to provide data center designers to conctruct a fully disaggreated and programmable data center.

本文概述了数据中心设计者构建要给完全分解和可编程数据中心的一条可能路径。


## 2 Background and Related Works

In programmable/reconfigurable networks, resource disaggregation and programming models.

### 2.1 Programmable and Reconfigurable Network

## 3 FDP-DC Design

All devices & networking hardware programmable and disaggregated from each other.
Network topology can be reconfigured to best match

FDP-DC: 4 components
Then, discuss in details.


在FDP-DC中，所有设备和网络硬件都是可编程和互相分离的。此外，网络拓扑可以被动态重置来匹配工作负载的需求。

图1：四个组成部分的FDP-DC方案。

抽象和编译器组成：面向用户，增强FDP-DC的可用性。

操作系统和硬件基础设施：FDP-DC的组成模块，和执行编译器输出的环境。

### 3.1 Abstractions and Usage Models

2 abstraction:

Propperties : 
·all devices and network hardware is programmable and disaggrated each other
·Network topology can be dynamically reconfigured to match the workload's requirements

两种类型的抽象：向后兼容的透明抽象（支持传统的服务器程序和无服务器程序）、以分解为本的抽象。

透明抽象：将底层FDP-DC硬件虚拟化为虚拟机的抽象。[55]证明了提供与Linux兼容接口的可行性。

除了基于服务器的接口外，大量应用程序部署在无服务器计算框架上。FDP-DC可以将一个无服务器函数映射到一个设备上，并使用无服务器函数的DAG作为FDP-DC编译器的输入。

以分解为本的抽象：核心思想是暴露异构设备的本质，并与之共同设计软件。开发人员可以使用新接口或对现有程序进行注释，以提供指示，或指定他们的程序如何在FDP-DC上运行。
好处：用户更自由、更紧密地管理执行程序的方式。
三种明确暴露FDP-DC基础设施信息的潜在方法：

1.可以注释他们的数据和代码，来控制硬件的选择、放置、共位和故障处理。

2.可以向开发者或应用系统管理员公开设备或网络故障域。然后可以将可能在同一故障域一起失败的函数或数据结构打包，并为每个域指定不同的故障处理机制。

3.程序员将任务流指定为一个DAG来表示他们的应用程序。也可以指定多个可行的DAG，供编译器和运行时系统根据FDP-DC中可用的网络拓扑和资源选择最佳方案。

Figure 1: FDP-DC 4 componements solutions
·Abstraction && complier components : users facing and aims to enhance useability of FDP-DC
·OS && hardware infrastructure : building blocks of FDP-DC and environment executes complier output.

More Details:



### 3.2 Intermediate Representation

我们提供一种以分解为本的中间表示，以支持异构硬件和应用程序。并且通过编译器来操作不同表示之间的映射。IR中调度和执行的最小单位是代码块(代码+数据）。程序中的代码块被组织成一个DAG。

在此基础上，进一步提出了基于MLIR的编译流程[40]。基本思想：使用多个级别的IR来捕获不同级别代码的优化机会。对于每一层，可以定义针对分解的特定优化器。工作方法如图2。
1）MLIR框架接受现有语言或者以分解为本的编程模型作为输入。
2）使用一个通用层来进行常见优化，然后将优化后的语言降级到基于代码块的IR上，这个IR打包一个与代码块密切相关的指令。
3）将不同的代码块组合成一个或多个DAG，之后代码块被转换到各种后端进行最终编译。
4）每个代码块将生成多个二进制文件或比特流，以及可以与代码块二进制文件内的状态交互生成API
5）代码块被执行，并且在负载变化时可以跨设备迁移。


### 3.3 Hardware Infrastructure

FDP-DC硬件基础设施包括两个组成部分：
1）分解的资源池:每个资源池托管一种资源类型。它可以被独立构建、扩展和管理。现有系统可以无更改地在这些资源池上运行。
2）有电路交换机和可编程网络设备组成。网络功能首先被从其他资源池中分解出来，根据[56]整合到一个独立的网络池中。通过将更多类型的可编程网络设备纳入网络池，通过提出一个数据中心范围内的网络解决方案和使用电路交换机来实现可重配置拓扑，对[56]的方法进行改进。


#### 3.3.1 Disaggregated Devices

分解设备的三项核心功能和提供方法：

1.网络连接性：所有分解设备的基本特性。但是，与传统服务器不同，只需要基本的连接功能。
唯一工作：在最后一跳大宋和接受数据包，只需要提供基本的物理层和链路层功能。

2.多用户和虚拟化支持：通过灵活的虚拟化接口实现了硬件资源的安全公平共享。
应用于分解设备的新挑战：
1）确保虚拟系统的可扩展性。因为一个设备可能同时服务于许多客户端。
2）需要确保多用户的所有不同类型的整体公平性。
3）设备设计者应寻求软硬件协同设计机会，并在软硬件、客户端和服务端之间适当分配虚拟化或多租户任务。例[80]

3.安全功能。分解设备可以提供三个级别的安全防御：
1）基本的数据加密、认证和授权，保护设备免受网络中恶意实体的侵害
2）提供保密计算来提供更强的安全保证，允许用户将高敏感的计算和数据卸载到不受信任的云服务器提供商和它的管理堆栈。它在应用于分解设备时面临挑战。
3）提供机制和工具来缓解隐蔽通道和旁路攻击。这个级别应高度可配置，因为一些安全特性[60,64]可能会对性能产生较大影响。

总结：未来的分解设备应该投资于这四个特性。关键的技术挑战在于处理多种或新兴的计算资源。


#### 3.3.2 Data-center Network

图3：设想的数据中心网络结构。
数据中心网络被组织为多个节点，并由逻辑上集中的SDN控制器[7,19,24]进行管理。

##### Topology and Deployment scale

将资源彼此分离，他们之间的网络通信延迟将成为FDP-DC端到端应用性能的关键限制因素。->尝试减少通信设备之间的网络跳数。
由于少量计算节点足以充分利用单个分解的内存设备，认为将来的FDP-DC将采用分层拓扑。
其中，最底层（pod）包含数百个预计会频繁通信的分解设备。一个pod最多连接一个交换机，并可能连接一个分解的网络资源池。跨pod通信较少，但可能扩展到整个数据中心。


##### Network Programability

目前执行网络内计算的硬件资源丰富，数据中心网络变得更加可编程。本质上可以将可编程交换机和网卡视为一个网络资源池，端点可以将网络任务卸载到这个池中。
可以通过网络资源池中的设备启用更紧密的共享和多用户，进一步整合网络资源。
分解的网络资源池可以降低总资本支出（CapEx）和运营支出（OpEx）成本，并使网络可编程性更易于管理。

##### Dynamic reconfigurable topology

提议在固定的物理网络部署上实现动态可重配置拓扑。即在选定设备之间建立概念性的点对点连接，避免网络内缓冲。
使用电路交换来创建临时链接，调整他们来实现可重配置拓扑。如图3.
关键挑战：开发一个与基础设施和工作负载共同设计的高效调度策略。


### 3.4 OS of FDP-DC

OS和控制平面：监督所有分解的硬件和网络基础设施，覆盖整个数据中心。
工作：资源分配、任务调度、健康监控、负载均衡、故障处理。

#### Resource Management

挑战：如何有效地将应用程序映射到FDP-DC资源上。
FDP-DC带来了超越传统整体服务器物理限制的承诺。为实现承诺，需要实现：
1.将代码块映射到资源池
2.使代码块不受单个设备资源性质的影响。
调度的公平性政策和分配机制应考虑到分解设备之间的结构差异。
为应对挑战并实现良好扩展性，采用了类似LegoOS[55]的两级方法：全局管理器执行粗粒度、架构无关分配；特定设备执行更精细的、架构感知的分配。


#### Fault Tolerance

为了实现容错性，提供一系列的故障处理抽象选项：尽力而为处理（暴露给应用程序）和透明处理(对应用程序隐藏)[81]。
由于引入可重置拓扑带来的机架级故障域变化，在满足容错性和SLA（服务级别协议）的同时需要考虑拓扑结构。


#### Load Balance

控制平面最具挑战性的任务，需要在运行时高速地在分解设备之间平衡计算、数据和网络带宽。
我们确定了两种非排他性方法来解决传统方法难以做出足够快的决策的问题。
1）使用强化学习转化为一个学习问题。
2）卸载到应用程序层。





## 4 Conclusion

4 solutions:

easily available abstraction

flexible compiler optimization

scalable system management 

high efficient programmable hardware.

四个关键组成部分：
1）易于使用的抽象
2）灵活的编译器优化
3）可扩展的系统管理
4）高效的可编程硬件



# Q&A:
## Q1. What is FDP-DC?
## A1:
## Q2. What is MLIR ？
## A2. Multi-Level Intermediate Representation(MLIR), A novel approach to building reusable and extensible compiler infrastructure[39]. Its aim is to solve software fragment problem, improve the compilation efficiency of heterogeneous hardware, significantly reduce the cost of building specific domain complier, connect existing compiler together.


[1]资源分解：将每种类型的硬件资源组织成一个单独的资源池，并允许应用程序从池中使用任何资源。




 
 








