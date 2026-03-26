---
title: "Filters"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Indexables
    - Features
    - Repos
---


## 简介

在处理这些过滤器之前，请确保您通过阅读[功能规范](/features/indexables/functional-specification)以及可能还有[技术规范](/features/indexables/technical-specification)来理解什么是 Indexables 及其用途。

## 完全禁用索引项

默认情况下，根据[技术规范](/features/indexables/technical-specification)，Yoast SEO 会持续为网站上所有可优化的内容创建索引项。这包括文章、页面、自定义文章类型，以及分类法（分类、标签和自定义类型）、用户等的归档页面。

在某些情况下，用户可能希望禁用索引项的创建。当 `Yoast\WP\SEO\should_index_indexables` 过滤器设置为 false 时，插件将停止存储新创建的索引项。

:::note
在核心函数 [`wp_get_environment_type()`](https://developer.wordpress.org/reference/functions/wp_get_environment_type/) 返回值**不是** `production` 的网站上，我们默认已经禁用了索引项。
:::

:::caution 禁用索引项时的差异
当禁用索引项创建时，以下功能无法完全正常工作：

* <a href="/features/blocks/breadcrumbs/">面包屑导航块</a>
* <a href="/features/schema/pieces/breadcrumb">面包屑导航模式片段</a>
* 当内容的突出显示图片被调整大小时，<a href="/features/opengraph/functional-specification">og:image</a> 元标签将显示调整大小后的图片，而非原始尺寸图片。
* 当内容的突出显示图片被调整大小时，<a href="/features/twitter/functional-specification">twitter:image</a> 元标签将显示调整大小后的图片，而非原始尺寸图片。
:::

### 代码示例

```php
add_filter( 'Yoast\WP\SEO\should_index_indexables', '__return_false' );
```

## 排除内容

在 Indexables 的[技术规范](/features/indexables/technical-specification)中，我们概述了何时为文章类型和分类法创建 Indexables。如果出于某种原因，您将文章类型或分类法注册为 `public`，并且（对于文章类型）它们具有 `publish` 文章状态，但您*不*在网站前端使用这些 URL，您*应该*将它们排除在 Indexables 创建之外。

:::danger 警告
**如果您需要这些过滤器，您可能“做错了”。**

如果您将文章类型或分类法注册为 `public`，它们总是会出现在*不*应该出现的地方。只有当您打算在网站前端使用这些 URL 时，才应该将它们注册为 `public`。使用这些过滤器当然由您自己承担风险，并且*不*意味着搜索引擎无法看到这些对象中的内容。
:::

:::caution 设置为 noindex 的文章类型
**如果页面出现在前端，我们就需要 Indexables。**

因此，即使您已将文章类型设置为 `noindex`，如果 URL 将显示在前端，我们仍然需要为其创建 Indexable。
:::

### 排除文章类型 {#post_types}

`wpseo_indexable_excluded_post_types` 过滤器接收一个被排除的文章类型数组。请确保不要破坏该数组，只需向其添加内容。示例如下：

```php
add_filter( 'wpseo_indexable_excluded_post_types', 'do_not_create_indexables_for_orders' );

/**
 * 将订单从 Yoast SEO 可索引对象创建中排除。
 *
 * @link https://developer.yoast.com/features/indexables/indexables-filters/#post_types
 * 
 * @param string[] $excluded_post_types 按名称排列的被排除文章类型数组。
 * 
 * @return string[]
 */
function do_not_create_indexables_for_orders( $excluded_post_types ) {
    $excluded_post_types[] = 'orders';
    return $excluded_post_types;
}
```

### 排除分类法 {#taxonomies}

`wpseo_indexable_excluded_taxonomies` 过滤器接收一个已排除文章类型的数组。请确保不要破坏该数组，只需向其添加内容。示例如下：

```php
add_filter( 'wpseo_indexable_excluded_taxonomies', 'do_not_create_indexables_for_order_types' );

/**
 * 从 Yoast SEO 可索引对象创建中排除订单类型。
 *
 * @link https://developer.yoast.com/features/indexables/indexables-filters/#taxonomies
 * 
 * @param string[] $excluded_taxonomies 按名称排列的已排除分类法数组。
 * 
 * @return string[]
 */
function do_not_create_indexables_for_order_types( $excluded_taxonomies ) {
    $excluded_taxonomies[] = 'order_type';
    return $excluded_taxonomies;
}
```

## 强制包含内容

### 强制包含文章类型 {#included_post_types}

`wpseo_indexable_forced_included_post_types` 过滤器接收一个文章类型数组，系统将为这些类型创建索引。请确保不要破坏该数组，只需向其添加内容。示例如下：

```php
add_filter( 'wpseo_indexable_forced_included_post_types', 'force_create_indexables_for_non_public_post_type' );

/**
 * 强制为原本不符合 Yoast SEO 索引创建条件的文章类型创建索引。
 *
 * @link https://developer.yoast.com/features/indexables/indexables-filters/#included_post_types
 * 
 * @param string[] $included_post_types 按名称排列的已包含文章类型数组。
 * 
 * @return string[]
 */
function force_create_indexables_for_non_public_post_type( $included_post_types ) {
    $included_post_types[] = 'not_public_post_type';
    return $included_post_types;
}

```

### 强制创建并使用附件索引化对象 {#force_creating_and_using_attachment_indexables}

要使用索引化对象而非内容扫描来查找所有内容图片，请按如下所示启用此过滤器。请参阅[自定义 Yoast SEO 处理附件的方式](/customization/yoast-seo/customizing-attachment-parsing)以了解此功能可能适用的场景。

```php
add_filter( 'wpseo_force_creating_and_using_attachment_indexables', '__return_true' );
```