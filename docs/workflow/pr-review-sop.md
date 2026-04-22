# PR 审核 SOP（PM / 产评）

## 审核依据（强制）

- 合并门禁真源：`docs/workflow/pr-merge-gate-spec.md`
- 审核结论必须基于门禁项与验证证据，不基于阶段判断。

## 审核标签体系

- `BLOCKER`：必须修，不修不合并
- `REQUIRED`：本 PR 合并前必须修
- `FOLLOW-UP`：不阻塞合并，转新 Issue

说明：标签是评论记录工具，不能替代门禁规则与证据判定。

## 评论模板

### BLOCKER

```text
[BLOCKER]
问题：
影响：
修复建议：
验收方式：
对应门禁项：
```

### REQUIRED

```text
[REQUIRED]
问题：
违反规则：
修复建议：
需补充证据：
```

### FOLLOW-UP

```text
[FOLLOW-UP]
优化点：
影响范围：
建议方向：
转 Issue：
```

## 评审结论

- `Approve`：门禁全部满足，证据可复核，未解决 BLOCKER = 0。
- `Approve with Follow-up Issues`：门禁满足，改进项已转 issue，不阻塞合并。
- `Request Changes`：命中任一阻断条件、证据缺失或无法复核。
- `Reject`：目标方向或边界不成立，需重新定义方案后重提。

## Request Changes 触发条件

出现以下任一项，应直接给出 `Request Changes`：

- PR 必填字段缺失（Issue/Spec/Contract/Rule/Harness/证据摘要）。
- 必需验证项（Build/Typecheck、Unit Test、Smoke）为失败或无有效证据。
- 证据链接失效，或日志/截图不足以支撑结论。
- 发现契约破坏、范围越界、无法回滚等高风险问题。
- 存在未解决 BLOCKER。

## Follow-up Issue 规则

每条 FOLLOW-UP 必须创建 Issue，且包含：

- 背景
- 预期收益
- 优先级
- 合并后跟进时间窗口
