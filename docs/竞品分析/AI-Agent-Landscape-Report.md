# AI 编程 / Agent 产品全景蓝图与深度剖析（2026）

> **生成时间**: 2026-05-18
> **报告范围**: 全球主流 + 国内主流 + 中型创新团队 + 独立开发者/微团队明星项目 + 个人创业者级工具
> **面向场景**: FNOS / AgentsOS 项目 —— 自建中文模型推理集群 (DeepSeek / MiniMax / GLM / Qwen / Kimi) 的中小企业二次开发选型决策

---

## 一、全景蓝图：6 层架构 × 4 个梯队

要看懂"这些产品到底是什么"，必须用 **二维坐标**：
- **纵向 6 层**：从模型权重到 IDE 集成，每一层解决不同问题
- **横向 4 梯队**：从大厂到个人开发者，每一层都有自己的玩家

```
                   T1 大厂主流         T2 中型创新       T3 独立开发者明星      T4 个人副业 / Skill 经济
L1 模型即 Agent     Claude 4.7         Kimi K2.6        DeepSeek V4           —
L2 框架            LangGraph/AutoGen  CrewAI            MetaGPT/OpenManus     CrewAI 模板出售
L3 编码 Harness    Claude Code/Codex  Aider/OpenHands   DeepSeek-TUI/Suna     OpenCode skill 包
L4 IDE             Cursor/Windsurf/   Cline/Roo/        —                    —
                   Trae/通义灵码     Continue
L5 网关/Hub        —                  Devin             OpenClaw/Hermes       ClawHub skill 卖家
L6 配置包          官方 Marketplace   Awesome 系列      everything-cc/awesome —
                                                       openclaw
```

---

## 二、六层架构详解

### L1 · 模型即 Agent —— 把编排吃进权重

**核心思想**：把"多 Agent 编排 / 子任务委派 / 长程规划"直接训进模型权重，省掉外部框架的"协调税"。

#### T1 梯队（一线大厂）
- **Claude 4.7 Opus**（Anthropic）：原生子 Agent 委派 + 1M 上下文 + extended thinking
- **GPT-5.4-Codex / 5.3-Codex-Spark**（OpenAI）：Spark 跑在 Cerebras 上，15x 快于早期 Codex
- **Gemini 3.1 Pro thinking-high**（Google）

#### T2 梯队（中型创新团队）
- **Kimi K2.6 Swarm**（Moonshot）：1T MoE / 32B 激活，PARL 训练让模型自己 spawn 子 Agent，**300 子 Agent / 4000 步**长程任务，SWE-Bench Pro 58.6 超过 GPT-5.4
- **GLM-4.6**（智谱）：开源，SWE-Bench Verified 78%
- **Qwen3 Coder 480B**（阿里）：开源 MoE，SWE-Bench Verified 65%（OpenHands 实测）

#### T3 梯队（开源/社区）
- **DeepSeek V4**：1M 上下文 + prefix cache + 原生 swarm dispatch，**最强中文开源编码模型**
- **Yi-Coder**（零一万物）
- **CodeLlama / DeepSeek-Coder-V2** 系列

**为什么这样设计**：Moonshot 用 PARL 解决了三个传统编排框架的痛点 —— 训练不稳定、credit assignment 模糊、serial collapse（orchestrator 退化成只跑一个 agent）。**编排能力下沉到权重的趋势已不可逆**。

**局限**：黑盒、不可精细控制路由、串行任务有 10–25% swarm 调度损耗、闭源占多数（K2.6 是少数开源例外）。

---

### L2 · Agent 框架 —— 编排原语之争

