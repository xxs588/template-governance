# 工作流端到端示例：MVP 第一阶段

> 用途：展示控制面工作流如何从“先统筹一个阶段目标”到“拆多个 Issue 下发执行”完整跑一遍。
> 这是一个虚构场景，用于说明流程，不作为实际业务需求。

---

## 先说明这个例子为什么这样改

这个例子不再用“单个小功能直接走完整套治理”的写法。

更贴近真实推进方式的做法，通常是：

1. 先围绕一个阶段或者一个 batch 讨论
2. 把这个阶段到底做什么、不做什么、做到什么算完成先冻结下来
3. 再从阶段级 Spec 往下拆成多个 Issue
4. 前后端和文档仓库各自按分工推进

也就是说，小功能里的很多分歧，最好在阶段讨论里就先收掉，而不是等进到具体仓库开发时再临时猜。

---

## 第 1 步：PM 发起阶段讨论

**在哪做**：讨论

```text
标题：[PM] MVP 第一阶段到底先交付什么？

内容：
- 当前项目要进入第一个可验证阶段
- 希望这一阶段完成后，至少能形成“最小可演示、最小可验证、最小可发布”的闭环
- 目前候选项有：
  1. 文章列表 / 文章详情
  2. 登录态
  3. 评论
  4. 分类标签
  5. 富文本编辑

问题：
1. MVP 第一阶段到底先做哪些，哪些明确不做？
2. 这一阶段的完成标准是什么？
3. 哪些能力必须一起冻结，不然下游会一直反复讨论？
```

---

## 第 2 步：讨论收成阶段决策

**AI Agent 帮忙整理输出**：

```text
建议把 MVP 第一阶段先冻结成“最小内容闭环”，目标是：

必须做：
1. 文章列表页
2. 文章详情页
3. 登录能力
4. 最小发布链路和验证闭环

这一阶段明确不做：
1. 评论
2. 分类标签
3. 富文本编辑
4. 推荐系统

为什么这样收：
- 先让核心内容浏览和登录闭环跑通
- 先把 FE / BE / Docs / Harness 的协作链条走顺
- 把容易引起持续分歧的小功能先延后，避免第一阶段目标一直漂

这一阶段 DoD：
- FE / BE 都有对应 Issue 并完成
- 核心接口和关键页面可验证
- 有一轮完整的 PR 证据和回写闭环
- 候选版本可冻结
```

**PM 确认**：

```text
OK，MVP 第一阶段先冻结成“最小内容闭环”。
评论、分类标签、富文本这一阶段都不做。
帮我起草阶段级 Spec。
```

---

## 第 3 步：AI 起草阶段级 Spec 并提交控制面 PR

**AI Agent 做的**：

在控制面仓库创建 OpenSpec change：

```text
openspec/changes/mvp-phase-1/
├── proposal.md
├── design.md
├── specs/
│   └── mvp-phase-1/spec.md
└── tasks.md
```

其中大致表达的是：

- `proposal.md`
  说明为什么第一阶段先做“最小内容闭环”，哪些能力延后

- `design.md`
  说明这一阶段需要冻结哪些分层、契约、页面和依赖关系

- `spec.md`
  说明这一阶段的目标、非目标、验收场景、DoD

- `tasks.md`
  把阶段目标拆成多个任务，而不是一个大任务

同时更新：

```text
contracts/ 对应阶段内涉及的 API / 数据契约
```

提交 PR 到控制面仓库，按 PR 模板填写：

- 为什么做这一阶段
- 为什么这些功能先做
- 这一阶段明确不做什么
- 这个阶段完成之后怎么判定闭环成立

---

## 第 4 步：PM 审核控制面 PR

**PM 看的是阶段，不是看某一个零散小功能**：

```text
proposal.md：
- 第一阶段目标是否收稳？ ✓
- 不做项是否明确？ ✓

design.md：
- FE / BE / Docs / Harness 的依赖关系是否清楚？ ✓

spec.md：
- 验收场景是否围绕阶段目标，而不是围绕零碎功能？ ✓

tasks.md：
- 是否真的拆成了多个可执行任务？ ✓
- 依赖关系是否写清楚？ ✓
```

确认后合并。

---

## 第 5 步：归档 + 更新运行层 + 拆成多个 Issue 下发

**AI Agent 做**（PM 触发后）：

1. 归档 OpenSpec change

```text
openspec/changes/mvp-phase-1/
-> openspec/changes/archive/mvp-phase-1/
```

2. 按 `tasks.md` 拆成多个 Issue，下发到对应仓库

例如拆成：

```text
BE:
- BE#101 文章列表接口
- BE#102 文章详情接口
- BE#103 登录接口与鉴权中间件

FE:
- FE#201 文章列表页
- FE#202 文章详情页
- FE#203 登录页与登录态接入

Docs:
- DOCS#301 阶段运行面板初始化与回写规则落地
```

3. 在 `docs/runtime/issue-dispatch.md` 记录阶段任务面板：

```text
| Priority | Repo | Issue（URL）    | Why now                    | Status  |
|----------|------|----------|----------------------------|---------|
| P0       | BE   | BE#101   | MVP Phase 1 核心接口       | ready   |
| P0       | BE   | BE#102   | 依赖文章详情页联调         | ready   |
| P0       | BE   | BE#103   | 登录能力是阶段闭环前置     | ready   |
| P0       | FE   | FE#201   | 依赖 BE#101               | blocked |
| P0       | FE   | FE#202   | 依赖 BE#102               | blocked |
| P0       | FE   | FE#203   | 依赖 BE#103               | blocked |
| P1       | Docs | DOCS#301 | 确保运行层同步闭环成立     | ready   |
```

