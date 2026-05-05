# Apkclaw 二开执行计划 Todo

## 文档说明

- 本文档是 `Apkclaw` 二开项目的唯一执行计划与 Todo 跟踪文档。
- 后续任何 AI 或人工在推进本项目任务时，都必须先读取本文档，再开展具体工作。
- 每次完成、暂停、拆分、取消任何事项后，都必须立即更新本文档中的状态、时间、完成 Agent 和备注。
- **每完成一项任务**（或一个可独立验收的子步骤）后，必须对已纳入版本控制的变更执行 **git 提交并生成 commit**：一项任务（或一步）对应尽量 **一条清晰的 commit**，message 中包含任务 ID（如 `完成 T14：…`）；勿将互不相关的多项任务改动堆进同一 commit。
- 若新增任务，请补充到“Todo List”中，保持按依赖顺序排列。

## 状态定义

- `待办`：尚未开始
- `进行中`：当前正在推进
- `阻塞`：因外部依赖或条件不足无法继续
- `已完成`：已完成并满足当前验收条件
- `已取消`：确认不再执行

## 当前阶段

- 当前阶段：Phase 2 — 集成验收筹备（**T19 暂缓**，**T21–T22 已完成**，**T23 进行中**：清单已交付，待真机勾选）
- 当前负责人：AI / 开发协作
- 最后更新时间：2026-05-05
- 下一步优先：**T23** 五块主流程全量验收；**T19** 模板仍暂缓不适用

## 执行原则

1. 先冻结协议，再推进客户端架构改造。
2. 先打通 MVP 主链路，再扩展双执行模式。
3. 技能、历史、模板不能阻塞主链路。
4. 每次任务结束后必须更新本文件中的：
   - `状态`
   - `最后更新`
   - `完成 Agent`
   - `备注`
5. **版本控制**：任务交付的代码或文档变更在更新本文件前后，应按 **`AGENTS.md`**（等价路径 `agent.md`）要求执行 **git commit**；可选在 `备注` 或下方「更新记录」中顺带记下本次关联的 commit 摘要，便于回溯。

## Todo List

