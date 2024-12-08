---
title: 【Hexo】 Waline--评论系统的添加(1)
cover: /img/article/2/th.webp
categories: Hexo
recommend: true
tags:
  - Hexo
  - Waline
ai_text: 这篇文章介绍了如何为博客集成Waline评论系统，强调评论的重要性和Waline的优势。首先，文章比较了多种主流评论系统，推荐Waline因其免费和易用。接着，详细说明了如何配置主题文件、获取Leancloud的APP ID和Key，以及通过Vercel和Deta快速部署Waline，最后总结Waline作为高性能且安全的选择，适合博客使用。
abbrlink: 56565
date: 2024-06-16 10:18:00
---

{% note 'info flat' 'fas fa-circle-info' %}预计完成时间43分钟，请放心食用！{% endnote %}



# 前言：

众所周知，一个博客是不能没有评论的！

`2024.5.2`：上线了`waline`评论系统

`2024.5.9`：关闭了匿名评论功能

`2024.5.14`：启动了双评论，另一个是`twikoo`

`2024.5.18`：关闭了双评论

`2024.6.14`：重新启动了双评论

但是很多人不知道用什么评论......

所以，今天还是老样子出教程！

# 实践

### Part1:了解主流的评论系统

| 评论系统 | [Valine](https://valine.js.org/) | [Waline](https://waline.js.org/) | [Twikoo](https://twikoo.js.org/) | [Artalk](https://artalk.js.org/) | [Gitalk](https://github.com/gitalk/gitalk) | [Giscus](https://giscus.app/zh-CN) | [畅言](https://changyan.kuaizhan.com/) | [Remark42](https://remark42.com) |
| :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: |
| 推荐指数 | ✔✔ | ✔✔✔✔ | ✔✔✔✔✔ | ✔✔✔ | ✔✔ | ✔✔ | ✔✔✔ | ✔✔ |
| 是否可以白嫖 | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |
| 说明 | 近期被爆出会泄露用户IP | 方便，免费 | 方便，免费 | 方便，要服务器 | 访问慢 | 访问慢 | 要登录，功能全 | 访问慢 |

### Part2:主题配置文件

1. 修改`主题配置文件`：
```bash
# 评论
# comment
# getting start: https://solitude.js.org/comments/comment
comment:
  use: waline # use
  commentBarrage: true # 热评开关 / Hot comment switch
  lazyload: true # 懒加载
  count: true # 评论数展示
  pv: false # 是否使用評論統計頁面訪問
  avatar: https://cravatar.cn # Gravatar link
  newest_comment:
    enable: ture
    storage: .5 # 缓存时间 1: 1天 / .5 : 半天 / Cache time 1: 1 day .5 : half a day
```

如果使用双评论，修改：
```bash
use: 你的评论1,你的评论2 # waline, twikoo, valine, artalk
```

随后修改Waline的配置：

```bash
# waline settings
waline: # https://waline.js.org/
  envId:  # 你的部署环境
  pageview: false # 懒加载
  option: # 更多设置，可以去官网找
```

# Part3:配置评论

教程出自：[Waline](https://waline.js.org)

特色：
- 轻量级与高性能：Waline优化了代码结构，确保快速加载和响应，不增加网站负担。
- 安全性：系统内置了多种安全机制，如垃圾邮件过滤（Akismet支持）、IP限制等，保护您的网站免受恶意评论的侵扰。
- Markdown 支持：全面支持Markdown语法，让用户可以方便地插入代码、链接、表情等元素，让评论更加生动。
- 多平台兼容：不仅有前端组件@waline/client，还有多种服务器端解决方案，如Vercel、Deta、CloudBase等，满足不同开发需求。

配置：

## 准备——配置**Leancloud**

1. 进入[Leancloud国际版](https://console.leancloud.app/register)进行注册并进入控制台：

![](/img/article/2/14bf60d9d7fd5964ccee88f86c9139cb.webp)

{% note 'warning flat' 'fas fa-triangle-exclamation' %}

如果你正在使用 Leancloud 国内版 (leancloud.cn)，我们推荐你切换到国际版 (leancloud.app)。否则，你需要为应用额外绑定已备案的域名，同时购买独立 IP 并完成备案接入:

登录国内版并进入需要使用的应用
选择 设置 > 域名绑定 > API 访问域名 > 绑定新域名 > 输入域名 > 确定。
按照页面上的提示按要求在 DNS 上完成 CNAME 解析。
购买独立 IP 并提交工单完成备案接入。(独立 IP 目前价格为 ￥ 50/个/月)

![](img/article/2/leancloud-3-CT_lZM0A.png)

{% endnote %}

2. 点击左上角**创建应用**，选择**开发板**（免费）

![](/img/article/2/e26c2a7f16d7a7164cb0ecb98c5b390e.webp)

3. 进入应用，选择左下角的 设置 > 应用 Key。你可以看到你的 APP ID,APP Key 和 Master Key，记录它们以便后续使用。

![](/img/article/2/58676c1bb4fd2f35fb23a213fd21dac3.webp)


## 方式1——Vercel

点击一键部署：

[![](/img/article/2/default.svg)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fwalinejs%2Fwaline%2Ftree%2Fmain%2Fexample)

1. 点击上面的按钮，输入你喜欢的名字，点击Create，等待部署：

![](/img/article/2/0574cea0bdc82870997e823195e5348b.webp)

2. 之后满屏的烟花，点击**Continue to Dashboard**

![](/img/article/2/2d8c921741af863b046ebbb8d7b7cfe5.webp)

3. 点击顶部的 Settings - Environment Variables 进入环境变量配置页，并配置三个环境变量 LEAN_ID, LEAN_KEY 和 LEAN_MASTER_KEY 。它们的值分别对应上一步在 LeanCloud 中获得的 APP ID, APP KEY, Master Key。

![](/img/article/2/c6f4d6bced949fdb7ec9bdef576dab88.webp)

4. 回到开头的免费域名，注册一下并关注作者，可以获得5免费域名，点击**创建**：

![](/img/article/2/85bae92761dd2705683b4185a7a26f6d.webp)

5. 输入名字，按如下填写：

![](/img/article/2/5bf63e5dfd90864d24ada804360e52ed.webp)

6. 你就等待审核，预计1小时~2天

![](/img/article/2/496a8213fcfe1b3eac0793bfc0ea0077.webp)

7. 审核完成后会显示**编辑**按钮，回到Vercel，点击**Domains**：

![](/img/article/2/1bbc317d1b20d9b5532b2850505292c9.webp)

8. 填写你申请的域名，点击**Add**：

![](/img/article/2/a438e0447a386031df099a74e7f39961.webp)

9. 等待他解析，然会就可以访问了！

![](/img/article/2/2244070e801aa2177e1925005fb5b874.webp)

## 方式2——Deta（已失效）

~~Deta 是一个可免费使用的 Serverless 部署平台。我们可以快速的将 Waline 部署到 Deta 平台上。~~

在 deta.space 的官网中指出：
```txt
Dear Deta Community,
After an incredible run, we've made the difficult decision to shut down Deta Space. You can continue to login until sunset on October 17, 2024, at which point we will start deleting all Horizons, apps (hosted or installed) and data in Collections.

We've built tools for exporting your data (and code for developers), with tutorials available in the docs.

We wanted to thank you for joining the journey so far. Everything we've done wouldn't have been possible without our community, mods, investors, and friends. We hope you'll stick around for what's next.
```

Deta Space 将于 **2024.10.17 下午日落** 关闭，请所有使用此方式部署的用户进入Waline控制台 -> 管理 -> 导入导出 -> 导出 导出你的数据！

## 方式3——Netlify

Netlify 是知名的静态网站部署服务提供商，Edge Functions 是 Netlify 平台推出的一种服务，它允许在网站的边缘节点上运行 JavaScript 代码。

1. 点击 [Fork](https://github.com/walinejs/netlify-starter/fork) Waline的Netlify数据库。

![](/img/article/2/04534a32d06f65ade0c7f53bb36c7e03e954e5360a2eaca53027caad3e428692.png)

2. 进入 https://app.netlify.com ，注册账号后登录控制台，点击`Add new site` -> `Import an exist project`，选择刚刚Fork的数据库（先绑定Github）。(不要着急Deploy ！！！)

![](/img/article/2/f9b2d3bfbfd1a5004c38c2fb0be33170311b7dd267a84276af6ab0ca2a1e207f.png)

![](/img/article/2/e42e4fe3e37f9989479c685e36456b00e9cc9dc56fc37b020d842ab35de70387.png)

3. 部署之前添加环境变量，以 LeanCloud 部署服务为例，我们在这里增加上 Waline 需要的数据库服务环境变量。点击底部的 Deploy 开始部署网站。

![](/img/article/2/f289d5ae538df8c6b7882685f62626de3681c625958f049df33ebc4cb6ddc1e9.png)

4. 部署完成，可通过一下的域名访问：

```txt
https://你设置的名称.netlify.app/.netlify/functions/comment
```
## 不推荐的其他方式：

1. Railway的免费额度不足以一个月连续使用......
2. Zeabur需要购买（5$每月），否则计划会在5天内被结束......

# 总结

Waline轻量方便，不亚于Twikoo，博客评论选择它也挺不错的！

版权所有©MC-Sep。

