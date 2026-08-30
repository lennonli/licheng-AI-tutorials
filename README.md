# licheng-AI教程

面向法律工作场景的 AI/Codex 本地环境与网络工具教程。

## 教程目录

1. [macOS + Codex 法律工作流依赖与环境安装教程](docs/macos-codex-legal-workflow-setup-ABL-20260707-V1.md)
2. [Windows + Codex 法律工作流依赖与环境安装教程](docs/windows-codex-legal-workflow-setup-ABL-20260707-V1.md)
3. [梯子使用全教程：macOS、Windows、Android、iOS](docs/proxy-clash-verge-full-guide-ABL-20260707-V1.md)
4. [Claude App 与 Codex App 通过 CC Switch 接入国产大模型教程](docs/claude-codex-app-ccswitch-domestic-models-ABL-20260707-V1.md)
5. [macOS Codex 跳过登录页面教程（使用第三方模型）](docs/macos-codex-skip-login-third-party-models-ABL-20260708-V1.md)
6. [智能体指令体系、工具选择培训讲义（HTML 翻页版）](docs/agent-instruction-tool-selection-training-ABL-20260708-V1.html)
7. [法律工作如何选择 AI 智能体（2026年7月）](docs/legal-ai-agent-selection-202607-ABL-20260708-V1.md)
8. [新建 VPS 后 OpenCode 安装与远程 Web UI 配置教程](docs/new-vps-opencode-web-ui-setup-ABL-20260709-V1.md)
9. [法律业务 Skill 制作教程：两种主要方案](docs/legal-business-skill-creation-guide-ABL-20260710-V1.md)
10. [竞聘法律顾问·公司基本情况及法律问题初步分析报告提示词](docs/ipo-legal-advisor-company-profile-prompt-ABL-20260713-V1.md)
11. [AI 智能体的记忆，到底记住了什么](docs/ai-agent-memory-comparison-ABL-20260821-V1.md)
12. [AI 智能体对话管理实务教程](docs/ai-agent-conversation-management-ABL-20260821-V1.md)
13. [别打断它：智能体"辅助对话"的律师用法](docs/ai-agent-side-conversation-ABL-20260821-V1.md)
14. [律师 MCP 搭建实用教程](docs/lawyer-mcp-setup-ABL-20260821-V1.md)
15. [额度没了，赶紧换另一个有额度的 AI 续上我的任务](docs/ai-task-handoff-ABL-20260817-V1.md)
16. [AI智能体智能调度工作教程](docs/ai-agent-dispatch-ABL-20260825-V1.md)
17. [年度上市挂牌审核问询法律问题回溯提示词（冷启动可复现）](docs/listing-inquiry-annual-coldstart-prompt-ABL-20260827-V1.md)
18. [民事诉讼 AI 实战系列·总览（先读）](docs/civil-litigation-series-overview-ABL-20260827-V1.md)
19. [诉讼 AI 工作总则](docs/civil-litigation-principles-ABL-20260827-V1.md)
20. [诉前评估与诉讼策略](docs/civil-litigation-pretrial-assessment-ABL-20260827-V2.md)
21. [证据整理与证据目录](docs/civil-litigation-evidence-organization-ABL-20260827-V2.md)
22. [起诉状、答辩状与上诉状起草](docs/civil-litigation-pleadings-drafting-ABL-20260827-V2.md)
23. [庭审举证与质证](docs/civil-litigation-trial-evidence-ABL-20260827-V1.md)
24. [庭前对抗演练与庭审提纲](docs/civil-litigation-moot-court-ABL-20260827-V3.md)
25. [代理词撰写](docs/civil-litigation-closing-argument-ABL-20260827-V1.md)
26. [庭后复盘与裁判文书深读](docs/civil-litigation-post-trial-review-ABL-20260827-V2.md)
27. [强制执行与财产线索](docs/civil-litigation-enforcement-ABL-20260827-V2.md)
28. [结案归档与知识沉淀](docs/civil-litigation-closing-archive-ABL-20260827-V1.md)
29. [公司舆情与法律风险监控系统·从零搭建教程](docs/company-monitoring-agent-tutorial-ABL-20260827-V2.md)
30. [Mac 代理分流排查提示词（跨机器复用版）](docs/mac-proxy-troubleshooting-ABL-20260828-V1.md)
31. [如何写一个规范的临时对话提示词](docs/temp-conversation-prompt-guide-ABL-20260830-V1.md)
32. [别只装一个 AI Agent：我为什么把 OpenCode 当作“救援工具”](docs/opencode-rescue-agent-guide-ABL-20260830-V1.md)

