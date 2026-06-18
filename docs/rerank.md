# Semantic Reranker API

> 官方文档：https://bocha-ai.feishu.cn/wiki/LHwfwDUGeihkJ2kOlj2cccuNndh

分析 query 与候选文档的语义相关度，给出打分和排序。基于 Transformer 架构，80M 参数，推理速度快。

**Endpoint**：`POST https://api.bocha.cn/v1/rerank`  
**计费**：0.005 元/次

---

## 请求 Body

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| model | String | 是 | `gte-rerank` |
| query | String | 是 | 查询语句 |
| documents | List\<String\> | 是 | 候选文档列表，最多 50 条 |
| top_n | Int | 否 | 返回前 N 条结果 |
| return_documents | Boolean | 否 | 是否在结果中返回文档原文 |

---

## 响应

```json
{
  "code": 200,
  "log_id": "56a3067f9b92dfd0",
  "data": {
    "model": "bocha-semantic-reranker",
    "results": [
      {
        "index": 0,
        "relevance_score": 0.7166,
        "document": { "text": "..." }
      },
      {
        "index": 1,
        "relevance_score": 0.5658,
        "document": { "text": "..." }
      }
    ]
  }
}
```

`index` 对应原始 documents 数组的下标，结果已按 `relevance_score` 降序排列。

---

## curl 示例

```bash
curl -s 'https://api.bocha.cn/v1/rerank' \
  -H "Authorization: Bearer $BOCHA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "gte-rerank",
    "query": "阿里巴巴2024年的ESG报告",
    "top_n": 3,
    "return_documents": true,
    "documents": [
      "阿里巴巴集团发布《2024财年ESG报告》...",
      "ESG的核心是围绕如何成为一家更好的公司..."
    ]
  }'
```
