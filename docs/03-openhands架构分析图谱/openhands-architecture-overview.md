# OpenHands 项目全景与当前仓库架构拆解

## 先讲结论：OpenHands 到底是什么

第一次接触 OpenHands，最容易掉进三个误区：

- 以为这个仓库就是全部 OpenHands
- 以为 OpenHands 只是一个 benchmark harness
- 以为它只是一个会调用大模型的前端页面

这三个理解都不准确。

更准确的说法是：

- `software-agent-sdk` 是底座，承载真正的 agentic tech、运行时和执行能力。
- 当前 `OpenHands` 主仓更像应用整合层，把这些能力做成 Local GUI、Cloud、Enterprise 能直接使用的产品界面与服务层。
- `OpenHands/benchmarks` 是独立的评测与 benchmark harness 仓库，不属于当前主仓内部模块。

所以，如果有人问 “OpenHands 的 harness 架构是什么”，先要分清三层：

1. **产品层**：用户通过 GUI、CLI、Cloud、Enterprise 接触到的 OpenHands
2. **能力层**：SDK 提供的 agent、runtime、sandbox、workspace 等底层能力
3. **评测层**：Benchmarks 提供的 evaluation harness

这三层互相关联，但不是同一个东西。

## 60 秒上手导览：第一次打开 OpenHands，你会看到什么

如果你先不关心代码，而只是想知道“这个东西怎么用”，可以先把 OpenHands 想成一个**带执行环境的 AI 软件工程工作区**。

### 你先会看到什么

根据官方介绍和 GUI 功能文档，OpenHands 的核心体验不是单一聊天框，而是一组围绕任务运行的工作面板。第一次打开时，你通常会接触到：

- **Chat**
  - 你在这里输入任务、目标、约束和补充说明。
  - 这是“你告诉 agent 要做什么”的地方。
- **Changes**
  - 你在这里看 agent 改了哪些文件、产生了什么差异。
  - 这是“你检查它到底动了什么”的地方。
- **VS Code**
  - 你可以从图形视角查看工作区文件与代码结构。
  - 这是“你把它当成可浏览工程”的地方。
- **Terminal**
  - 这里展示 agent 在环境里执行的命令与输出。
  - 这是“你确认它有没有真的跑起来”的地方。
- **Browser / App**
  - 当任务涉及 Web 应用时，你可以直接看运行中的页面或交互结果。
  - 这是“你验证它做出来的东西是不是活的”的地方。
- **Planner / Task 相关视图**
  - 某些模式下会显示计划、步骤或任务分解。
  - 这是“你看它准备怎么做”的地方。

### 你能做什么

第一次使用时，最常见的动作是：

1. 在 Chat 里提出一个软件任务
2. 看 agent 是否给出计划或开始执行
3. 在 Terminal 看命令是否真的运行
4. 在 Changes 看文件是否真的被修改
5. 在 Browser / App 看运行结果
6. 如果结果不对，再回到 Chat 继续纠偏

这也是 OpenHands 和普通聊天机器人最直接的区别：**它不只是回答问题，而是尝试在一个真实执行环境里做事，并把过程展示给你。**

## OpenHands 到底有哪几种进入方式

根 `README.md` 和官方介绍把 OpenHands 明确拆成几条产品入口。理解这些入口，比一上来读源码更重要。

### 1. OpenHands Software Agent SDK

- **它是什么**
  - 这是 OpenHands 底层能力最集中的地方。
  - 官方 README 明确说它包含 “all of our agentic tech”。
- **适合谁**
  - 想把 OpenHands 能力接入自己系统的人
  - 想用 Python 定义 agent、批量调度 agent 的开发者
- **和当前主仓什么关系**
  - 当前主仓不是它的替代品，而是在它之上做产品化包装。

### 2. OpenHands CLI

- **它是什么**
  - 命令行入口，使用体验更接近 Claude Code / Codex。
- **适合谁**
  - 喜欢终端工作流，不需要 Web UI 的用户
- **和当前主仓什么关系**
  - 它是独立产品与独立源码仓库，不是当前主仓里的一个子目录。

### 3. OpenHands Local GUI

- **它是什么**
  - 本地运行的图形化 OpenHands。
  - 这是当前主仓最直接对应的应用形态。
- **适合谁**
  - 想在浏览器里和 agent 协作的人
  - 想直观看到会话、终端、变更、页面运行效果的人
