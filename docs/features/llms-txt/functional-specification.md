---
title: "Functional Specification"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Llms Txt
    - Features
    - Repos
---


本文档解释了 [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) 如何生成 llms.txt 文件。

## Yoast SEO 的 llms.txt 功能有什么作用？
- 启用 llms.txt 功能将在您网站的根目录下创建一个 llms.txt 文件
- 该文件将通过计划任务每周更新一次

## Yoast SEO 如何选择要包含在 llms.txt 文件中的内容？
**文章/页面/自定义文章类型**
- Yoast SEO 在 llms.txt 文件中包含最近更新的 5 篇文章/页面/自定义文章类型（文章仅在最近 12 个月内发布过才会被包含）
  - 此逻辑包括优先考虑基石内容
  - 对于页面，如果您在 llms.txt 设置中选择 `手动选择页面`，则可以覆盖此逻辑
- 如果在“搜索外观”设置中为特定自定义文章类型勾选了“在搜索结果中显示标签”框，则会包含自定义文章类型

**分类/标签/自定义分类法术语**
- Yoast SEO 包含关联内容最多的 5 个分类/标签
- 如果在“搜索外观”设置中为特定分类法勾选了“在搜索结果中显示术语”框，则会包含自定义分类法的术语

## 如何删除 llms.txt 文件？
- 您可能会看到 Yoast SEO 的警告，提示由于已存在 llms.txt 文件而无法自动生成
- 您需要从服务器中移除现有文件，Yoast SEO 才能自动生成它

## 如何创建自己的 llms.txt 文件？
- 禁用该功能
- 在您网站的根目录中创建一个 llms.txt 文件
- 遵循 https://llmstxt.org/ 以获取最佳实践

## 已知限制
- 当存在另一个 llms.txt 文件时，我们会确保不覆盖它。但如果某个插件动态提供 llms.txt 文件，我们的 txt 文件仍会被创建，并且由于它具有更高的优先级，它将在 example.com/llms.txt 这个 URL 中显示。
- 如果您在启用 llms.txt 功能的情况下停用 Yoast SEO，然后在某个时间点重新激活它，在激活后的接下来 5 分钟内，设置中的“查看 llms.txt 文件”按钮将指向一个 404 页面。
- 我们尚不支持包含特殊 Markdown 字符的 Markdown 代码块。目前这些字符将被转义。例如：
  - 网站标语包含以下字符串：“This is \`the *tagline\`”
  - llms.txt 将输出为：“This is \\\`the \\*tagline\\\`“

## 过滤器

llms.txt 功能提供了几个可用的过滤器。

### 更改文件路径根目录的获取方式，以放置 llms.txt 文件

* llms.txt 文件使用 `get_home_path()` 将 llms.txt 文件放置在 WordPress 安装的根目录。
* 如果该目录不可写，则使用 `$_SERVER['DOCUMENT_ROOT']`（如果可用）。
* 对于这两种方式都不起作用的情况，可以使用 `wpseo_llmstxt_filesystem_path` 过滤器：

```php
add_filter( 'wpseo_llmstxt_filesystem_path', 'custom_llmstxt_file_path' );

/**
 * 使用 WP_CONTENT_DIR 常量来获取服务器的 Web 根目录，而不是默认方式。
 *
 * @return string 修改后的 Web 根目录。
 */
function custom_llmstxt_file_path() {
    return dirname( WP_CONTENT_DIR );
}
```

### 编辑 llms.txt 文件中添加的 BOM 前缀

* 出于编码目的，我们在 llms.txt 文件前添加了 UTF-8 的字节顺序标记 (BOM) (`"\xEF\xBB\xBF"`)。
* 如需将 BOM 更改为其他编码或完全移除，可以使用 `wpseo_llmstxt_encoding_prefix` 过滤器：

```php
add_filter( 'wpseo_llmstxt_encoding_prefix', 'custom_llmstxt_encoding_prefix' );

/**
 * 从 llms.txt 文件中移除 UTF-8 BOM 前缀并添加 UTF-16 BOM 前缀。
 *
 * @return string 编辑后的前缀。
 */
function custom_llmstxt_encoding_prefix() {
    return "\xFF\xFE";
}
```
* 如需完全移除编码前缀，可以使用相同的过滤器：
```php
add_filter( 'wpseo_llmstxt_encoding_prefix', 'custom_llmstxt_encoding_prefix' );

/**
 * 从 llms.txt 文件中移除 UTF-8 BOM 前缀。
 *
 * @return string 被移除的前缀。
 */
function custom_llmstxt_encoding_prefix() {
    return "";
}
```

### Edit the description in the posts lists

* By default, for posts that have a custom excerpt, we use that to add a description for each post:
```
## Posts
- [Post with excerpt](http://example.com/post-with-excerpt/): This is a custom excerpt.
- [Post without excerpt](http://example.com/post-without-excerpt/)
```
* For changing the default behavior, you can use the `wpseo_llmstxt_link_description` filter:

```php
<?php

/**
 * Changes the link description in the llms.txt file.
 *
 * @param string $link_description The description of the link.
 * @param string $post_id          The ID of the post that is being added as a link.
 * @param string $post_type        The post type of the post that is being added as a link.
 *
 * @return string The changed link description in the llms.txt file.
 */
function set_custom_llmstxt_link_description( $link_description, $post_id, $post_type ) {
    // Remove excerpt for specific post types.
    if ( in_array( $post_type, [ 'page' ], true ) ) {
      return '';
    }

    // Keep excerpt for other post types but make it more descriptive.
    return 'Custom description for post ID ' . $post_id . ', ' . $link_description;
}

add_filter( 'wpseo_llmstxt_link_description', 'set_custom_llmstxt_link_description', 10, 3 );
```
* The above snippet will remove descriptions from pages (if there are any) and adjust accordingly the descriptions for posts:
```
## Pages
- [Page with excerpt](http://example.com/page-with-excerpt/)

## Posts
- [Post with excerpt](http://example.com/post-1): Custom description for post ID 1, this is the custom excerpt.
```