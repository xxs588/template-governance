# template-governance

AI Native 项目的控制面模板仓库。

**核心主张**：当 AI Agent 替你写代码时，你需要一个独立于代码仓库的"控制面"来冻结决策、定义边界、验证产出。否则 Agent 会跑偏、重复犯错、越过你定的范围。

本模板提供这套控制面的完整骨架，基于《AI 原生工程》的治理模型和 OpenSpec 工具链。

---

## 为什么需要控制面仓库

AI Agent 写代码很快，但它不擅长：

- **收边界**：它会实现你提到的，也会实现你没提到但"相关"的东西
- **保持一致**：它不记得上周的决策，除非你写下来
- **自我约束**：它不会主动停下来问你"这个改动是不是超范围了"

传统开发靠 code review 和团队沟通来约束这些。AI Native 开发需要把约束前置成文档，让 Agent 在动手前就读到"能做什么、不能做什么"。

控制面仓库就是存放这些约束的地方。它不写代码，只管：

1. **决策冻结**：把讨论的结论变成可追溯的 spec
2. **边界定义**：明确范围、契约、变更边界
3. **验证闭环**：定义"什么叫做完"，没有证据不能合并

---

## 为什么分固定层和运行层

控制面文档有两种截然不同的生命周期：

| 类型 | 例子 | 生命周期 | 更新频率 |
|------|------|---------|---------|
| 固定规则 | "无证据不合并"、"API 契约是唯一真源" | 项目全周期有效 | 极低，重大变更才动 |
| 运行状态 | "当前阶段做评论功能"、"Issue #101 blocked" | 阶段性，过期作废 | 高频，随时更新 |

把两者混在一起会导致：

- Agent 把"本周做评论"当成永久规则，下一周还在做评论
- 规则文档里夹着状态叙事，越来越长，Agent 读不完也不想读
- 改一个状态不小心改了一条规则

所以本模板把这两类内容物理分离：

```
docs/
├── control-plane/    ← 固定层：红线、范围、治理规则
├── workflow/         ← 固定层：工作流 SOP
├── harness/          ← 固定层：验证闭环
├── standards/        ← 固定层：编写标准
└── runtime/          ← 运行层：阶段进度、Issue 清单、风险台账
```

约束：运行层可更新状态，但**不得改写固定层规则**。固定层变更需充分讨论确认影响面，重大变更走 OpenSpec。

---

## 为什么是 Spec-first 而不是 Code-first

传统流程：写代码 → 测试 → 补文档（如果还记得的话）。

AI Native 流程反过来：

```
讨论 → 决策冻结 → Spec → Issue → 代码 → PR（附证据）→ 回写
```

原因：

1. **Agent 需要明确的输入才能产出明确的输出**。模糊的口头需求 → 模糊的代码。Spec 把"要什么"冻结成文字，Agent 才有判断标准。
2. **Spec 是验收的锚点**。没有 Spec，PR review 只能凭感觉。有了 Spec，逐条 Scenario 检查即可。
3. **Spec 把讨论前置**。"做不做敏感词"、"一阶还是两阶评论"这些问题在 Spec 阶段就要回答，而不是写完代码才发现理解不一致。

核心工作流：

```
讨论 → 决策冻结 → OpenSpec change（proposal/design/specs/tasks）
  → 归档 → 回写 issue-dispatch → 下发 Issue
    → 执行 → PR（决策上下文+真源+证据）→ 审核 → 合并 → 回写
```

每一步的产物和规则在 `docs/workflow/` 中定义。完整示例见 `docs/workflow/workflow-example.md`。

---

## 为什么需要契约真源

前后端（或任何跨端）协作最大的坑：各自理解接口，联调时才发现不一致。

本模板在 `contracts/` 目录存放接口契约文件，作为跨端的**唯一真源**：

- 前端从契约生成类型定义
- 后端从契约验证实现是否对齐
- 任何契约变更必须先更新契约文件，再改代码

契约格式按项目技术栈自选（推荐 OpenAPI 3.0），在 `contracts/README.md` 中有选型指引。

---

## 为什么 Harness 不是"跑个命令"

Harness（验证闭环）经常被简化成"CI 通过就行"。但 AI Agent 场景下，你需要的不止是"命令通过"，而是：

1. **可追溯的证据**：Agent 说"已测试"，你要能看到它跑了什么命令、输出是什么
2. **场景覆盖**：Spec 里定义了 5 个 Scenario，PR 里要逐条确认覆盖
3. **经验回写**：验证过程中发现的新约束（比如"分页参数描述不清"），要回到控制面沉淀为规则

这就是为什么 Harness 规范里强调"执行 → 观察 → 验证 → 收敛 → 回写"的闭环，而不是一个 CI badge。

详见 `docs/harness/`。

---

## 为什么用 OpenSpec 管理变更

控制面仓库本身就是规则仓库，改规则和改代码一样需要管控：

- 谁提议改的？为什么？影响什么？
- 改之前是什么样的？改之后是什么样的？
- 改完后，相关的 spec 和文档有没有同步更新？

OpenSpec 提供了这套变更管理工具链：proposal → design → specs → tasks → validate → archive。每个 change 都有完整的上下文和追溯链。

快速开始时运行 `openspec init` 初始化目录结构。

---

## 目录结构总览

```
.
├── AGENTS.md                        ← 控制面 Agent 工作指南
├── openspec/                        ← OpenSpec 工具管理，运行 openspec init 初始化
├── contracts/                       ← 契约真源（OpenAPI / GraphQL Schema 等）
├── docs/
│   ├── control-plane/               ← 固定层：红线、范围、治理规则
│   ├── workflow/                    ← 固定层：工作流 SOP
│   ├── harness/                     ← 固定层：验证闭环
│   ├── standards/                   ← 固定层：编写标准
│   └── runtime/                     ← 运行层：阶段进度、Issue 清单、风险台账
├── templates/                       ← 可复用模板（Issue/PR/Spec/Harness）
└── .github/                         ← CI（按需配置）
```

各目录职责说明：

| 目录 | 管什么 | 关键文件 |
|------|--------|---------|
| `control-plane/` | 为什么这样治理、红线、范围 | README 中说明应放入的文档 |
| `workflow/` | 怎么推进任务和同步进度 | 变更分级、OpenSpec 基线、PR 门禁、发版策略等 9 篇 SOP |
| `harness/` | 怎么做验证闭环 | 闭环规范、门禁基线、阶段计划 |
| `standards/` | 怎么写规范 | AGENTS.md 标准、Spec 编写标准、采用门禁 |
| `runtime/` | 当前状态（可高频更新） | 阶段定义、Issue 下发清单、风险台账 |
| `templates/` | 可复用模板 | Issue/PR/Spec/Harness 四个模板 |

---

## 快速开始

1. 复制本仓库到你的项目组织下（如 `your-org/your-project-docs`）
2. 填写 `AGENTS.md.template`，去掉 `.template` 后缀，改为 `AGENTS.md`
3. 选择契约格式（推荐 OpenAPI 3.0），在 `contracts/` 下创建契约文件
4. 运行 `openspec init` 初始化 openspec 目录结构
5. 在 `docs/control-plane/` 填入项目特有的治理文档（范围冻结、系统一页设计等）
6. 在各代码仓库创建 `AGENTS.md`（参考 `docs/standards/agents-md-standard.md`）
7. 开始使用

---

## 参考

- 《AI 原生工程》https://tatsukimeng.github.io/ai-native-engineering/
- OpenSpec https://openspec.dev/
