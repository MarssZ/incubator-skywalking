#设计目标
该文档概述了skywalking的核心设计目标

##保持可观察性。

无论系统是如何部署的，SkyWalking都能够提供一种解决方案或集成方式使其成为可观察的。基于这个目的，SkyWalking提供了一系列的运行时形式和探针。

##从拓扑图到细节。

拓扑图应该是你理解分布式系统的第一步。它将整个复杂系统可视化为一个简单的拓扑图。基于这个拓扑图，我们可以了解到更多的关于服务、服务实例、端点和请求的信息。 并可以进一步通过详细的日志来追踪其细节。比如某个端点的延时很高，你可以看看最慢的相关请求以找到问题所在。如你所见，这是一个从全局到细节的过程，此过程的每个环节都是必须的。SkyWalking集成和提供了大量的功能来让这一切变得可能和易于理解。
Light Weight. There two parts of light weight are needed. (1) In probe, we just depend on network communication framework, prefer gRPC. By that, the probe should be as small as possible, to avoid the library conflicts and the payload of VM, such as permsize requirement in JVM. (2) As an observability platform, it is secondary and third level system in your project environment. So we are using our own light weight framework to build the backend core. Then you don't need to deploy big data tech platform and maintain them. SkyWalking should be simple in tech stack.
轻量化。有两个部分是轻量化的关键。 （1）我们把探针的实现放在像gRPC一样的网络通信框架上。这样的话，探针应该尽可能的足够小，以便防止可能的库冲突或是造成虚拟机负载压力。（2）作为一个可视化平台，他是你项目环境的第二和第三级系统。所以我们使用自己轻量级的框架来构建核心后端， 这样你并不需要再部署和维护额外的大数据平台。SkyWalking在技术上应该是简单易用的。
Pluggable. SkyWalking core team provides many default implementations, but definitely it is not enough, and also don't fit every scenario. So, we provide a lot of features for being pluggable.
插件化。SkyWalking的核心团队提供了许多基础实现，但显然这还不足以满足所有的场景。所以我们提供了大量的功能可以基于插件式的使用，实现其可插拔的特性。
Portability. SkyWalking can run in multiple environments, including: (1) Use traditional register center like eureka. (2) Use RPC framework including service discovery, like Spring Cloud, Apache Dubbo. (3) Use Service Mesh in modern infrastructure. (4) Use cloud services. (5) Across cloud deployment. SkyWalking should run well in all these cases.
便携性。SkyWalking可以在多种多样的环境中运行。并表现良好。包括
（1）传统的注册中心，如eureka
（2）基于RPC框架来实现服务发现的场景，如Spring Cloud, Apache Dubbo
（3）现在流行的服务网格架构
（4）云服务
（5）跨云部署
Interop. Observability is a big landscape, SkyWalking is impossible to support all, even by its community. As that, it supports to interop with other OSS system, mostly probes, such as Zipkin, Jaeger, OpenTracing, OpenCensus. To accept and understand their data formats makes sure SkyWalking more useful for end users. And don't require the users to switch their libraries.
与外部系统的可配合性。可视化是一个非常大的领域，SkyWalking即使有它的整个社区来帮助它，也不可能实现所有可视化的需求。因此，他被设计成可以与许多其它的外部系统进行交互，比如绝大多数探针系统像是Zipkin, Jaeger, OpenTracing, OpenCensus. SkyWalking能够接受和理解他们的数据格式，而且并不需要用户切换到他们的数据库，这使得SkyWalking对于用户来说更加友好。

What is next? 下一步？
See probe Introduction to know SkyWalking's probe groups.
阅读 探针 进一步了解SkyWalking的探针组
From backend overview, you can understand what backend does after it received probe data.
阅读  后端系统, 你可以了解到后端系统在收到探针发回的数据后做了哪些事情
If you want to customize UI, start with UI overview document.
如果你想自定义UI，你可以阅读  UI概述
