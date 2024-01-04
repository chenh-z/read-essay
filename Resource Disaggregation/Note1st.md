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

1.data center app more fine-grained
Fg computation units easier to scale and utilize d-c's hardware reasources
When couple with management layer, users can leave a burden to provide.
Because above benefits, f-g computing model are expected to keep wider adoption or become defalut comp. paradigms.
Hence, more exist app or sys software to small DAG-based parts.

2.Data center hardware infrastructure: more heterogeneous and progeammable
Cuz CPU meet its limitation, acceletators be deployed widely in major data centers.
Today's accelerators fully or partly programmable
Network resources: heterogeneous and programmable
Programmable switches and SmartNICs into data center.

3.Cuz old data center architecture make it difficlut to deploy and manage software and hardware, some major data centers to adopt 'resource disaggregation'(a new data-center atchitecture,etc. RD)
Idea of RD: organize each type hardware resources to a separate resource pool ,and allow apps to take any from a pool.
A Example

4.Three questions in today's disaggregation solutions:
1)What is a generic disaggregation design which fit heterogeneous types resources
2)How to efficiently connect disaggregated devices?
3)How to best map apps to a disaggregated hardware platform?

5.If above problem be solved, evolve DC into fully disaggregated and programmable.
This paper devide general goal to 4 sub prob(in figure 1)

6.1st sub prob:we need to decide how users can program and run apps
 on a FDP-DC
 2types abstraction, FDP-DC can choose 1-2
 1)backward-compatible, assume run on a VM or serverless model
 2)expose the underlying disaggregated and programmable nature to apps

8.2nd problem:how to map apps to FDP to the FDP-DC hardware and sys infrasturcture
Compare to traditional server setting:
Similarity: FDP-DC needs compiler
Difference: apps and hardware heterogeneous in FDP-DC
New propose to easily manage heterogeneity: Intermediate Representation(IR), which's center around disaggregared execution units'(that schedule and execution) or codelets' concept
In each codelet, encapsulate various execution features or hints.
Use MLIR[Q2] to decompose program to smaller codelets.
To dictate the execution order of codelets: use a companion DAG
Compiler further add features to DAG edges.

9.
Lack: FDP-DC's runtime system(etc. OS)
FDP-DC OS: Supervise all source and OS; reflect(schedule) code blocks DAG to base infrastructure
When schedule:use hint from compiler
And choose appropriate to initiate code blocks, supervise load(maybe transfer code blocks by load's variation)

10. To illstrate how 4 componets work, now discuss a data processing system server2server development and execution's process

11. Potential route: to provide data center designers to conctruct a fully disaggreated and programmable data center.

## 2 Background and Related Works

In programmable/reconfigurable networks, resource disaggregation and programming models.

### 2.1 Programmable and Reconfigurable Network

## 3 FDP-DC Design

All devices & networking hardware programmable and disaggregated from each other.
Network topology can be reconfigured to best match

FDP-DC: 4 components
Then, discuss in details.

### 3.1 Abstractions and Usage Models

2 abstraction:

Propperties : 
·all devices and network hardware is programmable and disaggrated each other
·Network topology can be dynamically reconfigured to match the workload's requirements

Figure 1: FDP-DC 4 componements solutions
·Abstraction && complier components : users facing and aims to enhance useability of FDP-DC
·OS && hardware infrastructure : building blocks of FDP-DC and environment executes complier output.

More Details:

### 3.1 

#### Transparent abstraction



## 4 Conclusion

4 solutions:

easily available abstraction

flexible compiler optimization

scalable system management 

high efficient programmable hardware.

# Q&A:
## Q1. What is FDP-DC?
## A1:
## Q2. What is MLIR ？
## A2. Multi-Level Intermediate Representation(MLIR), A novel approach to building reusable and extensible compiler infrastructure[39]. Its aim is to solve software fragment problem, improve the compilation efficiency of heterogeneous hardware, significantly reduce the cost of building specific domain complier, connect existing compiler together.




 
 








