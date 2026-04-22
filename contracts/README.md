# 契约目录（contracts）

存放 FE/BE 共同消费的接口契约文件，是跨端协作的唯一真源。

## 推荐格式

- **OpenAPI 3.0**（推荐）：RESTful API 项目首选，工具链成熟（spectral、redocly-cli、openapi-typescript）
- GraphQL Schema：GraphQL 项目
- Protobuf / gRPC：微服务项目
- JSON Schema：事件驱动或数据交换场景

选择后，在本目录创建对应文件（如 `api/openapi.yaml`），并在 AGENTS.md 中声明契约位置。
