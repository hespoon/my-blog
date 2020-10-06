---
title: 使用 Hexo 和 Git Page 搭建博客
date: 2019-04-16 19:40:42
tags:
---
# 搭建过程

* Hexo 的教程在官方文档中写的很详细，这里不再赘述。（实际上是内容太多了，官方文档写的详细，去看官方文档比较好）
* 利用 GitHub 提供的 Git Page 搭建自己的博客，免费的 .github.io 域名，不用购买服务器。
* 遇到的最大问题是 Hexo 主题在本地成功显示但是部署以后无法获取 .css 文件，浏览器报错 404。问题的原因是 Hexo 项目的 `_config.yml` 文件中的 `url` 和 `root` 配置出错。
  1. 如果博客的链接是 `https://yoursite.com/child`，则 `url` 设置为 `https://yoursite.com`，`root` 设置为 `/child`。
  2. 如果博客的的链接是 `https://yoursite.com`，则 `url` 设置为 `https://yoursite.com`，`root` 设置为 `/`。
* 使用 cnzz 站长工具时，如果访问者访问网站时浏览器中开启了广告屏蔽插件（如 AdGuard），则一般会导致 cnzz 脚本添加失败，无法统计该次访问。
# 使用问题

* 无法正确渲染 markdown 表格
* _config.yml 文件配置总是不起作用

