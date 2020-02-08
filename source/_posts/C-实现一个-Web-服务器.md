---
title: C++ 实现一个 Web 服务器
date: 2020-01-23 16:12:00
tags:
---
# Web 服务器
Web 服务器是 HTTP 请求的应答方。
# CDN Content Delivery Network
内容分发网络。依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，挑个用户访问速度和命中率。
# WAF Web Appalication Firewall
网络应用防火墙。通过执行一系列针对 HTTP/HTTPS 的安全策略来专门为 Web 应用提供保护的一款产品。它是应用层面的防火墙，专门检测 HTTP 流量，是防护 Web 应用的安全技术。WAF 通常位于 Web 服务器之前，可以阻止 SQL 注入、跨站脚本等攻击。
# URI Uniform Resource Identifier
统一资源标识符。使用 URI 可以唯一的标记互联网上的资源。
# URL Uniform Resource Locator 
统一资源定位符。它是 URI 的一个子集。
URL 分析 `http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument`
`http://` 是方案或协议，该部分告诉浏览器使用何种协议。
# URN Uniform Resource Name
统一资源名称。