---
title: "Cornerstone Post Types Filter"
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


Yoast SEO 允许您将公开的（自定义）文章类型标记为核心内容。在某些情况下，您可能希望限制可被标记的内容。
`wpseo_cornerstone_post_types` 过滤器允许您修改哪些文章类型能够被标记为核心内容。

## 使用方法

### 移除文章类型

下面的示例展示了如何使用过滤器来移除某个特定的自定义文章类型，使其无法被标记为核心内容。
```php
<?php

/**
 * 移除 'my_custom_post_type'，使其不允许被标记为核心内容。
 *
 * @param array $post_types 当前包含的文章类型。
 *
 * @return array 剩余的文章类型。
 */
function remove_custom_post_type_from_cornerstone( $post_types ) {
	if ( isset( $post_types['my_custom_post_type'] ) ) {
		unset( $post_types['my_custom_post_type'] );
	}

	return $post_types;
}

add_filter( 'wpseo_cornerstone_post_types', 'remove_custom_post_type_from_cornerstone' );
```