- **和当前主仓什么关系**
  - React 前端 + FastAPI app server 的主要实现就在这个仓库里。

### 4. OpenHands Cloud

- **它是什么**
  - 官方托管版的 GUI。
- **适合谁**
  - 想直接体验、不想先本地部署的人
- **和当前主仓什么关系**
  - 产品形态仍然基于 GUI + app server + 执行环境，只是部署在官方基础设施上。

### 5. OpenHands Enterprise

- **它是什么**
  - 面向企业私有化和更复杂组织需求的扩展版本。
- **适合谁**
  - 需要组织管理、权限、集成、私有部署的企业团队
- **和当前主仓什么关系**
  - 扩展代码在当前仓库 `enterprise/` 下，但授权和运行边界不同于 OSS 主体。

### 6. OpenHands Benchmarks

- **它是什么**
  - 独立的评测与 benchmark 仓库。
- **适合谁**
  - 想评估 agent 在任务集上表现的人
- **和当前主仓什么关系**
  - 它不是 `openhands/app_server/` 的一个模块，而是仓库之外的独立 evaluation harness。

## 谁把 OpenHands 做成了产品：生态与产品化证据

如果只看主仓代码，很容易把 OpenHands 理解成“一个还在成长中的开源项目”。但公开资料已经能说明，它不只是概念或实验，而是正在被做成不同层次的产品。

为了避免把传闻写成事实，这里只收录**有真实可直达链接**、且能明确证明与 OpenHands / OpenHands SDK 有关系的案例。

### 1. OpenHands 自己已经是产品

这不是第三方案例，但它是最强的产品化证据。

- **OpenHands Cloud**
  - 官方发布页：<https://www.openhands.dev/blog/introducing-the-openhands-cloud>
  - 说明：OpenHands 在 2025 年 3 月 31 日正式发布托管版 Cloud，公开强调 GitHub 深度集成、自动创建与更新 PR、支持多 agent 同时工作。
- **OpenHands Cloud Self-hosted**
  - 官方发布页：<https://www.openhands.dev/blog/openhands-cloud-self-hosted-secure-convenient-deployment-of-ai-software-development-agents>
  - 说明：官方把它定义成集中式、多租户 OpenHands server，并推出可在企业隔离 VPC 中自托管的版本。
- **OpenHands Enterprise**
  - 企业文档：<https://docs.openhands.dev/enterprise>
  - 企业发布页：<https://www.openhands.dev/blog/openhands-enterprise-agent-control-plane>
  - 说明：这已经不是“开源项目附带说明”，而是明确的企业产品形态，强调 SSO/SAML、RBAC、私有云 / 本地部署、组织级控制平面等能力。

### 2. 大企业 / 大公司侧的公开产品化与集成证据

这些案例不一定意味着“整家公司都完全基于 OpenHands”，但它们足以证明 OpenHands 已经进入较大组织的产品与平台语境。

#### AMD

- 技术文章：<https://www.amd.com/en/developer/resources/technical-articles/2025/OpenHands.html>
- 为什么能写进来：
  - AMD 官方技术文章明确把 OpenHands 作为可落地的开源方案来介绍。
  - 文中还提到商业客户可用 OpenHands 的 SaaS 与 self-hosted 方案。
- 通俗解释：
  - 这说明 OpenHands 已经进入大型芯片厂商的正式技术传播语境，不再只是社区层面的玩具项目。

#### C3 AI

- 官方博客：<https://c3.ai/blog/autonomous-coding-agents-beyond-developer-productivity/>
- 为什么能写进来：
  - C3 AI 公开写到其系统 “powered by OpenHands”，并明确表示把 OpenHands 作为其 autonomous coding agent 的核心选择之一。
- 通俗解释：
  - 这不是“某个开发者私下试了试”，而是企业级 AI 平台公开把 OpenHands 作为底层能力来源之一来讲。

#### Databricks / MLflow

- 官方博客：<https://mlflow.org/blog/mlflow-openhands>
- 为什么能写进来：
  - 这篇文章专门讲如何把 MLflow 的 observability / governance 加到 OpenHands agent 上。
  - 它证明了 OpenHands 已经开始被纳入更正式的模型治理和可观测性工具链。
- 通俗解释：
  - 这意味着 OpenHands 不只是“能跑起来”，而是已经有人开始给它接企业环境常见的治理和审计能力。

### 3. 中小企业 / 创业团队的产品化案例

