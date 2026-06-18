---
name: bocha-search
description: |
  使用博查（Bocha）搜索 API 搜索网页。博查是专为 AI 设计的中文搜索引擎，数据合规、质量高。
  四种 API：web-search（全网搜索）、ai-search（混合搜索+模态卡+可选AI回答）、rerank（语义排序）、fund/remaining（余额查询）。
  当用户需要搜索中文网页、获取实时信息（天气/股票/汇率/新闻）、对 RAG 候选结果重排序、或查询 API 余额时使用。
allowed-tools: Bash(curl *)
---

# 博查搜索 API

Base URL：`https://api.bocha.cn/v1/`  
Auth：所有请求需 Header `Authorization: Bearer $BOCHA_API_KEY`（在 https://open.bocha.cn 获取）

## API 目录

| API | 文档 | 用途 |
|-----|------|------|
| Web Search | [docs/web-search.md](docs/web-search.md) | 全网网页搜索，**默认首选** |
| AI Search | [docs/ai-search.md](docs/ai-search.md) | 混合搜索 + 模态卡 + 可选 AI 回答 |
| Rerank | [docs/rerank.md](docs/rerank.md) | 语义排序，用于 RAG 结果重排 |
| 余额查询 | [docs/fund.md](docs/fund.md) | 查询账户剩余额度 |

## 选择指南

- **绝大多数搜索** → `web-search`，开启 `summary:true` 获得完整摘要
- **需要结构化数据**（天气/股票/汇率/百科等模态卡）→ `ai-search`，`answer:false`
- **需要 AI 总结回答** → `ai-search`，`answer:true`，流式用 `stream:true`
- **RAG 重排序** → `rerank`
- **查余额** → `fund/remaining`
