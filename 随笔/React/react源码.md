
[react源码分析](https://segmentfault.com/a/1190000012834204)

[fiber 架构：司徒正美](https://zhuanlan.zhihu.com/p/37095662)

### 一、基础概念
1. 什么是 reconciliation？

reconciliation 是 React 算法，用来比较两颗树，以确定哪些部分需要改变。


2. 更新

用于呈现React应用的数据更改。通常是`setState`的结果。最终导致重新渲染。


3. Reconciliation 与渲染

DOM只是React可以渲染的渲染环境之一，还可以通过React Native进行本地iOS和Android视图。 （这就是为什么“虚拟DOM”有点用词不当）。

它可以支持如此多目标是因为React的设计使reconciliation和渲染是分开的阶段。reconciler完成了计算树的哪些部分已经改变的工作;渲染器然后使用该信息实际更新呈现的应用程序。

这种分离意味着React DOM和React Native可以使用他们自己的渲染器，同时共享由React核心提供的相同的reconciler。

Fiber重新实现了reconciler。虽然渲染者需要改变以支持（并利用）新的架构，但它并不主要关心渲染。

4. Scheduling 时序

确定何时应该进行工作的过程。


5. work

任何必须执行的计算。工作通常是更新的结果（例如setState）。
