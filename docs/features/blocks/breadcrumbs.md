---
title: "Breadcrumbs"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Blocks
    - Features
    - Repos
---


This documentation provides information about the breadcrumb block
that [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) provides.

:::caution Breadcrumbs in local environments
In local environments, or in websites that <a href="/features/indexables/indexables-filters/#disabling-indexables-completely">disable the indexable creation in general</a>, breadcrumbs are not working properly.

Specifically, in those setups the hierarchy of a breadcrumb path might be missing intermediate nodes like the category a post belongs to, or a parent page of a subpage. 
:::

## 过滤器

### 修改输出内容 {#breadcrumb_output}

`wpseo_breadcrumb_output` 过滤器用于处理面包屑导航的输出。请确保不要清空输出内容，仅进行替换或添加数据。示例如下：

```php
add_filter( 'wpseo_breadcrumb_output', 'override_home_icon' );

/**
 * 将首页文本替换为图标。
 *
 * @param  string $original_breadcrumbs 完整的面包屑导航 HTML。
 * @return string
 */
function override_home_icon( string $original_breadcrumbs ): string {
    $new_home = \str_replace( '>Home<', ' aria-label="Home"><i class="fa fa-homeicon" aria-hidden="true"></i><', $original_breadcrumbs );

    return $new_home;
}
```

### 修改单个面包屑链接 {#breadcrumb_single_link}

`wpseo_breadcrumb_single_link` 过滤器允许修改由 Yoast SEO 面包屑类输出的每个链接。它接收生成的链接和源面包屑信息。示例如下：

```php
add_filter( 'wpseo_breadcrumb_single_link', 'remove_current_page' );

/**
 * 从面包屑中移除当前页面。
 *
 * @param string $link       输出的字符串。
 * @param array  $breadcrumb 面包屑链接数组。
 */
function remove_current_page( $link ): string {
	if ( strpos( $link, 'breadcrumb_last' ) !== false ) {
		$link = '';
	}

	return $link;
}
```

### 更改 HTML ID {#breadcrumb_output_id}

`wpseo_breadcrumb_output_id` 允许更改 Yoast SEO 面包屑导航包装元素的 HTML ID。仅当当前不存在 ID 时才会调用此过滤器。示例：

```php
add_filter( 'wpseo_breadcrumb_output_id', 'add_breadcrumb_id' );

/**
 * 允许更改 Yoast SEO 面包屑导航包装元素的 HTML ID。
 *
 * @param string $id 要添加到包装元素的 ID。
 */
function add_breadcrumb_id( $id ): string {
  return 'my-id';
}
```

### 更改 HTML 类 {#breadcrumb_output_class}

`wpseo_breadcrumb_output_class` 过滤器允许更改 Yoast SEO 面包屑导航包装元素的 HTML 类。仅当当前不存在类时才会调用它。示例：

```php
add_filter( 'wpseo_breadcrumb_output_class', 'add_breadcrumb_class' );

/**
 * 允许更改 Yoast SEO 面包屑导航包装元素的 HTML 类。
 *
 * @param string $class 要添加到包装元素的类。
 */
function add_breadcrumb_class( $class ): string {
  return 'my-class my-other-class';
}
```

### 更改输出包装器元素 {#breadcrumb_output_wrapper}

`wpseo_breadcrumb_output_wrapper` 允许更改 Yoast SEO 面包屑导航的包装器元素。它默认以 `span` 值被调用。输出应为一个有效的 HTML 元素。示例：

```php
add_filter( 'wpseo_breadcrumb_output_wrapper', 'replace_breadcrumb_wrapper' );

/**
 * 替换面包屑导航的包装器
 *
 * @param  string $output 通常输出的内容。
 * @return string
 */
function replace_breadcrumb_wrapper( string $output ): string {
    $output = 'ol';

    return $output;
}
```

### 更改链接包装器 {#breadcrumb_link_wrapper}

`wpseo_breadcrumb_single_link_wrapper` 过滤器允许更改 Yoast SEO 的链接包装器元素。它默认以 `span` 值被调用。输出应为一个有效的 HTML 元素。示例如下：

```php
add_filter( 'wpseo_breadcrumb_single_link_wrapper', 'replace_link_wrapper' );

/**
 * 替换链接周围的包装器。
 *
 * @param  string $output 通常输出的内容。
 * @return string
 */
function replace_link_wrapper( string $output ): string {
    $output = 'li';

    return $output;
}
```

### 更改分隔符 {#breadcrumb_separator}

`wpseo_breadcrumb_separator` 过滤器允许更改 Yoast SEO 的分隔符。它仅在调用时带有一个从 Yoast SEO 设置中获取的默认值。输出应为有效的 HTML。以下是一个完全移除分隔符的示例：

```php
add_filter( 'wpseo_breadcrumb_separator', 'replace_breadcrumb_separator' );

/**
 * 替换面包屑导航的分隔符。
 *
 * @param  string $output 通常输出的内容。
 * @return string
 */
function replace_breadcrumb_separator( string $output ): string {
    $output = '';

    return $output;
}
```

## 架构输出

Yoast SEO 面包屑区块会输出一个面包屑架构片段，相关文档位于
[架构片段 - 面包屑](/features/schema/pieces/breadcrumb)