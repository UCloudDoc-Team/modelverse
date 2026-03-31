# API Key 精细化权限控制

## 简介

2026-03-20 新增 API Key 精细化权限控制功能，支持按额度、按模型、按IP白名单维度进行权限控制。

> 注意：此功能为实验性功能，如需使用，请联系产品开通新平台权限。

## 功能说明

### 额度控制

API Key 支持设置 `每日` 或 `每月` 费用额度上限，超过上限后将无法继续调用API。额度可以在`创建/更新` API Key时设置。
> 注意：额度为可选配置，不配置时默认不限制费用额度。
>      额度限制以该key真实消费的费用为准，而非预估费用。（费用订单为后付费，每小时结算一次，因此额度限制可能存在一定的延迟，建议设置时预留一定的余量）
>      额度在费用结算后可能因扣费流程存在短暂的延迟。

#### 操作步骤如下

1. 登录控制台，进入API Key管理页面
2. 点击"创建API Key"或"编辑API Key"
3. 在"额度控制"区域，填写"每日"或"每月"额度上限
4. 点击"创建"按钮或"保存"按钮

![额度控制](/images/api-doc/api-key-amount.png)
![额度控制](/images/api-doc/api-key-amount-2.png)

#### 调用示例

```bash
curl https://api.modelverse.cn/v1/responses\
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.4",
    "input": "Hello! Who are you?"
  }'

{"error":{"message":"Access forbidden: api key quota exceeded, key_id=uminferapikey-1j3gwjgyhkwc, daily_limit_amount=100 , monthly_limit_amount=1000","type":"permission_error","param":"c2ad1049-66a6-4640-b2b5-1711dc401092","code":"forbidden"}}
```

### 模型控制

API Key 支持设置可调用的模型列表，仅可调用列表中的模型。模型可以在`创建/更新` API Key时设置。
> 注意：模型控制为可选配置，不配置时默认可调用所有模型。
>      页面选择支持按照供应商等选择模型，但是不支持新增模型自动添加到列表中。如需添加新模型，需要手动添加。

> 例：创建 API Key 时，勾选全部OpenAI模型为可调用模型，在此时间点后推出新模型，该API Key仍无法调用新模型。如需调用新模型，需要更新API Key的可调用模型列表。

![模型控制](/images/api-doc/api-key-model.png)

#### 调用示例

```bash
 curl https://api.modelverse.cn/v1/responses\
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.4",
    "input": "Hello! Who are you?"
  }'

{"error":{"message":"No permission to use the model: apikey [uminferapikey-1j3gwjgyhkwc] not support model [gpt-5.4]","type":"invalid_request_error","param":"6d7248f8-ebaa-437e-bfc2-d79e7c54004c","code":"model_error"}}
```
### IP白名单控制

API Key 支持设置可调用的IP白名单，未设置时可从任意IP使用该API Key调用API，设置后仅可从IP白名单中的IP调用API。IP白名单可以在`创建/更新` API Key时设置。

> 注意：IP白名单为可选配置，不配置时默认可从任意IP使用该API Key调用API。当前仅支持IPv4地址。
>      支持IP和网段格式，如 `192.168.1.1`、`192.168.1.0/24`、`192.168.1.10-192.168.1.20`。

![IP白名单控制](/images/api-doc/api-key-ip.png)

#### 调用示例

```bash
 curl https://api.modelverse.cn/v1/responses\
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.4",
    "input": "Hello! Who are you?"
  }'

{"error":{"message":"Access forbidden: api key ip not in whitelist, key_id=uminferapikey-1j3gwjgyhkwc, ip=127.0.0.1","type":"permission_error","param":"c2ad1049-66a6-4640-b2b5-1711dc401092","code":"forbidden"}}
```