| 框架 | 抽象 | 哲学 | 状态 | 梯队 |
|---|---|---|---|---|
| **LangGraph** | 有向图+条件边+checkpoint | 显式状态机、时间旅行 | v0.4，生产首选 | T1 |
| **CrewAI** | Role+Task+Crew | 20 行起步、角色扮演 | 原型最快 | T2 |
| **AutoGen** | Agent 对话+GroupChat | 维护模式，已被 MS Agent Framework 接力 | 衰退 | T1 |
| **OpenAI Agents SDK**（前 Swarm） | Handoff 显式控制权移交 | 极简、贴 OpenAI API | 产品化 | T1 |
| **Microsoft Agent Framework** | 同上 | 整合 AutoGen + Semantic Kernel | 2026 主推 | T1 |
| **Smolagents** | "写代码即工具调用" | HF 生态、研究友好 | T2 |
| **MetaGPT** | SOP 标准化软件公司 | 模拟 PM/架构师/工程师 | 偏 demo | T3 |
| **OpenManus** | Planner-Executor + sandbox | MetaGPT 团队 3 小时复刻 Manus | 高曝光 | T3 |
| **AgentScope**（阿里）/ **ModelScope-Agent** | 国内可视化编排 | 私有部署友好 | T1（国内） |
| **Dify / FastGPT** | 低代码 + RAG + Agent | 中小企业生产可用 | T2（国内） |
| **Pydantic AI / Mastra** | 类型安全 / TS 优先 | 工程师友好 | T2 |

**为什么演化成这样**：
- LangGraph 出自 LangChain 反思：原 chain 不可循环、不可持久化
- CrewAI 出自业务流程模型论：把 Agent 当员工而非函数
- OpenAI Swarm 来自 cookbook，本质是 SDK 站台
- OpenManus 出自 MetaGPT 团队 —— Manus 火爆 3 小时后开源复刻，**这是 2026 年微团队最经典的"快速跟进"案例**

**统一局限**：除 LangGraph 外，可观测性、错误重放、成本归因都要自己补。

---

### L3 · 编码 Agent Harness —— 给 LLM 一个"身体"

**统一内核**：所有 Harness 都是同一个 while 循环 → `model_call → tool_use → tool_result → compact_context`。**差异全在循环周围的"系统"。**

#### T1 梯队（大厂闭源 / 半闭源）
| Harness | 沙箱 | 上下文 | 扩展点 | License |
|---|---|---|---|---|
| **Claude Code** | shell sandbox | 5 层压缩+懒加载 CLAUDE.md+子 Agent 摘要 | MCP/Plugin/Skill/Hook 四种 | 闭源 |
| **OpenAI Codex CLI** | macOS Seatbelt / Linux bubblewrap / Win 受限令牌 | App Server 跨客户端统一 | MCP | Apache-2.0 |
| **Codex Desktop**（2026.2） | 同上 | 跨设备会话同步 | 同 CLI | 闭源 |
| **Gemini CLI / Qwen Code / Kimi Code / CodeBuddy CLI** | 各自 | Claude Code 风格 fork | 类似 | 多数开源 |

#### T2 梯队（中型创新团队 / OSS 主流）
| Harness | 团队 | 特色 | License |
|---|---|---|---|
| **Aider** | Paul Gauthier 个人项目 | git diff 接口、repo map、终端 | Apache-2.0 |
| **OpenHands**（前 OpenDevin） | All Hands AI ($18.8M A 轮) | Docker 沙箱+CodeAct+SWE-Bench 77% | MIT |
| **SWE-agent** | Princeton/Stanford | ACI 抽象、研究取向 | MIT |
| **Goose** | Block (Square) | MCP-first | Apache-2.0 |
| **OpenCode** | sst.dev 团队 | 多模型 TUI | MIT |
| **Devin** | Cognition AI | 闭源云 VM、首个商用自主编码 Agent | $$$ SaaS |

#### T3 梯队（独立开发者 / 微团队明星）
| Harness | 作者 / 团队 | 高曝光事件 | 技术亮点 |
|---|---|---|---|
| **DeepSeek-TUI** | Hunter Bown (Hmbown) | DeepSeek V4 官方推荐链接 | Rust 13-crate workspace + Seatbelt/Landlock 沙箱 + Plan/Agent/YOLO 模式 |
| **AutoResearch** | Andrej Karpathy | 一周 32.6k stars | 630 行 Python，1 块 GPU 跑 700 次实验 |
| **MiroFish** | 北邮大四学生 | 10 天 25k stars，陈天桥 3000 万投资 | 用 Claude Code 搭群体智能预测引擎 |
| **OpenManus** | MetaGPT 团队 | Manus 火爆 3 小时后开源 | Planner-Executor 复刻 |
| **Suna** | Kortix AI | "Manus 倒拼"，Apache 2.0 | 自托管+OpenCode runtime+60 skills+3000 集成 |

