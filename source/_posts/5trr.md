---
title: Cloudflare Pages+Telegraph搭建简易图床
cover: /img/article/19/th.webp
katex: true
categories: 实用教程
tags:
  - Cloudflare Pages
ai_text: 
  这篇文章介绍了作者放弃 Vercel Blob 图床，转向使用 Cloudflare Pages 结合 Telegraph
  搭建简易图床的过程。详细讲解了域名准备、GitHub 仓库 Fork 和部署、管理员账户设置以及可选的自定义域名绑定，最后总结了这种搭建方式的适用性和效果。
abbrlink: 58383
date: 2024-10-27 03:24:00
---

# 前言

之前那个Vercel Blob图床太LAJI了，这次换一个新的Cloudflare Pages+Telegraph搭建简易图床！

~~总今天开始，本站的所有照片都会被托管到我的图床！~~

本站的图床出现了点小问题，所以图床从今天开始维护，开放时间暂定。

{% note 'warning flat' 'fas fa-triangle-exclamation' %}请不要滥用，所有人都不想失去这免费的东西！ {% endnote %}

# 实践

1. 准备好域名，绑定到Cloudflare（后称cf）上，当然你不绑也行，毕竟.pages.dev域名还没有被ban。。。

   ![](/img/article/19/2e37d166e54b37a35710ea5d0499552a3fe8da92c40c22d1b72ee91f24f56c8f.png)

2. fork这个仓库：

{% github 'cf-pages/Telegraph-Image' %}

3. 进入 CF 的 Workers和 pages页面，点击创建

   ![创建](/img/article/19/0fe8b05d2463f89cf2f13ed9d9d7a6663a02478d226cf8c7415ea82749fa8cb5.png)

   4. （这里用Pages不是Workers！）选择刚才的仓库，点击部署

   5. 部署完成后进入设置 -> 变量和机密，添加两个值：

      | BASIC_USER | 后台管理员的用户名 |
      | :--------: | :----------------: |
      | BASIC_PASS |   后台的登陆密码   |

      后台在 **https://你在cf pages的域名/admin** (有点丑。。。)

      {% note 'warning flat' 'fas fa-triangle-exclamation' %}当然你也可以不设置（个容易被制裁）{% endnote %}

   6. 回到 Workers和Pages，选择 KV ，创建命名空间，名字填 **img_url** （我这里已经搞了）

      ![KV](/img/article/19/349d5df03916a6fd05ec0003e8cf617dad7b24ac4d8b05691c14f77b6a43a810.png)

   7. 回到刚才的Pages，进入设置 -> 绑定，添加一个，名字还是**img_url**，选择刚才的KV

      ![em](/img/article/19/ec8d78269a423a223a25940d3b6227fd42a8fd1904fed0e3eba78f13a4de858d.png)

   8. 重新部署一下就完成了！可以测试一下，效果还可以awa

   9. （可选）自定义域名的绑定：

      ![](/img/article/19/a0cc918384e4fc93dccc3ef3cc6c32c624d51626af5ac3d039b7cb6d5f8ddd8f.png)

