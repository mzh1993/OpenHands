# OpenHands 商业竞品深度分析（2026）

> 更新时间：2026-05-18
> 研究方法：3 个 subagents 并行调研，主 agent 审计收敛
> 研究目标：回答这些产品到底卖什么、卖给谁、卖得怎么样、哪些最值得 OpenHands 模仿

---

## 先讲结论

这轮调研最重要的结论，不是“哪家模型最强”，而是：

1. **真正卖出去的不是 AI 会写代码，而是完整购买包。**
   这个购买包通常包含：`清晰定价 + 团队版 + 企业版 + SSO/审计/VPC + 客户案例 + 可验证 ROI`。
2. **OpenHands 当前最需要补的，不是技术想象力，而是产品包装和商业证明。**
   公开世界里，很多强样本并不靠 benchmark 取胜，而是靠“更容易被采购、更容易被治理、更容易被证明值回票价”取胜。
3. **直接同层里，最值得紧盯的是 `Devin / Cursor / Windsurf`。**
   它们分别代表三种路径：自治软件工程师、AI 原生 IDE、IDE 前台 + 云端 agent 协同。
4. **最值得 OpenHands 模仿的，不一定是最火的，而是最会卖的。**
   这一组主要是 `Augment / Cline / Dify / Trae / 通义灵码 / 文心快码`。
5. **相邻层爆款已经证明：开发工具的买单者不只是一线工程师。**
   `Replit Agent` 和 `Lovable` 证明了“产品、运营、支持、创新团队也会买 AI 开发能力”，这对 OpenHands 很关键。

---

## 一、样本为什么这么选

这份报告不追求“列全行业名单”，而是只保留对 OpenHands 路线有决策价值的样本。

### 正式样本（10）

#### 同层 / 强替代

- `Devin`
- `Cursor`
- `Windsurf`
- `Augment`
- `Cline`

#### 相邻层、但已证明能卖

- `Replit Agent`
- `Dify`
- `Lovable`

#### 中国市场强样本

- `Trae`
- `通义灵码`

### 候补观察样本

- `文心快码 / Comate AI IDE`
- `Aider`
- `Continue`
- `Manus`
- `Bolt`
- `CodeGeeX`
- `MarsCode`

### 过滤逻辑

- 必须能拿到公开的定价、企业能力或商业证明。
- 必须与 OpenHands 存在明确关系：`直接竞争`、`相邻升级`、`可模仿参考` 至少其一。
- 同类过于重叠的样本不多放，避免把报告写成“IDE 壳子大乱斗”。

---

## 二、市场已经验证了什么

### 1. “AI IDE / AI 软件工程师”已经不是单点工具，而是席位生意