#### T4 梯队（个人副业 / Skill 经济）
- **Skill 卖家**：在 ClawHub / Anthropic Marketplace 卖单个 Skill，单卖 $10–200，头部月入 $1000+
- **预设 Agent 包卖家**：把 prompt+hook+subagent 打包成 npm package 收费

**为什么演化成今天**：
1. 2024 Aider 证明 git-diff 是给 LLM 最好的接口
2. 2024 末 OpenDevin 证明 Docker 沙箱 = 自主性
3. 2025 Claude Code 用四扩展机制 + 子 Agent 立住"可编程平台"标杆，被全行业抄作业
4. 2025–2026 一票产品全是 Claude Code 形态克隆，差别仅在绑定哪家模型 + 用什么语言
5. 2026 Codex Desktop 标志"超级 App 收口"趋势

---

### L4 · IDE 集成 —— 重资产 Fork vs 轻资产插件

#### A. VS Code Fork 派（重资产）
**T1**:
- **Cursor** —— 云优先 + 多模型路由，订阅 $20/月，主流付费个人开发者首选
- **Windsurf** —— 已被 Cognition (Devin 团队) 收购；SWE-1.5 模型 950 tok/s（13x Sonnet 4.5）；Codemaps 可视化代码地图；HIPAA/FedRAMP/SOC2 合规

**T2（国内主导）**:
- **Trae**（字节）—— 41.2% 国内市占；SOLO 模式从需求到部署一条龙；600 万+ 注册；中文指令准确率比 Cursor 高 18%
- **Comate AI IDE**（百度）—— 设计稿一键转代码 + 多智能体协同
- **MarsCode**（字节）—— Trae 前身，完全免费

#### B. VS Code 插件派（轻资产）
**T1**:
- **GitHub Copilot** —— 老牌，生态最深
- **Augment** —— 企业代码库专门优化
- **通义灵码**（阿里）—— 18.5% 国内市占
- **文心快码**（百度）—— 12.3% 国内市占
- **CodeGeeX**（智谱）—— 唯一完全免费开源
- **Cody**（Sourcegraph）

**T2（OSS 明星，最适合二次开发）**:
- **Cline** —— MIT、1.5M VS Code 安装、Plan/Act 双模式、明确 BYOM
- **Roo Code** —— Cline fork、加入多模式 + 自定义模式
- **Continue.dev** —— 跨 IDE（VS Code+JetBrains）、Hub 配置中心
- **Tabby** —— 自托管全栈方案

**国内格局（2026 Q1 市占）**: Trae 41.2% > 通义灵码 18.5% > 文心快码 12.3% > MarsCode > CodeGeeX > CodeBuddy。

**国产 vs 国际**：国产中文指令理解显著领先，但 Agent 自主性仍落后 Claude Code/Codex 一档。Trae SOLO 在追赶。

---

### L5 · 个人助理网关 / 跨 Harness 编排

**这是 2025 末才浮现的物种**，最容易被误解 —— 它不是编码 Agent，而是 **"调度 + 接入 + 持久化"** 的中间层。

#### T2 梯队（中型团队）
- **Devin**（Cognition）—— 云 VM、自主 Slack 协作，闭源 SaaS

#### T3 梯队（独立开发者明星 —— 2026 现象级）
| 项目 | 作者 | 关键数据 | 定位 |
|---|---|---|---|
| **OpenClaw**（前 Clawdbot） | Peter Steinberger (PSPDFKit 创始人) | **60 天 25 万 stars**（GitHub 最快增长非聚合器项目，超过 React 用时） | 跨消息平台网关 + 心跳调度 + Skill 系统 |
| **Hermes Agent** | NousResearch | 15.3 万 stars | 持久记忆 + 自动技能生成 + 用户建模 + Atropos RL 训练数据飞轮 |
| **OpenHuman** | tinyhumansai 集体 | 2026.5 GitHub Trending #1，776 stars 起步 | "Agent 先读懂你，再开始干" 倒置范式 |

