---
title: "Hiding Version Number"
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


我们在 Yoast SEO Premium 2.3.3 版本中添加了一个过滤器，允许您在 HTML 源代码中隐藏版本号。

请将以下代码添加到您主题的 `functions.php` 文件中。

```php
add_filter( 'wpseo_hide_version', '__return_true' );
```