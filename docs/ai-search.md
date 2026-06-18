# AI Search API

> 官方文档：https://bocha-ai.feishu.cn/wiki/AT9VwqsrQinss7k84LQcKJY6nDh

混合搜索：网页 + 多模态结构化模态卡（天气/百科/股票等）+ 可选大模型生成答案，支持流式输出。

**Endpoint**：`POST https://api.bocha.cn/v1/ai-search`

---

## 请求 Body

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| query | String | 是 | 搜索内容 |
| freshness | String | 否 | 时间范围，同 web-search，默认 `noLimit` |
| include | String | 否 | 限定域名 |
| count | Int | 否 | 返回网页条数，最多 50，默认 10 |
| answer | Boolean | 否 | 是否用大模型生成回答，默认 `true` |
| stream | Boolean | 否 | 是否流式返回，默认 `false` |

---

## 响应：messages[] 结构

每条消息都有 `role`、`type`、`content_type`、`content` 字段。

| type | content_type | 说明 |
|------|--------------|------|
| source | webpage | 网页搜索结果（JSON 字符串） |
| source | image | 图片结果 |
| source | video | 视频结果 |
| source | *（模态卡类型）* | 结构化卡片数据（JSON 字符串） |
| answer | text | Markdown 格式 AI 回答（需 `answer:true`） |
| follow_up | text | 推荐追问问题 |

### 模态卡类型

| content_type | 说明 | 示例搜索词 |
|---|---|---|
| weather_china | 国内天气 | 北京天气 |
| weather_international | 国际天气 | 巴黎天气 |
| baike_pro | 百科专业版 | 西瓜的功效与作用 |
| medical_common | 医疗普通版 | 站着坐着哪个伤腰椎 |
| medical_pro | 医疗专业版 | 同上 |
| calendar | 万年历 | 万年历 |
| train_line | 火车线路/票价 | 长春到白城火车时刻表 |
| train_station_common | 火车时刻表 | K84次列车途经站点 |
| train_station_pro | 火车时刻表专业版 | G2556高铁时刻表 |
| star_chinese_zodiac_animal | 中国属相/生肖运势 | 属虎男最佳婚配属相 |
| star_chinese_zodiac | 属相年份 | 2024年是什么年 |
| star_western_zodiac_sign | 星座 | 水瓶座性格 |
| star_western_zodiac | 星座日期 | 12月是什么星座 |
| gold_price | 贵金属价格 | 今日金价 |
| gold_price_trend | 贵金属价格趋势 | 黄金今日价格 |
| exchangerate | 汇率 | 美元对人民币汇率 |
| oil_price | 油价 | 今日油价 |
| stock | 股票 | 东方财富股票 |
| phone | 手机参数/对比 | 华为pura70和mate60哪个好 |
| car_common | 汽车普通版 | 长安汽车 |
| car_pro | 汽车专业版 | 宝马x3 |

---

## curl 示例

```bash
# 仅搜索结果，不生成 AI 回答（省费用）
curl -s 'https://api.bocha.cn/v1/ai-search' \
  -H "Authorization: Bearer $BOCHA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "北京今天天气",
    "count": 10,
    "answer": false,
    "stream": false
  }'

# 开启 AI 回答（流式）
curl -s 'https://api.bocha.cn/v1/ai-search' \
  -H "Authorization: Bearer $BOCHA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "量子计算最新进展",
    "answer": true,
    "stream": true
  }'
```

---

## 流式响应格式（SSE）

每行格式：`data: {json}`

```
data: {"event":"message","message":{"role":"assistant","type":"source","content_type":"webpage","content":"{...}"},"is_finish":true,"index":0,"conversation_id":"...","seq_id":0}

data: {"event":"message","message":{"role":"assistant","type":"answer","content_type":"text","content":"..."},"is_finish":false,"index":2,"conversation_id":"...","seq_id":5}

data: {"event":"done"}
```

流式字段：
- `event`：`message` / `done` / `error`
- `is_finish`：当前消息是否结束（true = 一条消息完整发完，不代表全部结束）
- `index`：同一 index 的增量内容属于同一条消息
- `seq_id`：全局序号，从 0 开始
