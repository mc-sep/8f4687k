---
title: 【Hexo】如何提高静态博客访问速度？Vercel+Cloudflare+CDN让你的网站速度飞起！
cover: /img/article/20/th.webp
katex: true
categories: Hexo
tags:
  - Hexo
  - Vercel
  - Cloudflare
  - CDN
abbrlink: 59383
date: 2024-11-17 06:24:00
ai_text: 这篇文章介绍了通过优化博客平台、使用CDN加速、压缩图片和代码来提升网站加载速度。建议替换掉Github Pages，使用更快速的Vercel或Cloudflare，统一使用图床并压缩图片，利用Gulp自动化压缩JS、CSS和HTML，从而大幅提升博客性能，确保更流畅的用户体验。
---

# 前言

随着博客文章数量的增多，我们的博客可能会变得十分缓慢（比如前段时间的我），今天的教程可以让你的博客提速**90%**！

# 实操

## 整改1 扔掉Github Pages

Github Pages~~真垃圾~~很慢，看看对比：

|  名称  | Vercel | CF Workers（反代） | CF Pages | Netlify | Zeabur | Github Pages |
| :----: | :----: | :----------------: | :------: | :-----: | :----: | :----------: |
|  速度  | 🌟🌟🌟🌟🌟  |        🌟🌟🌟🌟        |  🌟🌟🌟🌟🌟   |  🌟🌟🌟🌟🌟  | 🌟🌟🌟🌟🌟  |      🌟🌟      |
| 稳定性 |   5    |         5          |    5     |    5    |   5    |      5       |

还不把Github Pages扔了！Vercel他不香吗！参考：

{% article '/post/43vc.html' %}

也可以使用 CF Workers反代，参考：[利用Cloudflare Workers搭建镜像网站 | Minecraft-Sep (mc-sep.top)](https://blog.mc-sep.top/post/437r.html)

## 整改2 域名

参考（有几个提别好的）：

{% article '/post/re8u.html' %}

## 整改3 CDN

CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。

通俗易懂的解释： 通过IP访问实际资源时，如果CDN上并没有缓存资源，则会到源站请求资源，并缓存到CDN节点上，这样，用户下一次访问时，该CDN节点就会有对应资源的缓存了。

Vercel CDN的优化

- 使用他人的Vercel CDN（自动使用节点）：[Vercel CDN:利用CNAME负载均衡实现的Vercel加速CDN (yt-blog.top)](https://vercel.cdn.yt-blog.top/)
- 在Vercel里使用Hong Kong节点

CF的优化

- 优化选项卡里的能开就开

{% note 'warning flat' 'fas fa-triangle-exclamation' %}

注意：最近推出的Speed Brain有问题，不建议使用！

{% endnote %}

## 整改4 图片问题

最拖慢速度二巨头：图片过大，图片源不同一（特别是butterfly系列主题深有体会）

建议统一使用一个图床，或者上传到自己的图床，这里提供两个教程

- CF+Telegraph（维护中）
- Vercel+Bulb

{% article '/post/239d.html' %}

其次，图片过大可以使用[TinyPNG – 智能压缩您的WebP、JPEG和PNG图片 (tinify.cn)](https://tinify.cn/)或者[TinyPNG – Compress WebP, PNG and JPEG images intelligently](https://tinypng.com/)压缩，是无损压缩，也可以转换为png,jpeg和webp格式。

或者使用[Jsdelivr (jsdelivr.net)](https://cdn.jsdelivr.net/)加速访问托管在github 的图片：

1. 将图片上传到github

2. 在网址栏输入：

   ```url
   https://cdn.jsdelivr.net/npm/你的github用户名/你保存图片的repo/[图片路径]
   ```

3. 如果出现图片就代表成功！

## 整改5 Gulp

Gulp是一个hexo插件，用于压缩js和css文件。

1. 安装：

   ```bash
   npm install gulp compress gulp-clean-css gulp-html-minifier-terser gulp-htmlclean gulp-terser --save-dev
   ```

2. 创建 `gulpfile.js` 文件：

   ```js
   var gulp = require('gulp');
   var cleanCSS = require('gulp-clean-css');
   var htmlmin = require('gulp-html-minifier-terser');
   var htmlclean = require('gulp-htmlclean');
   var terser = require('gulp-terser');
   // 压缩js
   gulp.task('compress', () =>
   gulp.src(['./public/**/*.js', '!./public/**/*.min.js'])
   .pipe(terser())
   .pipe(gulp.dest('./public'))
   )
   //压缩css
   gulp.task('minify-css', () => {
   return gulp.src(['./public/**/*.css'])
   .pipe(cleanCSS({
   compatibility: 'ie11'
   }))
   .pipe(gulp.dest('./public'));
   });
   //压缩html
   gulp.task('minify-html', () => {
   return gulp.src('./public/**/*.html')
   .pipe(htmlclean())
   .pipe(htmlmin({
   removeComments: true, //清除html注释
   collapseWhitespace: true, //压缩html
   collapseBooleanAttributes: true,
   //省略布尔属性的值，例如：<input checked="true"/> ==> <input />
   removeEmptyAttributes: true,
   //删除所有空格作属性值，例如：<input id="" /> ==> <input />
   removeScriptTypeAttributes: true,
   //删除<script>的type="text/javascript"
   removeStyleLinkTypeAttributes: true,
   //删除<style>和<link>的 type="text/css"
   minifyJS: true, //压缩页面 JS
   minifyCSS: true, //压缩页面 CSS
   minifyURLs: true  //压缩页面URL
   }))
   .pipe(gulp.dest('./public'))
   });
   
   // 运行gulp命令时依次执行以下任务
   gulp.task('default', gulp.parallel(
   'compress', 'minify-css', 'minify-html'
   ))
   ```

3. 运行gulp：

   ```bash
   hexo g&&gulp
   ```

# 加速你也做的差不多了

结束！

版权所有©MC-Sep.