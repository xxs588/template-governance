# Spec -> Issue -> Test 工作流

## 1. Spec 阶段

每个功能先提交 Spec，至少包含：

- 背景与目标
- 范围与非目标
- API/数据契约
- 验收标准
- 风险与回滚

模板：`templates/spec-template.md`

产物位置：`openspec/changes/<change-id>/specs/<feature>/spec.md`

## 2. 任务拆解阶段

从 Spec 拆出任务清单（tasks.md），按 FE/BE 分工。

每个 task 必须说明：

- 对应 Spec 场景
- 执行仓库
- 依赖关系（哪些 task 必须先完成）
- 验收标准

产物位置：`openspec/changes/<change-id>/tasks.md`

## 3. Issue 下发阶段

从 tasks.md 的每个 task 创建 GitHub Issue，**下发到对应代码仓库**：

| task 涉及的仓库 | Issue 创建在哪里 |
|-----------------|-----------------|
| 后端仓库 | 后端仓库的 GitHub Issue |
| 前端仓库 | 前端仓库的 GitHub Issue |
| 控制面仓库 | 控制面仓库的 GitHub Issue |

每个 Issue 必须包含：

- **Spec 链接**：指向 `openspec/changes/<id>/specs/<feature>/spec.md` 的具体 Scenario
- **契约链接**：指向 `contracts/openapi.yaml` 的具体端点（若涉及 API）
- **为什么现在做**：背景与前置依赖
- **验收标准**：从 Spec Scenario 直接映射（可验证表述）
- **非目标**：明确不做的事（防止范围漂移）
- **实现约束**：允许/禁止修改的目录
- **Harness 计划**：必跑命令 + smoke + 证据形式

模板：`templates/issue-template.md`

下发后，在 `docs/runtime/issue-dispatch.md` 记录下发状态（PM 在这里看全局进度）。

## 4. 开发与 PR 阶段

- 分支命名：`feat/FE-xxx-*`、`feat/BE-xxx-*`
- PR 描述必须包含：关联 Issue、测试证据、风险说明

模板：`templates/pr-template.md`

## 5. Harness 验证阶段

PR 合并前必须通过：

- 编译/构建通过
- 单元测试通过（若有）
- 关键链路 smoke 通过
- Spec Scenario 逐条覆盖

模板：`templates/harness-checklist.md`

## 6. 回写阶段

每次合并后，必须回写：

- Issue 状态更新（done / blocked）
- `docs/runtime/issue-dispatch.md` 状态列更新
- 新增风险记录到 `docs/runtime/risk-register.md`
- 新经验可沉淀为规则（提交控制面 PR）

记录规范见：`docs/workflow/progress-sync-protocol.md`

## 完整链路示例

见 `docs/workflow/workflow-example.md`
