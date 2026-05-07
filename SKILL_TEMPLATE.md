# Skill 名称

> 一句话描述该 Skill 的核心功能

## 基本信息

| 字段 | 内容 |
|------|------|
| **分类** | （例：网络搜索 / 数据分析 / 代码辅助 / 文档处理） |
| **适用模型** | （例：GPT-4o / Claude 3.5 / 通用） |
| **作者** | your-github-username |
| **版本** | 1.0.0 |

## 功能描述

详细说明该 Skill 能做什么、解决什么问题、适合哪些场景。

## 使用前提

- 列出必要的工具、API Key 或环境依赖
- 例：需要配置 `SEARCH_API_KEY`

## 提示词 / 配置

```
在此粘贴完整的 System Prompt 或配置内容
```

## 工具调用（可选）

如果该 Skill 依赖工具调用，在此描述工具签名：

```json
{
  "name": "tool_name",
  "description": "工具描述",
  "parameters": {
    "type": "object",
    "properties": {
      "param1": {
        "type": "string",
        "description": "参数描述"
      }
    },
    "required": ["param1"]
  }
}
```

## 使用示例

**输入：**
```
示例用户输入
```

**输出：**
```
示例 Agent 输出
```

## 注意事项

- 列出使用时的限制、边界条件或已知问题

## 更新日志

- `1.0.0` 初始版本