这些案例特别重要，因为它们更接近“别人怎样把 OpenHands 做成可卖、可集成、可部署的产品”。

#### Daytona

- 官方博客：<https://www.daytona.io/dotfiles/instant-ai-development-with-daytona-and-openhands>
- 产品页：<https://openhands.daytona.io/>
- 为什么能写进来：
  - Daytona 明确写到它把 OpenHands 和自己的基础设施结合成一套解决方案。
  - 公开页面强调即时访问、沙箱化、企业级基础设施。
- 通俗解释：
  - 可以把它看成“有人把 OpenHands 接上了更强的运行环境和托管能力，然后包装成更像产品的体验”。

#### TestChimp

- 集成文档：<https://docs.testchimp.io/integrations/openhands/>
- 为什么能写进来：
  - 文档明确要求用户创建 OpenHands 账户、连接 GitHub 仓库、配置 API key，再从 TestChimp 触发自动修复并发 PR。
- 通俗解释：
  - 这已经是很典型的 B2B SaaS 集成方式：OpenHands 作为修复执行层，被嵌进另一个测试 / 质量平台工作流中。

#### Lynxroute

- Azure Marketplace：<https://marketplace.microsoft.com/en-us/product/saas/lynxroute.openhands?tab=overview>
- 为什么能写进来：
  - 它不是博客或概念页，而是正式的市场上架条目。
  - 产品名直接就是 “OpenHands - Hardened Self-Hosted Autonomous AI Agent”。
- 通俗解释：
  - 这说明已经有中小厂商把 OpenHands 做成可采购、可部署、可交付的商业包装版本。

#### Spheron

- 官方博客：<https://www.spheron.network/blog/deploy-openhands-gpu-cloud/>
- 为什么能写进来：
  - 虽然更偏部署方案，而不是纯 SaaS 产品，但它明确以 OpenHands 的生产部署为主题，并讨论安全、成本和 GPU cloud 运行。
- 通俗解释：
  - 这说明 OpenHands 已经进入“云上部署与商业运行方案”层面的传播，而不只是开发者本地试玩。

### 4. 微型团队 / 个人开发者的二次开发产品

这部分最能回答“OpenHands 能不能被个人或小团队做成真正的产品”。

#### OmoiOS

- GitHub：<https://github.com/kivo360/OmoiOS>
- 官网：<https://www.omoios.dev/>
- 文档：<https://www.omoios.dev/docs>
- 为什么能写进来：
  - GitHub 项目明确写着 “powered by Claude & OpenHands”。
  - 官网提供了产品站、文档、定价、演示和注册入口。
- 通俗解释：
  - 这是很强的“个人 / 小团队把 OpenHands 做成可直接使用产品”的证据。

#### OpenSymphony

- GitHub：<https://github.com/kumanday/OpenSymphony>
- 官网：<https://opensymphony.dev/>
- About：<https://opensymphony.dev/about/>
- 包文档：<https://docs.rs/crate/opensymphony/latest>
- 为什么能写进来：
  - 官方站点明确说明用 OpenHands 作为 execution harness。
  - 不只是代码仓，还有公开站点、说明页和可安装包。
- 通俗解释：
  - 这说明个人开发者不仅能在 OpenHands 上做二开，还能把它包装成有身份、有分发形式的独立产品。

#### Thesis（Oraichain）

- Repo：<https://github.com/oraichain/Thesis>
- App：<https://thesis.io/>
- Oraichain 生态页：<https://orai.io/>
- Python package：<https://github.com/oraichain/thesis-py>
- 为什么能写进来：
  - 该 repo 明确 fork 自 `OpenHands/OpenHands`。
  - 同时它有自己的品牌、独立 app 和生态位置。
- 通俗解释：
  - 这是更接近“基于 OpenHands fork 出自己的产品壳层”的案例。

#### OpenHands AI Action

- Repo：<https://github.com/xinbenlv/openhands-action>
- GitHub Marketplace：<https://github.com/marketplace/actions/openhands-ai-action>
- 为什么能写进来：
  - 它不是停留在 repo，而是已经作为 GitHub Marketplace Action 公开分发。
  - 页面明确说明用 OpenHands 在 GitHub workflow 中无头运行。
- 通俗解释：
  - 这是“把 OpenHands 做成标准 CI/CD 产品部件”的典型例子。

#### openhands-infra

