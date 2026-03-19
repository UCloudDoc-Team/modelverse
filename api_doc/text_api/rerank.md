# Rerank 文本排序

Rerank（重排序）能提高检索的准确性和相关性，本文介绍Rerank接口的使用。

## 接口地址

`https://api.modelverse.cn/v1/rerank` 

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
| ---- | ---- | ---- | ---- |
| model      | string | 是 | 模型名称。此处为：`bge-reranker-v2-m3` |
| query      | string | 是 | 查询的内容。 |
| documents  | array[string] | 是 | 待排序的候选文档列表。每个元素是一个字符串。 |
| top_n      | int   | 否 | 返回排序后的top_n个文档。默认返回全部文档。如果top_n值大于文档总数，将返回全部文档。 |

## 注意事项

1. **文本长度限制**：一个 {query+ document} 的最大长度限制为 `8192`

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
```bash
  curl -X POST https://api.modelverse.cn/v1/rerank \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "bge-reranker-v2-m3",
    "query": "what is panda?",
    "documents": [
      "hi",
      "The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China."
    ],
    "top_n": 2
  }'
```

## 响应参数

| 参数 | 类型 | 说明 |
| ---- | ---- | ---- |
| results    | array | 排序结果，按 relevance_score 从高到低排列。 |
| document   | object | 文档原文对象。 |
| document.text   | string | 文档原文 |
| index | int | 表示对应于输入 documents 列表中的原始索引位置。 |
| relevance_score | double   | 文档与查询的语义相关性得分，取值范围为 0.0 到 1.0。分数越高，相关性越强。 |

## 响应示例

```json
{
	"model": "BAAI/bge-reranker-v2-m3",
	"usage": {
		"total_tokens": 53
	},
	"results": [{
		"index": 1,
		"document": {
			"text": "The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China.",
			"multi_modal": null
		},
		"relevance_score": 0.9948425889015198
	}, {
		"index": 0,
		"document": {
			"text": "hi",
			"multi_modal": null
		},
		"relevance_score": 0.0002801174996420741
	}]
}
```
