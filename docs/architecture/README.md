# OpenHands Architecture Docs

这组文档面向第一次接触 OpenHands 的读者，目标不是只讲“架构词汇”，而是同时回答两件事：

1. 第一次打开 OpenHands，你会看到什么、能做什么
2. 当前主仓、SDK、Benchmarks、Cloud、Enterprise 到底是什么关系

## 阅读顺序

1. [OpenHands 项目全景与当前仓库架构拆解](./openhands-architecture-overview.md)
2. [事实来源与延伸阅读](./references.md)
3. [独立静态导览页](./openhands-architecture-overview.html)

## 这组文档回答什么

- OpenHands 到底是不是一个 “harness” 项目
- 第一次打开 OpenHands 后，会看到哪些核心工作面板
- 当前主仓到底负责什么，不负责什么
- SDK、CLI、Local GUI、Cloud、Enterprise、Benchmarks 之间是什么关系
- 本仓库内部最关键的模块是什么
- 应该从哪里体验项目、怎么用、能达到怎样的效果
- 现有架构文档和使用指南分散在哪里

## 先记住这条边界

OpenHands 不是一个只有单仓库、单进程、单界面的系统。

- `OpenHands` 当前主仓主要承载 Local GUI / Cloud / Enterprise 相关的 React 前端、FastAPI app server、集成和配置层。
- `software-agent-sdk` 是外部独立仓库，README 明确把它定义为 “all of our agentic tech” 的引擎。
- `OpenHands/benchmarks` 也是外部独立仓库，负责 evaluation / benchmark harness。

因此，如果有人说 “OpenHands 的 harness 架构”，至少要先分清：

1. 产品层：GUI / Cloud / CLI / Enterprise 的使用入口
2. 能力层：SDK 提供的 agent、runtime、sandbox、workspace 能力
3. 评测层：benchmark / evaluation harness

这三层有关联，但不是同一个东西。
