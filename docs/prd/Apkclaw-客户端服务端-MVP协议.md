# Apkclaw 客户端服务端 MVP 协议

## 1. 目标

本协议用于冻结 `Apkclaw` 二开 MVP 阶段的最小通信面，覆盖：

- 设备注册
- 设备心跳
- 拉取任务
- 任务状态回传
- 技能包下载
- 模型配置获取

MVP 阶段采用客户端主动轮询，不引入 WebSocket / SSE。

## 2. 通用约定

### 2.1 鉴权

- 设备完成绑定后，服务端返回 `deviceToken`
- 后续所有请求统一带 `Authorization: Bearer {deviceToken}`

### 2.2 通用响应

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_20260501_001",
  "data": {}
}
```

说明：

- `code = 0` 表示成功
- 非 `0` 由服务端定义错误码
- `requestId` 用于客户端日志追踪

### 2.3 时间与状态

- 时间字段统一使用 ISO-8601 UTC 字符串，例如 `2026-05-01T08:00:00Z`
- 枚举值统一使用大写下划线

## 3. 数据模型

### 3.1 DeviceProfile

```json
{
  "deviceId": "android_aaabbbccc",
  "deviceName": "Xiaomi 14",
  "brand": "Xiaomi",
  "model": "24031PN0DC",
  "androidVersion": "15",
  "apiLevel": 35,
  "appVersionName": "1.0.0",
  "appVersionCode": 1,
  "abi": "arm64-v8a"
}
```

### 3.2 RemoteTask

```json
{
  "taskId": "task_001",
  "content": "打开微信给张三发送今天下午三点开会",
  "status": "PENDING",
  "executionMode": "ACCESSIBILITY",
  "allowModeFallback": false,
  "skillPackageIds": ["wechat-basic"],
  "priority": 50,
  "timeoutSeconds": 300,
  "createdAt": "2026-05-01T08:00:00Z",
  "traceId": "trace_001",
  "replyTarget": {
    "type": "ACCOUNT_INBOX",
    "id": "inbox_001"
  },
  "metadata": {
    "templateId": "tpl_001"
  }
}
```

字段要求：

- `taskId`：服务端任务唯一标识
- `content`：下发给 Agent 的原始任务文本
- `status`：服务端任务状态，拉取时通常为 `PENDING`
- `executionMode`：可选，任务级执行模式覆盖
- `allowModeFallback`：可选，默认 `false`
- `skillPackageIds`：可为空数组
- `priority`：数值越大越优先
- `timeoutSeconds`：客户端兜底超时
- `traceId`：链路追踪 ID
- `replyTarget`：服务端回执目标
- `metadata`：预留扩展字段

### 3.3 SkillPackageMeta

```json
{
  "skillId": "wechat-basic",
  "version": "1.0.0",
  "checksum": "sha256:abc123",
  "downloadUrl": "https://example.com/skills/wechat-basic-1.0.0.zip"
}
```

### 3.4 RemoteAgentConfig

```json
{
  "configVersion": "2026-05-01.1",
  "provider": "OPENAI",
  "baseUrl": "https://api.example.com/v1",
  "apiKeyRef": "vault://agent/default",
  "modelName": "gpt-4.1",
  "temperature": 0.1,
  "maxIterations": 60,
  "streaming": true,
  "systemPrompt": "你是手机任务执行助手"
}
```

说明：

- `apiKeyRef` 表示服务端托管引用；客户端若需要真实密钥，后续由服务端换成可直接使用的下发字段
- MVP 阶段客户端至少消费 `provider/baseUrl/modelName/temperature/maxIterations/streaming/systemPrompt`

## 4. 接口清单

### 4.1 设备注册

- `POST /api/v1/device/register`

请求：

```json
{
  "bindCode": "123456",
  "deviceProfile": {
    "deviceId": "android_aaabbbccc",
    "deviceName": "Xiaomi 14",
    "brand": "Xiaomi",
    "model": "24031PN0DC",
    "androidVersion": "15",
    "apiLevel": 35,
    "appVersionName": "1.0.0",
    "appVersionCode": 1,
    "abi": "arm64-v8a"
  }
}
```

响应：

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_register_001",
  "data": {
    "accountId": "acct_001",
    "deviceId": "android_aaabbbccc",
    "deviceToken": "token_001",
    "heartbeatIntervalSeconds": 60,
    "taskPollIntervalSeconds": 15
  }
}
```

### 4.2 设备心跳

- `POST /api/v1/device/heartbeat`

请求：

