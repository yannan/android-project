# Dual Mode Verification Matrix Summary

- exportedAt: 2026-05-03 10:53:10
- totalRuns: 12
- allowModeFallback: false
- device: vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys

| Case | Mode | Fallback | Task ID | Terminal | Latest Message |
| --- | --- | --- | --- | --- | --- |
| tap | ACCESSIBILITY | false | local_direct | FAILED | tap(216,468), first=false, retry=true, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.bbk.launcher2 |
| tap | SHIZUKU | false | local_direct | SUCCESS | tap(216,468), first=true, retry=false, mode=SHIZUKU |
| swipe | ACCESSIBILITY | false | local_direct | FAILED | swipe(540,1755->540,819), first=false, retry=true, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.bbk.launcher2 |
| swipe | SHIZUKU | false | local_direct | SUCCESS | swipe(540,1755->540,819), first=true, retry=false, mode=SHIZUKU |
| open_app | ACCESSIBILITY | false | local_direct | SUCCESS | launchApp(com.android.bbkcalculator), home=true |
| open_app | SHIZUKU | false | local_direct | SUCCESS | launchApp(com.android.bbkcalculator), home=true |
| screenshot | ACCESSIBILITY | false | local_direct | FAILED | Accessibility screenshot requires Android 11+ (current SDK 29) |
| screenshot | SHIZUKU | false | local_direct | FAILED | screenshot failed |
| key | ACCESSIBILITY | false | local_direct | SUCCESS | home=true, recent_apps=true, home_again=true |
| key | SHIZUKU | false | local_direct | SUCCESS | home=true, recent_apps=true, home_again=true |
| shell | ACCESSIBILITY | false | local_direct | SUCCESS | exitCode=0, stdout=10 |
| shell | SHIZUKU | false | local_direct | SUCCESS | exitCode=0, stdout=10 |

## Paste Into Validation Record

| 日期 | 设备/ROM | 用例 | 请求模式 | fallback | 终态 | 页面观察 | 状态日志摘要 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `ACCESSIBILITY` | `false` | FAILED | 待补充 | tap(216,468), first=false, retry=true, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.bbk.launcher2 | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `SHIZUKU` | `false` | SUCCESS | 待补充 | tap(216,468), first=true, retry=false, mode=SHIZUKU | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `ACCESSIBILITY` | `false` | FAILED | 待补充 | swipe(540,1755->540,819), first=false, retry=true, mode=ACCESSIBILITY, hasWindow=true, activePkg=com.bbk.launcher2 | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `SHIZUKU` | `false` | SUCCESS | 待补充 | swipe(540,1755->540,819), first=true, retry=false, mode=SHIZUKU | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | launchApp(com.android.bbkcalculator), home=true | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `SHIZUKU` | `false` | SUCCESS | 待补充 | launchApp(com.android.bbkcalculator), home=true | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `ACCESSIBILITY` | `false` | FAILED | 待补充 | Accessibility screenshot requires Android 11+ (current SDK 29) | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `SHIZUKU` | `false` | FAILED | 待补充 | screenshot failed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | home=true, recent_apps=true, home_again=true | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `SHIZUKU` | `false` | SUCCESS | 待补充 | home=true, recent_apps=true, home_again=true | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | exitCode=0, stdout=10 | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `SHIZUKU` | `false` | SUCCESS | 待补充 | exitCode=0, stdout=10 | 待补充 |

## Fallback Record Template

| 日期 | 设备/ROM | 请求模式 | fallback | 前置故障注入 | 终态 | 实际解析模式 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `SHIZUKU` | `true` | 待补充 | 待补充 | 待补充 | 待补充 |

## Raw Mock Statuses
