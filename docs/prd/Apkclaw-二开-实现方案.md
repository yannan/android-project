# ApkClaw 二开实现方案

## 1. 目标定义

本次二开的核心不是在 `ApkClaw` 上“继续增加一个渠道”，而是把产品形态从“外部 IM 渠道驱动的远程控机 Agent”调整为“自有 App 派单 + 手机本地执行 + 服务器托管模型配置/技能包”的单机任务执行器。

目标落地后，终端用户的主路径应为：

1. 在自有 App 里绑定账号并下发任务
2. 二开后的 ApkClaw 拉取任务并展示执行态
3. Agent 根据服务器下发的模型参数、技能包执行任务
4. 设备操作支持无障碍模式与 Shizuku 模式，由用户或任务配置选择
5. 本地保留任务历史、模板、日志和技能缓存

## 2. 需求拆解

结合 PRD，需求可以拆成 6 个能力域。

### 2.1 控制入口切换

- 删除“钉钉/飞书/QQ/Telegram 等外部渠道”作为核心入口
- 新增“自有 App 渠道”
- 一个设备绑定一个账号
- 任务由服务端下发，不再依赖用户在聊天工具里直接发自然语言

### 2.2 执行能力升级

- 保留现有 Agent 循环能力
- 执行层从“纯无障碍”升级为“无障碍模式 / Shizuku 模式”双实现
- 支持用户在设置中选择默认执行模式
- 支持服务端任务按任务粒度指定执行模式
- 支持 Shell、截图、点击、滑动、按键、启动 App
- 兼容小米、华为、OPPO 等 ROM

### 2.3 模型配置改造

- 模型参数由服务端统一下发
- 本地不再暴露 OpenAI/Anthropic/BaseURL 手工配置入口
- 客户端只负责拉取、缓存、热更新

### 2.4 技能包机制

- 支持 Markdown 格式 skill
- skill 从服务端下载并保存在本地
- 任务中可指定使用某个技能包
- 当前至少落地微信 skill

### 2.5 产品形态改造

- 保留本地悬浮窗/执行态展示
- 增加任务模板、任务历史
- 增加“绑定账号/设备状态/任务收件箱”相关页面

### 2.6 运维与可靠性

- 设备在线状态上报
- 任务拉取、执行、回执闭环
- 失败重试、任务取消、日志追踪

## 3. 现有基础能力评估

现有 `ApkClaw` 已经具备可复用骨架，没必要推倒重来。

### 3.1 可直接复用

- `TaskOrchestrator`
  - 已有任务锁、生命周期管理、取消与回调
- `DefaultAgentService`
  - 已有多轮 Agent 循环、死循环检测、历史压缩、finish 收尾
- `ToolRegistry`
  - 已有工具注册机制，适合继续扩展双执行模式工具
- `FloatingCircleManager`
  - 已有执行态反馈入口
- `KVUtils`
  - 已有本地配置存储能力，可扩展到账号、设备、技能元数据

### 3.2 需要弱化或替换

- `ChannelManager`
  - 现状是多 IM 渠道聚合
  - 二开后应降级为“任务入口抽象层”，自有 App 渠道成为主实现
- `SettingsActivity`
  - 现状围绕各类 bot 渠道和 LLM 手工配置
  - 二开后要改为账号绑定、设备绑定、服务端配置状态、Shizuku 状态、技能管理
- `ConfigServerManager`
  - 当前是局域网页面配置信道
  - 二开后价值降低，可保留为 debug 能力，不再是主配置入口

### 3.3 当前最大差距

- 没有 Shizuku 执行层
- 没有执行模式选择与任务级模式覆盖机制
- 没有任务拉取/状态回传协议
- 没有本地任务历史中心
- 没有技能下载、索引、装载机制
- 没有账号绑定和设备注册

## 4. 总体方案

建议采用“保留 Agent 内核，替换入口并扩展双执行层”的方案，而不是从 `AutoGLM-For-Android` 整体迁移。

原因：

- `ApkClaw` 的 Agent 编排已经成熟，且更符合 PRD 中“完整 Agent 循环”的要求
- `AutoGLM` 的价值主要在 Shizuku 执行模型、单机交互和历史展示，可按模块吸收
- 整体迁移会把现有工具体系、消息回调、UI 组织方式全部打散，成本更高

建议的新架构如下：

