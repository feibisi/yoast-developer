---
title: "Primary Term Taxonomies Filter"
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


Yoast SEO 允许您将分类设置为文章和页面的主要术语。在某些情况下，您可能希望允许用户使用额外的自定义分类法作为主要术语。
`wpseo_primary_term_taxonomies` 过滤器允许您修改哪些分类法可以被指定为主要术语。

## 使用方法
### 添加分类法

下面的示例展示了如何使用过滤器将分类法添加到潜在主要术语列表中。
```php
<?php

/**
 * 添加自定义分类，使其可作为主要术语被选择。
 *
 * @param array  $taxonomies     当前可用作主要术语的分类法。
 * @param string $post_type      当前文章类型。
 * @param array  $all_taxonomies 所有已注册的分类法。
 *
 * @return mixed 可用作主要术语的分类法。
 */
function add_additional_primary_term_taxonomies( $taxonomies, $post_type, $all_taxonomies ) {
	if ( $post_type === 'my_custom_post_type' && isset( $all_taxonomies['my_custom_category'] ) ) {
		$taxonomies['my_custom_category'] = $all_taxonomies['my_custom_category'];
	}

	return $taxonomies;
}

add_filter( 'wpseo_primary_term_taxonomies', 'add_additional_primary_term_taxonomies', 10, 3 );
```

### 移除分类法

下面的示例展示了如何使用过滤器移除特定分类法，使其无法被选为主要术语。
```php
<?php

/**
 * 移除自定义分类，使其不可作为主要术语被选择。
 *
 * @param array  $taxonomies     当前可用作主要术语的分类法。
 * @param string $post_type      当前文章类型。
 * @param array  $all_taxonomies 所有已注册的分类法。
 *
 * @return mixed 可用作主要术语的分类法。
 */
function remove_primary_term_taxonomies( $taxonomies, $post_type, $all_taxonomies ) {
	if ( isset( $taxonomies['my_custom_category'] ) ) {
		unset( $taxonomies['my_custom_category'] );
	}

	return $taxonomies;
}

add_filter( 'wpseo_primary_term_taxonomies', 'remove_primary_term_taxonomies', 11, 3 );
```