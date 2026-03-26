---
title: "Enhancing Search Results"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Local Seo
    - Customization
    - Repos
---


Local SEO 插件的一项隐藏功能是使用您的位置内容增强网站的内部搜索结果。在极少数情况下，增强的搜索结果可能会与其他插件或主题功能产生冲突。因此，Local SEO 7.1 版本引入了开发者过滤器来禁用这些增强功能。

这些过滤器应放置在您主题的 `function.php` 文件中。如果您不确定该文件的位置，请咨询您的主题开发者。

我们建议在添加过滤器或编辑主题文件之前，先对您的网站进行备份。

过滤器接受布尔值 `true` 或 `false`。

* `yoast_local_seo_enhanced_search_enabled`
* `yoast_local_seo_enhanced_search_result_enabled`

要禁用对位置元字段的搜索，请使用：

```php
add_filter( 'yoast_local_seo_enhanced_search_enabled', '__return_false' );
```

要同时禁用对已找到位置的标记，请使用：

```php
add_filter( 'yoast_local_seo_enhanced_search_result_enabled', '__return_false' );
```