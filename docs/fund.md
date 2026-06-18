# 余额查询 API

> 官方文档：https://bocha-ai.feishu.cn/wiki/PriLwr2jyizFuhkSAjGcozSqnrd

实时查询账户余额。

**Endpoint**：`GET https://api.bocha.cn/v1/fund/remaining`

---

## curl 示例

```bash
curl -s -X GET 'https://api.bocha.cn/v1/fund/remaining' \
  -H "Authorization: Bearer $BOCHA_API_KEY" \
  -H 'Content-Type: application/json'
```

## 响应

```json
{
  "success": true,
  "code": "200",
  "msg": "success",
  "data": {
    "remaining": 10820856.78
  },
  "timestamp": 1739845090213
}
```

`data.remaining`：账户余额，单位元，保留两位小数。