**OpenClaw 现象学**：
- Steinberger 2025.11 静默上线 → 2026.1.25 在 HN/PH 发布"Show HN: Clawdbot, installs in one command" → 9000→60000 stars 几天爆发 → 60 天 24.7 万 stars → NVIDIA 收编 + OpenAI 招聘作者 → 5 个月后 35.5 万 stars + 320 万用户 + 50 万运行实例
- 衍生 180 家创业公司月营收合计 $320K+，单个 Skill 卖家头部月入 $1000+
- **2026 微团队成功剧本的样板**

#### 局限（普遍）
- 安全/合规弱（OpenClaw 已发生用户 secret 泄露事故）
- 多用户/团队权限模型几乎为零
- 当作 "L5 网关 + L3 Harness 后端" 组合时整栈可观测性差

---

### L6 · 配置/插件包 —— 依附 Harness 生存

不是产品，是 **"提示词 + Hook + Skill" 的最佳实践包**。

| 项目 | 类型 | 适用 Harness |
|---|---|---|
| **everything-claude-code**（affaan-m） | 18.5 万 stars + 28.6k forks，AgentShield 102 安全规则 | Claude Code/Codex/OpenCode/Cursor |
| **awesome-claude-code** | 索引 | Claude Code |
| **awesome-openclaw**（SamurAIGPT） | 索引 | OpenClaw |
| **awesome-ai-agents-2026** | 300+ Agent 资源索引 | 通用 |

**T4 梯队（Skill 经济）**：
- Anthropic 官方 Plugin Marketplace 通过 `/plugin marketplace add` 一键装
- 中小开发者把企业最佳实践打包卖钱
- 头部 Skill 卖家月入 $1000+，单 Skill 售价 $10–200

---

## 三、独立开发者 / 微团队 / 个人创业者 —— 2026 现象级案例

### 微团队成功剧本（T3 通用模式）

```
1. 单人/小团队周末项目，先不宣传 (Peter Steinberger 11 月静默上线 OpenClaw)
2. 在 Hacker News / Product Hunt 用"installs in one command"语气低调发布
3. 选对时机 (DeepSeek/Manus 等同期话题事件加持)
4. 24-48 小时内冲上 GitHub Trending
5. 60 天达到 6 位数 stars
6. 大厂主动接洽 (NVIDIA 把它做成企业产品 / OpenAI 直接招人)
7. 衍生生态 (Skill 卖家、监控工具、安全层、教育内容、咨询)
```

### 案例库

| 项目 | 创始人 | 数据 | 关键洞察 |
|---|---|---|---|
| **OpenClaw** | Peter Steinberger（奥地利 PSPDFKit 创始人） | 5 月 35.5 万 stars / 320 万用户 / 50 万实例 / 180 衍生公司 / 3 亿+ 美元 GMV | "Installs in one command" 是入场券；模块化 Skill 系统是火箭燃料 |
| **AutoResearch** | Andrej Karpathy | 1 周 32.6k stars | 个人科学家级影响力 + 极简代码（630 行） |
| **MiroFish** | 北邮大四学生 | 10 天 25k stars + 陈天桥 3000 万投资 | Claude Code 让学生独自完成原本需要团队的工程 |
| **DeepSeek-TUI** | Hunter Bown | DeepSeek 官方推荐 | 在 DeepSeek 火爆窗口期做 Rust TUI = 站对风口 |
| **OpenManus** | MetaGPT 团队 5 人 | Manus 火爆 3 小时后开源 | 学术团队反应速度 = 商业模仿者的 10x |
| **Suna** | Kortix AI | 商业化 + 开源双线 | "Manus 反拼"做品牌锚定，Apache 2.0 做信任 |
| **Lovable** | Anton Osika 等（瑞典 4 人团队） | 12 个月达 $50M ARR | Vibe coding 让"非开发者建 SaaS"成立 |
| **Replit Agent** | Replit（小型公司放大） | Agent 4 (2026.3) | 把 IDE 体验整合到 Agent，Pro 用户用最强模型 |
| **Manus / Butterfly Effect** | 肖弘（武汉→北京→新加坡） | Meta $20 亿收购被中国发改委 4 月叫停 | 国内首个商用 Agent 出海标杆 |
| **秘塔 AI** | 秘塔科技 | 法律 + AI 搜索代表 | 垂直行业 Agent 范式 |

