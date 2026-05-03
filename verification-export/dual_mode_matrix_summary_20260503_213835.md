# Dual Mode Verification Matrix Summary

- exportedAt: 2026-05-03 21:38:35
- totalRuns: 12
- allowModeFallback: false
- device: vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys

| Case | Mode | Fallback | Task ID | Terminal | Latest Message |
| --- | --- | --- | --- | --- | --- |
| tap | ACCESSIBILITY | false | mock_1777815268067 | SUCCESS | Task completed |
| tap | SHIZUKU | false | mock_1777815281397 | SUCCESS | Task completed |
| swipe | ACCESSIBILITY | false | mock_1777815295878 | SUCCESS | Task completed |
| swipe | SHIZUKU | false | mock_1777815345402 | SUCCESS | Task completed |
| open_app | ACCESSIBILITY | false | mock_1777815374777 | SUCCESS | Task completed |
| open_app | SHIZUKU | false | mock_1777815398795 | SUCCESS | Task completed |
| screenshot | ACCESSIBILITY | false | mock_1777815423777 | SUCCESS | Task completed |
| screenshot | SHIZUKU | false | mock_1777815431716 | SUCCESS | Task completed |
| key | ACCESSIBILITY | false | mock_1777815441857 | SUCCESS | Task completed |
| key | SHIZUKU | false | mock_1777815469097 | SUCCESS | Task completed |
| shell | ACCESSIBILITY | false | mock_1777815486636 | SUCCESS | Task completed |
| shell | SHIZUKU | false | mock_1777815504277 | SUCCESS | Task completed |

## Paste Into Validation Record

| 日期 | 设备/ROM | 用例 | 请求模式 | fallback | 终态 | 页面观察 | 状态日志摘要 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `tap` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `swipe` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `open_app` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `screenshot` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `key` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `ACCESSIBILITY` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `shell` | `SHIZUKU` | `false` | SUCCESS | 待补充 | Task completed | 待补充 |

## Fallback Record Template

| 日期 | 设备/ROM | 请求模式 | fallback | 前置故障注入 | 终态 | 实际解析模式 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `SHIZUKU` | `true` | 待补充 | 待补充 | 待补充 | 待补充 |

- 推荐使用 Device Config 页的「一键 fallback 专项」：直连、`SHIZUKU`、固定 `tap`，关闭或撤销 Shizuku 后执行，导出 `dual_mode_fallback_smoke_*.md` 回填本表。

