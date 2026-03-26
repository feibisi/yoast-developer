---
title: "Assessment Markers Filter"
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


在某些编辑器中，Yoast SEO 的 SEO 和可读性分析里的评估标记可能无法正常工作。为避免混淆，可以禁用这些标记。

## 使用方法
### 禁用评估标记

以下示例展示了如何使用此过滤器来禁用评估标记。
```php
<?php
add_filter( 'wpseo_enable_assessment_markers', '__return_false' );
```