`Cursor` 公开价是 `Pro $20/月`、`Teams $40/用户/月`，企业版强调 `SAML/OIDC SSO`、`SCIM`、`audit logs`、`granular admin controls`。  
来源：[Cursor Pricing](https://cursor.com/pricing)

`Windsurf` 公开价是 `Pro $20/月`、`Max $200/月`、`Teams $40/用户/月`，企业侧也强调 admin、policies 和集中计费。  
来源：[Windsurf Pricing](https://windsurf.com/pricing)

`Devin` 已经不是概念演示，官方公开了 `Free / Pro / Max / Teams` 分层。  
来源：[Devin Pricing](https://devin.ai/pricing)

**这说明开发类 agent 已经完成了最关键的一步：不是实验，而是可以按 seat 卖。**

### 2. 企业客户不是只买“模型能力”，而是买“可控性”

这类可控性反复出现在成功样本里：

- `SSO / SCIM`
- `RBAC`
- `Audit logs`
- `Private network / VPC`
- `Region selection`
- `Marketplace procurement`
- `BYOK / BYO inference`

`Replit`、`Dify`、`Trae`、`通义灵码`、`文心快码` 都把这些东西直接写在定价页或企业版页面里。  
来源：[Replit Pricing](https://replit.com/pricing) | [Dify Enterprise](https://dify.ai/enterprise) | [Trae Enterprise](https://www.trae.cn/enterprise) | [Lingma Pricing](https://lingma.aliyun.com/pricing) | [Comate Pricing](https://comate.baidu.com/docs/%E4%BA%A7%E5%93%81%E5%AE%9A%E4%BB%B7/%E4%BA%A7%E5%93%81%E5%AE%9A%E4%BB%B7.html)

**这说明企业采购 AI 编程产品时，已经默认把它当“生产系统”看，而不是当玩具看。**

### 3. 最火的相邻层产品，证明“买单人群”比工程团队更广

`Replit Agent` 的公开客户案例里，不只是工程团队，也包括企业内部工具、跨职能使用场景。  
来源：[Replit Customers](https://replit.com/customers)

`Lovable` 把自己卖成“任何人都能做 app”，并公开披露高增长、项目量和企业客户背书。  
来源：[Lovable Pricing](https://lovable.dev/pricing) | [Lovable Series B](https://lovable.dev/pt-br/blog/series-b)

**这意味着 OpenHands 如果只把自己定义成“给最强工程师的高级代理”，市场可能会被自己做窄。**

---

## 三、重点竞品深拆

以下不是“产品介绍”，而是围绕 4 个问题拆：

1. 它到底卖什么
2. 它卖给谁
3. 它卖得怎么样
4. OpenHands 应该从它身上学什么

### 1. Devin

**它卖什么**  
Devin 卖的是“云端自治软件工程师”，不是简单 IDE 插件。它把自主规划、终端执行、代码提交、代码审查、知识管理和团队协作打成一包。  
来源：[Cognition / Devin](https://cognition.ai/) | [Devin Pricing](https://devin.ai/pricing)

**它卖给谁**  
卖给愿意为“一个更像员工而不是助手的 agent”付钱的团队，尤其是把 bugfix、PR、review、内部工具工作流交给 agent 的组织。

**卖得怎么样**  
- 官方定价已公开：`Free / Pro / Max / Teams`。  
  来源：[Devin Pricing](https://devin.ai/pricing)
- 媒体报道 Cognition 于 `2025-09-08` 以约 `$10.2B` 估值完成 `$400M` 融资。  
  来源：[CNBC](https://www.cnbc.com/2025/09/08/cognition-valued-at-10point2-billion-two-months-after-windsurf-.html?msockid=3e17488da18d6fa81e495eefa0646ef8)

**OpenHands 该学什么**  
- 先把“自治软件工程师”定义清楚，再卖 seat
- 让用户知道 agent 可以接手哪些可重复工作
- 不只卖 chat，要卖 review、subtasks、knowledge、team workflows

**不该盲抄什么**  
- 高估值和高热度不是能力本身
- 如果没有稳定的企业控制与 ROI 证据，只学“像 Devin 的营销语言”没有意义

### 2. Cursor

**它卖什么**  
Cursor 卖的是 AI 原生 IDE，不只是补全，而是把 `Agent / Cloud agents / Bugbot / Rules / CLI / Enterprise control` 打包成开发主工作台。  
来源：[Cursor Pricing](https://cursor.com/pricing) | [Cursor Enterprise](https://cursor.com/en-US/enterprise)

**它卖给谁**  
个人开发者、创业团队、中大型企业研发组织都可以买；它的路径非常标准：`免费试用 -> Pro -> Teams -> Enterprise`。

**卖得怎么样**  
- 官方价格明确：`Pro $20/月`、`Teams $40/用户/月`。  
  来源：[Cursor Pricing](https://cursor.com/pricing)
- 官方企业页声称 `50,000+ businesses`、`64% of Fortune 500` 在用。  
  来源：[Cursor Enterprise](https://cursor.com/en-US/enterprise)

**OpenHands 该学什么**  
- IDE 前台能力不能忽略
- 团队规则、插件市场、集中计费、组织分析都是成单点
- 企业页面要写给采购和安全团队看，而不是只写给开发者看

### 3. Windsurf

**它卖什么**  
Windsurf 卖的是“本地 IDE + 云端 agent”双栈体验，本地 `Cascade` 和云端 agent 由一个控制中心串起来。  
来源：[Windsurf Homepage](https://windsurf.com/) | [Windsurf Pricing](https://windsurf.com/pricing)

**它卖给谁**  
既卖给个人和团队，也卖给想要集中治理与企业 policies 的组织。

**卖得怎么样**  
- 公开价格与 Cursor 接近：`Pro $20/月`、`Teams $40/用户/月`。  
  来源：[Windsurf Pricing](https://windsurf.com/pricing)
- 官方宣称 `1M+` 用户、`4,000+` 企业客户。  
  来源：[Windsurf Homepage](https://windsurf.com/)

**OpenHands 该学什么**  
- “前台 IDE + 后台 agent”可能比纯 Web agent 更容易扩大日常使用时长
- 团队版控制、企业 policies、统一 usage 视图很重要

### 4. Augment

**它卖什么**  
Augment 卖的是“理解复杂代码库的 AI 开发平台”，核心卖点不是炫技，而是 `Context Engine`。  
来源：[Augment Pricing](https://www.augmentcode.com/pricing/) | [Augment Customers](https://www.augmentcode.com/customers)

**它卖给谁**  
主要卖给大代码库、长生命周期、多人协作的软件组织。

**卖得怎么样**  
- 官方公开价格：`Indie $20/月`、`Standard $60/月/开发者`。  
  来源：[Augment Pricing](https://www.augmentcode.com/pricing/)
- TechCrunch 报道其出隐身时累计融资 `$252M`。  
  来源：[TechCrunch](https://techcrunch.com/2024/04/24/eric-schmidt-backed-augment-a-github-copilot-rival-launches-out-of-stealth-with-252m/)
- 官方展示 Pure Storage、MongoDB 等客户与大型工程团队案例。  
  来源：[Augment Customers](https://www.augmentcode.com/customers)

**OpenHands 该学什么**  
- 不要只强调“agent 会动手”，也要强调“agent 真的读懂大型代码库”
- 把 review、context、rules、CLI、enterprise governance 打成一套

### 5. Cline

**它卖什么**  
Cline 卖的是开源、BYOK、可企业治理的 coding agent 形态。  
来源：[Cline Pricing](https://cline.bot/pricing) | [Cline Enterprise](https://docs.cline.bot/enterprise-solutions/overview)

**它卖给谁**  
先卖给开源社区和高级开发者，再卖给需要自带模型、自带推理、自带安全边界的企业。

**卖得怎么样**  
- 个人版免费，企业版走销售。  
  来源：[Cline Pricing](https://cline.bot/pricing)
- 官方企业材料直接摆出大量大客户 logo，并写明 SSO、RBAC、OpenTelemetry、集中管理。  
  来源：[Cline Pricing](https://cline.bot/pricing) | [Enterprise Overview](https://docs.cline.bot/enterprise-solutions/overview)

**OpenHands 该学什么**  
- OSS 分发 + 企业回收这条路仍然非常有效
- “代码不出环境”“可自带推理供应商”是非常强的卖点

### 6. Replit Agent

**它卖什么**  
Replit 卖的不只是 coding agent，而是“从描述需求，到生成应用，到部署、协作、治理”的整个平台。  
来源：[Replit Pricing](https://replit.com/pricing) | [Replit Customers](https://replit.com/customers)

**卖给谁**  
卖给开发者，也卖给非工程职能。它证明 AI 开发预算不止存在于研发团队。

**卖得怎么样**  
- 公开价格：`Core $20/月`、`Teams $35/用户/月`。  
  来源：[Replit Pricing](https://replit.com/pricing)
- 官方宣布 `$250M` 融资，并写明“millions of users”和 `500,000+ professional users`。  
  来源：[Funding Announcement](https://replit.com/news/funding-announcement)
- 有可命名客户与 ROI 案例。  
  来源：[Replit Customers](https://replit.com/customers)

**OpenHands 该学什么**  
- agent 必须接上部署和运行结果
- 非工程预算值得单独设计产品包装

### 7. Dify

**它卖什么**  
Dify 卖的是“AI 工作流与 agent 平台”，不是纯 coding agent。  
来源：[Dify Pricing](https://dify.ai/pricing) | [Dify Enterprise](https://dify.ai/enterprise)

**卖给谁**  
卖给做 AI 平台、中台、业务自动化和企业内部 AI 应用的团队。

**卖得怎么样**  
- 官方价格：`Professional $59/workspace/月`。  
  来源：[Dify Pricing](https://dify.ai/pricing)
- 官方宣布 `Pre-A $30M`，并写“over a million apps running”。  
  来源：[Funding Announcement](https://dify.ai/blog/dify-raises-30m-tomorrow-s-organizations-will-be-built-by-people-and-agents)
- 官方强调 `SOC 2 Type II`、`ISO 27001`、`GDPR`，且上架 Azure Marketplace。  
  来源：[Compliance Post](https://dify.ai/blog/dify-achieves-soc-2-iso-27001-gdpr-compliance-for-the-second-year-running)

**OpenHands 该学什么**  
- 开源不是终点，企业版包装才是收入回收点
- Self-host、LTS、合规、Marketplace 是清晰 B2B 套路

### 8. Lovable

**它卖什么**  
Lovable 卖的是“任何人都能做 app”的产品化承诺，并把生成、托管、连接器、治理接成一条链。  
来源：[Lovable Pricing](https://lovable.dev/pricing) | [Lovable Enterprise](https://lovable.dev/enterprise-landing)

**卖给谁**  
个人创作者、小团队、企业创新团队都能买。

**卖得怎么样**  
- 官方价格：`Pro $25/月`、`Business $50/月`。  
  来源：[Lovable Pricing](https://lovable.dev/pricing)
- 官方宣布 `Series B $330M`、估值 `$6.6B`。  
  来源：[Series B](https://lovable.dev/pt-br/blog/series-b)
- 官方还公开过 `ARR`、项目量、访问量等增长信号。  
  来源：[Agent Post](https://lovable.dev/pt-br/blog/agent)

**OpenHands 该学什么**  
- 把增长和商业证明公开化
- 把产品叙事从“工程师工具”扩展到“软件创建平台”

### 9. Trae

**它卖什么**  
Trae 卖的是中国市场的 AI 原生开发环境，而且已经把 IDE、插件、CLI、企业版打通。  
来源：[Trae Pricing](https://www.trae.cn/pricing) | [Trae Enterprise](https://www.trae.cn/enterprise)

**卖给谁**  
个人开发者、团队、企业客户，尤其是中文研发组织和对价格敏感的中国团队。

**卖得怎么样**  
- 个人免费，企业定价从 `¥49/人/月` 到 `¥199/人/月`。  
  来源：[Trae Pricing](https://www.trae.cn/pricing)
- 官方披露个人版注册用户超 `600 万`，MAU `100 万+`。  
  来源：[Volcano Engine Article 1](https://developer.volcengine.com/articles/7598407398764019721) | [Volcano Engine Article 2](https://developer.volcengine.com/articles/7516755904457343002)

**OpenHands 该学什么**  
- 免费切入非常重要
- 企业版价格可以比国际头部更激进
- 把安全与治理条款直接写进企业页面，降低销售摩擦

### 10. 通义灵码

**它卖什么**  
通义灵码卖的是“阿里云企业关系 + AI 编码助手 + 私域知识与治理”的组合。  
来源：[Lingma Pricing](https://lingma.aliyun.com/pricing) | [Lingma Help Center](https://help.aliyun.com/zh/lingma/)

**卖给谁**  
卖给已经在阿里云生态中的企业，也卖给需要内网、专属版、VPC、知识管理的研发组织。

**卖得怎么样**  
- 官方价格：企业标准版 `¥79/人/月`，企业专属版 `¥159/人/月`。  
  来源：[Lingma Pricing](https://lingma.aliyun.com/pricing)
- 官网案例页称已服务 `超万家企业`、`百万开发者`。  
  来源：[Lingma Use Cases](https://lingma.aliyun.com/community/use-cases)
- 阿里帮助中心说明该产品将于 `2026-05-20` 更名为 `Qoder CN`。  
  来源：[Help Center](https://help.aliyun.com/zh/lingma/)

**OpenHands 该学什么**  
- 企业案例库要写得像销售武器，不只是品牌展示
- 标准版 / 专属版 / 私有化 三段式对中国市场很有效

---

## 四、横向比较：哪些真的值得我们扑上去学

### 立即模仿

#### 1. 企业包装要前置

最值得抄的不是“产品文案”，而是这些按钮和条款：

- `SSO / SAML / SCIM`
- `Audit logs`
- `RBAC`
- `Private network / VPC / IP whitelist`
- `Marketplace`
- `Invoice / PO billing`

这些几乎出现在所有会卖 enterprise 的产品里。

#### 2. 公开客户和 ROI 证据

OpenHands 如果只讲 benchmark，会显得像技术项目。  
成功样本都在讲：

- 席位数
- 使用率
- 多少时间从 weeks 变成 hours
- 多少研发效率提升
- 多少业务团队在用

#### 3. 多入口，不要只押一个前台

成功产品的入口很少只有一个：

- Web GUI
- IDE
- CLI
- API
- Slack / 企业集成

OpenHands 的 Web 体验很重要，但如果没有 IDE/CLI/API/集成组合，市场会显得窄。

### 中期重点关注

#### 1. 非工程买家的预算

`Replit Agent` 和 `Lovable` 说明：AI 开发能力不是只能卖给研发。  
OpenHands 如果能把“内部工具自动化”“工作流开发”“问题修复代理”讲给产品、运营、支持团队听，市场会更大。

#### 2. 大代码库上下文与规则系统

`Augment` 和 `Cursor` 都证明：

- 真正的企业买家非常在意 agent 能否理解整个代码库
- 规则、审查、组织级控制，不是附加功能，而是主购买理由

### 高商业价值，但不宜硬抄

#### 1. Lovable 式高速消费增长

它的增长很亮眼，但并不等于 OpenHands 也应该走“全民 app builder”路线。  
OpenHands 更自然的长处仍然是：`真实代码库 + 真实执行环境 + 自治工程任务`。

#### 2. Devin 式“AI 员工”叙事

可以借鉴，但不能只学口号。  
如果没有稳固的治理、知识、审查与交付闭环，学这套叙事很容易失真。

---

## 五、第二版矩阵：候补样本观察层

第一版矩阵只覆盖了 10 个正式样本。第二版矩阵把候补样本也拉进来，但定位不是“和主样本平起平坐”，而是帮助回答：

- 哪些样本现在不该进主叙事，但必须持续观察
- 它们最强的商业信号是什么
- 它们为什么还不足以升格为主样本

| 产品 | 当前定位 | 商业化方式 | 公开商业证据强度 | 不进主样本的原因 | 最强一维 | 最弱一维 |
|---|---|---|---|---|---|---|
| `文心快码 / Comate` | 中国企业研发 Agent IDE | 订阅 + 企业专属版 + 私有化 | 高 | 与 Trae / 通义灵码重叠较高，更偏政企深耕 | 企业采购与案例公开度 | 开发者全球心智 |
| `Continue` | PR / CI 层 AI 质量控制 | 按 token + seat | 中 | 更像质量门禁层，不是通用自治软件工程师 | 组织级标准沉淀 | 端到端自主交付 |
| `Aider` | 终端结对编程 OSS | 开源 + BYOK | 低到中 | 社区强，但公开商业包装不足 | OSS 渗透与终端效率 | 企业化包装 |
| `Manus` | 通用 AI agent + builder | 订阅 + Team + API | 中到高 | 增长叙事强，但企业采购证明不够密 | 订阅增长速度 | 企业客户可验证度 |
| `Bolt` | prompt-to-production app builder | 订阅 + Teams + Enterprise | 中到高 | 包装强，但公开经营透明度不足 | 企业包装完成度 | ARR / 客户规模透明度 |
| `CodeGeeX` | 私有化企业代码助手 | 销售驱动 / 项目制 | 低 | 公开价格、客户、规模数据不足 | 内网与私有部署适配 | 商业透明度 |
| `MarsCode` | 已并入 Trae 插件体系的前身品牌 | 免费流量入口，后并轨 | 低 | SKU 独立性弱，已不适合作为单独商业产品比较 | 免费获客入口能力 | 商业主体清晰度 |

### 观察层里的三类产品

#### 1. 证据并不弱，但被主样本覆盖

`文心快码` 是这一类最典型的例子。它的企业能力、定价、案例和中标信息其实很强，只是主样本里已经有 `Trae` 和 `通义灵码` 代表了中国市场的两条更核心路径：

- 一条是开发者增长与轻量切入
- 一条是云厂商 ToB 与专属版回收

文心快码更像“政企纵深样本”，值得长期盯，但不一定需要和国际主样本并列。

#### 2. 产品化成熟，但不在 OpenHands 主战场

`Continue` 很值得研究，但它现在更像 `AI code review / policy / CI gate` 层，而不是从任务到实现的通用执行代理。  
这类产品的价值不在“替代 OpenHands”，而在提醒 OpenHands：

- 质量门禁是独立产品层
- 代码代理不一定都要变成自治工程师

#### 3. 热度高、包装强，但证据还不够厚

`Manus` 和 `Bolt` 都属于这类：

- `Manus` 强在增长叙事、订阅化速度、Team/API/SSO 的变现结构
- `Bolt` 强在 enterprise packaging、Microsoft 渠道、设计系统与部署故事

但它们都还缺一点：

- 更密集的命名大客户
- 更稳定的公开采购与扩张证据
- 更完整的公开经营数据

### 第二版双轴判断

如果只用一句话压缩候补样本，可以这样看：

- `文心快码`：**强销售证明，弱全球心智**
- `Continue`：**强治理价值，弱自治闭环**
- `Aider`：**强社区采用，弱企业包装**
- `Manus`：**强增长速度，弱采购验证**
- `Bolt`：**强企业包装，弱经营透明度**
- `CodeGeeX`：**强私有化适配，弱公开商业证明**
- `MarsCode`：**强入口意义，弱独立商业性**

---

## 六、中国市场给 OpenHands 的提醒

### 中国市场更看重什么

- 明确价格
- 企业专属版
- 私有部署
- 内网接入
- 审计与白名单
- 可交付案例

这也是为什么 `Trae`、`通义灵码`、`文心快码` 更容易卖进企业。

### 中国市场不太吃什么

- 只有全球热度，没有中文场景与企业交付细节
- 只有 benchmark，没有安全、采购、部署语言
- 定价模糊，全靠销售解释

---

## 七、给 OpenHands 的产品路线建议

### P0：先把企业可采购形态补齐

要尽快形成一眼能懂的包装：

- `Community / Pro / Team / Enterprise`
- `Self-hosted / VPC / Private deployment`
- `SSO / SCIM / RBAC / Audit logs`
- `Invoice / PO / Marketplace`

### P0：把“能交付结果”做成可展示资产

不要只展示 benchmark。  
要建立对外证据库：

- 哪些公司在用
- 用来干什么
- 节省了什么
- 哪些任务适合 OpenHands
- 哪些任务不适合

### P1：多入口统一

继续强化：

- Web GUI
- CLI
- IDE/Editor integration
- API / SDK
- 企业集成入口

### P1：突出 OpenHands 真正独特的地方

OpenHands 不必和每家都拼“最像 IDE”。  
更好的打法是把自己的强项讲清楚：

- 有真实执行环境
- 可以做跨文件、跨命令、跨工具任务
- 更适合长任务与自治任务
- 可以自托管、可扩展、可二开

### P2：建立中国市场版本语言

如果要面向中国团队或中国出海企业，产品页面语言要更像：

- “企业专属版”
- “内网部署”
- “模型可替换”
- “知识库 / 代码库权限”
- “审计留痕”
- “费用与并发上限”

---

## 八、哪些不用急着学

- 不用急着学所有消费级 vibe coding 叙事
- 不用急着学一切品牌化 IDE 外壳
- 不用把资源先砸在“最花哨的演示页面”

更该优先做的是：**把 OpenHands 从“强技术项目”变成“能被采购、能被试点、能被续费的产品”。**

---

## 附录：主要来源

### 直接同层

- [Devin Pricing](https://devin.ai/pricing)
- [Cognition / Devin](https://cognition.ai/)
- [Cursor Pricing](https://cursor.com/pricing)
- [Cursor Enterprise](https://cursor.com/en-US/enterprise)
- [Windsurf Pricing](https://windsurf.com/pricing)
- [Windsurf Homepage](https://windsurf.com/)
- [Augment Pricing](https://www.augmentcode.com/pricing/)
- [Augment Customers](https://www.augmentcode.com/customers)
- [Cline Pricing](https://cline.bot/pricing)
- [Cline Enterprise Overview](https://docs.cline.bot/enterprise-solutions/overview)

### 相邻层

- [Replit Pricing](https://replit.com/pricing)
- [Replit Customers](https://replit.com/customers)
- [Replit Funding Announcement](https://replit.com/news/funding-announcement)
- [Dify Pricing](https://dify.ai/pricing)
- [Dify Enterprise](https://dify.ai/enterprise)
- [Dify Funding Post](https://dify.ai/blog/dify-raises-30m-tomorrow-s-organizations-will-be-built-by-people-and-agents)
- [Lovable Pricing](https://lovable.dev/pricing)
- [Lovable Series B](https://lovable.dev/pt-br/blog/series-b)

### 中国市场

- [Trae Pricing](https://www.trae.cn/pricing)
- [Trae Enterprise](https://www.trae.cn/enterprise)
- [通义灵码 Pricing](https://lingma.aliyun.com/pricing)
- [通义灵码 Use Cases](https://lingma.aliyun.com/community/use-cases)
- [通义灵码 Help Center](https://help.aliyun.com/zh/lingma/)
- [文心快码 Pricing](https://comate.baidu.com/docs/%E4%BA%A7%E5%93%81%E5%AE%9A%E4%BB%B7/%E4%BA%A7%E5%93%81%E5%AE%9A%E4%BB%B7.html)

### 候补样本

- [Continue Pricing](https://www.continue.dev/pricing)
- [Continue Docs](https://docs.continue.dev/)
- [Aider Homepage](https://aider.chat/)
- [Aider GitHub](https://github.com/Aider-AI/aider)
- [Manus Pricing Help](https://help.manus.im/en/articles/11711111-what-is-the-current-membership-pricing-for-manus)
- [Manus ARR Update](https://manus.im/blog/manus-100m-arr)
- [Bolt Pricing](https://bolt.new/pricing?country=166)
- [Bolt Enterprise](https://bolt.new/enterprise/)
- [CodeGeeX Enterprise Manual](https://v4-enterprise.codegeex.cn/docs/guide/user_manual.html)
- [MarsCode / Trae Plugin Update](https://developer.volcengine.com/articles/7496711867822243881)
