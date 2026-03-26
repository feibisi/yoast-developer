---
title: "Surfaces Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Apis
    - Customization
    - Repos
---


在 Yoast SEO 14.0 中，我们引入了一种将 Yoast SEO 集成到您代码中的正式方法。我们添加了一个称为*表面*的功能，名为 `YoastSEO()`。这个表面让您能轻松访问 Yoast SEO 提供的众多功能。

## 轻松获取当前页面的 SEO 数据
当前页面的所有 SEO 数据都可以通过我们的 surface 轻松访问。

例如，以下代码将页面标题作为变量提供：

```php
$title = YoastSEO()->meta->for_current_page()->title;
```

此代码直接输出页面的元描述：

```php
echo YoastSEO()->meta->for_current_page()->description;
```

此代码输出当前页面的预计阅读时间。

```php
echo (string) YoastSEO()->meta->for_current_page()->estimated_reading_time_minutes, " minutes";
```

`current_page` surface 暴露了我们拥有的关于当前页面的所有数据，它们的工作方式都相同；这是一个很长的列表：

| Variable | Type | Description |
| --- | --- | --- |
| canonical | string | The canonical URL for the current page. |
| description | string | The meta description for the current page, if set. |
| title | string | The SEO title for the current page. |
| id | string | The requested object ID. |
| site_name | string | The site name from the Yoast SEO settings. |
| wordpress_site_name | string | The site name from the WordPress settings. |
| site_url | string | The main URL for the site. |
| company_name | string | The company name from the Knowledge Graph settings. |
| company_logo_id | int | The attachment ID for the company logo. |
| site_user_id | int | If the site represents a 'person', this is the ID of the accompanying user profile. |
| site_represents | string | Whether the site represents a 'person' or a 'company'. |
| site_represents_reference | array|false | The schema reference ID for what this site represents. |
| breadcrumbs_enabled | bool | Whether breadcrumbs are enabled or not. |
| schema_page_type | string | The Schema page type. |
| main_schema_id | string | Schema ID that points to the main Schema thing on the page, usually the webpage or article Schema piece. |
| page_type | string | The Schema page type. |
| meta_description | string | The meta description for the current page, if set. |
| robots | array | An array of the robots values set for the current page. |
| googlebot | array | The meta robots values we specifically output for Googlebot on this page. |
| rel_next | string | The next page in the series, if any. |
| rel_prev | string | The previous page in the series, if any. |
| open_graph_enabled | bool | Whether OpenGraph is enabled on this site. |
| open_graph_publisher | string | The OpenGraph publisher reference. |
| open_graph_type | string | The og:type. |
| open_graph_title | string | The og:title. |
| open_graph_description | string | The og:description. |
| open_graph_images | array | The array of images we have for this page. |
| open_graph_url | string | The og:url. |
| open_graph_site_name | string | The og:site_name. |
| open_graph_article_publisher | string | The article:publisher value. |
| open_graph_article_author | string | The article:author value. |
| open_graph_article_published_time | string | The article:published_time value. |
| open_graph_article_modified_time | string | The article:modified_time value. |
| open_graph_locale | string | The og:locale for the current page. |
| schema | array | The entire Schema array for the current page. |
| twitter_card | string | The X card type for the current page. |
| twitter_title | string | The X card title for the current page. |
| twitter_description | string | The X card description for the current page. |
| twitter_image | string | The X card image for the current page. |
| twitter_creator | string | The X card author for the current page. |
| twitter_site | string | The X card site reference for the current page. |
| source | array | The source object for most of this page data. |
| breadcrumbs | array | The breadcrumbs array for the current page. |
| estimated_reading_time_minutes | int | The estimated reading time in minutes for the content. | 
| post_author | string | The name of the post author |

无论你需要的是*OpenGraph描述*还是*robots数组*，这里都已涵盖。请习惯打开你最喜欢的IDE，输入`YoastSEO()->meta->for_current_page()->`，然后查看类型提示以获取你所需的精确数据片段。

## 已弃用的属性
| 变量名 | 类型 | 描述 | 弃用版本 |
| --- | --- | --- | --- |
| open_graph_fb_app_id | string | Facebook 应用 ID。 | Yoast SEO 15.5 (2020年12月) |

## 对于其他页面
获取任何页面的数据，其工作方式与获取当前页面数据几乎完全相同。您只需要提供一个 ID 或一个 URL。

例如，获取 ID 为 *2* 的文章的规范 URL 值。

```php
YoastSEO()->meta->for_post( 2 )->canonical;
```

例如，获取 URL 为 `https://www.example.com/example-page/` 的页面的标题：
```php
YoastSEO()->meta->for_url( 'https://www.example.com/example-page/' )->title;
```

注意：如果某个 URL 在我们的 Indexables 表中不存在，则此方法将返回 `false`。

## 访问我们的辅助函数

有时您需要的不仅仅是页面的原始 SEO 数据。例如，您可能需要知道当前文章类型是否应该被索引。我们的 `post_type` 辅助函数可以解决这个问题：

```php
YoastSEO()->helpers->post_type->is_indexable( get_post_type() );
```

这将返回一个简单的 `布尔值`。

如果您想要获取可索引的文章类型列表，应该使用：

```php
$public_post_types = YoastSEO()->helpers->post_type->get_public_post_types();
```

同样的方法也适用于分类法：

```php
YoastSEO()->helpers->taxonomy->is_indexable( 'category' );
```

我们提供了相当多的这类辅助函数，并非所有函数对您都同样有用。但请在您的 IDE 中查看我们提供了哪些函数，这同样是一个相当长的列表！