### Vibe Coding 工作流（个人创业者标配）

```
原型 (Lovable/Bolt/v0) → 突破天花板 (Cursor + Claude Code) → 部署 (Vercel/Replit) → 监控 (Sentry/OpenClaw)
                ↓
        非技术创始人也能跑通
```

- **Lovable**：12 个月 $50M ARR，一非开发者用它做的虚拟试穿 9 个月跑到 $80 万 ARR
- **Bolt.new**：浏览器内最快出 prototype（28 分钟）
- **v0.dev**（Vercel）：UI 质量最高（9/10），但仅前端
- **Replit Agent 4**：SOC 2 Type II，唯一带合规的 vibe coding 平台
- **Cursor + Claude Code**：原型生产化的标配

### Indie Hacker 经济结构（2026）

- 生产级 SaaS 月成本 $85–$200（2019 年同等需要 $5000+）
- 单人 AI 代理公司报告月入 $10K–$30K，服务 20+ 客户，AI 自动化 80%
- "百万美元单人公司" 从预测变为可观测事实
- 70% 重复人力可被 AI 替代（Kortix 视角）

### 中文圈独立开发者圈层（必关注）

| 博主/项目 | 平台 | 方向 |
|---|---|---|
| **idoubi** (@idoubicc) | X / 公众号 | 前工程师转独立开发，build in public |
| **向阳乔木** (@vista8) | X / 公众号 | AI 产品工作流、Vibe Coding、趋势判断 |
| **Tom Huang** (@tuturetom) | X / Twitter | 开源 Agent 产品与创业工具链 |
| **MiroFish** (@miorfish_team) | GitHub | 群体智能预测引擎 |
| **Aperant** | GitHub | Agent 自治型应用，12 个 agent terminal 并行 |
| **Dify / FastGPT** 创始团队 | GitHub | 国内最成功的 Agent SaaS |

---

## 四、横向对比表

### 4.1 SWE-Bench Verified 性能对比

| 模型/Harness | SWE-Bench Verified | 备注 |
|---|---:|---|
| Kimi K2.6 | 80.2 | 开源、256K 上下文 |
| GPT-5.4 (xhigh) | 80 左右 | 闭源 |
| Claude Opus 4.6 | 76 左右 | 闭源 |
| OpenHands + Claude 4.5 | **77** | 开源 Harness + 闭源模型 |
| OpenHands + Qwen3 Coder 480B | 65 | 全开源栈 |
| Gemini 3.1 Pro thinking-high | 75 左右 | 闭源 |
| Manus AI | (GAIA L1 86.5) | 不可直接对比 |

### 4.2 Vibe Coding 平台（个人 MVP 速度）

| 平台 | 出原型时间 | 代码质量 | 全栈 | 合规 |
|---|---|---|---|---|
| Bolt.new | 28 min | 6/10 | ✅ JS only | ❌ |
| Lovable | 35 min | 7/10 | ✅ Supabase | ❌ |
| Replit Agent | 45 min | 7/10 | ✅ 多语言 | ✅ SOC 2 |
| Windsurf | 65 min | 8.5/10 | ✅ | ✅ HIPAA/FedRAMP |
| v0.dev | — | 9/10 | ❌ UI only | ❌ |

### 4.3 OSS Harness 二次开发友好度

| 项目 | License | 代码可读性 | OpenAI 兼容 | Docker 沙箱 | 适合自建集群 |
|---|---|---|---|---|---|
| OpenHands | MIT | ★★★★ | ✅ | ✅ | ★★★★★ |
| Cline / Roo Code | MIT | ★★★★ | ✅ | ❌ (IDE 内) | ★★★★★ |
| Continue.dev | Apache | ★★★★ | ✅ | ❌ | ★★★★ |
| Aider | Apache | ★★★★★ | ✅ | ❌ | ★★★★ |
| DeepSeek-TUI | MIT | ★★★ (Rust) | ✅ | ✅ | ★★★★ |
| Suna / Kortix | Apache | ★★★ | ✅ | ✅ | ★★★ |
| OpenManus | MIT | ★★★ | ✅ | ✅ | ★★★ |
| Hermes Agent | MIT | ★★★ | ✅ | 7 种 backend | ★★★ |
| OpenClaw | MIT | ★★★ | ✅ | ❌ | ★★ (个人向) |