```json
{
  "deviceId": "android_aaabbbccc",
  "status": "ONLINE",
  "batteryPercent": 88,
  "isCharging": true,
  "networkType": "WIFI",
  "accessibilityEnabled": true,
  "shizukuReady": false,
  "defaultExecutionMode": "ACCESSIBILITY",
  "currentTaskId": "task_001",
  "timestamp": "2026-05-01T08:10:00Z"
}
```

响应：

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_hb_001",
  "data": {
    "serverTime": "2026-05-01T08:10:00Z"
  }
}
```

### 4.3 拉取任务

- `POST /api/v1/tasks/pull`

请求：

```json
{
  "deviceId": "android_aaabbbccc",
  "supportedExecutionModes": ["ACCESSIBILITY"],
  "maxTasks": 1
}
```

响应：

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_pull_001",
  "data": {
    "tasks": [
      {
        "taskId": "task_001",
        "content": "打开微信给张三发送今天下午三点开会",
        "status": "PENDING",
        "executionMode": "ACCESSIBILITY",
        "allowModeFallback": false,
        "skillPackageIds": ["wechat-basic"],
        "priority": 50,
        "timeoutSeconds": 300,
        "createdAt": "2026-05-01T08:00:00Z",
        "traceId": "trace_001",
        "replyTarget": {
          "type": "ACCOUNT_INBOX",
          "id": "inbox_001"
        },
        "metadata": {}
      }
    ]
  }
}
```

约束：

- MVP 一次最多返回一个任务
- 若无任务，返回 `tasks: []`

### 4.4 任务状态回传

- `POST /api/v1/tasks/{taskId}/status`

请求：

```json
{
  "deviceId": "android_aaabbbccc",
  "status": "RUNNING",
  "round": 2,
  "message": "正在执行工具 open_app",
  "errorCode": null,
  "errorMessage": null,
  "resultSummary": null,
  "startedAt": "2026-05-01T08:11:00Z",
  "finishedAt": null,
  "traceId": "trace_001"
}
```

终态示例：

```json
{
  "deviceId": "android_aaabbbccc",
  "status": "SUCCESS",
  "round": 6,
  "message": "任务执行完成",
  "errorCode": null,
  "errorMessage": null,
  "resultSummary": "已向张三发送消息",
  "startedAt": "2026-05-01T08:11:00Z",
  "finishedAt": "2026-05-01T08:12:15Z",
  "traceId": "trace_001"
}
```

客户端必须上报的状态：

- `RECEIVED`
- `RUNNING`
- `SUCCESS`
- `FAILED`
- `CANCELLED`

### 4.5 获取模型配置

- `GET /api/v1/agent/config`

响应：

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_cfg_001",
  "data": {
    "configVersion": "2026-05-01.1",
    "provider": "OPENAI",
    "baseUrl": "https://api.example.com/v1",
    "apiKeyRef": "vault://agent/default",
    "modelName": "gpt-4.1",
    "temperature": 0.1,
    "maxIterations": 60,
    "streaming": true,
    "systemPrompt": "你是手机任务执行助手"
  }
}
```

### 4.6 获取技能元数据

- `GET /api/v1/skills/{skillId}/meta`

响应：

```json
{
  "code": 0,
  "message": "ok",
  "requestId": "req_skill_meta_001",
  "data": {
    "skillId": "wechat-basic",
    "version": "1.0.0",
    "checksum": "sha256:abc123",
    "downloadUrl": "https://example.com/skills/wechat-basic-1.0.0.zip"
  }
}
```

### 4.7 下载技能包

- `GET /api/v1/skills/{skillId}/download`

说明：

- 返回 zip 文件
- 解压后目录结构固定为：

```text
manifest.json
skill.md
assets/
```

## 5. 任务状态机

```text
PENDING
  -> RECEIVED
  -> RUNNING
  -> SUCCESS
  -> FAILED
  -> CANCELLED
```

规则：

- 客户端拉到任务后先回传 `RECEIVED`
- 真正进入 Agent 执行后回传 `RUNNING`
- 任一终态只能上报一次
- 超时统一回传 `FAILED`
- 用户主动取消统一回传 `CANCELLED`

## 6. 客户端本地映射建议

### 6.1 TaskContext

- `taskId <- RemoteTask.taskId`
- `source = REMOTE_SERVER`
- `replyTarget <- RemoteTask.replyTarget.id`
- `skillIds <- RemoteTask.skillPackageIds`
- `metadata <- RemoteTask.metadata + traceId`

### 6.2 执行模式

- `RemoteTask.executionMode` 有值时优先使用
- 否则回退到本地默认执行模式
- `allowModeFallback` 默认 `false`

## 7. MVP 非目标

本协议暂不覆盖：

- WebSocket / SSE 实时推送
- 多任务并发执行
- 增量日志流式上传
- 技能差量更新
- 服务端主动取消中的双向实时中断
