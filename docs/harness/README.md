# 验证闭环（harness）

## 这目录管什么

定义"怎么验证是否合规"：合并门禁、发版门禁、Harness 闭环标准。

这里规定的是**验证规则**，具体执行由 FE/BE 仓库的 CI 落地。

## 包含文件

| 文件 | 职责 |
|------|------|
| `harness-closed-loop.md` | Harness 四步闭环：执行 → 观察 → 验证 → 收敛 |
| `harness-gate-baseline.md` | 合并门禁 + 发版门禁基线，含例外规则 |

## 和其他目录的关系

- 门禁触发前提（什么算越界）→ `docs/control-plane/`
- PR 提交流程（门禁怎么卡）→ `docs/workflow/pr-merge-gate-spec.md`
- 验证清单模板 → `templates/harness-checklist.md`
- 当前阶段该验证什么 → `docs/runtime/phase-current.md`