- Repo：<https://github.com/zxkane/openhands-infra>
- 博客：<https://kane.mx/posts/2026/serverless-multi-tenant-openhands-on-aws/>
- 为什么能写进来：
  - 它不是单纯部署脚本，而是带有多租户、自托管、成本模型、AWS CDK 的完整方案。
- 通俗解释：
  - 更适合被理解成“围绕 OpenHands 做出的可复用基础设施产品模板”。

### 5. 可以提到，但不应当写成强结论的弱证据

下面这些可以作为“生态存在感”的补充，但不应和上面的强证据混为一谈：

- Flextract 在 OpenHands 官网上的 testimonial：<https://openhands.dev/>
- OpenHands 企业页上的 C3.ai testimonial：<https://www.openhands.dev/enterprise>
- `slack-openhands-bot`：<https://github.com/yourself-q/slack-openhands-bot/>
- `lxa`：<https://github.com/jpshackelford/lxa>

这些案例的问题通常是：

- 证据来自供应商自己首页
- 只有 repo，没有清晰的产品面
- 有集成但缺少清晰商业化或产品化证明

## 这组证据说明了什么

把这些公开案例合在一起，至少能得出三个更可靠的结论：

1. **OpenHands 自己已经完成了从开源项目到产品形态的跃迁**
   - Cloud、Self-hosted Cloud、Enterprise 都是明确产品信号。
2. **OpenHands 已经被拿来做企业级集成和商业包装**
   - AMD、C3 AI、MLflow/Databricks、Daytona、TestChimp、Lynxroute 都说明它已经进入企业或 B2B 语境。
3. **OpenHands 也已经被个人或微型团队做成独立产品 / 工具 / 市场组件**
   - OmoiOS、OpenSymphony、Thesis、OpenHands AI Action 这些都说明它不是只能被大公司用。

## 当前主仓的真正定位

如果把 OpenHands 整体想成一套完整机器，那当前主仓更像：

- **前台工作区**
  - 用户通过浏览器看到会话、终端、代码、变更和运行结果
- **中台协调层**
  - 把 conversation、settings、events、sandbox、用户上下文组织起来
- **产品化接口层**
  - 把 SDK 的能力接到 GUI、Cloud 和 Enterprise 形态上

也就是说，当前主仓不是“所有底层能力的唯一来源”，但它是**第一次接触者最容易真正用起来的那层**。

从代码入口看，这个定位很清楚：

- `openhands/app_server/app.py` 创建 FastAPI 应用
- `openhands/app_server/v1_router.py` 聚合 `/api/v1` 路由
- `frontend/src/routes.ts` 定义前端页面结构
- `Makefile` 中 `make run` / `make start-backend` / `make start-frontend` 提供本地体验入口

## 当前主仓的架构怎么拼起来

## 1. 应用入口层

### 后端入口：`openhands/app_server/app.py`

这个入口做了几件关键事：

- 初始化 FastAPI 应用
- 挂载 `v1_router`
- 挂载健康检查路由
- 挂载 MCP 相关 HTTP app
- 在有 `frontend/build` 时，把前端构建产物直接托管在 `/`

对第一次接触的人，可以把它理解成：

- 它是这个产品后端的“总接线板”
- 既负责 API，也能直接把前端页面一起对外提供

### 前端入口：`frontend/src/routes.ts`

前端主路由包括：

- `/` 首页
- `/settings/*` 设置体系
- `/conversations/:conversationId` 会话页
- `/shared/conversations/:conversationId` 共享只读页
- `/login`、`/onboarding`、`/launch` 等辅助页面

这说明 OpenHands 前端不是一个单页聊天壳，而是一套完整的应用导航。

## 2. API 聚合层：`openhands/app_server/v1_router.py`

这里把主仓最重要的后端能力都挂到了 `/api/v1` 下：

- `event`
- `app_conversation`
- `pending_messages`
- `sandbox`
- `sandbox_spec`
- `settings`
- `secrets`
- `user`
- `skills`
- `webhooks`
- `web_client`
- `git`
- `config_api`

对新人来说，这一层最重要的意义是：

**OpenHands 的 GUI 不是直接在前端里调用 SDK 对象，而是通过 app server 把会话、事件、配置、执行环境和用户状态统一组织起来。**

## 一次会话是怎么跑起来的

理解 OpenHands 最容易的方法，不是先读模块目录，而是沿着一次真实任务走一遍。

### 1. 你进入界面

你打开 Local GUI 或 Cloud，前端会先获取：

- 当前部署是什么模式
- 哪些 provider 可用
- 哪些功能被打开
- 用户设置和模型设置是什么

