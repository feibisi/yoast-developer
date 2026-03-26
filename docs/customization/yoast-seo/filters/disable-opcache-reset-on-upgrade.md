---
title: "Disable Opcache Reset On Upgrade"
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


Yoast SEO 每次升级到新版本时都会运行 `opcache_reset()`，以处理过时和新的 PHP 文件。

同时，还提供了 `Yoast\WP\SEO\should_invalidate_opcache` 过滤器，可用于禁用此行为。

## 使用方法

以下示例展示了如何使用过滤器在 Yoast SEO 升级时禁用 opcache 重置。
```php
<?php

add_filter( 'Yoast\WP\SEO\should_invalidate_opcache', '__return_false' );
```