# 博查搜索 Skill

给 AI 用的中文搜索 Skill，基于[博查开放平台](https://open.bocha.cn) API。

## 安装

```bash
npx skills add 1c7/bocha-search
```

或手动把本仓库克隆到你的 agent skills 目录。

## 需要

在 https://open.bocha.cn 注册后获取 API KEY，设置环境变量：

```bash
export BOCHA_API_KEY="your-api-key"
```

## API

| API | 用途 |
|-----|------|
| [Web Search](docs/web-search.md) | 全网网页搜索，默认首选 |
| [AI Search](docs/ai-search.md) | 混合搜索 + 模态卡（天气/股票/汇率等）+ 可选 AI 回答 |
| [Rerank](docs/rerank.md) | 语义排序，用于 RAG 结果重排 |
| [余额查询](docs/fund.md) | 查询账户剩余额度 |
