---
title: 申请 Personal Access Token
toc: true
---

本指南说明如何申请含有 `repo` 权限的 Github Personal Access Token，并通过此 Token 管理您的 Hexo 博客。

> 遇到问题？请查看 [常见问题](/faq/)。

首先请登录您的 Github 账号，并访问 [新的个人访问令牌（经典版） - github](https://github.com/settings/tokens/new)

- Note 部分填写 `Blog-CMS` 即可。

- Expiration 根据您使用 CMS 的时间情况而定，我们建议您选择 90 天或者永久（No expiration）。

> ⚠️注意：当令牌过期时，**CMS 将无法工作**，请在令牌过期后及时更新以保证正常的使用。

- Select scopes 部分，只勾选 `repo` 权限即可。

然后划到页面底部，点击 `Generate Token` 按钮等待页面跳转。

接下来，将格式为 `ghp_XXX` 的令牌密钥复制到一个安全的位置（建议密码管理器），稍后将会用到它。

> ⚠️注意：**该令牌内容只会显示一次**，刷新后无法再查看它。如果您没有成功保存，请点击右侧删除并重新执行刚才的操作。