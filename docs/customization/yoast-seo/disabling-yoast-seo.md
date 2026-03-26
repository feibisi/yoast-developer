---
title: "Disabling Yoast Seo"
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


自 Yoast SEO 14.0 起，我们已更改您与 Yoast SEO 输出交互的方式。
但在某些情况下，您可能希望禁用特定文章上的 Yoast SEO 输出。

请将以下代码添加到您主题的 `functions.php` 文件中。

```php
add_action( 'template_redirect', 'remove_wpseo' );

/**
 * 在前端为特定文章、页面或自定义文章类型移除 Yoast SEO 的输出。
 */
function remove_wpseo() {
	if ( is_single ( 1 ) ) {
		$front_end = YoastSEO()->classes->get( Yoast\WP\SEO\Integrations\Front_End_Integration::class );

		remove_action( 'wpseo_head', [ $front_end, 'present_head' ], -9999 );
	}
}
```

以上示例将禁用 ID 为 1 的文章的输出。您也可以调整代码以适应多种不同情况：

* 要禁用页面的输出，可以将 `is_single` 替换为 `is_page` 函数：

```php 
if ( is_page ( 1 ) ) { //... }
```

* 要禁用多篇文章的输出，可以向 `is_single` 函数传递一个数组。此方法也可用于之前的 `is_page` 方案：

```php 
if ( is_single( [ 123456, 234567, 345678 ] ) ) { //... }
```

* 要禁用自定义文章类型的输出，需要将 `is_single` 改为 `is_singular`，然后将自定义文章类型的别名传递给 `is_singular` 函数：

```php 
if ( is_singular( 'my_custom_posttype' ) ) { //... }
```