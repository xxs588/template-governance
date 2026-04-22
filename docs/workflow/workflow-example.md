# 工作流端到端示例：评论功能

> 用途：展示控制面工作流如何从"PM 发起讨论"到"PR 合并回写"完整跑一遍。
> 这是一个虚构场景，用于说明流程，不作为实际业务需求。

---

## 第 1 步：PM 发起聚焦讨论

**在哪做**：讨论

```
标题：[PM] 下一个 batch 是否做评论功能？

内容：
- MVP scope 里还没做的是：评论、分类标签、富文本
- 按用户价值优先级，评论（核心互动）应排在分类标签前面
- 技术风险：评论涉及关联查询和级联删除

问题：
1. 确认下一个 batch 是评论功能吗？
2. 一阶评论（回复文章）还是两阶（回复评论）？
3. 有没有技术约束我没想到的？
```

---

## 第 2 步：讨论收成决策

**AI Agent 帮你做的**：

```
回复：
1. 评论在 MVP scope 内，优先级 ✓
2. 建议：先做一阶，两阶放到 Phase 2
3. 技术约束：
   - Comment 需要 article_id 外键
   - 删除文章要级联处理评论
   - 需要确认"只能删自己的评论"的权限逻辑

建议决策：
- 做一阶评论，必须登录，不做敏感词过滤
```

**PM 确认**：

```
"OK。一阶评论，必须登录，先不做敏感词。帮我起草 spec。"
```

---

## 第 3 步：AI 起草 Spec 并提交控制面 PR

**AI Agent 做的**：

在控制面仓库创建 OpenSpec change：

```
openspec/changes/comment-system/
├── proposal.md    ← "只做一阶评论，不做回复、不做敏感词"
├── design.md      ← Comment 模型 + API 端点
├── specs/comment-system/spec.md  ← 可验证场景
└── tasks.md       ← 任务 + 依赖关系
```

同时更新契约：

```
contracts/ 新增对应端点定义
```

提交 PR 到控制面仓库（按 PR 模板填写决策上下文：为什么做、为什么是现在、明确不做）。

---

## 第 4 步：PM 审核控制面 PR

**PM 看决策上下文 + spec**：

```
proposal.md："只做一阶评论，不做回复、不做审核" → ✓
spec.md：场景覆盖了创建/列表/删除/未认证/非本人删除 → ✓
契约：端点定义清晰 → ✓
tasks.md：BE 先行，FE 后行，有依赖关系 → ✓
```

合并 PR。

---

## 第 5 步：归档 + 更新运行层 + 下发 Issue

**AI Agent 做**（PM 触发后）：

1. 归档 OpenSpec change：`openspec/changes/comment-system/` → `openspec/changes/archive/comment-system/`

2. 更新 `docs/runtime/issue-dispatch.md`：

```
| Priority | Repo | Issue | Why now | Status |
|----------|------|-------|---------|--------|
| P0 | BE | BE#101 | 评论是 MVP 核心互动，契约已冻结 | ready |
| P0 | FE | FE#102 | 评论是 MVP 核心互动，依赖 BE #101 完成 | blocked(#101) |
```

3. 更新 `docs/runtime/phase-current.md`。

4. 在代码仓库创建对应 Issue，Issue body 包含：
   - 控制面真源表格（每个真源要读什么）
   - 自包含的目标描述（不依赖外部链接就能理解要做什么）
   - 验收标准
   - 参考规范链接（补充阅读）

---

## 第 6 步：开发者执行

**Agent 接到 Issue 后**：

```
控制面真源（Issue 里写明了）：
- Spec: openspec/changes/archive/comment-system/specs/comment-system/spec.md
- 契约: contracts/ 对应端点
- AGENTS.md: 代码仓库的 AGENTS.md

要做什么（Issue 里自包含写明了）：
1. 创建 Comment 模型（article_id 外键 + user_id）
2. 实现 handler/service/dao
3. 写测试
4. 跑 smoke 验证

不做什么（Issue 里写明了）：
- 不做回复评论
- 不做敏感词过滤
```

---

## 第 7 步：提交 PR

