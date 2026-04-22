# PR 模板

## 决策上下文
- 为什么做：
- 为什么是现在：
- 明确不做：

## 变更摘要

## 关联真源
- Issue:
- Spec:
- Contract/Rule:
- 验收标准在：

## 发布标签
- Level: `l0-direct` / `l1-openspec` / `l2-blocker`
- Freeze: `freeze:locked` / `freeze:change-request`
- Status: `status:ready` / `status:blocked` / `status:deferred`

## 冻结边界检查
- [ ] 只修改授权范围内文件
- [ ] 未引入范围外功能
- [ ] 未破坏既定契约
- [ ] 如涉及契约变更，已先更新文档真源

## 内容验证
- 变更文件在正确分层目录内：
- 路径引用有效（无断链）：
- L0/L1 规则变更已经充分讨论确认影响面（如适用）：
- 新增 Spec 包含 Purpose/Requirements/Scenario/Boundaries/References（如适用）：

## 风险与回滚
- 风险:
- 回滚:

## 回写
- [ ] 已更新必要文档（规则/模板/同步记录）
- [ ] 如涉及 Issue 下发，已回写到 `docs/runtime/issue-dispatch.md`