```text
自有 App / 服务端
    -> 设备绑定 / 任务下发 / 模型配置 / 技能包分发
    -> DeviceTaskGateway
    -> TaskInbox / TaskDispatcher
    -> TaskOrchestrator
    -> DefaultAgentService
    -> ToolRegistry
    -> ExecutionModeResolver
    -> DeviceCommandExecutor
        -> AccessibilityExecutor
        -> ShizukuExecutor
    -> HistoryManager / SkillManager / TemplateManager
    -> 浮窗 / 首页 / 历史页 / 设置页
```

## 5. 模块设计

## 5.1 任务入口层

新增统一任务入口抽象，替代当前“聊天渠道就是入口”的设计。

### 建议新增模块

- `task/inbox/DeviceTaskGateway`
  - 负责与服务端通信
  - 提供设备注册、心跳、拉取任务、确认接单、回传执行结果
- `task/inbox/TaskDispatcher`
  - 将服务端任务转换为本地 `TaskOrchestrator.startNewTask(...)`
- `task/model/RemoteTask`
  - 包含 `taskId`、`content`、`skillPackageId`、`priority`、`timeout`、`callbackUrl/status`

### 实现建议

- MVP 阶段采用“客户端主动轮询”
  - 简单、稳定、易排错
- 后续再升级为 WebSocket / SSE 推送
- `TaskOrchestrator` 不要依赖聊天消息 ID
  - 抽象成 `TaskContext`
  - 字段建议包括 `taskId`、`source`、`replyTarget`、`skillIds`

## 5.2 设备执行层

这是本次二开最关键的新增模块。

### 设计原则

- 工具层不直接感知 Shizuku 或无障碍
- 同一组工具协议同时支持两种执行模式
- 模式选择是显式配置，不依赖隐式自动回退
- 执行模式可由“本地默认配置 + 任务覆盖配置”共同决定

### 建议新增抽象

- `device/DeviceCommandExecutor`
  - `tap(x, y)`
  - `longPress(x, y, duration)`
  - `swipe(start, end, duration)`
  - `pressKey(keyCode)`
  - `screenshot()`
  - `launchApp(packageName)`
  - `shell(command)`
- `device/ShizukuDeviceExecutor`
  - 参考 `AutoGLM` 的 `DeviceExecutor`
- `device/AccessibilityDeviceExecutor`
  - 基于现有 `ClawAccessibilityService`
- `device/ExecutionMode`
  - `ACCESSIBILITY`
  - `SHIZUKU`
- `device/ExecutionModeResolver`
  - 按“任务指定模式 > 用户默认模式 > 系统建议模式”决议最终执行模式

### 模式选择规则

- 设置页提供默认执行模式切换
- 任务模型 `RemoteTask` 增加 `executionMode` 可选字段
- 若任务未指定，则使用本地默认模式
- 若用户选择 `SHIZUKU` 但设备未授权，任务启动前直接报错并提示切换或授权
- 若用户选择 `ACCESSIBILITY`，则完全走现有无障碍执行链路

### 自动回退策略

- 不把“自动回退”当成主执行语义
- 可以增加可选开关：`allowModeFallback`
- 仅在用户明确开启时，允许 `SHIZUKU -> ACCESSIBILITY` 的失败回退
- 默认关闭，避免用户误判执行语义

### 工具改造方式

现有 `tap/swipe/long_press/open_app/take_screenshot/system_key` 等工具不要重写协议，只改底层调用：

- 旧：直接调用 `ClawAccessibilityService`
- 新：调用 `DeviceCommandExecutor`

这样可以最大限度复用 `ToolRegistry` 和 Agent 侧提示词，同时保持两种执行模式的外部行为一致。

## 5.3 Agent 与技能系统

PRD 虽然提到“单步自然语言任务”，但同时又要求服务端下发技能包、指定 skill、执行自动回复，这本质上还是多步任务系统。

因此建议：

- UI 层对用户暴露“单次任务输入/单次任务执行”
- 内核继续保留多轮 Agent
- skill 作为系统提示词和工具使用约束的增强层，而不是单独脚本引擎

### 建议新增模块

- `skill/SkillManager`
  - 负责下载、校验、缓存、启用 skill
- `skill/SkillRepository`
  - 管理本地 skill 索引
