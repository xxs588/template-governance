# template-governance

AI Native 项目的控制面模板仓库。

**核心定位**：这个仓库不是文档库，而是**推进项目的引擎**。通过 OpenSpec 把模糊的讨论变成冻结的决策，再拆解成 Issue 下发到代码仓库执行，最后验收回写，形成闭环。每一步都有 Harness（约束机制）兜底，确保 Agent 不跑偏、决策不丢失、产出可追溯。

补充说明：这里说的“控制面”指的是**整个文档仓库**，不是单独的 `docs/control-plane/` 目录。`docs/control-plane/` 只是控制面里的宪法层；真正的控制面由 `docs/control-plane/`、`docs/workflow/`、`docs/harness/`、`docs/runtime/`、`templates/`、`contracts/`、`openspec/` 等共同组成。

延伸阅读：[repo-control-plane-sharing.md](./repo-control-plane-sharing.md)

---

## 项目推进模型

控制面仓库的终极目标是**让项目一步一步可控地推进**。推进方式：

```
PM 发起讨论
  → 讨论收敛成决策
    → OpenSpec 冻结决策（proposal / design / specs / tasks）
      → 代码仓库创建 Issue（自包含目标 + 真源 + 验收标准）
        → Agent 执行 → PR（决策上下文 + 证据）
          → PM 审核 → 合并 → 回写控制面
            → 开启下一轮讨论
```

每一轮循环交付一个可验证的增量。控制面仓库不写代码，它驱动代码仓库写代码。

---

## 为什么需要控制面仓库

AI Agent 写代码很快，但它不擅长：

- **收边界**：它会实现你提到的，也会实现你没提到但"相关"的东西
- **保持一致**：它不记得上周的决策，除非你写下来
- **自我约束**：它不会主动停下来问"这个改动是不是超范围了"
- **按需加载**：它倾向于把所有文档都塞进上下文，导致跑偏

传统开发靠 code review 和团队沟通来约束。AI Native 开发需要把约束前置成文档和机制，让 Agent 在动手前就读到"能做什么、不能做什么、读什么、怎么算做完"。

---

## Harness：不只是跑命令

Harness 通常被理解为"跑测试命令"。但在 AI Native 治理中，Harness 是**贯穿项目所有层面的约束机制**——确保每一步都有兜底、可追溯、不失控。

### 治理层 Harness

**管的是项目往哪走、走到哪了。**

- `runtime/phase-current.md`：当前阶段目标 + DoD（什么时候算这阶段做完）
- `runtime/issue-dispatch.md`：Issue 下发清单，PM 在这里看全局状态
- `runtime/risk-register.md`：风险台账，风险不记录就等于不存在
- 边界冻结：scope / contract / change 三类边界默认冻结，未获准入不得改写

这一层 Harness 确保项目不会"做着做着就偏了"——阶段目标冻结了，Issue 下发有据可查，风险有人盯。

### 规范层 Harness

**管的是怎么写才对、怎么评才准。**

- `templates/`：Issue / PR / Spec / Harness 四个模板，定义了必填字段，缺项即不合规
- `standards/agents-md-standard.md`：AGENTS.md 怎么写才能让 Agent 行为可预测
- `standards/spec-writing-standard.md`：Spec 怎么写才能被 Agent 和人共同消费
- `standards/fe-be-adoption-gate.md`：代码仓库接入控制面的最低要求
- 真源定义：契约文件是跨端唯一真源，Spec 是验收唯一锚点

这一层 Harness 确保所有人（和 Agent）用同一种语言沟通——模板保证了结构一致性，标准保证了内容质量。

### 开发层 Harness

**管的是代码合不合规、能不能发。**

- Issue 门禁：没有 Spec 链接 + 契约链接 + Harness 计划的 Issue 不进 Ready
- PR 门禁：没有验证证据不合并，证据不可复核视为未验证
- Spec 场景覆盖：PR 要逐条确认覆盖了 Spec 里的 Scenario
- 发版门禁：最小 smoke 清单 + 风险记录 + 回滚方案

详见 `docs/workflow/pr-merge-gate-spec.md`、`docs/harness/`。

### 上下文层 Harness

**管的是 Agent 读取什么、忽略什么。**

这是 AI Native 治理独有的层面。Agent 的上下文窗口有限，塞太多无关信息会导致跑偏。所以：

