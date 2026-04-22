# 控制面（control-plane）

冻结的项目红线：范围边界、契约边界、变更边界、治理规则。

## 这里放什么

| 文件 | 职责 |
|------|------|
| `control-plane-handbook.md` | 治理主真源：durable rules、冻结边界、真源层级 |
| `ai-native-governance.md` | 三层控制面总则（Prompt/Context/Harness） |
| `mvp-scope.md` | 项目范围冻结 |
| `system-design-on-pager.md` | 系统一页设计（分层、官方路径、关键边界） |

## 规则

- 这里不能随便改。小调整通过 PR + 审核即可，涉及边界/契约/门禁的重大变更需经充分讨论后走 OpenSpec。
- 不写高时效状态叙事（周计划、临时分工等放 `docs/runtime/`）。

## 和其他目录的关系

- 具体流程怎么走 → `docs/workflow/`
- 怎么验证是否合规 → `docs/harness/`
- 当前阶段推进到哪了 → `docs/runtime/`
