---
title: "Markdown Enabled Filter"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Filters
    - Yoast Seo
    - Customization
---


Yoast SEO 可以在内容分析中提供 Markdown 支持。默认情况下此功能是禁用的，除非启用了 Jetpack 的 Markdown 模块。可以将其扩展到其他在内容中解析 Markdown 的插件。

## 使用方法
### 启用 Markdown 支持

下面的示例展示了如何使用此过滤器来启用 Markdown 支持。
```php
<?php
add_filter( 'wpseo_is_markdown_enabled', '__return_true' );
```