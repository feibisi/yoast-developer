---
title: "Orphaned Content Count Cache Filters"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo Premium
    - Customization
    - Repos
---


Yoast SEO Premium 允许您禁用孤立内容计数缓存，或更改其失效方式。
`wpseo_premium_orphaned_count_cache` 过滤器允许您完全禁用缓存。
`wpseo_premium_orphaned_count_cache_invalidation_method` 过滤器允许您更改缓存失效的方式。

## 使用方法

### 禁用缓存

以下示例展示了如何使用过滤器禁用孤立计数缓存。
```php
<?php

add_filter( 'wpseo_premium_orphaned_count_cache', '__return_false' );
```
### 更改缓存失效方式

以下示例展示了如何使用过滤器更改缓存失效的方式。

有两种选项：
* `flush`，这是默认行为。使用此选项时，当缓存失效时，会使用 `wp_cache_flush_group()` 清空整个缓存组 `orphaned_counts`。
* 或 `delete`。使用此选项时，仅使用 `wp_cache_delete()` 删除与已更改文章的文章类型相关的缓存键。

要将失效方法更改为 `delete`，请使用以下代码：
```php
<?php

add_filter( 'wpseo_premium_orphaned_count_cache_invalidation_method', function() {
    return 'delete';
} );
```