---
title: "Disable Table Of Content Block"
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


我们在 Yoast SEO Premium 21.5 版本中添加了一个过滤器，用于禁用“目录”区块。当你完全未使用该区块时，这可以有效防止在注册“目录”区块时自动为标题生成 HTML 锚点。

请在你的主题 `functions.php` 文件中添加如下代码片段：

```php
add_filter( 'Yoast\WP\SEO\disable_table_of_content_block', '__return_true' );
```