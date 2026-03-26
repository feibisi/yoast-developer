---
title: "Deprecations"
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


我们有时会弃用过滤器和操作。本文档重点介绍 Yoast SEO Premium 中的弃用项，并尝试指引您找到替代方案。

## Yoast SEO Premium 12.9 中弃用的过滤器和操作

### 过滤器

#### `wpseo_premium_post_redirect_slug_change`
页面或文章别名更改时的自动重定向现在可以通过 `Yoast\WP\SEO\post_redirect_slug_change` 过滤器实现。

#### `wpseo_premium_term_redirect_slug_change`
分类项别名更改时的自动重定向现在可以通过 `Yoast\WP\SEO\term_redirect_slug_change` 过滤器实现。