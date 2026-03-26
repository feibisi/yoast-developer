---
title: "Functional Specification"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Http Headers
    - Features
    - Repos
---


Yoast SEO 构建并优化多种 HTTP 标头，以管理索引、协助调试并补充其他功能。

## x-robots-tag
有时需要防止搜索引擎抓取或索引“非页面”资源（如 PDF 或 XML 文件等不支持 HTML `<head>` 标记的资源）。
当内容管理系统提供这些资源时，我们可以操作其 HTTP 标头，通过 `x-robots-tag` HTTP 标头提供抓取和索引指令。

我们在以下情况下执行此操作：
- 所有 XML 站点地图返回 `noindex, follow` 指令。*注意：这不会影响站点地图的读取或处理。*
- XMLRPC 文件返回 `noindex, follow` 指令。

## x-redirected-by
当我们的重定向管理器执行重定向时，我们将 'Yoast SEO Premium' 值传递给 WordPress 的 `wp_redirect()` 函数。这会在响应中添加 `x-redirected-by` 标头，有助于调试。