### 2. 你创建或打开一个 conversation

conversation 可以理解为“这次任务的主档案”。

它会记录：

- 这次任务是谁发起的
- 当前状态是什么
- 关联了哪个 sandbox / runtime
- 哪些事件属于它

### 3. app server 组织这次任务

这一步不一定直接“执行任务”，但会负责把关键信息串起来：

- conversation 元数据
- 用户上下文
- settings / secrets
- sandbox 入口
- 事件流通道

### 4. sandbox / runtime 开始做事

agent 真正执行命令、读写文件、跑代码、操作仓库，发生在隔离执行环境里。

对第一次接触的人，可以这样理解：

- **Chat 是你在下指令**
- **Sandbox 是它真的去干活的地方**

### 5. event 把过程变成可展示的信息

运行过程中的输出、动作和状态变化，会被组织成事件。

这些事件再被用来：

- 在前端展示消息流
- 在终端展示命令输出
- 在变更页展示结果
- 在共享页或恢复流程中回放状态

### 6. WebSocket 和查询层把结果送回前端

前端不是只在最后刷新一次，而是尽量持续同步运行中的状态。

会话页相关代码可以看出：

- 页面会拉取 active conversation
- 会接上 WebSocket provider
- 会处理恢复逻辑
- 会根据 `sandbox_status` 区分正常会话和 archived conversation

### 7. 你在多个面板里看到结果

这时你才真正感受到它像一个工作区，而不是一个回答器：

- Chat 里看到计划或回应
- Terminal 里看到命令执行
- Changes 里看到代码改动
- Browser / App 里看到运行中的结果

### 8. 如果启用了集成，结果还能接回外部系统

在 Cloud / Enterprise 场景里，结果还可能继续回写到：

- GitHub
- GitLab
- Slack
- Jira
- Linear

所以 OpenHands 的目标不只是“在本地生成点东西”，而是把 agent 工作接回真实研发流程。

## 核心模块拆解：把术语翻译成人话

## `app_conversation/`

- **它是什么**
  - 会话管理层。
- **你什么时候会遇到它**
  - 你创建一个任务、打开一个历史会话、分享一个会话时，背后都离不开它。
- **为什么它关键**
  - 因为它把“用户视角的一次任务”变成了系统里能跟踪、能恢复、能共享的一条主线。

## `sandbox/`

- **它是什么**
  - 隔离执行环境管理层。
- **你什么时候会遇到它**
  - 每当 agent 需要跑命令、改文件、启动项目、访问运行时，它都在后面工作。
- **为什么它关键**
  - 因为没有 sandbox，OpenHands 就退化成只能说、不能做的聊天机器人。

## `event/`

- **它是什么**
  - 事件存储和流转层。
- **你什么时候会遇到它**
  - 你在界面里看到状态更新、消息推进、输出流和回放结果时，它都在参与。
- **为什么它关键**
  - 因为 OpenHands 需要把“执行过程”变成“可展示的过程”。

## `settings/` 和 `secrets/`

- **它们是什么**
  - 配置和敏感信息管理层。
- **你什么时候会遇到它们**
  - 配模型、填 API key、切换行为开关、管理应用设置时。
- **为什么它们关键**
  - 因为产品不能靠写死配置运行，尤其要同时支持本地 GUI、Cloud 和 Enterprise。

## `user/` 和 `services/`

- **它们是什么**
  - 用户身份、用户上下文和辅助服务层。
- **你什么时候会遇到它们**
  - 登录、访问控制、用户关联 sandbox 或 provider token 时。
- **为什么它们关键**
  - 因为系统必须知道“谁在发起请求、谁能访问哪些能力”。

## `web_client/`

- **它是什么**
  - 给前端提供启动配置的模块。
- **你什么时候会遇到它**
  - 你一打开前端，系统就需要知道当前是 OSS 还是 SaaS、有哪些 provider、哪些功能开关是开的。
- **为什么它关键**
  - 因为它决定前端一开始怎么呈现自己。

## `integrations/`

- **它是什么**
  - 对接外部开发协作系统的能力层。
- **你什么时候会遇到它**
  - 当你希望 agent 和 GitHub、GitLab、Slack、Jira、Linear 交互时。
- **为什么它关键**
  - 因为这决定 OpenHands 能不能融入团队的真实工作流。

## `enterprise/`

- **它是什么**
  - 面向企业部署的扩展层。
