# 运行态目录说明（runtime）

## 目录定位

`docs/runtime/` 用于承载**项目推进中的运行态内容**，例如：

- 当前阶段推进信息
- 阶段性 issue 下发清单
- 当期 DoD 跟踪
- 风险台账与同步记录入口
- 运行面板

本目录不是治理真源目录，不承载长期冻结规则。

## 与固定层的关系

固定层（高权重、长期规则）仍在：

- `docs/control-plane/`
- `docs/workflow/`
- `docs/harness/`
- `docs/standards/`

运行态（可高频更新）放在：

- `docs/runtime/`

## 使用规则

1. 运行态文档必须引用对应固定层规则来源。
2. 运行态文档可更新状态，但不得改写固定层规则定义。
3. 阶段结束后，运行态文档应保留必要结论，避免长期堆积过程叙事。

## 当前文件

- `phase-current.md`：当前阶段定义（目标 / 非目标 / DoD / Harness 重点）
- `issue-dispatch.md`：本阶段 Issue 下发与跟踪清单
- `risk-register.md`：本阶段风险台账

## 运行态标签约定

阶段/批次标签（例如 `week:*`、`phase:*`）属于运行态配置。

- 固定层只要求稳定标签类别：`level:*`、`freeze:*`、`status:*`、`release:*`
- 具体阶段/批次标签由当前阶段文档定义并维护
