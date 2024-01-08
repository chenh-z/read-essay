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


[1]资源分解：将每种类型的硬件资源组织成一个单独的资源池，并允许应用程序从池中使用任何资源。




 
 








