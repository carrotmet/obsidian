# 为什么需要分布式？

以前的数据库面对现在的业务处理能力太低。慢，太tm慢了

于是引入两种扩展方法：垂直拓展 or  水平拓展
```python
简单理解为:

垂直扩展加容量和单核处理能力

横向扩展加节点，强调分布式/多节点数据一致/安全性 

垂直拓展没前途(数据库数据量无限增长的前提下)，水平拓展不好弄

所以引入————云计算
许多云服务提供商提供了自动扩展的服务，可以根据系统负载自动进行垂直或水平扩展，这为数据库扩展提供了更多的灵活性和便利性。
```

1. **垂直扩展（Vertical Scaling）**：
   - 加容量：通过增加单个服务器的存储容量（如硬盘空间）、内存或更快的CPU来提升性能。
   - 提升单核处理能力：通过使用更强大的单个处理器或增加CPU核心数来提高单个服务器的处理能力。
   - 优点在于简单易操作，管理和维护相对容易。
   - 缺点是受限于硬件的物理限制，存在扩展上限，且成本随着硬件增强而提高。

2. **水平扩展（Horizontal Scaling）**：
   - 加节点：通过增加更多的服务器或数据库节点来分散负载，每个节点可以处理一部分请求或存储一部分数据。
   - 分布式：强调在多个节点间分布数据和计算任务，利用多个服务器的计算和存储能力。
   - 数据一致性：在分布式数据库中，需要处理多节点之间的数据一致性问题，这通常涉及到复杂的分布式算法和协议。
   - 优点是理论上可以线性扩展，成本较低，且可以提高系统的容错性和可用性。
   - 缺点是系统设计和管理更为复杂，需要处理分布式系统中的各种问题，如网络通信、数据分片、分布式事务、一致性保证等。

在实际应用中，垂直扩展和水平扩展可以根据业务需求和系统特点灵活选择或结合使用：

- 对于小型或数据量不大的系统，垂直扩展可能是一个快速且成本效益高的选择。
- 对于大型或高增长的系统，水平扩展可以提供更好的可伸缩性和容错性。



# 为什么需要云原生？

## 云原生是以云厂商提供的托管服务为server，以容器与节点为基本单位，以CI/CD为指导原则进行容器编排与服务设计。
- 定义：
	- 云原生（Cloud Native）是一种构建和运行应用程序的方法论，它充分利用了云计算模型的优势，以提高应用程序的可扩展性、可靠性和灵活性。云原生的核心概念包括微服务架构、持续集成和持续部署（CI/CD）、以及容器化等。

### 云原生的特点：

1. **微服务架构**：应用程序被拆分成一系列小的、独立的服务，每个服务实现特定功能，并且可以独立部署和扩展。

2. **容器化**：应用程序及其依赖被封装在轻量级的容器中，这些容器在任何环境中都能一致运行，简化了部署和扩展过程。

3. **动态管理**：云原生应用通常由容器编排工具（如 Kubernetes）管理，这些工具可以自动处理容器的部署、扩展和故障转移。

4. **持续集成和持续部署（CI/CD）**：自动化的流程确保代码的快速迭代和部署，提高了开发效率和软件质量。

5. **DevOps文化**：开发和运维团队紧密合作，共同负责软件的开发、部署和运行。

### 云原生与云计算的关系：
####  云计算的本质是“一切皆服务”+弹性
云计算提供了基础设施即服务（IaaS）、平台即服务（PaaS）和软件即服务（SaaS）等不同层次的服务，云原生应用程序则是在这种云计算环境中设计和运行的。

1. **基础设施即服务（IaaS）**：提供了虚拟化的计算资源，如 CPU、内存、存储等，云原生应用可以部署在这些资源上。

2. **平台即服务（PaaS）**：提供了应用程序开发和运行的平台，包括数据库服务、中间件等，云原生应用可以利用这些服务来简化开发和运维。

3. **软件即服务（SaaS）**：提供了通过互联网访问的软件应用程序，云原生应用可以作为 SaaS 提供给最终用户。

4. **云服务的弹性**：云计算提供了按需扩展的资源，云原生应用可以利用这一点来应对不同的负载需求。

5. **云原生技术栈**：许多云服务提供商提供了专门为云原生应用设计的服务和工具，如 AWS 的 ECS 和 EKS、Azure 的 AKS、Google Cloud 的 GKE 等。

总的来说，云原生是一种理念和方法论，而云计算提供了实现云原生应用的技术和平台。云原生应用程序设计为在云计算环境中运行，充分利用云计算的弹性、可扩展性和服务多样性。


