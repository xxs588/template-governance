# 任务发布与任务同步 SOP

## 1. 任务发布流程

固定顺序：

1. 创建 Spec
2. 产评评审
3. 拆解 Issue
4. 分配负责人与流程位置
5. 发布到任务池（Ready）

## 1.1 发布与发版的区别

- `任务发布`：把需求或修复投放进开发流
- `版本发版`：从冻结 commit 产出候选版本或正式版本

固定口径：

- 日常开发走 `develop`
- 版本发版不依赖 `develop -> main`
- 发版通过 `tag` 或 `release/*` 分支完成

## 2. 任务发布门槛

没有以下字段禁止发布：

- 业务目标
- 范围边界
- 验收标准
- 依赖关系
- 优先级
- 所属流程位置

## 2.1 发布标签要求（Labels）

发布相关 Issue / PR 必须打对应标签（若仓库尚未创建同名标签，先创建后再发布）：

- `level:l0-direct` / `level:l1-openspec` / `level:l2-blocker`（三选一）
- `freeze:locked` / `freeze:change-request`（二选一）
- `status:ready` / `status:blocked` / `status:deferred` / `status:done`（四选一）
- 纳入候选版本的项必须额外打 `release:candidate`
- 如需阶段/批次标签（例如 week/phase），在 `docs/runtime/` 当前阶段文档中定义

执行规则：

- 进入 `Ready` 前，至少具备 `week + level + freeze + status` 四类标签。
- 进入 `Ready` 前，至少具备 `level + freeze + status` 三类稳定标签。
- 命中 L2 或 BLOCKER 时，30 分钟内补齐 `level:l2-blocker + status:blocked`。
- 候选版本冻结后，发布清单中的项必须为 `release:candidate + freeze:locked`，且不得保留 `status:blocked`。
- 标签用于索引和同步，不替代门禁与证据判定。
- 同步记录需精简：只写变更结论与阻塞，不写长过程描述。

## 3. 任务同步流程

### 日同步（15 分钟）

- 更新状态：todo/doing/verify/done/blocked
- 报告阻塞与依赖
- PM 决定是否升级 BLOCKER

### 周期同步（30 分钟）

- 回顾完成率
- 评估交付偏差
- 调整下一批任务包
- 决定是否冻结候选版本

## 4. 状态流转规则

- `todo -> doing`：负责人确认开始
- `doing -> verify`：提交 PR + 验证证据
- `verify -> done`：通过审核
- 任意 -> `blocked`：依赖中断或风险升级

## 5. 升级规则

满足任一条件升级：

- P0 阻塞超过 4 小时
- 核心链路任务延期超过 1 天
- 跨端契约冲突未解决

## 6. 候选版本冻结规则

满足以下条件时可以冻结候选版本：

- 冻结范围明确
- 核心链路未再新增功能
- 有最小 smoke 清单
- 风险和回滚口径已记录

冻结后：

- 不再向候选版本追加新功能
- 只允许修发布阻塞项
- 如需持续开发，切 `release/*` 分支承载候选版修复