## 附加版本

- [梯子使用全教程 HTML 自包含版](docs/proxy-clash-verge-full-guide-ABL-20260707-V2.html)

## 更新记录

| 日期 | 文件 | 更新内容 |
| --- | --- | --- |
| 2026-08-30 | [别只装一个 AI Agent：我为什么把 OpenCode 当作“救援工具”](docs/opencode-rescue-agent-guide-ABL-20260830-V1.md) | 新增备用 Agent 思路短文：主力 Agent（Codex/Claude Code/ZCode）自身损坏时无 AI 可用的死锁问题，建议额外常备一个轻量 OpenCode 作"救援 Agent"，列明它可排查的八类问题（启动失败、API Key/环境变量、MCP 连接、配置文件、Node/Python/Docker 环境、端口冲突、网络代理 DNS、看日志修复），提出主备互查的双 Agent 体系（重要任务可一个改配置、另一个复核），并建议在 VPS 上也装一个 OpenCode 充当随时调用的 AI 运维助手（Docker/Nginx/Cloudflare Tunnel/Tailscale/磁盘/日志/部署 GitHub 项目九类场景），结论是不求最强、但求稳定的 AI 工具体系，一个干活一个救场。 |
| 2026-08-30 | [如何写一个规范的临时对话提示词](docs/temp-conversation-prompt-guide-ABL-20260830-V1.md) | 新增临时对话提示词写作方法短文：已配置全局指令、Skill 和 MCP 的本地 Agent，临时提示词只需按"任务＋输入＋工作内容＋输出＋落地位置"五要素交代本次任务的变量，附可直接替换占位符整段复用的提示词模板；逐一说明输入材料写绝对路径、明确任务边界、指定交付格式、指定输出目录且不得覆盖原始文件、要求列明生成文件与待人工确认事项各要素解决什么问题，补充需要外部数据时"如需核验请调用某 MCP"的一句话写法，结论是全局指令管长期规则、Skill 管标准流程、MCP 管外部数据，临时提示词只负责说明这次具体做什么，越像工作指令、越不像提示词作文效率越高。 |
| 2026-08-28 | [Mac 代理分流排查提示词（跨机器复用版）](docs/mac-proxy-troubleshooting-ABL-20260828-V1.md) | 新增 Mac 代理分流排查提示词（发给另一台 Mac 上的 AI 直接执行）：Tailscale MagicDNS 引发 DNS 污染的诊断与修复（fake-ip 判读表、国内 DoH 同样被投毒的原因）、Shadowrocket 劫持 tailnet 路由与 UDP 被 TUN 抓走两个路由冲突的诊断修复、hysteria2 优于 Tailscale exit node 的选型依据（高丢包链路 QUIC+BBR）、四组验收命令和排查纪律；入库时已将服务器公网 IP、tailnet IP、节点密码等真实凭证全部脱敏为占位符。 |
| 2026-08-27 | [公司舆情与法律风险监控系统·从零搭建教程](docs/company-monitoring-agent-tutorial-ABL-20260827-V2.md) | 新增面向 AI Agent 的舆情与法律风险监控日报系统复刻教程：架构总览与四条铁律（MCP 拿数据、Skill 定规则、Cron 只触发；单任务遍历主体清单；Baseline 全量后只报增量；判断交给 LLM、记忆交给 SQLite）、数据源 A/B/B2/C 分档与第三方仓库选型纪律、十步每日 SOP 与红橙黄三级分级响应时限、SQLite 指纹去重引擎设计、微博移动端 API 等社媒三通道采集实现要点（含同域 CORS 修复等 14 条踩坑表，V2 新增小红书半自动通道（Agent-Reach/OpenCLI 桥接浏览器登录会话）接入教程与合规口径）、md 转 PDF 转 Drive 转邮件交付管线三脚本、Baseline 首日执行 SOP 与 10 项完工自查清单；正文不含任何客户名称与联系方式，具体配置以部署目录 targets.yaml 与 mail.json 为准。 |
| 2026-08-27 | 民事诉讼 AI 实战系列（总则+九篇） | 全面更换为按案件推进时间轴组织的民事诉讼 AI 实战系列：诉讼 AI 工作总则（能力边界、保密分级、反编造纪律、模板复用四步法、期限速查表）＋诉前评估与诉讼策略、证据整理与证据目录、起诉状答辩状与上诉状起草、庭审举证与质证、庭前对抗演练与庭审提纲（含完整模拟开庭）、代理词撰写、庭后复盘与裁判文书深读（含再审情形与二审处理依据）、强制执行与财产线索（含执行管辖与追加被执行人依据）、结案归档与知识沉淀。各篇附可直接整段复制的提示词模板与官方来源核验记录，通用化表述适配各类 AI 智能体；文书起草优先以律师提供的其他项目更优模板为基准调整完善；此前旧版九篇已全部由本批替代下线。另有系列总览收录为仓库导航页。 |
| 2026-08-25 | [AI智能体智能调度工作教程](docs/ai-agent-dispatch-ABL-20260825-V1.md) | 新增 AI 智能体调度外部智能体（含高级分析模型）工作教程（docx 提示词版转 Markdown 入库）：调度原理、四类常用场景与可直接复制的提示词模板（审查对方修订并指定高级模型、制作清洁版定稿、指定模型或工具、检查调度链路可用性）、提示词写作五要素（背景/位置/要求/去向/特别指令）、结果四块内容的律师把关重点和故障直报式排障。 |
| 2026-08-27 | [年度上市挂牌审核问询法律问题回溯提示词（冷启动可复现）](docs/listing-inquiry-annual-coldstart-prompt-ABL-20260827-V1.md) | 新增一整套可冷启动复现的年度回溯任务提示词：从东财公开接口建{YEAR}年A股五板块与新三板挂牌清单，双通道抓取沪深北交易所及股转官网的问询回复与补充法律意见书（含股转2025-08-02新旧审核系统切换的双接口参数和WAF串行纪律），按分公司模板提炼（20类法律主题强制成节、各板块篇幅下限、回复要点逐子问对应具体数据点），配 codex/内置子代理/agy/opencode 四档执行引擎优先级与队列化断点方案，最终聚合为年度报告并同步云端盘；入库时将 Google 云盘路径中的个人邮箱替换为 {GOOGLE_ACCOUNT} 占位符。 |
| 2026-08-23 | [额度没了，赶紧换另一个有额度的 AI 续上我的任务](docs/ai-task-handoff-ABL-20260817-V1.md) | 新增跨 AI 任务接力短文（原稿 2026-08-17）：额度用尽时把未完成对话的本地文件路径交给另一家有额度的 AI 续做——三步操作（问出对话文件路径、贴给新 AI 续做、让新 AI 先当验收员复核旧 AI 的交付），附可直接照抄的验收指令、左右手互搏原理（不同家 AI 犯错路子不同、互查才查出真问题）和只给路径不贴内容、查改分开两条提醒。 |
| 2026-08-21 | [律师 MCP 搭建实用教程](docs/lawyer-mcp-setup-ABL-20260821-V1.md) | 新增律师 MCP 搭建实用教程（docx 精简版转 Markdown 入库）：北大法宝、元典、企查查、见微、腾讯 ima 五家数据库的计费与接入形态对照，四个 AI 客户端（ZCode/Codex/Claude 桌面版/Claude Code）配置位置，本地聚合桥与检索子代理两个自建组件，以及可直接粘贴给 AI 的六个安装提示词（新装数据库、跨客户端同步、双 Key 轮换、接入 ima、排障、建检索子代理）和八条注意事项。 |
| 2026-08-21 | [别打断它：智能体"辅助对话"的律师用法](docs/ai-agent-side-conversation-ABL-20260821-V1.md) | 新增所内 AI 工具使用教程系列第三篇：Claude Code `/btw`、Codex `/side`、ZCode 辅助对话的机制对比与律师用法——不打断长任务问出处、核对口径、探路、交接自检六个场景，四条红线（只回忆不检索、不留痕不作底稿、保密义务不降低、看不见正在生成的回复），与子代理/分支/压缩的分工对照，三工具操作细节表和可直接照抄的九句话。 |
| 2026-08-21 | [AI 智能体对话管理实务教程](docs/ai-agent-conversation-management-ABL-20260821-V1.md) | 新加以 Claude app、Codex app 图形界面为主的对话管理实务教程：上下文窗口机制、模型/思维强度/思考开关三旋钮、计划模式与目标模式、一对话一任务原则、消息编辑分支与文件隔离、分阶段推进与四要素提示词、上下文输入技巧、指令文件与记忆分层、Mac 路径与权限、本地与云端边界、并行自动化和跨工具接力，附一页速查表；入库时对正文中的客户名与案件当事人姓名作了脱敏。 |
| 2026-08-21 | [AI 智能体的记忆，到底记住了什么](docs/ai-agent-memory-comparison-ABL-20260821-V1.md) | 新增 Claude、Codex、ZCode 三家智能体记忆机制对比文章：指令层与记忆层的分工、全局与项目 AGENTS.md 分层方法、自动记忆的内容边界、三家实现收敛的共同形状、记忆的四种代价，以及带保密义务的法律业务场景下的记忆管理建议。 |
| 2026-07-13 | [竞聘法律顾问·公司基本情况及法律问题初步分析报告提示词](docs/ipo-legal-advisor-company-profile-prompt-ABL-20260713-V1.md) | 新增拟上市或拟挂牌企业法律顾问竞聘提示词，覆盖资料读取、公开信息核验、资本市场历史、法律风险分级、Word 报告结构、格式规范和可直接复用的完整提示词模板。 |
| 2026-07-10 | [法律业务 Skill 制作教程：两种主要方案](docs/legal-business-skill-creation-guide-ABL-20260710-V1.md) | 新增法律业务 Skill 制作教程，分别说明对话内完整跑通流程后总结生成 Skill、使用 Record & Replay 录制外部应用工作流后生成 Skill 两种方案，并补充适用边界、测试方法和质量标准。 |
| 2026-07-10 | [智能体指令体系、工具选择培训讲义（HTML 翻页版）](docs/agent-instruction-tool-selection-training-ABL-20260708-V1.html) | 完善概览弹层的无障碍语义与焦点管理，避免在按钮、输入框等交互元素上误触翻页快捷键；使用无历史污染的页码定位，并支持减少动态效果偏好。 |
| 2026-07-10 | [梯子使用全教程 HTML 自包含版](docs/proxy-clash-verge-full-guide-ABL-20260707-V2.html) | 增加移动端响应式内边距、字号、表格和代码块横向滚动规则，避免窄屏正文被过度压缩。 |
| 2026-07-10 | [新建 VPS 后 OpenCode 安装与远程 Web UI 配置教程](docs/new-vps-opencode-web-ui-setup-ABL-20260709-V1.md) | 将教程纳入统一教程仓库，补齐中文标题、章节结构、安全提示和可搜索正文。 |
| 2026-07-10 | [Claude App 与 Codex App 通过 CC Switch 接入国产大模型教程](docs/claude-codex-app-ccswitch-domestic-models-ABL-20260707-V1.md) | 修正 6 处因中文标点被并入地址而产生的错误 Base URL 链接。 |
| 2026-07-08 | [法律工作如何选择 AI 智能体（2026年7月）](docs/legal-ai-agent-selection-202607-ABL-20260708-V1.md) | 新增面向中国法律工作场景的 AI 智能体选择教程，覆盖不同智能体适用边界、法律业务分类选择表、诉讼/非诉/行政办公实操场景和可复用提示词模板。 |
| 2026-07-08 | [智能体指令体系、工具选择培训讲义（HTML 翻页版）](docs/agent-instruction-tool-selection-training-ABL-20260708-V1.html) | 新增面向律师团队的 AI Agent 培训讲义翻页版，覆盖大模型与 Agent 区分、指令体系、Codex 工具、Skill/MCP/Plugin、客户秘密保护和团队落地；优化翻页版工具栏位置和全屏展示比例，避免遮挡正文内容并减少底部大面积留白。 |
| 2026-07-08 | [macOS Codex 跳过登录页面教程（使用第三方模型）](docs/macos-codex-skip-login-third-party-models-ABL-20260708-V1.md) | 新增使用第三方模型时跳过 Codex 登录页的 macOS 操作教程，包含配置原因、备份修改步骤、验证方式、常见排错和一键脚本。 |

## 说明

- 本仓库内容主要面向合法合规的法律工作流、Codex 环境配置、文档处理、OCR、PDF 验证和工具使用。
- 涉及网络代理工具的教程仅说明客户端安装、订阅导入、模式选择、验证与排错，不提供节点购买、自建服务端或规避监管的方法。
- 订阅链接、二维码、节点地址、日志和客户资料均可能含敏感信息，不应公开提交到仓库。
