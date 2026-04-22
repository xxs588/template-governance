# FE/BE 采用门禁（控制面落地）

## 目标

确保代码仓库在日常开发中真实使用控制面文档，而不是只在文档仓库里存在。

## 强制接入点

1. Issue 准入门禁
- 没有 `Spec` 链接、`Contract/Rule` 链接、`Harness 计划` 的 Issue 不进入 Ready

2. PR 合并门禁
- 没有 Harness 验证证据的 PR 禁止合并
- 涉及契约变更但未更新文档真源的 PR 禁止合并

3. Agent 上下文门禁
- FE/BE 仓库 `AGENTS.md` 必须声明读取顺序：
  - 控制面边界规则
  - 契约规则
  - Harness 清单

4. 发版门禁
- 候选版冻结后只允许阻塞修复
- 未通过最小 smoke 禁止发布

## 最低落地动作

- 将 `templates/issue-template.md` 和 `templates/pr-template.md` 同步到 FE/BE 仓库
- 在 FE/BE 仓库 CI 或人工审核中启用 Harness 检查清单
- 将联调与发版记录回写到同步记录
