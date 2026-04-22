# 进度同步协议（Docs <-> FE/BE）

## 同步频率

- 最低要求：每周 1 次
- 建议：每次合并关键 PR 后立即同步

## 同步内容

每次同步应至少包含：

1. FE 进度：已完成 / 进行中 / 阻塞
2. BE 进度：已完成 / 进行中 / 阻塞
3. 跨端阻塞：接口、数据结构、联调环境
4. 新增风险：严重度、影响范围、应对策略
5. 候选版本状态：未冻结 / 已冻结 / 验证中 / 已发布
6. 下一批行动：按优先级排序

## 状态定义

- `todo`: 未开始
- `doing`: 开发中
- `verify`: 待验证
- `done`: 完成
- `blocked`: 阻塞

## 记录模板

```markdown
## YYYY-MM-DD Sync

### FE
- FE-xxx: doing - 描述

### BE
- BE-xxx: blocked - 描述

### Cross Blockers
- xxx

### Risks
- [P0/P1/P2] xxx

### Release
- status: none / frozen / verifying / released
- target: tag or release branch
- note: xxx

### Next
1. xxx
2. xxx
```