- Issue body 自包含目标描述，Agent 读一个 Issue 就能干活，不需要加载整个控制面
- 真源表格明确"只加载这些文档"，避免 Agent 自己翻仓库找"有用"的信息
- 固定层/运行层分离：Agent 不需要读阶段进度来决定怎么写代码
- AGENTS.md 定义读取顺序：先读边界规则 → 再读契约 → 最后看 Harness 清单

这一层 Harness 确保给 Agent 的信息**精准且足够**，不多不少。

---

## 四层 Harness 如何配合

```
PM 想推进下一个功能
│
├─ 治理层：阶段目标冻结了吗？边界清楚吗？→ control-plane/ + runtime/phase-current.md
│
├─ 规范层：Spec 写了吗？模板用了吗？→ templates/ + standards/
│
├─ 开发层：Issue 下发了吗？PR 有证据吗？→ workflow/ + harness/
│
└─ 上下文层：Agent 只加载了该加载的吗？→ 真源表格 + AGENTS.md 读取顺序
```

任何一层缺失，都会在下游暴露问题。比如：
- 治理层缺失（没有冻结目标）→ 规范层写不出 Spec → 开发层 Issue 下发不了
- 规范层缺失（没有 Spec）→ 开发层验收凭感觉 → 回写出不来有价值的东西
- 上下文层缺失（没有真源约束）→ Agent 加载一堆无关文档 → 产出偏移目标

---

## 目录结构

```
.
├── AGENTS.md                        ← 控制面 Agent 工作指南
├── openspec/                        ← OpenSpec 工具管理，运行 openspec init 初始化
├── contracts/                       ← 契约真源（推荐 OpenAPI 3.0，按技术栈自选）
├── docs/
│   ├── control-plane/               ← 固定层：项目定义与红线
│   ├── workflow/                    ← 固定层：工作流 SOP
│   ├── harness/                     ← 固定层：验证闭环
│   ├── standards/                   ← 固定层：编写标准
│   └── runtime/                     ← 运行层：阶段进度、Issue 清单、风险台账
└── templates/                       ← 规范层 Harness：可复用模板
```

### control-plane/ 该放什么

这个目录是项目的"宪法层"，定义项目做什么、不做什么、怎么治理：

| 文档类型 | 说明 |
|---------|------|
| **项目范围冻结** | 类似 PRD，但只冻结范围和边界，不写实现细节。明确"必须做"和"不做" |
| **系统一页设计** | 一页纸说清楚系统分层、官方路径、关键边界。让 Agent 快速理解全局架构 |
| **治理规则** | 控制面总则、真源层级、冲突裁决顺序、变更准入 |
| **外部参考与范式** | 项目参考的外部标准、技术范式、设计模式等（引用即可，不复制全文） |

原则：这里只放**长期有效的规则和定义**，不写阶段状态、临时分工、周计划。

### workflow/ 该放什么

定义项目推进的每一步怎么走：

- 变更分级（什么改动可以直接做、什么必须走 OpenSpec、什么要升级 BLOCKER）
- OpenSpec 治理基线（变更的最小产物和归档规则）
- PR 合并门禁与审核 SOP
- 发版策略
- 任务下发与同步节奏
- 端到端示例

### runtime/ 该放什么

运行态数据，可高频更新：

- `phase-current.md`：当前阶段定义（目标 / 非目标 / DoD / Harness 重点）
- `issue-dispatch.md`：Issue 下发清单（先创建 Issue，再贴 URL 回来）
- `risk-register.md`：风险台账

---

## 快速开始

1. 复制本仓库到你的项目组织下（如 `your-org/your-project-docs`）
2. 运行 `openspec init` 初始化 openspec 目录结构
3. 填写 `AGENTS.md.template`，去掉 `.template` 后缀，改为 `AGENTS.md`
4. 在 `docs/control-plane/` 填入：
   - 项目范围冻结（必须做 / 不做 / 延后）
   - 系统一页设计（分层、官方路径、关键边界）
   - 治理规则（真源层级、变更准入）
5. 选择契约格式（推荐 OpenAPI 3.0），在 `contracts/` 下创建契约文件
6. 在各代码仓库创建 `AGENTS.md`（参考 `docs/standards/agents-md-standard.md`）
7. 发起第一个讨论，开始推进

---

## 参考

- 《AI 原生工程》https://tatsukimeng.github.io/ai-native-engineering/
- OpenSpec https://openspec.dev/
