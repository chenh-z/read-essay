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
When couple with manageme layer, users can leave burden to provide.
Because above benefirs, f-g computing model are expected to keep wider adoption or become defalut comp. paradigms.
Hence, more exist app or sys software to small DAG-based parts.


