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




 
 