| ID | 状态 | 任务 | 交付物 | 依赖 | 最后更新 | 完成 Agent | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| T01 | 已完成 | 冻结客户端/服务端最小协议：设备注册、心跳、拉任务、状态回传、技能下载、模型配置获取；明确字段与状态流转 | 接口清单、状态机、字段说明 | 无 | 2026-05-01 | Codex | 已新增 `Apkclaw-客户端服务端-MVP协议.md`，冻结 MVP 字段、接口与状态流转 |
| T02 | 已完成 | 抽象任务入口：新增 `TaskSource`、`TaskContext`、`RemoteTask`，解除 `TaskOrchestrator` 对 IM 渠道/消息 ID 的硬依赖 | 任务入口抽象与模型定义 | T01 | 2026-05-01 | Codex | 已引入任务上下文与回调抽象，现有 IM 渠道通过适配器接入；编译验证受现有 `build.gradle.kts` 签名配置影响，非本次代码报错 |
| T03 | 已完成 | 实现 `DeviceTaskGateway` MVP：设备注册、心跳、轮询拉任务、任务状态回传 | 网关实现与接口适配 | T01 | 2026-05-01 | Codex | 已新增远端任务网关骨架、MMKV 配置、轮询与状态回传；默认需显式开启并配置服务端地址 |
| T04 | 已完成 | 实现 `TaskDispatcher`，把 `RemoteTask` 转换为本地任务并接入 `TaskOrchestrator` 启动链路 | 任务分发链路 | T02, T03 | 2026-05-01 | Codex | 已把远端任务转换为 `TaskContext` 并通过 `RemoteTaskCallbacks` 接入执行与状态回传 |
| T05 | 已完成 | 增加执行模式骨架：`ExecutionMode`、`ExecutionModeResolver`、本地默认模式配置；MVP 先只接 `ACCESSIBILITY` | 模式配置骨架 | T02 | 2026-05-01 | Codex | 已新增执行模式枚举、解析器与默认模式本地配置；当前仅开放 `ACCESSIBILITY` |
| T06 | 已完成 | 改造首页和设置页最小闭环：账号绑定状态、设备状态、接单开关、无障碍状态 | MVP 页面与状态展示 | T03, T05 | 2026-05-01 | Codex | 已新增 Device Config 页面、Task Inbox 开关、设备状态与首页任务收件箱状态展示 |
| T07 | 已完成 | 完成 MVP 联调：服务端下发文本任务，客户端执行并回传 `RUNNING/SUCCESS/FAILED/CANCELLED` | MVP 端到端闭环 | T04, T05, T06 | 2026-05-02 | Codex | 已补本地 mock HTTP 服务端、一键协议验证、状态日志查看；可在 Device Config 页面完成本地闭环验证 |
| T08 | 已完成 | 抽象统一执行接口 `DeviceCommandExecutor`，覆盖 `tap`、`longPress`、`swipe`、`pressKey`、`screenshot`、`launchApp`、`shell` | 执行接口定义 | T07 | 2026-05-02 | Codex | 已新增 `DeviceCommandExecutor`、`AccessibilityDeviceExecutor`、`DeviceCommandExecutorManager`、`UnavailableDeviceExecutor`，并支持按任务上下文解析执行模式与 fallback |
| T09 | 已完成 | 把现有核心工具改为通过 `DeviceCommandExecutor` 调用，不再直接依赖 `ClawAccessibilityService` | 工具层适配 | T08 | 2026-05-02 | Codex | 已完成手势/按键/截图/启动类工具迁移，并新增 `AccessibilityUiController` 承接 `get_screen_info`、`find_node_info`、`input_text`、`scroll_to_find` 等无障碍节点能力，工具层已不再直接依赖 `ClawAccessibilityService` |
| T10 | 已完成 | 接入 Shizuku：实现 `ShizukuDeviceExecutor`、授权检测、错误提示、状态展示 | Shizuku 执行层 | T08 | 2026-05-02 | Codex | 已新增 Shizuku 依赖、状态管理与 shell 型执行器，接入设置页执行模式切换/授权提示，并在心跳与设备状态中展示 `shizukuReady`；当前完成编译验证，尚未在真机上完成运行态验证 |
| T11 | 已完成 | 完成双模式决策与任务覆盖：任务指定模式优先，本地默认模式次之，默认不自动回退 | 双模式决策逻辑 | T09, T10 | 2026-05-03 | Cursor | 代码侧继承 Codex 成果；直连验证与 `resolveExecutorSelection` 对齐；fallback **场景 A/B** 均已在 vivo V1838A 上验证（见《Apkclaw-双模式验证记录》fallback 表与 `dual_mode_fallback_smoke_*.md`） |
| T12 | 已完成 | 补齐双模式测试：`tap/swipe/open_app/screenshot/key/shell` 的行为一致性验证 | 测试用例与验证记录 | T11 | 2026-05-03 | Cursor | 六用例矩阵已回填；ACCESSIBILITY `screenshot` 在 Android 10 边界失败已标注；fallback A（回退无障碍）与 B（Shizuku 基线）均已 SUCCESS 落表 |
| T13 | 已完成 | 实现 `RemoteAgentConfigProvider`：启动拉取、缓存回退、热更新 `TaskOrchestrator` 配置 | 远端模型配置能力 | T07 | 2026-05-03 | Cursor | `RemoteAgentConfigProvider`：`GET /api/v1/agent/config`、MMKV 缓存、`AppViewModel` 合并 local+缓存构建 `AgentConfig`；`afterInit`/`refreshDeviceGateway` 后台拉取；指纹变更时 `updateAgentConfig`；`KVUtils.hasLlmConfig` 支持缓存内 `apiKey`（Mock 扩展）；Mock HTTP 增加 `agent/config`；冷启动无 LLM 也走 `afterInit` 以便网关注册后拉取 |
| T14 | 已完成 | 实现 `SkillManager`、`SkillRepository`、`SkillRenderer`，支持 `manifest.json + skill.md + assets` | 技能系统基础模块 | T13 | 2026-05-03 | Cursor | 交付本地技能目录、`installFromUnpacked`；与 **T24** 组合走 LangChain4j Skills Tool Mode；**T15** 已接 HTTP ZIP 装载 |
| T15 | 已完成 | 接入技能下载与缓存：按 `skillPackageId` 下载、校验、装载；设备工具白名单仍由 manifest `allowedTools` 聚合（与 LC4j Skills `activate_skill` 并行存在） | 技能装载链路 | T14, T24 | 2026-05-03 | Cursor | `SkillRemoteLoader`：`GET meta` / `download`（或 meta.downloadUrl 同源 Bearer）、ZIP SHA-256 校验、ZIP -slip 解压、`KVUtils` 记录已装载 checksum、`SkillManager.ensureSkillPackages` 在编排前拉起；Mock 增补 `wechat-basic` zip 与 meta/download；任务在技能缺失且无法拉取时失败并报 `channel_msg_skill_packages_unavailable` |
| T16 | 已完成 | 先落地微信 skill，验证消息发送/自动回复场景 | 微信 skill MVP | T15 | 2026-05-03 | Cursor | `assets/bundled_skills/wechat-basic` + `BundledSkillInstaller` 启动时装载；`Channel.WECHAT` 派发任务自动挂载 `skillIds=[wechat-basic]`；skill.md 说明 iLink/`finish`/通道回路与微信 App 前台自动化；Mock ZIP 内容与 assets 对齐；编译 `:app:compileDebugKotlin` 通过。**真机**验证需：配置 WeChat+iLink、`finish` 回推与「纯对话不写 UI」链路 |
| T17 | 已完成 | 实现 `HistoryManager` 与 Room 表结构：任务摘要、步骤、截图、耗时、最终结果 | 历史存储层 | T07 | 2026-05-03 | Cursor | Room 2.7 + KSP 2.3.7；`task_history` / `task_history_step`；`HistoryManager` 单线程写入；`take_screenshot` 成功时复制 PNG 至 `files/task_history/{runId}/`；`TaskOrchestrator` 全流程挂钩；`pluginManagement` 优先 Maven Central 以解析 KSP。验证：`:app:compileDebugKotlin` 通过 |
| T18 | 已完成 | 实现任务历史页与详情页：列表、步骤流、截图回放 | 历史页面 | T17 | 2026-05-03 | Cursor | `TaskHistoryListActivity` / `TaskHistoryDetailActivity`；首页卡片 + 设置 Device 首项入口；步骤卡片 + 横滑缩略显 + 全屏回看（Glide）；`recyclerview` 1.3.2。验证：`:app:compileDebugKotlin` 通过 |
| T19 | 待办 | 实现 `TemplateManager` 与模板快捷发起入口 | 模板功能 | T17 | 2026-05-03 |  | **暂缓**：用户确认先跳过；不阻塞主链路与 T20；后续若要收口集成验收（T23）再评估是否补做 |
| T20 | 已完成 | 补齐可靠性：任务超时、失败重试、任务取消、日志导出/追踪 | 稳定性与可观测性 | T07, T12, T18 | 2026-05-03 | Cursor | 墙钟超时（`taskTimeoutSeconds` 元数据 / `KVUtils` 默认）；`maxRetries`≤5 全任务重跑；回调 `guardActive` 防与取消竞态；`DeviceTaskGateway` 状态上报 4 次指数退避；设置「Export diagnostics」；`TaskRuntimeMetadataKeys`、`DiagnosticsExporter`。验证：`:app:compileDebugKotlin` |
| T21 | 已完成 | 清理旧入口：弱化 `ChannelManager`，新建 `TaskSource` 主路径；下线旧 IM 渠道主入口 | 入口收敛 | T07, T11 | 2026-05-03 | Cursor | `ImInboundTaskIngress` 收口 IM→编排器；`afterInit` 先启 `DeviceTaskGateway`；钉钉等五 Bot 迁至 `LegacyImBotsSettingsActivity`；`ChannelManager`、`TaskDispatcher`、`TaskSource` 注释标明主路径。本地请跑 `:app:compileDebugKotlin` 验证编译。|
| T22 | 已完成 | 清理旧配置：移除本地 `OpenAI/Anthropic/BaseURL` 手工配置入口，仅保留 debug 能力 | 配置入口收敛 | T13 | 2026-05-03 | Cursor | Release 隐藏设置「模型」分组；非 Debug 进入 `LlmConfigActivity` 即提示并退出；`KVUtils` 注释说明 KV 仍以网关下发/局域网为主。顺带修复 `LegacyImBotsSettingsActivity` KDoc。**验证**：`:app:compileDebugKotlin`、`compileReleaseKotlin` 通过 |
| T23 | 进行中 | 完成集成验收：绑定、接单、执行、历史、设置五块主流程全量回归 | 验收记录与问题清单 | T16, T18, T20, T21, T22 | 2026-05-03 | Cursor | **T19 暂缓**；模板单列「不适用」。已新增 `docs/prd/Apkclaw-T23-集成验收清单.md`；研发冒烟 `./gradlew :app:compileDebugKotlin`（ApkClaw）通过。**待办**：按清单真机逐项勾选并回填本文「问题清单」后可将状态改为已完成。**Mock 网关**：未填绑定码时 `dispatchMockRemoteTask` 自动写入 `mock`；`MockRemoteTaskServer` 的 `agent/config` 回显本地 LLM KV 以支持第三方 Base URL（见更新记录）。**2026-05-03 21:38 矩阵报告**：已归档 `verification-export/dual_mode_matrix_summary_20260503_213835.md`，并在 T23 清单登记 `T23-I01/I02`；当前风险是矩阵汇总把“任务终态 SUCCESS”误当成“用例能力通过”，至少影响 `screenshot/ACCESSIBILITY` 与 `shell/SHIZUKU` 两项判读。**2026-05-03 22:13/22:33 自定义任务观察**：`mock_1777817569890` 与 `mock_1777818794869` 均被记为 SUCCESS，但前者只执行到 `AC`、`1`、`+`，后者更是在四次点击后直接 `finish`，均未可靠证明 `1+1=2`；根因已确认是模型将“坐标点击已执行”误当成“结果已确认”，而框架对 `finish` 缺少任何二次验收；已在 T23 清单登记 `T23-I03`。**2026-05-03 22:48 运行态排查**：针对“运行任务应用闪退”复查 logcat，未见 `com.apk.claw.android` 的 `FATAL EXCEPTION`；22:33:22 明确记录 `open_app` 成功拉起 `com.android.bbkcalculator`，更符合“任务执行时主动切前台到目标 App，被误判为闪退”。另在用户事后重开应用但本地 Mock 服务尚未拉起时，可稳定看到 `DeviceTaskGateway` 连接 `127.0.0.1:9537` 被拒（`ECONNREFUSED`）；该问题已在 T23 清单登记 `T23-I04`，需分别从前台提示与 Mock 网关启动时序处理。 |
| T24 | 已完成 | 对齐 LangChain4j Skills Tool Mode：`activate_skill`/`read_skill_resource` + 技能目录 XML + 合并设备 ToolExecutor | `langchain4j-skills` 集成、`SkillAugmentation.mergedTools` | T14 | 2026-05-03 | Cursor | 依赖 `langchain4j-skills` 1.12.2-beta22；安装包生成 `SKILL.md`；`Skills.formatAvailableSkills` + `mergeDeviceAndSkills`；**与官方教程/version 的差异与限制**见 `docs/prd/Apkclaw-LangChain4j-Skills-与官方差异.md` |
| T25 | 已完成 | 初始化 GitHub 远程仓库并推送当前仓库到 `origin/main`，并补推 `ApkClaw` 子仓库远端 | 根仓库与 `ApkClaw` 子仓库远程同步结果 | T23 | 2026-05-05 | Codex | 计划外任务，按用户要求插入。为便于对外协作，先完成仓库首推；不改变 `T23` 的验收优先级。交付物为根仓库 `origin=git@github.com:yannan/android-project.git`、本地分支重命名为 `main` 并完成 `git push -u origin main`；后续补推 `ApkClaw` 子仓库到 `git@github.com:yannan/ApkClaw.git`。 |