## Raw Mock Statuses
- [2026-05-03T13:35:56.906Z] task=mock_1777815345402 status=RUNNING msg=[滑动{"start_x": 540, "start_y": 1800, "end_x": 540, "end_y": 1000, "wait_after": 2000}]：成功
- [2026-05-03T13:35:56.924Z] task=mock_1777815345402 status=RUNNING msg=[滑动{"start_x": 540, "start_y": 1800, "end_x": 540, "end_y": 1000, "wait_after": 2000}]：成功
- [2026-05-03T13:35:56.973Z] task=mock_1777815345402 status=RUNNING msg=Round 2 started
- [2026-05-03T13:36:01.253Z] task=mock_1777815345402 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:01.263Z] task=mock_1777815345402 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:01.315Z] task=mock_1777815345402 status=RUNNING msg=Round 3 started
- [2026-05-03T13:36:12.159Z] task=mock_1777815345402 status=RUNNING msg=完成任务: 已完成从屏幕中下部向上滑动的操作，等待页面稳定后检查发现：当前处于主屏幕界面，该界面仅支持左右分页切换，上下滑动未触发页面滚动，页面内容无变化。
- [2026-05-03T13:36:12.180Z] task=mock_1777815345402 status=SUCCESS msg=Task completed
- [2026-05-03T13:36:14.881Z] task=mock_1777815374777 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:36:14.944Z] task=mock_1777815374777 status=RUNNING msg=Task started
- [2026-05-03T13:36:15.003Z] task=mock_1777815374777 status=RUNNING msg=Round 1 started
- [2026-05-03T13:36:18.313Z] task=mock_1777815374777 status=RUNNING msg=[打开应用{"package_name": "com.android.calculator2", "wait_after": 2000}]：失败
- [2026-05-03T13:36:18.321Z] task=mock_1777815374777 status=RUNNING msg=[打开应用{"package_name": "com.android.calculator2", "wait_after": 2000}]：失败
- [2026-05-03T13:36:18.364Z] task=mock_1777815374777 status=RUNNING msg=Round 2 started
- [2026-05-03T13:36:21.532Z] task=mock_1777815374777 status=RUNNING msg=[获取应用列表{"keyword": "计算器"}]：成功
- [2026-05-03T13:36:21.538Z] task=mock_1777815374777 status=RUNNING msg=[获取应用列表{"keyword": "计算器"}]：成功
- [2026-05-03T13:36:21.585Z] task=mock_1777815374777 status=RUNNING msg=Round 3 started
- [2026-05-03T13:36:27.675Z] task=mock_1777815374777 status=RUNNING msg=[打开应用{"package_name": "com.android.bbkcalculator", "wait_after": 2000}]：成功
- [2026-05-03T13:36:27.695Z] task=mock_1777815374777 status=RUNNING msg=[打开应用{"package_name": "com.android.bbkcalculator", "wait_after": 2000}]：成功
- [2026-05-03T13:36:27.756Z] task=mock_1777815374777 status=RUNNING msg=Round 4 started
- [2026-05-03T13:36:29.878Z] task=mock_1777815374777 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:29.891Z] task=mock_1777815374777 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:29.943Z] task=mock_1777815374777 status=RUNNING msg=Round 5 started
- [2026-05-03T13:36:33.946Z] task=mock_1777815374777 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:36:33.970Z] task=mock_1777815374777 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:36:34.084Z] task=mock_1777815374777 status=RUNNING msg=Round 6 started
- [2026-05-03T13:36:37.033Z] task=mock_1777815374777 status=RUNNING msg=完成任务: 已成功打开系统计算器（包名：com.android.bbkcalculator），确认应用已正常启动，随后已返回桌面，任务完成。
- [2026-05-03T13:36:37.048Z] task=mock_1777815374777 status=SUCCESS msg=Task completed
- [2026-05-03T13:36:38.899Z] task=mock_1777815398795 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:36:39.376Z] task=mock_1777815398795 status=RUNNING msg=Task started
- [2026-05-03T13:36:39.388Z] task=mock_1777815398795 status=RUNNING msg=Round 1 started
- [2026-05-03T13:36:42.320Z] task=mock_1777815398795 status=RUNNING msg=[获取应用列表{"keyword": "计算器"}]：成功
- [2026-05-03T13:36:42.324Z] task=mock_1777815398795 status=RUNNING msg=[获取应用列表{"keyword": "计算器"}]：成功
- [2026-05-03T13:36:42.367Z] task=mock_1777815398795 status=RUNNING msg=Round 2 started
- [2026-05-03T13:36:50.880Z] task=mock_1777815398795 status=RUNNING msg=[打开应用{"package_name": "com.android.bbkcalculator", "wait_after": 2000}]：成功
- [2026-05-03T13:36:50.894Z] task=mock_1777815398795 status=RUNNING msg=[打开应用{"package_name": "com.android.bbkcalculator", "wait_after": 2000}]：成功
- [2026-05-03T13:36:50.940Z] task=mock_1777815398795 status=RUNNING msg=Round 3 started
- [2026-05-03T13:36:53.231Z] task=mock_1777815398795 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:53.242Z] task=mock_1777815398795 status=RUNNING msg=[获取屏幕信息{}]：成功
- [2026-05-03T13:36:53.294Z] task=mock_1777815398795 status=RUNNING msg=Round 4 started
- [2026-05-03T13:36:58.537Z] task=mock_1777815398795 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 2000}]：成功
- [2026-05-03T13:36:58.554Z] task=mock_1777815398795 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 2000}]：成功
- [2026-05-03T13:36:58.601Z] task=mock_1777815398795 status=RUNNING msg=Round 5 started
- [2026-05-03T13:37:01.810Z] task=mock_1777815398795 status=RUNNING msg=完成任务: 已成功打开系统计算器（包名：com.android.bbkcalculator），确认应用已正常启动，随后已返回手机桌面。
- [2026-05-03T13:37:01.825Z] task=mock_1777815398795 status=SUCCESS msg=Task completed
- [2026-05-03T13:37:03.858Z] task=mock_1777815423777 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:37:03.912Z] task=mock_1777815423777 status=RUNNING msg=Task started
- [2026-05-03T13:37:03.964Z] task=mock_1777815423777 status=RUNNING msg=Round 1 started
- [2026-05-03T13:37:06.417Z] task=mock_1777815423777 status=RUNNING msg=[截屏{}]：失败
- [2026-05-03T13:37:06.434Z] task=mock_1777815423777 status=RUNNING msg=[截屏{}]：失败
- [2026-05-03T13:37:06.494Z] task=mock_1777815423777 status=RUNNING msg=Round 2 started
- [2026-05-03T13:37:09.371Z] task=mock_1777815423777 status=RUNNING msg=完成任务: 截图失败，因为当前设备系统版本为Android 10（API 29），而截图功能需要Android 11+（API 30）及以上系统版本支持。
- [2026-05-03T13:37:09.382Z] task=mock_1777815423777 status=SUCCESS msg=Task completed
- [2026-05-03T13:37:11.874Z] task=mock_1777815431716 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:37:12.341Z] task=mock_1777815431716 status=RUNNING msg=Task started
- [2026-05-03T13:37:12.354Z] task=mock_1777815431716 status=RUNNING msg=Round 1 started
- [2026-05-03T13:37:16.415Z] task=mock_1777815431716 status=RUNNING msg=[截屏{}]：成功
- [2026-05-03T13:37:16.422Z] task=mock_1777815431716 status=RUNNING msg=[截屏{}]：成功
- [2026-05-03T13:37:16.467Z] task=mock_1777815431716 status=RUNNING msg=Round 2 started
- [2026-05-03T13:37:19.399Z] task=mock_1777815431716 status=RUNNING msg=完成任务: 截图已成功完成，截图文件保存路径为：/data/user/0/com.apk.claw.android/cache/screenshots/1777815435504.png
- [2026-05-03T13:37:19.409Z] task=mock_1777815431716 status=SUCCESS msg=Task completed
- [2026-05-03T13:37:21.959Z] task=mock_1777815441857 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:37:22.023Z] task=mock_1777815441857 status=RUNNING msg=Task started
- [2026-05-03T13:37:22.080Z] task=mock_1777815441857 status=RUNNING msg=Round 1 started
- [2026-05-03T13:37:29.324Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:29.333Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:29.456Z] task=mock_1777815441857 status=RUNNING msg=Round 2 started
- [2026-05-03T13:37:37.634Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "recent_apps", "wait_after": 1000}]：成功
- [2026-05-03T13:37:37.640Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "recent_apps", "wait_after": 1000}]：成功
- [2026-05-03T13:37:37.694Z] task=mock_1777815441857 status=RUNNING msg=Round 3 started
- [2026-05-03T13:37:43.684Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:43.710Z] task=mock_1777815441857 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:43.761Z] task=mock_1777815441857 status=RUNNING msg=Round 4 started
- [2026-05-03T13:37:46.460Z] task=mock_1777815441857 status=RUNNING msg=完成任务: 已成功执行以下操作：1. 按下HOME键，成功回到桌面；2. 按下RECENTS键，成功打开最近任务界面；3. 再次按下HOME键，成功返回桌面。所有按键操作均执行成功。
- [2026-05-03T13:37:46.507Z] task=mock_1777815441857 status=SUCCESS msg=Task completed
- [2026-05-03T13:37:49.201Z] task=mock_1777815469097 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:37:49.683Z] task=mock_1777815469097 status=RUNNING msg=Task started
- [2026-05-03T13:37:49.695Z] task=mock_1777815469097 status=RUNNING msg=Round 1 started
- [2026-05-03T13:37:54.160Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:54.169Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:37:54.224Z] task=mock_1777815469097 status=RUNNING msg=Round 2 started
- [2026-05-03T13:37:58.107Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "recent_apps", "wait_after": 1000}]：成功
- [2026-05-03T13:37:58.126Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "recent_apps", "wait_after": 1000}]：成功
- [2026-05-03T13:37:58.182Z] task=mock_1777815469097 status=RUNNING msg=Round 3 started
- [2026-05-03T13:38:01.810Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:38:01.901Z] task=mock_1777815469097 status=RUNNING msg=[系统按键{"key": "home", "wait_after": 1000}]：成功
- [2026-05-03T13:38:01.962Z] task=mock_1777815469097 status=RUNNING msg=Round 4 started
- [2026-05-03T13:38:04.886Z] task=mock_1777815469097 status=RUNNING msg=完成任务: 已成功完成所有按键操作：1. 按下HOME键，成功返回桌面；2. 按下RECENTS键，成功打开最近任务界面；3. 再次按下HOME键，成功返回桌面。
- [2026-05-03T13:38:04.900Z] task=mock_1777815469097 status=SUCCESS msg=Task completed
- [2026-05-03T13:38:06.736Z] task=mock_1777815486636 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:38:06.798Z] task=mock_1777815486636 status=RUNNING msg=Task started
- [2026-05-03T13:38:06.859Z] task=mock_1777815486636 status=RUNNING msg=Round 1 started
- [2026-05-03T13:38:21.656Z] task=mock_1777815486636 status=RUNNING msg=完成任务: 命令 `getprop ro.build.version.release` 的执行输出结果为：10（对应设备的Android版本为10）
- [2026-05-03T13:38:21.681Z] task=mock_1777815486636 status=SUCCESS msg=Task completed
- [2026-05-03T13:38:24.346Z] task=mock_1777815504277 status=RECEIVED msg=Task pulled by device
- [2026-05-03T13:38:24.829Z] task=mock_1777815504277 status=RUNNING msg=Task started
- [2026-05-03T13:38:24.846Z] task=mock_1777815504277 status=RUNNING msg=Round 1 started
- [2026-05-03T13:38:33.573Z] task=mock_1777815504277 status=RUNNING msg=当前可用工具中无执行 shell 命令的功能，无法完成该命令执行请求。
- [2026-05-03T13:38:33.589Z] task=mock_1777815504277 status=RUNNING msg=当前可用工具中无执行 shell 命令的功能，无法完成该命令执行请求。
- [2026-05-03T13:38:33.644Z] task=mock_1777815504277 status=SUCCESS msg=Task completed
