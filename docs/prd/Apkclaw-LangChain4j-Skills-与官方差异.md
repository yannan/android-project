# Apkclaw 与 LangChain4j Skills 官方机制的差异说明（版本限制）

本文记录 ApkClaw 当前集成 **LangChain4j Skills（Tool Mode）** 时与官方文档/tutorial 行为的差异及原因，便于升级依赖或排障时查阅。

## 当前依赖基线

| 组件 | 版本 |
|------|------|
| `langchain4j-core` 等主链 | `1.12.2`（release） |
| `langchain4j-skills` | `1.12.2-beta22`（与 1.12.2 core 同基线） |

官方文档中部分示例使用更新 beta（如 `1.14.0-beta24`）；**API 与行为以本仓库实际解析到的 JAR 为准**。

## 与官方一致的部分

- **Tool Mode 语义**：技能内容在构建时载入内存；模型通过 **`activate_skill`** 拉取指令，通过 **`read_skill_resource`** 读取预载入的 `SkillResource`；无 Shell Mode 下的文件系统遍历。
- **`Skills.formatAvailableSkills()`**：以 XML 片段形式注入 system 侧技能目录（名称与描述）。
- **技能包目录互操作**：本地安装时在 `files/skills/{id}/` 下除 `manifest.json`、`skill.md` 外，**生成顶层 `SKILL.md`（YAML front matter）**，对齐 [Agent Skills](https://agentskills.io/) 的常见文件布局，便于与社区技能包或官方 `FileSystemSkillLoader` 思路对照（运行时仍以程序化 `Skill.builder()` 为准）。

## 与官方文档的差异（版本 / 架构限制）

### 1. Skill-scoped tools（按技能激活后才暴露工具）

- **官方较新版**支持：将部分工具挂在 **单个 `Skill` 上**，仅在模型调用 `activate_skill` 之后才进入工具列表，从而减少无关工具暴露（见 LangChain4j Skills tutorial 中 *Skill-Scoped Tools* 小节）。
- **本仓库当前 `1.12.2-beta22`**：`DefaultSkill`/`Skill.builder()` 在本基线**不包含**上述链式 API；因此 **未实现**「仅激活后可见」的技能专属工具列表。
- **ApkClaw 折中**：通过各技能 `manifest.json` 的 **`allowedTools` 交集**（并强制保留 `finish`、`get_screen_info`），在 **`ToolProviderResult` 层面裁剪设备侧 LangChain `ToolSpecification`**，与 **`Skills.toolProvider()`** 产出的 **`activate_skill` / `read_skill_resource`** **合并**。效果是「任务级白名单 + 激活技能内容」，语义接近但不等同于官方的 skill-scoped 动态挂载。

### 2. AiServices 与 ToolProviderRequest

- 官方推荐路径多为 **`AiServices.builder(...).toolProvider(skills.toolProvider())`**，由框架在回合间解析 **动态 ToolProvider**（尤其与 skill-scoped 组合时）。
- ApkClaw 仍为 **手写多轮 Agent 循环**（`DefaultAgentService` + `LlmClient`），在每轮开始时使用 **预先合并好的** `ToolProviderResult.tools()` 生成 `toolSpecs`，并通过 **`ToolExecutor`** 派发执行；因此未使用 `AiServices` 的整条装配链。**本基线下 `Skills.toolProvider()` 对请求近似无分支**，占位 `ToolProviderRequest` 仅用于保持 API 形状一致。

### 3. 非 JSON 工具返回与历史消息

- `activate_skill` / `read_skill_resource` 的返回值往往不是 `ToolRegistry` 的 `{ isSuccess, data, error }` JSON。
- Agent 在历史中写入 **LangChain 工具原始返回字符串**，应用内状态（如 `finish`、系统弹窗检测）再通过 **`parseToolOutcome`**：非 JSON 按成功正文处理 JSON 再走 `ToolResult.fromToolRegistryJson`。

### 4. Experimental 与未来升级

- LangChain4j Skills 标明 **experimental**；升级 **`langchain4j-skills` / BOM** 时可能出现 **破坏性 API 变更**。
- 若将来升级到支持 **skill-scoped tools** 的版本，可考虑：将 `allowedTools` 映射为 **`Skill` 挂载的 specifications + ToolExecutor**，并逐步收窄任务级全局设备工具列表。

## 相关代码入口（便于对照）

| 用途 | 位置 |
|------|------|
| 依赖版本 | `ApkClaw/gradle/libs.versions.toml`（`langchain4j`、`langchain4j-skills`） |
| Skill 构建与合并 | `ApkClaw/.../skill/SkillManager.kt`、`LangChainSkillRuntime.kt` |
| 设备工具 `ToolProviderResult` | `ApkClaw/.../agent/langchain/LangChain4jToolBridge.java`（`buildDeviceToolProviderResult`） |
| 执行派发 | `ApkClaw/.../agent/DefaultAgentService.kt` |
| Todo 跟踪 | `docs/prd/Apkclaw-二开-执行计划-TODO.md`（**T24**） |
