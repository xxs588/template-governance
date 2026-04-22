# Release Strategy

## 核心结论

- `develop` 是持续开发分支
- 发版不通过 `develop -> main` 合并完成
- 发版以 `develop` 的冻结 commit 为准，通过 `tag` 或 `release/*` 分支完成

## 为什么这样做

如果把发版继续绑定在 `develop -> main` 上，会把“持续开发”和“发布快照”混在一起，导致：

- 发布范围不断漂移
- review 范围失真
- 回滚边界不清晰
- 大型 PR 长期积压

## 推荐发版流程

1. 从 `develop` 选一个冻结 commit
2. 打候选版本 tag，例如 `<service>-vX.Y.Z-rcN`
3. 部署测试 / 预发环境
4. 跑最小 smoke
5. 如需修发布阻塞项，可切 `release/*` 分支
6. 验证通过后打正式 tag，例如 `<service>-vX.Y.Z`
7. 在 GitHub 创建 Release
8. 将 release 分支修复回流到 `develop`

## 何时切 release 分支

满足任一条件时建议切 `release/*` 分支：

- `develop` 仍要继续并行开发新功能
- 候选版需要 1 次以上修复
- 发布阻塞修复不希望污染日常开发节奏

## 候选版本最小 smoke

- auth login
- auth refresh / me
- article list
- article create
- article update / delete
- categories / tags list

## 发布 DoD

发布前必须满足：

- 范围已冻结
- 候选版本已标识（tag 或 release 分支）
- 最小 smoke 通过
- 风险点已记录
- 回滚方案已明确
- dashboard / progress sync 已回写

## 发布标签约定（GitHub Labels）

发布清单中的 Issue / PR 应具备以下稳定标签类别：

- `level:*`（变更级别）
- `freeze:*`（冻结状态）
- `status:*`（执行状态）
- `release:*`（候选/已发布状态）

说明：

- 标签用于发布面板筛选与同步，不替代门禁判断。
- 实际是否可发，仍以 smoke、风险、回滚、BLOCKER 清零为准。
- 具体阶段/批次标签（例如 week/phase）属于运行态配置，在 `docs/runtime/` 维护，不在稳定策略中写死。

## 对现有大 PR 的处理

如果仓库中存在 `develop -> main` 的超大草稿 PR，而既定策略又不走 `main` 发版：

- 不再继续向该 PR 追加新功能
- 在 PR 中留言说明发版策略已切换
- 将其关闭，避免继续误导团队
