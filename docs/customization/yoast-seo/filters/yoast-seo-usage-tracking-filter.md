---
title: "Yoast Seo Usage Tracking Filter"
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


Yoast SEO 具备使用情况追踪功能，用于收集基础站点与服务器数据、插件使用情况、站点上安装的其他插件以及当前使用的主题。
通过 `wpseo_enable_tracking` 过滤器，您可以修改 Yoast SEO 是否追踪这些数据。
在 Yoast SEO 免费版中，仅当用户在配置向导中明确同意时才会启用使用情况追踪。而在我们的附加组件或 Yoast SEO Premium 中，默认会启用数据追踪。

## 使用方法

### 启用 Yoast SEO 数据追踪

以下示例展示了如何使用过滤器来启用 Yoast SEO 数据追踪。
```php
<?php

/**
 * 启用 Yoast SEO 数据追踪
 */
add_filter( 'wpseo_enable_tracking', '__return_true' ); 
```