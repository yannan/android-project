# Apkclaw 双模式验证记录

## 目的

- 用于记录 `ACCESSIBILITY` 与 `SHIZUKU` 两种执行模式在核心工具上的行为一致性。
- 本文档对应执行计划中的 `T11` 真机验证与 `T12` 双模式测试。
- 每次完成一轮验证后，必须回填结果、设备信息与异常说明。

## 验证前提

- 已在设备配置页完成：
  - 默认执行模式可切换
  - Mock 远端任务可下发
  - 协议验证可跑通
- 真机已具备以下条件：
  - `ACCESSIBILITY` 模式：无障碍服务已开启
  - `SHIZUKU` 模式：Shizuku 已连接且授权完成
- 建议使用同一台设备连续执行两种模式，避免环境噪声。

## 验证步骤

1. 打开 `Device Config` 页面。
2. 通过“选择双模式验证用例”依次选择 `tap / swipe / open_app / screenshot / key / shell`。
3. 先以 `ACCESSIBILITY` 模式运行一遍，再切到 `SHIZUKU` 模式运行同一用例。
4. 对每次任务记录：
   - 请求模式
   - `allowModeFallback` 是否开启
   - 终态 `SUCCESS/FAILED/CANCELLED`
   - 页面实际表现
   - Mock 状态日志中的关键消息
5. 若开启了 fallback，还要额外验证：
   - 请求 `SHIZUKU`
   - 关闭或撤销 Shizuku 能力
   - 确认是否按预期回退到 `ACCESSIBILITY`

## 一致性矩阵

| 用例 | 任务意图 | ACCESSIBILITY 预期 | SHIZUKU 预期 | 关注点 |
| --- | --- | --- | --- | --- |
| `tap` | 点击屏幕可见区域 | 能成功下发点击，页面有反馈或结果说明 | 能成功下发点击，页面有反馈或结果说明 | 坐标换算是否一致 |
| `swipe` | 执行一次纵向滑动 | 能滑动页面或给出未滚动说明 | 能滑动页面或给出未滚动说明 | 时长/距离是否一致 |
| `open_app` | 启动计算器并返回桌面 | 能启动 App 并返回 | 能启动 App 并返回 | 包名解析、启动稳定性 |
| `screenshot` | 截图并返回结果 | 成功截图或给出明确失败 | 成功截图或给出明确失败 | 截图耗时、图片可用性 |
| `key` | 执行 HOME/RECENTS 等系统键 | 能执行支持的系统键 | 能执行支持的系统键 | 键值映射是否一致 |
| `shell` | 执行只读 shell 命令 | 默认应失败或提示不支持 | 应成功返回 stdout | 模式能力边界是否清晰 |

## 结果记录模板

> **归档**：真机 `Android/data/com.apk.claw.android/files/verification/` 下的导出已同步至仓库目录 [`verification-export/`](../../verification-export/)（含矩阵汇总、`dual_mode_fallback_smoke_*.md`、`dual_mode_verification_*.md`）。

> **最新一轮汇总导出**：`dual_mode_matrix_summary_20260503_120622.md`（设备路径：`/storage/emulated/0/Android/data/com.apk.claw.android/files/verification/`，`exportedAt: 2026-05-03 12:06:22`，`allowModeFallback: false`，12 次顺序执行）。相较当日较早一轮（桌面 `com.bbk.launcher2` 场景），`tap` / `swipe` 在应用内验证面（`com.apk.claw.android`）上已可观测通过；桌面场景遗留差异不重复记入本表，如需对比可查仓库 `verification-export/dual_mode_matrix_summary_20260503_110826.md`。

