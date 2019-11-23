# 开放应用模型

开放应用模型（OAM）是一个用于构造云原生应用的规范。

_Open Application Model_ 聚焦于将开发与运维角色的关注点分离，秉持模块化、可拓展、可移植的设计原则，帮助用户在K8S等平台上构造并交付应用。

本项目正处于快速迭代阶段，可能会出现不兼容的修改。本项目对社区公众完全开放，如果您对社区贡献感兴趣，那么您可以从浏览项目的问题队列（Issue Queue）开始。我们正在**云原生应用建模领域**不断探索，期待您的积极参与！

## 导读

### OAM是如何工作的

![How it works](../../assets/how-it-works.png)

每当涉及应用开发与部署时，我们认为将开发人员与运维人员的职责（即 关注点）区分开来是非常重要的。假如二者不加以区分，而是职责错乱交叠，那将会给团队成员间的沟通交流带来诸多不便，甚至会给系统引入缺陷，更甚者可能会导致系统崩溃。

_Open Application Model_ 将【构造应用（开发）】、【部署&运维应用（应用运维）】及【运维基础设施（基础设施运维）】 三种角色的职责加以区分，并基于此对应用进行建模，从而解决上述问题。

* _Developers_ 即**开发人员**的职责是，描述某个微服务或组件“能做什么”以及该如何配置才能使其正常工作。开发人员掌控着和编写组件代码有关的一切。

* _Application Operators_ 即**应用运维人员**的职责是，配置这些微服务或组件部署及运行时所需的一切。运维人员掌控着和平台（指应用的运行时环境所在的平台）运维有关的一切。

* _Infrastructure Operators_ 即**基础设施运维人员**的职责是，搭建并维护应用运行所需的基础设施。基础设施运维人员掌控着和平台底层实现（泛指提供计算、存储、网络等服务的基础设施）有关的一切。

了解关于此的更多细节及示例讲解, 请参考[导读](./introduction.md)。

## OAM的实现工具

[Rudr](https://github.com/oam-dev/rudr) 是一个基于K8S的OAM参考实现，您可以通过该[Rudr教学实例](https://github.com/oam-dev/rudr/blob/master/docs/tutorials/deploy_and_update.md)开始学习。

## 目录

[标志约定](notational_convention.md)

  1. [目标与愿景](1.purpose_and_goals.md)
  2. [概述与术语](2.overview_and_terminology.md)
  3. [组件模型 The Component Model](3.component_model.md)
  4. [应用边界 Application Scopes](4.application_scopes.md)
  5. [运维能力 Traits](5.traits.md)
  6. [应用配置 Application Configuration](6.application_configuration.md)
  7. [工作负载类型 Workload Types](7.workload_types.md)
  8. [实践考虑因素](8.practical_considerations.md)
  9. [设计原则](9.design_principles.md)

## 社区

### 里程碑阶段

请通过 [里程碑阶段](https://github.com/microsoft/hydra-spec/milestones) 页面查看项目的里程牌阶段。

### 阶段划分

在每两周一次的社区电话会议中，项目中的工作事项等将被划分（加入、移除、调整）到特定的里程碑阶段中。

### 社区贡献

请参考 [社区贡献](../../CONTRIBUTING.md) 指南，了解如何参与本项目的社区工作。

其中最简单的参与方式是参与讨论，多种参与途径如下：

| 参与方式        | 链接  |
|---------------------|---|
| 社区邮件 | https://groups.google.com/forum/#!forum/oam-dev |
| 社区会议时间 | [自2019年10月22日起 每个双周 周三 02:30AM（北京时间）](https://calendar.google.com/calendar?cid=dDk5YThyNGIwOWJyYTJxajNlbWI0a2FvdGtAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ)  |
| 社区会议Zoom | https://zoom.us/j/271516061 |
| 即时通讯频道      | https://gitter.im/oam-dev/ |
| Twitter      | [@oam_dev](https://twitter.com/oam_dev) 