4. 更新 `docs/runtime/phase-current.md`

```text
阶段名称：MVP Phase 1
阶段目标：最小内容闭环
本阶段非目标：评论 / 分类标签 / 富文本编辑
```

---

## 第 6 步：开发者 / Agent 执行某一个 Issue

到这一步，真正执行的入口已经不是“做一个模糊的大阶段”，而是“做一个已经从阶段级 Spec 里拆出来的具体 Issue”。

以 `BE#101` 为例：

```text
控制面真源：
- Phase Spec: openspec/changes/archive/mvp-phase-1/specs/mvp-phase-1/spec.md
- Tasks: openspec/changes/archive/mvp-phase-1/tasks.md
- Contract: contracts/ 对应端点
- 仓库内 AGENTS.md

Issue 自包含写清楚：
1. 做什么：实现文章列表接口
2. 为什么现在做：这是 FE 列表页的前置依赖
3. 非目标：不在本 Issue 中处理评论、分类标签、富文本
4. 验收标准：接口字段、分页、错误语义、Smoke 要求
5. Harness 计划：跑哪些命令，提供什么证据
```

这里就能看出来，阶段讨论里已经把“小功能做不做、做到多深”这些分歧先收掉了，所以进入具体 Issue 后，不需要再临时边写边猜。

---

## 第 7 步：提交 PR

```markdown
## 决策上下文
- 为什么做：MVP Phase 1 的文章浏览闭环前置接口
- 为什么是现在：FE#201 依赖本接口进入开发
- 明确不做：评论、分类标签、富文本能力

## 变更摘要
实现文章列表接口，支持分页与基础错误返回。

## 关联真源
- Issue: BE#101
- Phase Spec: 控制面仓库 openspec/changes/archive/mvp-phase-1/...
- Contract: 控制面仓库 contracts/...

## 冻结边界检查
- [x] 只修改授权范围内文件
- [x] 未引入阶段范围外功能
- [x] 未破坏既定契约

## Harness 验证
- Command: <构建命令>
- Result: PASS
- Evidence: ...

- Command: <测试命令>
- Result: PASS
- Evidence: ...

### Spec / Task 覆盖
- [x] 覆盖 MVP Phase 1 中“文章列表接口”对应验收点

## 风险与回滚
- 风险：分页参数默认值可能影响 FE 对接
- 回滚：回退本次接口变更

## 回写
- [ ] 若发现契约描述不清，回写控制面仓库
```

---

## 第 8 步：PM / Reviewer 审核 PR

**这时候审核的是“这个 Issue 有没有忠实地执行阶段级冻结结果”**

```text
1. 决策上下文清楚吗？             → ✓
2. 关联真源齐了吗？               → ✓
3. 有没有引入阶段外功能？         → ✓
4. Harness 证据够不够？           → ✓
5. 风险和回滚交代了吗？           → ✓
6. 如果契约有偏差，是否提醒回写？ → ✓
```

合并。

---

## 第 9 步：回写运行层

**PM 触发 AI 回写**：

```text
"BE#101 合并了，帮我同步阶段面板。"
```

**AI Agent 做**：

1. 更新 `docs/runtime/issue-dispatch.md`

```text
BE#101 -> done
FE#201 -> ready
```

2. 更新 `docs/runtime/risk-register.md`（如有）
3. 如果 PR 中发现契约或规则问题，补控制面 PR
4. 建议下一批已解除阻塞的 Issue 可以进入执行

---

## 第 10 步：整个阶段完成后

当这个阶段拆出去的一组 Issue 都完成后，AI Agent 汇总：

```text
docs/runtime/phase-current.md 更新：
  MVP Phase 1 完成 ✓

docs/runtime/issue-dispatch.md 更新：
  本阶段任务全部 done ✓

建议下一步：
  是否进入 MVP Phase 2？
  候选项：评论 / 分类标签 / 富文本编辑
```

然后再开启下一轮阶段讨论。

---

## 这个例子真正想强调什么

这个例子真正想强调的，不是“某个小功能怎么走流程”，而是：

1. 开始时先统筹一个阶段目标
2. 先冻结这个阶段做什么、不做什么
3. 小功能里的分歧尽量在阶段讨论里提前收掉
4. 再从阶段级 Spec 往下拆成多个 Issue
5. 前后端和文档仓库围绕同一份阶段真源协作

这样整个控制面工作流才更像是在统筹项目推进，而不是在给某一个零散功能补流程。

---

## 整个流程里各文件分别在做什么

| 文件 | 在流程中的角色 |
|------|--------------|
| `docs/control-plane/` | 冻结长期有效的边界、范围、治理规则 |
| `docs/workflow/spec-issue-test-flow.md` | 描述 Spec -> Issue -> Test 主流程 |
| `docs/workflow/task-release-and-sync-sop.md` | 描述任务发布、Ready 门槛、同步节奏 |
| `docs/workflow/pr-merge-gate-spec.md` | 规定 PR 合并门禁 |
| `docs/harness/` | 定义验证闭环和证据要求 |
| `docs/runtime/phase-current.md` | 记录当前阶段目标、非目标、DoD |
| `docs/runtime/issue-dispatch.md` | 记录阶段任务面板和状态变化 |
| `docs/runtime/risk-register.md` | 记录阶段风险 |
| `templates/` | 提供 Spec / Issue / PR / Harness 模板 |
| `contracts/` | 维护跨端契约真源 |
| `openspec/` | 承接重大变更的冻结、审核、归档 |