- `skill/SkillRenderer`
  - 将 Markdown skill 转为 Agent 可注入的提示词片段

### skill 建议结构

```text
skillId/
  manifest.json
  skill.md
  assets/
```

`manifest.json` 建议字段：

- `id`
- `name`
- `version`
- `targetApps`
- `description`
- `entryPrompt`
- `allowedTools`

### 技能装载策略

- 任务指定 `skillPackageId` 时，执行前加载对应 skill
- 将 skill 内容拼接到 `AgentConfig.systemPrompt` 之后
- 如 skill 声明 `allowedTools`，则对 `ToolRegistry` 做白名单裁剪

这样微信 skill 就能约束 Agent 只做消息发送、查找联系人、回复会话等动作。

## 5.4 模型配置中心

当前 `AgentConfig` 完全本地化，不符合 PRD。

### 建议改造

新增 `RemoteAgentConfigProvider`：

- 启动时拉取远端配置
- 失败时回退本地缓存
- 配置变化时热更新 `TaskOrchestrator.updateAgentConfig()`

### 远端配置字段建议

- `provider`
- `baseUrl`
- `apiKeyRef` 或服务端代理标识
- `modelName`
- `temperature`
- `maxIterations`
- `systemPromptPatch`
- `toolPolicy`

### 推荐做法

不要把真实第三方 `apiKey` 下发到客户端。

更合理的方式是：

- 客户端请求业务服务端
- 服务端再代理到模型服务

这样可以避免密钥泄漏、便于灰度和审计。如果短期无法代理，再退而求其次下发临时 token。

## 5.5 本地数据与历史

二开后需要具备本地可追溯能力，便于调试和用户查看。

### 建议新增模块

- `history/HistoryManager`
  - 可参考 `AutoGLM`
  - 存任务摘要、步骤、截图、耗时、最终结果
- `template/TemplateManager`
  - 管理任务模板
- `account/AccountManager`
  - 管理账号 token、设备 ID、绑定状态

### 本地存储建议

- 轻量配置：继续使用 MMKV
- 结构化记录：优先 Room
- 技能文件、截图、日志：文件系统

原因：

- 任务历史和步骤天然适合结构化查询
- 后续要做筛选、分页、失败分析，Room 比纯 JSON 文件更稳

## 5.6 UI 改造

建议不要照搬 `AutoGLM` 全量 UI，只吸收其“历史、模板、悬浮窗状态”的产品形态。

### 首页建议

- 设备绑定状态
- Shizuku 权限状态
- 无障碍状态
- 悬浮窗状态
- 最近任务状态
- “开始/暂停接单”开关

### 任务页建议

- 当前任务
- 模板快捷发起
- 当前执行模式展示
- 最近执行记录入口

### 历史页建议

- 任务列表
- 详情页
- 步骤瀑布流
- 截图回放

### 设置页建议

- 账号绑定
- 服务端地址
- 默认执行模式
- Shizuku 授权与状态
- 无障碍状态
- 模型配置状态
- 技能包管理
- 日志导出
- Debug 配置

## 6. 服务端接口建议

PRD 没给服务端协议，但客户端要落地必须先约定最小接口。

### 6.1 设备注册

- `POST /api/device/register`

请求体建议：

- `deviceId`
- `userToken`
- `brand`
- `model`
- `androidVersion`
- `appVersion`
- `capabilities`

### 6.2 心跳

- `POST /api/device/heartbeat`

字段建议：

- `deviceId`
- `online`
- `battery`
- `network`
- `shizukuReady`
- `accessibilityReady`
- `currentTaskId`

### 6.3 拉取任务

- `GET /api/device/tasks/pull?deviceId=...`

返回体建议：

- `taskId`
- `content`
- `skillPackageId`
- `executionMode`
- `configVersion`
- `timeoutSec`

### 6.4 状态回传

- `POST /api/device/tasks/{taskId}/status`

状态建议：

- `PENDING`
- `RUNNING`
- `WAITING_USER`
- `SUCCESS`
- `FAILED`
- `CANCELLED`

### 6.5 技能包下载

- `GET /api/skills/{skillId}/manifest`
- `GET /api/skills/{skillId}/bundle`

### 6.6 模型配置获取

- `GET /api/device/agent-config?deviceId=...`

## 7. 分阶段落地建议

建议按 4 个阶段实施，避免一次性改造过大。

