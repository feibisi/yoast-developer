---
title: "Disabling Primary Category"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo
    - Customization
    - Repos
---


自 Yoast SEO 3.1 版本起，我们新增了[为文章设置主要分类](https://yoast.com/help/how-to-select-a-primary-category/)的功能。
但在某些情况下，您可能不希望使用此功能。因此，我们添加了一个过滤器，允许您完全禁用它。

请将以下代码添加到您主题的 `functions.php` 文件中。

```php
add_filter( 'wpseo_primary_term_taxonomies', '__return_empty_array' );
```