- **你什么时候会遇到它**
  - 当你需要组织、多用户、权限、认证、计费、私有部署这些能力时。
- **为什么它关键**
  - 因为它让 OpenHands 从单用户工具向团队级产品扩展。

## 前端为什么不只是“显示聊天”

`frontend/README.md` 和实际路由结构能看出，前端是完整的应用交互层：

- React + Vite + React Router
- TypeScript
- Redux
- TanStack Query
- Tailwind CSS
- i18next

从数据流上看，前端也有明确分层：

- UI components
- query / mutation hooks
- `src/api`
- 后端 `/api/v1`

所以它不是随便堆页面，而是：

- 用 hooks 管理读取和修改
- 用 WebSocket 维持运行中状态
- 用 routes 把会话、设置、组织、共享等功能拆成完整应用结构

## 通过哪里可以体验 OpenHands

## 1. 最快体验：OpenHands Cloud

- 入口：`https://app.all-hands.dev`
- 适合：
  - 想最快体验完整产品的人
  - 不想先本地装依赖的人

## 2. 最贴近当前主仓：Local GUI

根据 `Development.md` 和 `Makefile`，常见本地路径是：

- `make build`
- `make run`

或者分开启动：

- `make start-backend`
- `make start-frontend`

前端开发态也可以：

- `cd frontend && npm run dev`
- `npm run dev:mock`
- `npm run dev:mock:saas`

这条路径最适合真正理解当前仓库承载了什么。

## 3. 更偏工具使用：CLI

如果你关心的是命令行里的 agent 体验，而不是 GUI，应该直接看：

- 官方文档：`docs.openhands.dev/openhands/usage/run-openhands/cli-mode`
- 外部源码仓：`OpenHands-CLI`

## 4. 更偏平台接入：SDK

如果你关心的是“怎么把 OpenHands 作为能力接到我的系统里”，应该直接看：

- 官方文档：`docs.openhands.dev/sdk`
- 外部源码仓：`OpenHands/software-agent-sdk`

## 现有架构文档和使用指南看哪里

## 当前仓库内部

最值得优先读的入口有：

- `README.md`
- `Development.md`
- `frontend/README.md`
- `openhands/app_server/README.md`
- `openhands/app_server/sandbox/README.md`
- `openhands/app_server/app_conversation/README.md`
- `openhands/app_server/event/README.md`
- `skills/README.md`
- `enterprise/doc/architecture/README.md`
- `enterprise/doc/architecture/authentication.md`
- `enterprise/doc/architecture/external-integrations.md`

## 官方在线文档

除了仓库内资料，还应该配合官方文档一起看：

- `overview/introduction`
- `usage/key-features`
- `usage/run-openhands/local-setup`
- `usage/run-openhands/cli-mode`
- `usage/architecture/backend`
- `sdk`

## 外部相关仓库

- `OpenHands/software-agent-sdk`
- `OpenHands/OpenHands-CLI`
- `OpenHands/benchmarks`

## 最容易出现的误解

### 误解 1：这个主仓就是全部 OpenHands

不是。它是最重要的应用层主仓，但不是全部底层能力的唯一载体。

### 误解 2：OpenHands 就是 benchmark harness

不是。benchmark harness 在外部 `OpenHands/benchmarks` 仓库里。

### 误解 3：OpenHands 只是前端套壳调模型

不是。conversation、sandbox、event、settings、user、integrations 这些层说明它是完整工作区系统。

### 误解 4：skills 和 microagents 是两套完全无关系统

不是。`skills/README.md` 已明确说明它们是 V0 / V1 术语演化中的同一条能力组织脉络。

## 如果你是第一次接触，建议怎么继续读

推荐路线：

1. 先看这篇总览，建立“产品入口 + 主仓边界”的大图
2. 再看 `frontend/README.md` 和 `openhands/app_server/README.md`
3. 然后看 `sandbox/`、`app_conversation/`、`event/` 这三个模块 README
4. 如果你更关注底层 agent 能力，再跳到 SDK 文档
5. 如果你更关注评测，再跳到 `OpenHands/benchmarks`

## 这些物料该怎么放置

对长期保留的说明材料，当前最合理的落点仍然是：

- `docs/architecture/`

不建议放到：

- `.pr/`，因为那是临时 PR 物料
- `frontend/public/`，因为那更像运行时静态资源
- `enterprise/doc/architecture/`，因为这里讲的不只是 Enterprise
