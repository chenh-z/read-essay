## 摘要

这篇论文的摘要概述了以下几个主要内容：

1. **数据中心的两大趋势**：首先，摘要指出当前数据中心领域存在两大趋势。一是应用程序正变得更加细粒度【1】，这主要是由于最近无服务器计算【2】和微服务【3】的流行。二是数据中心硬件正变得更加异构和定制化，以满足不同的计算需求。

2. **向解耦架构的转变**：由于这些趋势，以及为了更好的可管理性，多个主要数据中心正转向解耦架构。在这种架构下，像存储和加速器这样的不同硬件资源被组织为独立的、网络连接的资源池【4】。

3. **现有的局限性**：当前的数据中心仍以服务器为中心，严重依赖传统的基于CPU的服务器【5】。

4. **全面解耦数据中心的探索**：论文进一步探讨了构建一个完全解耦的数据中心的可能性，其中每种类型的资源都是解耦的。此外，论文还探讨了使每个解耦设备可编程的要求和影响。

5. **设计指南和初步解决方案**：提出了数据中心设计者在导航设计权衡时可以遵循的指导原则和初步解决方案。

6. **问题分解和解决方案**：将主要问题分解为四个子问题，并为每个子问题提出解决方案。**包括探索两种类型的抽象，并提出一种原生解耦的设计方法论；描述构建解耦设备所需的硬件和关键特性，以及连接它们的网络基础设施。**

7. **桥接不同层次的方案**：提出了一个静态时间组件【6】，用于通过原生解耦【7】的中间表示将不同用户程序编译到异构解耦设备中。还提出了一个运行时系统【8】，用于管理硬件资源和调度编译器生成的执行单元。

8. **期望对未来的影响**：作者希望他们的提议能为未来解耦和可编程数据中心的部署铺平道路。

总体而言，这篇论文探讨了如何构建一个全面解耦和可编程的数据中心，提出了多种解决方案和设计指南，旨在解决现有数据中心在应对当前趋势时所面临的挑战。

【1】应用程序细粒度（fine-grained）：一种软件架构设计方法，它将传统的大型、单一的应用程序拆分为更小、更独立的组件或服务。这种方法的核心在于将一个复杂的应用程序解构为一系列单一职责的小型服务，每个服务都围绕着特定的业务功能或任务构建。

【2】无服务器计算（serverless computing）：是一种云计算执行模型，其中云服务提供商自动管理底层基础设施，并允许用户运行代码而无需关注服务器的管理和维护。在无服务器架构中，开发者可以专注于编写和部署应用程序代码，而**不需要管理或调整服务器资源。**

【3】微服务（microservices）:是一种软件架构风格，它将应用程序构建为一组小型、相互独立的服务。每个服务都围绕特定的业务功能构建，并且可以独立地开发、部署、运行和扩展。微服务架构是对传统的单体应用（Monolithic Application）架构的一种回应，旨在提高大型复杂应用程序的可扩展性、可维护性和敏捷性。

【4】资源池（resource pool）：是一种在计算、网络和存储等资源管理中常见的概念。在这种模型中，资源不是分配给单一的任务或实例，而是集中管理并根据需要动态分配给不同的任务或用户。这种方法提高了资源的利用率和灵活性，同时降低了成本。

【5】以服务器为中心的数据中心(server-centric data center):是一种传统的数据中心架构，其中核心计算、存储和网络资源主要集中在物理服务器上。这种架构依赖于标准的、多用途的CPU来处理大部分的计算任务。

【6】静态时间组件(Static-time componment)：常指的是在程序或系统运行之前，即编译时或部署前阶段进行的处理或配置的部分。这个概念与动态时间或运行时处理相对，后者是指在程序运行时发生的处理。

【7】原生解耦(Native Disaggregation)：从设计之初就将系统的不同组件（如计算、存储、网络资源等）物理上和逻辑上分离，并作为独立的单元进行管理和操作的方法。

【8】运行时系统(Runtime System)：支持计算机程序或应用程序执行的一套软件服务和环境。它为正在运行的程序提供了必要的服务和资源管理，从而使得程序能够正常执行。
