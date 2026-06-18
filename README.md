# 博查搜索 Skill

这是符合 [Agent Skill](https://agentskills.io/home) 规范的 "博查搜索 Skill"。
用途：使用 [博查开放平台](https://open.bocha.cn) 提供的 "搜索 API"    

## 安装方法
运行命令： 
```bash
npx skills add 1c7/bocha-search
```

## 使用 Skill 前的必做事情

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


## 重要声明
1. 本仓库的 bocha-search skill 是第三方开发的。不是官方开发的，  
官方的 Github 组织是 https://github.com/BochaAI    
截止至 2026 年 6 月 18 号，官方尚未发布 Skill。    

2. 我的目的是：方便在 Claude Code 里面使用博查进行搜索。

3. 本 Skill 基于 [博查官方开发文档](https://bocha-ai.feishu.cn/wiki/HmtOw1z6vik14Fkdu5uc9VaInBb)