### Phase 1：最小可跑通版本

目标：先打通“自有 App/服务端下发任务 -> ApkClaw 执行 -> 回传结果”。

范围：

- 新增设备注册、心跳、轮询拉任务
- 新增自有任务入口 `DeviceTaskGateway`
- `TaskOrchestrator` 支持非 IM 任务上下文
- 保留现有无障碍执行层
- 增加执行模式配置骨架，先落地 `ACCESSIBILITY`
- 下掉大部分外部渠道入口 UI

交付标准：

- 单设备可绑定账号
- 能收到并执行文本任务
- 能回传成功/失败状态

### Phase 2：Shizuku 执行层接入

目标：补齐双模式执行能力。

范围：

- 接入 Shizuku
- 增加 `ShizukuDeviceExecutor`
- 增加 `ExecutionModeResolver`
- 改造核心工具走统一执行接口
- 增加执行模式切换页、Shizuku 状态页和授权流程

交付标准：

- `tap/swipe/key/screenshot/open_app` 支持双实现
- 用户可选择无障碍模式或 Shizuku 模式
- 服务端任务可覆盖本地默认模式

### Phase 3：技能、模板、历史

目标：补齐可运营能力。

范围：

- skill 下载与缓存
- 微信 skill 落地
- 本地模板管理
- 历史记录与详情页

交付标准：

- 任务可指定 skill
- 用户可查看执行历史
- 支持模板快速发起

### Phase 4：体验与运维增强

目标：提升稳定性和可观测性。

范围：

- Push/WebSocket 下发
- 失败重试与超时取消
- 日志上传
- 远端灰度配置
- 设备在线面板

## 8. 关键风险与应对

### 风险 1：双执行模式导致工具行为一致性要求更高

应对：

- 核心工具建立统一返回协议
- 对 `tap/swipe/open_app/screenshot/input` 做双模式对齐测试
- Agent 提示词不要暴露底层差异，只暴露工具语义

### 风险 2：Shizuku 在不同 ROM 上行为不一致

应对：

- 将 Shizuku 定义为独立模式而不是全局唯一依赖
- 保留无障碍模式长期可用
- 建立 ROM 兼容性测试矩阵

### 风险 3：skill 仅用 Markdown 时约束力不足

应对：

- skill 增加 `manifest.json`
- Markdown 负责语义说明，manifest 负责结构化约束

### 风险 4：服务端直接下发第三方模型密钥存在泄漏风险

应对：

- 优先服务端代理模型调用
- 至少使用短期 token 和设备绑定

### 风险 5：任务入口从聊天渠道改为自有 App 后，`ChannelManager` 设计会变得别扭

应对：

- 不要继续扩充 `ChannelManager`
- 新建 `TaskSource` 抽象，逐步把 IM 渠道降为兼容实现

### 风险 6：PRD 中“单步任务”表述可能与“完整 Agent 循环”冲突

应对：

- 产品层定义为“单次任务”
- 技术层保留多轮 Agent
- 避免为了迎合文案而削弱执行能力

## 9. 推荐实施结论

推荐路线是：

1. 以 `ApkClaw` 为主干继续演进
2. 保留 `TaskOrchestrator + DefaultAgentService + ToolRegistry`
3. 新增 `TaskSource/DeviceTaskGateway`
4. 新增 `DeviceCommandExecutor` 与 `ExecutionModeResolver`，同时支持无障碍与 Shizuku 两种模式
5. 把技能、远端模型配置、历史记录做成独立模块
6. UI 重构为“绑定、接单、执行、历史、设置”五块，并提供模式选择入口

这条路线开发成本更可控，复用现有代码最多，也最符合 PRD 对完整 Agent 能力的要求。

## 10. 建议的开发顺序

1. 抽象 `TaskContext` 和 `TaskSource`，去掉 `TaskOrchestrator` 对消息渠道的硬依赖
2. 增加 `ExecutionMode` 配置与解析骨架，先跑通无障碍模式
3. 接入服务端任务轮询、设备注册、状态回传
4. 接入 Shizuku，并把核心工具迁移到 `DeviceCommandExecutor`
5. 完成账号绑定页、首页状态页、设置页改造，补齐模式切换入口
6. 增加历史记录、模板、技能缓存
7. 最后清理旧渠道和旧 LLM 手工配置页面
