---
title: "Disabling Autoloading Redirect Options"
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


自 Yoast SEO Premium 17.7 版本起，用于计算重定向目标的两个选项（`wpseo-premium-redirects-export-plain` 和 `wpseo-premium-redirects-export-regex`）已设置为自动加载，以减少两次查询。
如果重定向数量变得庞大，您可能希望禁用这些选项的自动加载，例如当您使用对象缓存且自动加载的选项超出最大对象大小限制时。
仅在数据库中将这两个选项的 `autoload` 值改为 `no` 是不够的，因为 Yoast SEO Premium 会强制这些选项再次自动加载。

在 Yoast SEO Premium 20.13 版本中，我们引入了 `Yoast\WP\SEO\redirects_options_autoload` 来帮助您更改此行为。您可以将以下代码添加到主题的 `functions.php` 文件中，以防止 Yoast SEO Premium 强制自动加载重定向选项：

```php
add_filter( 'Yoast\WP\SEO\redirects_options_autoload', '__return_false');
```

请注意，仅此操作不足以停止自动加载：您仍需在数据库中将这两个选项的 `autoload` 值改为 `no`。
您也可以在重定向管理器中添加/删除重定向，或编辑一个重定向（只要它确实被更改且未按原样保存），但如果这是一个 _普通_ 重定向，只有 `wpseo-premium-redirects-export-plain` 的自动加载会改变，对于 _正则表达式_ 重定向也是如此。

除了 `autoload` 的值外，该过滤器还接受两个参数：重定向的类型（`plain` 或 `regex`）以及当前定义的重定向数组。
因此，例如，您可以确保 `plain` 重定向的选项不会被自动加载，同时保持 `regex` 重定向的默认行为。在主题的 `functions.php` 文件中添加如下代码片段：

```php
function yoast_set_autoload_no_plain( $autoload, $type, $redirects ) {
	if ( $type === 'plain' ) {
		return false;
	}
	return $autoload;
}

add_filter( 'Yoast\WP\SEO\redirects_options_autoload', 'yoast_set_autoload_no_plain', 3, 10);
```