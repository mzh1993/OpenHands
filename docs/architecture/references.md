# References

这份清单记录本次架构文档与独立 HTML 直接依赖的事实来源，方便后续维护时回查。

## 仓库内文档

- 根项目总览：`README.md`
- 开发与运行方式：`Development.md`
- 前端说明：`frontend/README.md`
- App server 总览：`openhands/app_server/README.md`
- Sandbox 模块：`openhands/app_server/sandbox/README.md`
- Conversation 模块：`openhands/app_server/app_conversation/README.md`
- Event 模块：`openhands/app_server/event/README.md`
- Skills / Microagents：`skills/README.md`
- Enterprise 架构索引：`enterprise/doc/architecture/README.md`
- Enterprise 认证流：`enterprise/doc/architecture/authentication.md`
- Enterprise 外部集成：`enterprise/doc/architecture/external-integrations.md`

## 仓库内关键代码入口

- FastAPI app 入口：`openhands/app_server/app.py`
- V1 路由聚合：`openhands/app_server/v1_router.py`
- 启动命令与开发流程：`Makefile`
- 前端主路由：`frontend/src/routes.ts`
- 会话页：`frontend/src/routes/conversation.tsx`
- 会话 WebSocket 包装层：`frontend/src/contexts/websocket-provider-wrapper.tsx`

## 官方在线文档

- 项目介绍：<https://docs.openhands.dev/overview/introduction>
- 核心功能：<https://docs.openhands.dev/openhands/usage/key-features>
- Local GUI 使用：<https://docs.openhands.dev/openhands/usage/run-openhands/local-setup>
- CLI 使用：<https://docs.openhands.dev/openhands/usage/run-openhands/cli-mode>
- Backend Architecture：<https://docs.openhands.dev/openhands/usage/architecture/backend>
- SDK 入口：<https://docs.openhands.dev/sdk>

## 外部相关仓库

- OpenHands Software Agent SDK：<https://github.com/OpenHands/software-agent-sdk/>
- OpenHands CLI：<https://github.com/OpenHands/OpenHands-CLI>
- OpenHands Benchmarks：<https://github.com/OpenHands/benchmarks>

## 产品化与生态案例

- OpenHands Cloud launch：<https://www.openhands.dev/blog/introducing-the-openhands-cloud>
- OpenHands Cloud self-hosted：<https://www.openhands.dev/blog/openhands-cloud-self-hosted-secure-convenient-deployment-of-ai-software-development-agents>
- OpenHands Enterprise docs：<https://docs.openhands.dev/enterprise>
- OpenHands Enterprise launch：<https://www.openhands.dev/blog/openhands-enterprise-agent-control-plane>
- AMD technical article：<https://www.amd.com/en/developer/resources/technical-articles/2025/OpenHands.html>
- C3 AI blog：<https://c3.ai/blog/autonomous-coding-agents-beyond-developer-productivity/>
- MLflow / Databricks blog：<https://mlflow.org/blog/mlflow-openhands>
- Daytona blog：<https://www.daytona.io/dotfiles/instant-ai-development-with-daytona-and-openhands>
- Daytona OpenHands page：<https://openhands.daytona.io/>
- TestChimp integration docs：<https://docs.testchimp.io/integrations/openhands/>
- Lynxroute Azure Marketplace：<https://marketplace.microsoft.com/en-us/product/saas/lynxroute.openhands?tab=overview>
- Spheron blog：<https://www.spheron.network/blog/deploy-openhands-gpu-cloud/>
- OmoiOS repo：<https://github.com/kivo360/OmoiOS>
- OmoiOS site：<https://www.omoios.dev/>
- OpenSymphony repo：<https://github.com/kumanday/OpenSymphony>
- OpenSymphony site：<https://opensymphony.dev/>
- Thesis repo：<https://github.com/oraichain/Thesis>
- Thesis app：<https://thesis.io/>
- OpenHands AI Action repo：<https://github.com/xinbenlv/openhands-action>
- OpenHands AI Action Marketplace：<https://github.com/marketplace/actions/openhands-ai-action>
- openhands-infra repo：<https://github.com/zxkane/openhands-infra>
- openhands-infra blog：<https://kane.mx/posts/2026/serverless-multi-tenant-openhands-on-aws/>

## 维护约束

- 当根 `README.md` 对产品线定义发生变化时，优先更新 `openhands-architecture-overview.md` 的“产品入口”与“主仓边界”章节。
- 当官方 `overview/introduction` 或 `usage/key-features` 结构变化时，检查“60 秒上手导览”和“体验入口”是否需要同步。
- 当“生态与产品化证据”章节更新时，只收录有明确 OpenHands / OpenHands SDK 关系、并且带可直达产品页 / 市场页 / 官方文档页 / repo 的强证据案例。
- 当 `v1_router.py` 的路由聚合发生变化时，检查架构总览中的模块列表。
- 当 `frontend/src/routes.ts` 的导航结构发生变化时，检查 HTML 导览页和总览文档中的“前端结构”“工作面板”“体验入口”描述。