```markdown
## 决策上下文
- 为什么做：评论是 MVP 核心互动，契约已冻结
- 为什么是现在：BE 评论 API 是 FE 评论组件的前置依赖
- 明确不做：不做回复评论、不做敏感词过滤

## 变更摘要
新增 Comment 模型 + handler/service/dao，实现创建/列表/删除 API 端点。

## 关联真源
- Issue: #101
- Spec: 控制面仓库 openspec/changes/archive/...
- Contract: 控制面仓库 contracts/...
- 验收标准在: Issue #101

## 冻结边界检查
- [x] 只修改授权范围内文件
- [x] 未引入范围外功能
- [x] 未破坏既定契约

## Harness 验证
- Command: <构建命令>
- Result: PASS
- Evidence: ...

- Command: <测试命令>
- Result: PASS
- Evidence: ...

### Spec 场景覆盖
- [x] 场景"创建评论" ✓
- [x] 场景"获取评论列表" ✓
- [x] 场景"删除自己的评论" ✓
- [x] 场景"未认证拒绝" ✓
- [x] 场景"非本人删除拒绝" ✓

## 风险与回滚
- 风险：新增表 migration
- 回滚：删除 migration 文件，重启服务

## 回写
- [ ] 发现契约描述不够清晰，建议后续修正
```

---

## 第 8 步：PM 审核 PR

**PM 逐条检查**：

```
1. 决策上下文清楚吗？            → ✓ 为什么做/为什么现在/不做
2. 关联真源链接都有吗？          → ✓ Issue/Spec/Contract/验收标准
3. Harness 证据完整吗？         → ✓ build + test + smoke
4. Spec 场景全覆盖了吗？        → ✓ 5/5
5. 没改不该改的文件吗？         → ✓
6. 风险回滚清楚吗？             → ✓
7. 回写提醒有吗？               → ✓
```

合并。

---

## 第 9 步：PM 触发 AI 回写

**PM 说**："BE 评论 PR 合并了，帮我回写。"

**AI Agent 做**：

1. 标记 Issue 为 done
2. 更新 `docs/runtime/issue-dispatch.md`（BE → done，FE → ready）
3. 发现 PR 里的回写提醒，提 PR 修正契约描述
4. 更新 `docs/runtime/risk-register.md`（如需）
5. 建议下一个阻塞已解除的 Issue 可以开始

---

## 第 10 步：另一端执行（同理）

流程相同：读 Issue → 按 AGENTS.md 读真源 → 实现 → 提 PR → PM 审核 → 合并 → 回写。

---

## 第 11 步：功能完成后

**AI Agent 汇总**：

```
docs/runtime/phase-current.md 更新：
  评论功能全链路完成 ✓

docs/runtime/issue-dispatch.md 更新：
  所有 Issue → done

建议下一步：
  "当前 batch 已完成。建议下一个 batch。是否开一个新的聚焦讨论？"
```

PM 确认 → 回到第 1 步，开启下一轮。

---

## 整个流程中每个文件的职责

| 文件 | 在流程中的角色 |
|------|--------------|
| `docs/control-plane/control-plane-handbook.md` | 红线：定义边界不能随便改 |
| `docs/control-plane/mvp-scope.md` | 范围冻结：确认功能在范围内 |
| `docs/control-plane/ai-native-governance.md` | 治理总则：先 spec 后代码 |
| `docs/workflow/spec-issue-test-flow.md` | 流程：Spec→Issue→Test 五步 |
| `docs/workflow/change-intake-and-escalation.md` | 分级：L0/L1/L2 怎么判定 |
| `docs/workflow/pr-merge-gate-spec.md` | 门禁：PR 必须有什么才能合 |
| `docs/harness/harness-closed-loop.md` | 验证：Harness 四要素标准 |
| `docs/harness/harness-gate-baseline.md` | 基线：合并门禁 + 发版门禁 |
| `docs/standards/agents-md-standard.md` | 标准：AGENTS.md 怎么写 |
| `docs/runtime/phase-current.md` | **运行态**：当前在哪个阶段 |
| `docs/runtime/issue-dispatch.md` | **运行态**：本阶段发了哪些 Issue |
| `docs/runtime/risk-register.md` | **运行态**：当前风险台账 |
| `contracts/` | 契约真源：FE/BE 共同消费 |
| `openspec/changes/archive/<id>/` | 归档：确认后的 spec 产物 |
| `templates/*` | 模板：Issue/PR/Spec/Checklist |
