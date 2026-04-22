# template-governance

AI Native 项目的控制面模板仓库。基于《AI 原生工程》的治理模型，提供 Spec-first 工作流的完整骨架。

## 快速开始

1. 复制本仓库到你的项目组织下（如 `your-org/your-project-docs`）
2. 填写 `AGENTS.md.template`，去掉 `.template` 后缀，改为 `AGENTS.md`
3. 选择契约格式（推荐 OpenAPI 3.0），在 `contracts/` 下创建契约文件
4. 运行 `openspec init` 初始化 openspec 目录结构
5. 在 `docs/control-plane/` 填入项目特有的治理文档（范围冻结、系统一页设计等）
6. 在各代码仓库创建 `AGENTS.md`（参考 `docs/standards/agents-md-standard.md`）
7. 开始使用

## 目录结构

```
.
├── AGENTS.md                        ← Agent 工作指南
├── openspec/                        ← OpenSpec 工具管理，运行 openspec init 初始化
├── contracts/                       ← 契约真源（OpenAPI / GraphQL Schema 等）
├── docs/
│   ├── control-plane/               ← 固定层：红线、范围、治理规则
│   ├── workflow/                    ← 固定层：工作流 SOP
│   ├── harness/                     ← 固定层：验证闭环
│   ├── standards/                   ← 固定层：编写标准
│   └── runtime/                     ← 运行层：阶段进度、Issue 清单、风险台账
├── templates/                       ← 可复用模板
└── .github/                         ← CI（按需配置）
```

## 分层规则

| 层 | 目录 | 特点 | 可变性 |
|---|------|------|--------|
| 固定层 | `control-plane/` `workflow/` `harness/` `standards/` | 长期规则、红线、门禁 | 变更需充分讨论确认影响面，重大变更走 OpenSpec |
| 运行层 | `runtime/` | 阶段推进、Issue 清单、风险台账 | 可高频更新 |

运行层文档可更新状态，但**不得改写固定层规则定义**。

## 核心工作流

```
讨论 → 决策冻结 → OpenSpec change（proposal/design/specs/tasks）
  → 归档 → 回写 issue-dispatch → 下发 Issue
    → 执行 → PR（决策上下文+真源+证据）→ 审核 → 合并 → 回写
```

完整示例见 `docs/workflow/workflow-example.md`

## 参考

- 《AI 原生工程》https://tatsukimeng.github.io/ai-native-engineering/
- OpenSpec https://openspec.dev/