---

## 五、面向 FNOS / AgentsOS 的二次开发选型建议

### 5.1 你的核心约束

- **OpenAI-Compatible API** 是底线
- **私有部署 / 模型不出墙**
- **可改源码**（License 友好）
- **接入 DeepSeek V4 / MiniMax M2 / GLM-4.6 / Qwen3 Coder / Kimi K2.6 等**

### 5.2 强推荐栈（一线选择）

```
[模型层] DeepSeek V4 主力 + Qwen3 Coder / GLM-4.6 备份 + MiniMax 长上下文备选
[编排层] LangGraph (生产关键链路) + 直接 OpenAI SDK (简单链路)  
[Harness] OpenHands (自建版) + Cline/Roo Code (给工程师 IDE 选项)
[网关层] (按需) 自研 LiteLLM 网关统一协议
[配置层] 抄 everything-claude-code 的 Hook/Skill 结构，造自家合规 Skill 包
```

### 5.3 决策矩阵

| 场景 | 强推 | 次推 | 不推 |
|---|---|---|---|
| 内部 Devin（5–30 人技术团队） | **OpenHands + V1 SDK** | DeepSeek-TUI / Suna | Devin (闭源 SaaS) |
| VS Code 团队铺开（10–50 人） | **Cline + LiteLLM** | Roo Code / Continue | Cursor (闭源) |
| 跨 IDE + 企业合规 | **Continue.dev + Hub** | Tabby (自托管全栈) | Trae (字节绑定) |
| 老派工程师 CLI 派 | **Aider + DeepSeek V4** | OpenCode | Claude Code (协议非公开) |
| 多 Agent 业务流（客服/HR/研报） | **LangGraph + 自家执行层** | CrewAI (原型) | AutoGen (维护模式) |
| 想做"Manus 自托管版" | **Suna** | OpenManus | Manus (闭源+收购阻断) |
| 极客 / 单人 7x24 助手 | OpenClaw / Hermes Agent | DeepSeek-TUI | (不适合企业生产) |

### 5.4 关键决策原则

1. **L1 选 1–2 个主力模型**（DeepSeek V4 + 备份），不要把所有鸡蛋放一个篮子
2. **L3 主力 Harness 选 OpenHands**：MIT、生产级、SWE-Bench 验证、V1 SDK 模块化
3. **L4 给工程师选项**：Cline 或 Roo Code 让团队按习惯选
4. **L2 别上 AutoGen**：已进入维护，主推 LangGraph
5. **L6 借鉴 everything-claude-code 结构**：把企业合规规则做成自家 Skill 包
6. **避免双重编排**：跑 K2.6 swarm 时关掉外层 LangGraph，普通任务走外层
7. **MCP 作为统一工具协议**：所有自研工具优先 MCP server，保证 Harness 切换时工具复用

### 5.5 谨慎选用

- **Claude Code + LiteLLM 反代**：技术可行（DeepSeek-TUI 证明），但 Claude Code 协议非公开、Anthropic 可能反制、长期维护风险
- **Codex CLI**：Apache-2.0 但 App Server 强绑定 OpenAI
- **Cursor/Windsurf/Trae**：能配自定义 endpoint 但二次开发空间几乎没有
- **Kimi Swarm**：能力强但只能用 K2.6 本身，编排不可拆解

### 5.6 不推荐

- **Devin**：闭源 SaaS，不可能私有化
- **OpenClaw / Hermes Agent** 做企业生产：个人助理形态，多用户合规未达标

---

## 六、趋势研判（2026–2027）

