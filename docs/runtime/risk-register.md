# 风险台账（risk-register）

> 用途：记录当前阶段风险与处理进度。该文件属于运行态资产，可高频更新。

## 风险分级

- P0：阻塞关键链路，需立即处理
- P1：高优先风险，需本阶段关闭或降级
- P2：一般风险，持续观察

## 台账模板

| Level | Risk | Impact | Owner | Mitigation | Trigger | Status |
|-------|------|--------|-------|------------|---------|--------|
| P0 | | | | | | open |

字段说明：

- `Mitigation`：缓解/回滚动作（可执行）
- `Trigger`：触发升级条件（例如超时、契约冲突）
- `Status`：open / mitigating / resolved / accepted

## 规则真源引用

- 升级规则：`docs/workflow/change-intake-and-escalation.md`
- 任务同步：`docs/workflow/task-release-and-sync-sop.md`
- Harness 门禁：`docs/harness/harness-gate-baseline.md`
