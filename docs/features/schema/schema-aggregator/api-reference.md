---
title: "Get first page of posts"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Schema Aggregator
    - Schema
    - Features
---


## REST API 端点

### Get Schema for Post Type (JSON-L)

Retrieve aggregated schema pieces for a specific post type in JSON-L format.

**Endpoint:**
```
GET /wp-json/yoast/v1/schema-aggregator/get-schema/{post_type}[/{page}]
```

**Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `post_type` | string | Yes | - | The post type to aggregate (e.g., `post`, `page`, `product`) |
| `page` | integer | No | 1 | Page number for pagination |

**Pagination:**

- Standard post types: 1000 items per page
- Big schema post types (e.g., `product`): 100 items per page

You can customize pagination using filters:
- `wpseo_schema_aggregator_per_page`: Default items per page (1000)
- `wpseo_schema_aggregator_per_page_big`: Items per page for big schema post types (100)
- `wpseo_schema_aggregator_big_schema_post_types`: Define which post types use big pagination (default: `['product']`)

**Response Format:**

```json lines
{
   "@context": "https://schema.org",
   "@type": "Article",
   "@id": "https://example.com/hello-world/#article",
   "headline": "Hello World",
   "description": "A brief description of the article",
   "articleBody": "The full article content...",
   "keywords": ["hello", "world"],
   "datePublished": "2024-01-15T10:30:00+00:00",
   "dateModified": "2024-01-20T14:45:00+00:00",
   "author": {
       "@id": "https://example.com/#/schema/person/1"
   },
   "publisher": {
       "@id": "https://example.com/#organization"
   }
    
}
```

**Cache Headers:**

Responses include cache control headers with a 5-minute (300 seconds) cache duration:

```
Cache-Control: max-age=300
```

**Example Request (cURL):**

```bash
# Get first page of posts
curl https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/post

# Get second page
curl https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/post/2
```

---

### Get XML Schemamap

Retrieve an XML sitemap of all available schema endpoints.

**Endpoint:**
```
GET /wp-json/yoast/v1/schema-aggregator/get-xml
```

**Parameters:**

None.

**Response Format:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <url contentType="structuredData/schema.org">
      <loc>https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/page</loc>
      <lastmod>2026-01-01T14:03:56Z</lastmod>
      <changefreq>daily</changefreq>
      <priority>0.8</priority>
   </url>
   <url contentType="structuredData/schema.org">
      <loc>https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/post</loc>
      <lastmod>2026-01-01T14:03:56Z</lastmod>
      <changefreq>daily</changefreq>
      <priority>0.8</priority>
   </url>
</urlset>
```


**Cache Headers:**

Responses include cache control headers with a 5-minute (300 seconds) cache duration:

```
Cache-Control: max-age=300
```

**robots.txt Integration:**

The schemamap is automatically referenced in your site's `robots.txt`:

```
Sitemap: https://example.com/wp-json/yoast/v1/schema-aggregator/get-xml
```

**Example Request:**

```bash
curl https://example.com/wp-json/yoast/v1/schema-aggregator/get-xml
```

**Customizing Post Types:**

By default, the schemamap includes all indexable post types. You can customize this using the `wpseo_schema_aggregator_post_types` filter:

```php
add_filter( 'wpseo_schema_aggregator_post_types', 'customize_schema_post_types' );

/**
 * Customize which post types appear in schema aggregator.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/api-reference/#get-schema-map-xml
 *
 * @param array $post_types Array of post type names.
 *
 * @return array Modified array of post type names.
 */
function customize_schema_post_types( $post_types ) {
    // Only include posts and pages.
    return [ 'post', 'page' ];
}
```

---

## CLI 命令

### 聚合站点架构

通过 WP-CLI 检索文章类型的聚合架构。

**命令：**
```bash
wp yoast aggregate_site_schema <post_type> [--page=<page>]
```

**参数：**

| 参数 | 类型 | 必需 | 默认值 | 描述 |
|----------|------|----------|---------|-------------|
| `<post_type>` | 字符串 | 是 | - | 要聚合的文章类型 |
| `--page` | 整数 | 否 | 1 | 分页的页码 |

**输出格式：**

JSON-LD 输出将打印到标准输出。

**使用示例：**

```bash
# 获取文章的第一页
wp yoast aggregate_site_schema post

# 获取产品的第二页
wp yoast aggregate_site_schema product --page=2
```

### 清除模式缓存

使所有或特定文章类型的缓存模式数据失效。

**命令：**
```bash
wp yoast clear_schema_aggregator_cache [<post_type>]
```

**参数：**

| 参数 | 类型 | 必需 | 默认值 | 描述 |
|----------|------|----------|---------|-------------|
| `<post_type>` | 字符串 | 否 | - | 可选，指定要清除缓存的文章类型 |

**使用示例：**

```bash
# 清除所有模式缓存
wp yoast clear_schema_aggregator_cache

