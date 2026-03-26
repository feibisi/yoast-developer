---
title: "Changing Location Post Type"
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


自 13.2 版本起，您或您的开发者可以使用 `wpseo_local_post_type` 过滤器来更改默认的地点文章类型。此过滤器接受一个参数：`$post_type`（必需）。
当您为地点定义了自定义文章类型时，此过滤器尤其有用。

:::caution

如果更改文章类型，请确保[重新索引 Yoast SEO 的可索引内容](docs/features/wp-cli/reindex-indexables)。

:::

在您的自定义插件或主题的 `functions.php` 文件中，添加以下代码行，并将默认值更改为您所需的文章类型。

```php
/** 
 * 更改用于地点的 Yoast SEO: Local 文章类型。
 *
 * @param string $post_type 我们要替换的文章类型。
 *
 * @return string 我们的地点文章类型。
 */
function my_custom_locations_post_type( $post_type ) {
    return 'my_custom_locations_post_type';
}

add_filter( 'wpseo_local_post_type', 'my_custom_locations_post_type', 10, 1 );
```