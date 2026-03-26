---
title: "Deprecations"
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


我们有时会弃用过滤器和操作。本文档重点介绍 Yoast SEO 中的弃用项，并尝试为您指出替代方案。

## Yoast SEO 14.0 中弃用的过滤器和操作

### 过滤器

#### `wpseo_twitter_taxonomy_image`
现在您始终可以通过 `wpseo_twitter_image` 过滤器来筛选任何页面（包括分类页面）的 X 图片。

#### `wpseo_twitter_metatag_key`
我们移除了此过滤器且未提供替代方案。如需添加更多值，请根据[此文档](/customization/apis/metadata-api)添加新的元数据呈现器。

### 操作

#### `wpseo_opengraph`
我们移除了此操作且未提供替代方案。如需添加更多值，请根据[此文档](/customization/apis/metadata-api)添加新的元数据呈现器。

#### `wpseo_twitter`
我们移除了此操作且未提供替代方案。如需添加更多值，请根据[此文档](/customization/apis/metadata-api)添加新的元数据呈现器。