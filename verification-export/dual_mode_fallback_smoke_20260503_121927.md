# Fallback smoke (direct)

- case: tap
- requestedMode: SHIZUKU
- allowModeFallback: true
- terminalStatus: SUCCESS
- detail: tap(540,714), first=true, retry=false, taps=0->1, mode=SHIZUKU; requested=SHIZUKU, resolved=SHIZUKU, fallbackApplied=false

## Paste into 《Apkclaw 双模式验证记录》fallback 表

| 日期 | 设备/ROM | 请求模式 | fallback | 前置故障注入 | 终态 | 实际解析模式 | 结论 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-05-03 | vivo V1838A / Android 10 (SDK 29) / QP1A.190711.020 release-keys | `SHIZUKU` | `true` | Shizuku 未授权或未运行（按需注入） | SUCCESS | `SHIZUKU` | fallback=false，详见导出 detail |
