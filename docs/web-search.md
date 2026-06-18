# Web Search API

> 官方文档：https://bocha-ai.feishu.cn/wiki/RXEOw02rFiwzGSkd9mUcqoeAnNK

从全网搜索任何网页信息和网页链接，结果准确、摘要完整，更适合 AI 使用。Response 格式兼容 Bing Search API。

**Endpoint**：`POST https://api.bocha.cn/v1/web-search`

---

## 请求

### Header

| 参数 | 值 |
|------|----|
| Authorization | `Bearer {API_KEY}` |
| Content-Type | `application/json` |

### Body

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| query | String | 是 | 搜索词 |
| freshness | String | 否 | 时间范围（见下），默认 `noLimit` |
| summary | Boolean | 否 | 是否返回长文本摘要，默认 `false` |
| include | String | 否 | 限定域名，多个用 `\|` 或 `,` 分隔，最多 100 个 |
| exclude | String | 否 | 排除域名，格式同 include |
| count | Int | 否 | 返回条数 1-50，默认 10 |

**freshness 可填值：**
- `noLimit` — 不限（默认，推荐。搜索算法会自动优化时间范围）
- `oneDay` / `oneWeek` / `oneMonth` / `oneYear`
- `YYYY-MM-DD..YYYY-MM-DD` — 日期范围，如 `"2025-01-01..2025-04-06"`
- `YYYY-MM-DD` — 指定日期

> ⚠️ 推荐用 `noLimit`。指定时间范围可能导致范围内无结果。

---

## 响应

```json
{
  "code": 200,
  "log_id": "d71841ad20095f61",
  "msg": null,
  "data": {
    "_type": "SearchResponse",
    "queryContext": {
      "originalQuery": "阿里巴巴2024年的esg报告"
    },
    "webPages": {
      "webSearchUrl": "",
      "totalEstimatedMatches": 8912791,
      "someResultsRemoved": true,
      "value": [
        {
          "id": null,
          "name": "网页标题",
          "url": "https://...",
          "displayUrl": "https://...",
          "snippet": "网页内容简短描述",
          "summary": "长文本摘要（需 summary:true）",
          "siteName": "网站名称",
          "siteIcon": "https://th.bochaai.com/favicon?domain_url=...",
          "datePublished": "2024-07-22T00:00:00+08:00",
          "dateLastCrawled": "2024-07-22T00:00:00Z",
          "cachedPageUrl": null,
          "language": null,
          "isFamilyFriendly": null,
          "isNavigational": null
        }
      ]
    },
    "images": {
      "value": [
        {
          "thumbnailUrl": "https://...",
          "contentUrl": "https://...",
          "hostPageUrl": "https://...",
          "width": 553,
          "height": 311
        }
      ]
    },
    "videos": null
  }
}
```

> ⚠️ `dateLastCrawled` 返回的 `Z` 结尾时间戳实际是 UTC+8，非 UTC。请用 `datePublished` 字段，或把 `Z` 替换成 `+08:00` 后解析。

> ℹ️ 当前版本 `videos` 始终为空，视频结果请用 AI Search API。

### WebPageValue 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| name | String | 网页标题 |
| url | String | 网页 URL |
| displayUrl | String | URL decode 后的展示 URL |
| snippet | String | 简短描述 |
| summary | String | 长文本摘要（需开启 `summary:true`） |
| siteName | String | 网站名称 |
| siteIcon | String | 网站 favicon URL |
| datePublished | String | 发布时间，UTC+8，如 `2025-02-23T08:18:30+08:00` |
| dateLastCrawled | String | 同发布时间（历史命名问题），末尾 `Z` 实为 UTC+8 |

---

## curl 示例

```bash
curl -s 'https://api.bocha.cn/v1/web-search' \
  -H "Authorization: Bearer $BOCHA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "阿里巴巴2024年的ESG报告",
    "summary": true,
    "freshness": "noLimit",
    "count": 10
  }'
```

## Python 示例

```python
import requests, json

response = requests.post(
    "https://api.bocha.cn/v1/web-search",
    headers={
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    },
    json={
        "query": "天空为什么是蓝色的？",
        "summary": True,
        "count": 10
    }
)
data = response.json()
for page in data["data"]["webPages"]["value"]:
    print(page["name"], page["url"])
```

---

## 错误码

| HTTP 码 | message | 原因 |
|---------|---------|------|
| 400 | Missing parameter query | 缺少 query 参数 |
| 400 | The API KEY is missing | Header 缺少 Authorization |
| 401 | Invalid API KEY | API KEY 无效 |
| 403 | You do not have enough money | 余额不足，前往 https://open.bocha.cn 充值 |
| 429 | You have reached the request limit | 请求频率超限 |
| 500 | （各种） | 服务内部错误 |
