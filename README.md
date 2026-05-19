# GPT Session 转换站

一个纯前端单页工具，用于把 ChatGPT / Codex Session JSON 转换为多种中转站或工具可导入的 JSON 格式。

## 支持输出

- CPA auth JSON
- sub2api 导入 JSON
- Cockpit Tools Codex JSON
- 9router Codex OAuth JSON
- AxonHub Codex auth.json

## 获取 Session JSON

先在浏览器登录 ChatGPT，然后打开：

```text
https://chatgpt.com/api/auth/session
```

复制页面返回的完整 JSON，粘贴到本工具输入框。

## 支持输入

工具会尽量识别这些常见来源：

- ChatGPT Web session JSON
- Codex / OpenAI OAuth JSON
- CPA auth JSON
- 9router Codex OAuth JSON
- AxonHub Codex auth.json
- 包含多个账号对象的 JSON 数组或嵌套对象
- 拖入的一个或多个 `.json` 文件

常见字段包括：

```text
accessToken
access_token
refreshToken
refresh_token
idToken
id_token
sessionToken
session_token
user.email
account.id
account.planType
providerSpecificData.chatgptAccountId
providerSpecificData.chatgptPlanType
tokens.access_token
tokens.refresh_token
tokens.id_token
```

页面也会尝试从 `accessToken` 或 `idToken` 的 JWT payload 中补充：

```text
email
exp
https://api.openai.com/auth.chatgpt_account_id
https://api.openai.com/auth.chatgpt_plan_type
https://api.openai.com/auth.chatgpt_user_id
https://api.openai.com/profile.email
```

## 使用方式

直接打开：

```text
index.html
```

或放到任意静态网站目录中访问。这个项目不需要 Node.js、后端服务或构建步骤。

## 功能

- 粘贴 JSON 后自动解析
- 手动点击生成
- 支持格式化输入 JSON
- 支持拖入多个 JSON 文件
- 支持复制输出
- 支持下载输出 JSON
- 支持一次识别多个账号
- 支持在 CPA / sub2api / Cockpit / 9router / AxonHub 之间切换输出
- 缺少关键字段时显示提示

## 关于缺失字段

ChatGPT Web session 通常不包含 `refresh_token`。如果输出格式需要 `refresh_token`，工具会保留空值或写入占位提示。此时 `access_token` 过期后通常不能自动刷新。

如果输入缺少真实 `id_token`，但能识别到 `account_id`，工具会生成一个 Codex 可解析的占位 JWT claims，用于让 CPA 等工具识别账号和套餐信息。这个占位 token 不是 OpenAI 签发的真实 `id_token`，实际请求仍依赖 `access_token`。

AxonHub 格式在缺少真实 `refresh_token` 时会写入：

```text
__missing_refresh_token__
```

这只适合在 `access_token` 过期前试用。

## 安全说明

本工具是纯前端页面，所有转换都在浏览器本地完成。

页面不包含：

- `fetch`
- `XMLHttpRequest`
- `sendBeacon`
- `localStorage`
- `sessionStorage`
- `document.cookie`
- 第三方统计脚本
- CDN 脚本

也不会主动请求 `https://chatgpt.com/api/auth/session`。请用户自行打开该地址复制 JSON，再粘贴到页面。

Session JSON、access token、refresh token、session token 都是敏感登录凭证，不要发送给不可信的人或网站。

## 部署

把 `index.html` 上传到任意静态站点即可，例如：

```text
/www/wwwroot/cpa-auth-json/index.html
```

## 更新说明

本项目用于个人自用测试，主要目标是把 ChatGPT / Codex Session JSON 转成常见中转站或工具可导入的 JSON 格式。

当前版本支持：

- CPA auth JSON
- sub2api 导入 JSON
- Cockpit Tools Codex JSON
- 9router Codex OAuth JSON
- AxonHub Codex auth.json
- 单账号和多账号 JSON 识别
- 粘贴输入和本地 JSON 文件拖入

后续更新会优先围绕字段兼容、页面易用性和输出格式稳定性处理。

## 免责声明

本项目仅供个人学习、自用测试和数据格式转换使用。使用者应自行确认账号、Token、Session JSON 的来源合法性，并自行承担使用本工具及导入第三方系统后的全部风险。

本项目不会主动上传、保存或转发用户粘贴的 Token、Session JSON 或生成结果，但浏览器环境、部署环境和使用方式由使用者自行控制。请不要在不可信设备、不可信网络或不可信站点上处理敏感登录凭证。

本项目不保证转换结果适用于所有版本的 CPA、sub2api、Cockpit、9router、AxonHub 或其他第三方工具。第三方工具字段格式变更、接口策略变化、账号权限限制、Token 失效等情况，均可能导致导入失败或无法使用。

如因使用本项目造成账号异常、Token 泄露、服务不可用、数据损失或其他问题，项目作者不承担相关责任。

## 联系方式

有问题可联系：

```text
duanxinyua@gmail.com
```