# 仅清除文章缓存
wp yoast clear_schema_aggregator_cache post

# 仅清除产品缓存
wp yoast clear_schema_aggregator_cache product
```

## 筛选与增强

### 被过滤的内容

默认情况下，Schema 聚合器会移除以下类别的模式片段：

- **操作**：潜在的操作（例如 `SearchAction`、`ReadAction`）
- **枚举**：类型定义（例如 `ItemAvailability`、`OfferItemCondition`）
- **元数据**：Schema.org 元数据（例如 `DataType`、`Class`）
- **网站结构**：网站结构元素（例如，当非主要实体时的 `WebSite`、`WebPage`）

此筛选可减少干扰，专注于有意义的内容实体。

### What Gets Enhanced

The Schema Aggregator enhances certain schema types with additional data:

#### Article Enhancement

Articles (and subtypes like `BlogPosting`, `NewsArticle`) are enhanced with:

1. **articleBody**: Full post content (configurable max length, default: 500 characters)
2. **description**: Post excerpt (configurable max length, default: no limit)
3. **keywords**: Post tags as keyword array (optionally includes categories)

**Configuration:**

```php
// Adjust article body length.
add_filter( 'wpseo_article_enhance_config_article_body_max_length', function() {
    return 2000; // Increase to 2,000 characters.
} );

// Adjust excerpt length.
add_filter( 'wpseo_article_enhance_config_excerpt_max_length', function() {
    return 500; // Limit to 500 characters.
} );

// Include categories as keywords.
add_filter( 'wpseo_article_enhance_config_categories_as_keywords', '__return_true' );

// Disable article body enhancement.
add_filter( 'wpseo_article_enhance_article_body', '__return_false' );

// Disable excerpt enhancement.
add_filter( 'wpseo_article_enhance_use_excerpt', '__return_false' );

// Disable keywords enhancement.
add_filter( 'wpseo_article_enhance_keywords', '__return_false' );
```

#### 人员信息增强

人员模式 (`@type: Person`) 已通过以下方式增强：

1. **职位头衔**：从 WordPress 用户元数据中获取 (`job_title` 字段)

**配置方法：**

```php
// 禁用职位头衔增强功能。
add_filter( 'wpseo_person_enhance_person_job_title', '__return_false' );
```

### 自定义过滤

Schema Aggregator 采用**策略模式**进行过滤。默认策略使用元素到上下文的映射对模式片段进行分类，并过滤掉整个类别（Actions、Enumerations、Meta、Website）。您可以在三个级别自定义过滤：

#### 替换整个筛选策略

实现 `Filtering_Strategy_Interface` 并通过 `wpseo_schema_aggregator_filtering_strategy` 钩子注册：

```php
add_filter( 'wpseo_schema_aggregator_filtering_strategy', function( $default_filter ) {
    return new My_Custom_Filter();
} );
```

你的自定义类必须实现 `filter` 方法，该方法接收一个 `Schema_Piece_Collection` 并返回一个筛选后的 `Schema_Piece_Collection`。

#### 修改元素到上下文的映射

默认过滤策略将模式类型映射到类别（例如 `website`、`actions`、`meta`）。您可以使用两个过滤器修改此映射：

```php
// 替换整个映射。
add_filter( 'wpseo_schema_aggregator_elements_context_map', function( $map ) {
    $map['my_category'] = [ 'WebPage', 'SomeOtherType' ];
    return $map;
} );

// 修改单个类别（例如，将 Table 添加到 website 类别）。
add_filter( 'wpseo_schema_aggregator_elements_context_map_website', function( $types ) {
    $types[] = 'Table';
    return $types;
} );
```

#### 条件与属性级过滤

默认策略还支持通过节点级过滤器实现细粒度控制：

- **节点过滤器** (`{Type}_Schema_Node_Filter`)：有条件地决定是否应过滤给定类型的模式片段。例如，`WebPage` 仅在表示 `Article` 时被过滤；否则保留。
- **属性过滤器** (`{Type}_Schema_Node_Property_Filter`)：从模式片段中移除特定属性，而非过滤整个片段。

## See Also

- [Overview](docs/features/schema/schema-aggregator/overview): Introduction to Schema Aggregator
- [Filters](docs/features/schema/schema-aggregator/filters): Complete filter reference with examples
- [Schema.org documentation](https://schema.org/)
- [JSON-LD specification](https://json-ld.org/)