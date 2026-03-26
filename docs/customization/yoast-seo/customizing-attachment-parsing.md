---
title: "Customizing Attachment Parsing"
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


Yoast SEO 有多种方式处理文章中添加的附件。我们添加了多个过滤器以确保在各种场景下的性能。

## 场景

### Not so many attachments < 10.000

The normal implementation which implements a file scan with `WP_HTML_Tag_Processor` is most likely performant enough.

### A huge amount of attachments > 75.000 and a fast disk
The normal implementation which implements a file scan with `WP_HTML_Tag_Processor` is most likely performant enough.

### 附件数量巨大（超过 75,000 个）且磁盘速度慢

如果在保存文章时遇到性能问题，以下过滤器可以帮助加速处理过程。

```php
add_filter( 'wpseo_indexable_forced_included_post_types', function( $post_types ) {
    $post_types[] = 'attachment';
    return $post_types;
} );
add_action( 'init', function( $post_types ) {
    add_filter( 'wpseo_indexable_excluded_post_types', function( $post_types ) {
       $filtered_post_types = [];
       foreach ( $post_types as $post_type ) {
          if ( $post_type !== 'attachment' ) {
             $filtered_post_types[] = $post_type;
          }
       }
       return $filtered_post_types;
    } );
} );
add_filter('wpseo_force_creating_and_using_attachment_indexables', '__return_true'  );
```
使用这组过滤器组合，系统可以为附件创建索引对象，而无需同时启用媒体归档页面。首次启用这些过滤器时，您需要在工具页面运行 SEO 优化。
此场景中使用的过滤器在 [Indexables: Filters](/features/indexables/indexables-filters) 中有详细说明。

### 大量附件超过 75,000 个，内容扫描会降低性能。

如果新的内容扫描解决方案或可索引对象解决方案导致性能下降，也可以选择恢复使用正则表达式。

```php
add_filter('wpseo_force_skip_image_content_parsing', '__return_true'  );
```

### 当使用的图片或相册实现与 WordPress 的 HTML 结构不同时
某些相册和图片插件使用的 HTML 标记与 WordPress 不同。我们的内容扫描实现依赖于在图片标签的类中找到图片 ID。
有两个过滤器允许用户更改此设置，以支持不同类型的标记。以下示例基于 envira-gallery 插件。该插件使用 `id` 字段结合 `envira-gallery-image-<image_ID>` 格式，而不是类字段。
```php
add_filter( 'wpseo_image_attribute_containing_id', 'image_attribute_containing_id', 10 );

/**
 * 过滤器 'wpseo_image_attribute_containing_id' - 允许过滤用于从图片标签中提取图片 ID 的属性。
 *
 * 默认为 "class"，这是 WordPress 原生存储图片 ID 的位置，格式为 `wp-image-<ID>`。
 *
 * @api string 用于提取图片 ID 的属性。
 */
function image_attribute_containing_id() {
    return 'id';
}

add_filter( 'wpseo_extract_id_pattern', 'extract_id_pattern', 10 );

/**
 * 过滤器 'wpseo_extract_id_pattern' - 允许过滤用于从类/属性名称中提取图片 ID 的正则表达式模式。
 *
 * 默认为从核心的 `wp-image-<ID>` 原生格式（在图片类中）提取图片 ID 的模式。
 *
 * @api string 用于从类名中提取图片 ID 的正则表达式模式。如果应返回整个类/属性，则为空字符串。
 */
function extract_id_pattern() {
    return '/(?<!\S)envira-gallery-image-(\d+)(?!\S)/i';
}
```