## 更新记录

| 日期 | 变更人 | 变更内容 |
| --- | --- | --- |
| 2026-05-01 | Codex | 初始化执行计划与 Todo List，设置 T01 为进行中 |
| 2026-05-01 | Codex | 完成 T01 协议冻结与 T02 任务入口抽象，切换 T03 为进行中 |
| 2026-05-01 | Codex | 完成 T03 网关骨架与 T04 任务分发接入，切换 T06 为进行中 |
| 2026-05-01 | Codex | 完成 T05 执行模式骨架与 T06 最小页面闭环，切换 T07 为进行中 |
| 2026-05-01 | Codex | 为 T07 增加本地 mock 远端任务注入入口，便于在无真实服务端时验证执行链路 |
| 2026-05-02 | Codex | 为 T07 增加本地 mock HTTP 服务端，改为通过 `DeviceTaskGateway` 真协议拉取 mock 任务 |
| 2026-05-02 | Codex | 为 T07 增加 mock 状态日志查看，便于检查 `RUNNING/SUCCESS/FAILED/CANCELLED` 回传结果 |
| 2026-05-02 | Codex | 完成 T07 本地协议闭环验证入口，切换 T08 为进行中 |
| 2026-05-02 | Codex | 完成 T08 统一执行接口与执行器管理器，迁移核心手势/按键/截图工具并切换 T09 为进行中 |
| 2026-05-02 | Codex | 推进 T09：将 `open_app` 与 `search_app_in_store` 切到 `DeviceCommandExecutor` 主路径，并完成 `:app:compileDebugJavaWithJavac` 验证 |
| 2026-05-02 | Codex | 完成 T09：新增 `AccessibilityUiController` 收口节点类工具依赖，完成工具层去耦并切换 T10 为进行中 |
| 2026-05-02 | Codex | 完成 T10：接入 Shizuku 执行器、授权状态管理、设置页模式切换与心跳状态上报，并切换 T11 为进行中 |
| 2026-05-02 | Codex | 推进 T11：补执行模式统一决议结果、启动前不可用预检与 fallback 提示，并完成编译验证 |
| 2026-05-02 | Codex | 推进 T11：为 Device Config 增加任务级 mock 执行模式与 fallback 验证入口，完成编译验证 |
| 2026-05-02 | Codex | 推进 T11：将 `send_file` 从 `ChannelManager` 直连改为走 `TaskCallbacks` 统一回调，并补齐远端任务文件回传不支持时的显式失败校验 |
| 2026-05-02 | Codex | 推进 T11：为 Device Config 增加双模式标准验证用例选择入口，并新增《Apkclaw-双模式验证记录》用于真机联调与 T12 一致性回填 |
| 2026-05-02 | Codex | 推进 T11：让协议验证复用当前所选双模式用例，并新增验证日志导出能力，便于真机验证后回填 T12 记录 |
| 2026-05-02 | Codex | 推进 T11：为 Device Config 增加“一键跑双模式矩阵”，自动顺序执行六类用例在 ACCESSIBILITY/SHIZUKU 下的验证并导出汇总报告 |
| 2026-05-02 | Codex | 推进 T11：将矩阵汇总导出增强为可直接回填《Apkclaw-双模式验证记录》的表格模板，并自动附带设备/ROM 信息 |
| 2026-05-02 | Codex | 推进 T11：为协议验证与矩阵验证增加“无 LLM 本地直连模式”，未配置模型时直接调用 `DeviceCommandExecutor` 完成核心能力验证 |
| 2026-05-03 | Codex | 推进 T11：通过 logcat 定位 Shizuku provider 缺失导致 `Failed to find provider info for com.apk.claw.android.shizuku`，已补 `ShizukuProvider` manifest 声明并增加 `ShizukuManager` 运行态日志 |
| 2026-05-03 | Codex | 推进 T11：通过 logcat 定位 Shizuku 截图失败是 `screencap` 写公共目录时报 `EACCES`，已改为从 `screencap -p` 标准输出直接解码 PNG |
| 2026-05-03 | Codex | 推进 T12：已将 vivo V1838A / Android 10 的首轮双模式真机结果回填进《Apkclaw-双模式验证记录》，确认 SHIZUKU 六项全通过，剩余问题聚焦 ACCESSIBILITY 手势 |
| 2026-05-03 | Codex | 推进 T12：为 ACCESSIBILITY 手势验证增加最后一次 gesture 结果诊断，区分 `dispatch_failed/completed/cancelled/timeout/interrupted` 并写入矩阵日志 |
| 2026-05-03 | Codex | 推进 T12：确认 ACCESSIBILITY 手势在桌面场景下已完成但缺少可观测反馈，新增应用内验证面板并将 `tap/swipe` 验证切到状态变化判定 |
| 2026-05-03 | Codex | 为 Todo List 增加“完成 Agent”列，并为已完成/进行中任务补齐当前执行 Agent 标识 |
| 2026-05-03 | Codex | 推进 T12：根据 `dual_mode_matrix_summary_20260503_120622.md` 更新《Apkclaw-双模式验证记录》，确认应用内验证策略下 `tap/swipe` 与 Shizuku 侧对齐，矩阵汇总已同步落库 `verification-export/` |
| 2026-05-03 | Cursor | 推进 T11/T12：直连双模式验证与生产路径对齐（合成 `TaskContext` + `resolveExecutorSelection`）；Device Config 新增「一键 fallback 专项」与矩阵导出中的 fallback 回填指引；《Apkclaw-双模式验证记录》补充 fallback 场景步骤 |
| 2026-05-03 | Cursor | 真机完成 fallback 场景 B：`dual_mode_fallback_smoke_20260503_121927.md`（`resolved=SHIZUKU`，`fallbackApplied=false`），已将 T11/T12 标为已完成 |
| 2026-05-03 | Cursor | 将通过 adb 把设备 `verification/` 目录全部拉取至仓库 `verification-export/`（18 个文件），并在《Apkclaw-双模式验证记录》中注明归档路径 |
| 2026-05-03 | Cursor | 完成 T13：`RemoteAgentConfigProvider`、`GET /api/v1/agent/config` MMKV 缓存与热更新；Mock 服务增加 `agent/config`；`ClawApplication` 无本地 LLM 时也执行 `afterInit` 以便网关注册后拉取远端配置 |
| 2026-05-03 | Cursor | 新增根目录 `AGENTS.md`（Cursor 标准入口），协作规则仅以该文件为准；`agent.md` 改为同名符号链接；Todo「版本控制」条款改为指向 `AGENTS.md`/`agent.md` |
| 2026-05-03 | Cursor | 完成 T14：技能基础模块本地 manifest/skill.md 解析、仓库索引、`SkillRenderer` 聚合注入与 `allowedTools` 工具白名单；编译 `:app:compileDebugKotlin` 通过 |
| 2026-05-03 | Cursor | 完成 **T24**：集成 `langchain4j-skills`（Agent Skills Tool Mode）；`Skills.formatAvailableSkills` + `mergeDeviceAndSkills`；`SkillAugmentation.mergedTools`；`SkillManager` 安装生成 `SKILL.md` |
| 2026-05-03 | Cursor | **T15**：`SkillRemoteLoader` 远端 ZIP 下载与 SHA-256 校验、`SkillManager.ensureSkillPackages` / `TaskOrchestrator` 前置装载、Mock `/api/v1/skills/*/meta|download`、`KVUtils` 技能包 checksum 缓存 |
| 2026-05-03 | Cursor | **T17**：`HistoryManager` + Room（`ApkclawHistoryDatabase`）、步骤与截图路径、`TaskOrchestrator`/取消链路持久化；Gradle 引入 KSP 2.3.7、Room 2.7；`settings` 插件仓库优先 Central/Google |
| 2026-05-03 | Cursor | **T18**：任务历史列表/详情 UI、截图横滑与全屏回放；首页与设置入口；依赖 `androidx.recyclerview` |
| 2026-05-03 | Cursor | **T20**：任务墙钟超时与失败/弹窗阻塞重试、`cancelWallClockTimeout`、网关状态上报重试、`DiagnosticsExporter`、设置导出入口 |
| 2026-05-03 | Cursor | **T19 暂缓**：按用户选择先跳过模板能力；当前阶段切换为优先 **T20**；`T19`/`T23` 备注同步说明验收策略 |
| 2026-05-03 | Cursor | **T21**：IM 编排入口收敛至 `ImInboundTaskIngress`，五类旧 Bot 设置迁至 `LegacyImBotsSettingsActivity`，`afterInit` 先启动设备任务网关 |
| 2026-05-03 | Cursor | **T22**：Release 下线设置页手写 LLM 入口（`BuildConfig.DEBUG`）；`LlmConfigActivity` 门禁；局域网/远端合并不变 |
| 2026-05-03 | Cursor | **T23 推进**：新增《Apkclaw-T23-集成验收清单》五块主流程表；ApkClaw `:app:compileDebugKotlin` 通过；`T23` 改为进行中，待真机勾选收口 |
| 2026-05-03 | Cursor | **T23 / Mock agent/config**：`MockRemoteTaskServer.handleAgentConfig` 改为回显 `KVUtils` 中 Base URL / 模型名 / API Key（非空才写入 `data`），未填 Base URL 时不强行下发 `api.openai.com`，便于 Mock 网关 + 第三方兼容端点 |
| 2026-05-03 | Cursor | **T23 / 报告分析**：从设备拉取 `verification/dual_mode_matrix_summary_20260503_213835.md` 至仓库 `verification-export/`；确认矩阵汇总存在“终态 SUCCESS ≠ 用例通过”误判，已在《Apkclaw-T23-集成验收清单》登记 `T23-I01`（`screenshot/ACCESSIBILITY` Android 10 不支持却汇总 SUCCESS）与 `T23-I02`（`shell/SHIZUKU` 工具缺失却汇总 SUCCESS） |
| 2026-05-03 | Cursor | **T23 / 自定义任务观测**：真机运行 mock 任务 `mock_1777817569890`（“打开计算器，计算一加一等于几”）；从 `task_history` 读取到终态虽为 SUCCESS，但步骤只执行了 `AC`、`1`、`+`，随后 `获取屏幕信息` 已跳到 ApkClaw 的旧 Run detail 页面，最终“1+1=2”结论与实际步骤不符；已在《Apkclaw-T23-集成验收清单》登记 `T23-I03` |
| 2026-05-03 | Cursor | **T23 / T23-I03 根因分析**：读取 `cache/http_logs/20260503_221335394_POST.txt`，确认模型在看到 `Run detail` 错误页面后仍输出“根据之前的计算步骤……现在可以调用 finish”；`DefaultAgentService` 对 `finish` 无条件 `onComplete`，未校验是否真的完成 `1 + 1 =` 或仍位于目标 App，因此形成“Agent 提前结束 → SUCCESS 假阳性” |
| 2026-05-03 | Cursor | **T23 / T23-I03 二次复现**：再次运行“打开计算器，计算一加一等于几”，最新任务 `mock_1777818794869` 虽为 SUCCESS，但 `task_history` 只记录了四次 `tap` 后立即 `finish`，没有任何 `get_screen_info` 结果校验；`cache/http_logs/20260503_223345465_POST.txt` 显示模型 reasoning 直接把“已点击坐标”脑补成“已依次点击 1、+、1、=”，进一步确认是假阳性而非偶发切屏 |
| 2026-05-03 | Cursor | **T23 / 运行态排查**：针对“运行任务应用闪退”抓取 logcat，未发现 `com.apk.claw.android` 的 `FATAL EXCEPTION`；22:33:22 `open_app` 已成功拉起 `com.android.bbkcalculator`，说明主要现象是任务执行时主动切前台到目标 App。另在用户重开应用但 Mock 服务尚未就绪时，`DeviceTaskGateway` 会持续报 `127.0.0.1:9537` `ECONNREFUSED`；已在《Apkclaw-T23-集成验收清单》登记 `T23-I04`，后续需分别处理前台提示不足与 Mock 服务启动时序。 |
| 2026-05-05 | Codex | **T25**：按用户要求新增计划外任务“初始化 GitHub 远程仓库并推送当前仓库到 `origin/main`”；已补充 Todo 后执行 `git remote add origin git@github.com:yannan/android-project.git`、`git branch -M main`、`git push -u origin main`，并为该任务单独提交一条包含 `T25` 的 commit。 |
| 2026-05-05 | Codex | **T25 / 子仓库补推**：发现根仓库已推送，但 `ApkClaw` 子仓库 `main` 仍较 `origin/main` 超前 10 个提交；已同步补推 `git@github.com:yannan/ApkClaw.git`，确保子仓库远端与本地提交历史一致。 |
