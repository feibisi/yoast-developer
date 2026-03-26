---
title: "Change Metabox Prio Filter"
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


默认情况下，Yoast SEO 元框的优先级为 `high`。这意味着它将显示在编辑器正下方。如有需要，可以降低优先级，使其显示在其他元框下方。

元框优先级可接受的值如下（按显示顺序排列）[^1]：

* `high`
* `core`
* `default`
* `low`

在大多数情况下，`core` 优先级已足以将元框置于重要元框下方，例如在使用 WooCommerce 时。

## 降低元框优先级

以下示例展示了如何降低元框的优先级。
```php
add_filter( 'wpseo_metabox_prio', 'lower_yoast_metabox_priority' );

/**
 * 将 Yoast SEO 元框的优先级降低为 'core'。
 *
 * @param string $priority 当前优先级。
 *
 * @return string $priority 可能调整后的优先级。
 */
function lower_yoast_metabox_priority( $priority ) {
  return 'core';
}
```

[^1]: 根据 [`add_meta_box`](https://developer.wordpress.org/reference/functions/add_meta_box/) 的文档。