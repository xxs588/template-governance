# Spec 编写标准

## 目标

让 spec 可被 AI Agent 和人类共同消费，确保每条规则可验证、可追溯。

## 必填章节

1. **Purpose**：一段话说明这个 spec 管什么、不管什么
2. **Requirements**：使用 MUST/SHALL/MUST NOT 语义描述行为要求
3. **Boundaries**：明确范围内/范围外
4. **References**：上游 spec、受影响仓库、契约文件

## Scenario 写法

每个 Requirement 下至少一个 Scenario，格式：

```
#### Scenario: <场景名>
- **GIVEN** <前置条件>
- **WHEN** <触发动作>
- **THEN** <期望结果>
```

## 写作原则

- **可验证**：每条 THEN 必须能通过测试或人工检查判定 pass/fail
- **长期有效**：不引用具体的 issue 编号、日期、负责人
- **单一职责**：一个 Requirement 只描述一个行为约束
- **不重复**：跨 spec 共享的规则引用上游 spec，不重写

## 反例

- "系统应该有良好的错误处理"（不可验证）
- "本周完成用户模块"（包含时间锚点）
- 同一条规则在三个 spec 里各写一遍（重复）