1. **编排下沉到模型权重**：K2.6 PARL 是先行者，预计 2027 多家跟进。中型 Agent 框架（CrewAI/AutoGen）会被进一步压缩
2. **超级 App 收口**：OpenAI 已宣布 ChatGPT + Codex + Atlas 合并成单一桌面端，Anthropic 可能跟进
3. **MCP 成事实标准**：所有 Harness 都说支持，但兼容性仍在磨合 —— 2026 下半年应趋稳
4. **国产工具 Agent 自主性追平**：Trae SOLO + 通义灵码 + 文心快码多智能体矩阵都在追赶 Claude Code
5. **L5 网关 / 个人助手成为新风口**：OpenClaw 是开端，预计 2026 出现 5+ 同形态产品
6. **Vibe Coding 平台分化**：Lovable / Bolt / v0 / Replit 已开始分场景占位，下一步是垂直化（行业模板）
7. **独立开发者 / 微团队成为重要力量**：MiroFish 这种"学生 10 天 25k stars"案例会变多，AI Coding 工具消除团队规模壁垒

---

## 主要参考来源

### 核心架构 / 平台
- [Inside Claude Code Architecture (penligent.ai)](https://www.penligent.ai/hackinglabs/inside-claude-code-the-architecture-behind-tools-memory-hooks-and-mcp/)
- [OpenAI Codex CLI & Sandbox docs](https://developers.openai.com/codex/concepts/sandboxing)
- [Cursor vs Windsurf vs Claude Code 2026 (dev.to)](https://dev.to/pockit_tools/cursor-vs-windsurf-vs-claude-code-in-2026-the-honest-comparison-after-using-all-three-3gof)
- [Trae vs Cursor vs Windsurf 2026 (zoer.ai)](https://zoer.ai/posts/zoer/trae-cursor-windsurf-comparison-2026)

### L1 模型 / L2 框架
- [Kimi K2.6 Agent Swarm (MarkTechPost)](https://www.marktechpost.com/2026/04/20/moonshot-ai-releases-kimi-k2-6-with-long-horizon-coding-agent-swarm-scaling-to-300-sub-agents-and-4000-coordinated-steps/)
- [LangGraph vs AutoGen vs CrewAI vs Swarm 2026 (BSWEN)](https://docs.bswen.com/blog/2026-04-29-agent-framework-production-comparison/)

### L3 编码 Harness
- [OpenHands V1 / Index Benchmark](https://www.openhands.dev/blog/openhands-index)
- [Devin vs OpenHands vs SWE-agent 2026 (ToolHalla)](https://toolhalla.ai/blog/devin-vs-openhands-vs-swe-agent-2026)
- [DeepSeek-TUI by Hmbown](https://github.com/Hmbown/DeepSeek-TUI)

### L4 IDE / L5 网关 / L6 配置包
- [国产 5 大 AI 编程工具横评 2026](https://blog.csdn.net/csdngouwei/article/details/160726130)
- [OpenClaw 2026 Timeline](https://inbounter.com/blog/openclaw-2026-timeline)
- [Hermes Agent (NousResearch)](https://github.com/nousresearch/hermes-agent)
- [everything-claude-code by affaan-m](https://github.com/affaan-m/everything-claude-code)

### 独立开发者 / 微团队 / Vibe Coding
- [Manus AI Review 2026 (MIT Technology Review)](https://www.technologyreview.com/2025/03/11/1113133/manus-ai-review/)
- [OpenManus (FoundationAgents)](https://github.com/FoundationAgents/OpenManus)
- [Suna by Kortix AI](https://github.com/kortix-ai/suna)
- [Best Vibe Coding Tools 2026 (Lovable)](https://lovable.dev/guides/best-vibe-coding-tools-2026-build-apps-chatting)
- [Bolt vs Replit vs Lovable 2026](https://lovable.dev/guides/bolt-vs-replit-vs-lovable)
- [Vibe Coding 完全指南 2026 (ofox.ai)](https://ofox.ai/zh/blog/vibe-coding-ai-workflow-guide-2026/)
- [2026 Agent 生态爆发 5 个项目 (知乎)](https://zhuanlan.zhihu.com/p/2020798560287897282)
- [awesome-ai-agents-2026 (caramaschiHG)](https://github.com/caramaschiHG/awesome-ai-agents-2026)

---

*报告完。下一步可考虑：（1）针对 OpenHands + 自建 DeepSeek 集群的完整私有化部署蓝图；（2）OpenAI-Compatible 适配层让集群同时接 Cline / Aider / OpenHands / DeepSeek-TUI；（3）"从 issue 到 PR" 端到端对比 Trae SOLO vs OpenHands vs Devin。*
