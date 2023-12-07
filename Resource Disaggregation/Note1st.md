# Towards a fully disaggregated and programmable data center
  通往一种完全分解和可编程的数据中心

## Abstract
   摘要
   
2 trends in data centers:
1)application more fine-grained
2)data-center hardware heterogeneous and customized to diffierent computing needs
Several major data center go towards disaggregated architecture(different hardware resources are organized independently and network-attached pools) because of the 2 trends.
But, today data centers still server-centric, rely on traditional CPU-based servers.

数据中心的2个趋势：
1）应用程序更细化
2）数据中心硬件异构，并且为不同的计算定制。
由于这两个趋势，几个不同的数据中心走向法分离的结构（不同的硬件资源被独立地组织为网络连接池）
但是，目前数据中心仍然服务器中心化，依赖传统的基于CPU的服务器。


Works:
1.explore the posibility of building a fully disaggreated data senter
2.explore the requirements and implications of making disaggreated data centers programmable
Present guidlines and initial solutions, to navigate design trade-offs
Decompose problem to 4 sub-p.
Top layer: 2 abstractions, propose a disaggregation-native design methodology
Bottom layer: describe hardware and key features, to build disaggregared devices and networking infrastructure to connect
To bridge the 2: static-time component and run-time system.


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

 
 