| 日期 | 设备/ROM | 用例 | 请求模式 | fallback | 终态 | 页面观察 | 状态日志摘要 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `ACCESSIBILITY` | `false` | SUCCESS | 验证面内点击计数由 0 增至 1，可见反馈 | tap(540,714), first=true, retry=false, taps=0->1, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.apk.claw.android, gesture=completed | 通过（应用内可观测策略） |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `SHIZUKU` | `false` | SUCCESS | 同上 | tap(540,714), first=true, retry=false, taps=0->1, mode=SHIZUKU | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `ACCESSIBILITY` | `false` | SUCCESS | 验证面内滚动偏移由 0 增至 629 | swipe(540,1970->540,1317), first=true, retry=false, scroll=0->629, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.apk.claw.android, gesture=completed | 通过（应用内可观测策略） |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `SHIZUKU` | `false` | SUCCESS | 验证面内滚动偏移由 0 增至 705 | swipe(540,1970->540,1317), first=true, retry=false, scroll=0->705, mode=SHIZUKU | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `ACCESSIBILITY` | `false` | SUCCESS | 可启动 vivo 计算器并返回桌面 | launchApp(com.android.bbkcalculator), home=true | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `SHIZUKU` | `false` | SUCCESS | 可启动 vivo 计算器并返回桌面 | launchApp(com.android.bbkcalculator), home=true | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `ACCESSIBILITY` | `false` | FAILED | Android 10 下无障碍截图能力不支持，失败符合预期 | Accessibility screenshot requires Android 11+ (current SDK 29) | 能力边界清晰，按预期失败 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `SHIZUKU` | `false` | SUCCESS | 可成功返回截图尺寸 | screenshot 1080x2340 | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `ACCESSIBILITY` | `false` | SUCCESS | HOME/RECENTS/HOME 执行正常 | home=true, recent_apps=true, home_again=true | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `SHIZUKU` | `false` | SUCCESS | HOME/RECENTS/HOME 执行正常 | home=true, recent_apps=true, home_again=true | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `ACCESSIBILITY` | `false` | SUCCESS | 成功返回系统版本号（`getprop ro.build.version.release`，stdout 长度为 10） | exitCode=0, stdout=10 | 通过 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `SHIZUKU` | `false` | SUCCESS | 同上 | exitCode=0, stdout=10 | 通过 |

## fallback 补充记录

验证步骤（推荐）：

1. 确认未配置 LLM（与本仓库「直连矩阵」前提一致），或在未配置模型密钥的构建上验证。
2. 打开 Device Config，保持无障碍开启。
3. **场景 A（Shizuku 不可用 + 允许回退）**：关闭 Shizuku 或未授权，点击「一键 fallback 专项」；导出报告中的 `resolved` 应为 `ACCESSIBILITY`，`fallbackApplied=true`，`tap` 终态期望为 `SUCCESS`。
4. **场景 B（Shizuku 可用，基线）**：正常连接 Shizuku 后再点同一按钮；`resolved` 应为 `SHIZUKU`，`fallbackApplied=false`。

| 日期 | 设备/ROM | 请求模式 | fallback | 前置故障注入 | 终态 | 实际解析模式 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10（与同表矩阵一致；导出：`dual_mode_fallback_smoke_20260503_121608.md`） | `SHIZUKU` | `true` | Shizuku 未运行（`primaryUnavailable=Shizuku is not running`） | `SUCCESS` | `ACCESSIBILITY` | 场景 A 通过：`fallbackApplied=true`，验证面 tap 计数变化，`tap(540,714)` + gesture 诊断链路与无障碍一致 |
| 2026-05-03 | vivo V1838A / Android 10（与同表矩阵一致；导出：`dual_mode_fallback_smoke_20260503_121927.md`） | `SHIZUKU` | `true` | Shizuku 已运行且已授权（无故障注入） | `SUCCESS` | `SHIZUKU` | 场景 B 通过：`fallbackApplied=false`，`tap(540,714)`，`mode=SHIZUKU`，与决议逻辑一致 |

## 结论口径

- 若两种模式都完成了同一意图，允许实现细节不同，但最终用户可见结果应一致。
- 若某模式天然不支持某能力，例如 `ACCESSIBILITY` 下的 `shell`，必须输出明确失败原因。
- 任何“工具返回成功，但页面/结果无变化”的情况都记为不一致，需要继续排查。
