---
title: "Overview"
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


Schema Aggregator 会收集您 WordPress 站点上所有内容的结构化数据（schema.org 标记），并通过统一的 REST API 提供这些数据。
这使得搜索引擎和其他使用者无需爬取单个页面，即可轻松访问全面的结构化数据。

### 哪些内容会被聚合

默认情况下，模式聚合器会处理所有**公开文章类型**（文章、页面、自定义文章类型）。

您可以使用 `wpseo_schema_aggregator_post_types` 过滤器来自定义要包含的文章类型。

### 模式类型

聚合器处理 schema.org 类型，分为 10 个上下文类别：

- **网站**：站点信息
- **内容**：文章、博客文章、创意作品
- **商业**：产品、优惠、订单
- **实体**：组织、人员、地点
- **事件**：事件、日程安排
- **数据**：数据集、统计信息
- **医疗**：医疗状况、疗法
- **操作**：潜在操作（默认过滤）
- **枚举**：类型定义（默认过滤）
- **元数据**：Schema.org 元数据（默认过滤）
- **网站元数据**：站点结构元素、面包屑导航（默认过滤）

具体被过滤的模式类型在[源代码](https://github.com/Yoast/wordpress-seo/blob/trunk/src/schema-aggregator/infrastructure/elements-context-map/default-elements-context-map.php)中指定。

## 主要特性

### REST API 端点

聚合后的架构可通过以下 REST 端点访问：

```
GET /wp-json/yoast/v1/schema-aggregator/get-schema/{post_type}[/{page}]
GET /wp-json/yoast/v1/schema-aggregator/get-xml
```

### CLI commands

Automate schema aggregation with WP-CLI:

```bash
wp yoast aggregate_site_schema <post_type> [--page=<page>]
wp yoast clear_schema_aggregator_cache [<post_type>]
```

### Caching

The Schema Aggregator implements a dynamic caching strategy:

- **Dynamic TTL**: Cache duration adapts to site size
  - Small sites (< 100 posts): 24 hours
  - Medium sites (100-1000 posts): 12 hours
  - Large sites (> 1000 posts): 6 hours
- **Automatic invalidation**: Cache clears when posts are updated
- **Per-post-type caching**: Each post type has its own cache

### Schemamap（结构化数据的 XML 映射）

schemamap 提供了所有可用 schema 端点的 XML 索引：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<schemalist>
    <schema>
        <loc>https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/post</loc>
    </schema>
    <schema>
        <loc>https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/page</loc>
    </schema>
</schemalist>
```

schemamap 会自动在您网站的 `robots.txt` 中被引用：

```
Schemamap: https://example.com/wp-json/yoast/v1/schema-aggregator/get-xml
```

### 外部数据源支持

Schema Aggregator 可与以下系统无缝集成：

- **WooCommerce**：自动包含产品架构
- **Easy Digital Downloads**：自动包含下载架构
- **自定义数据源**：通过您自己的外部架构仓库进行扩展

## 开始使用

### 启用功能

在 Yoast SEO 中，模式聚合器默认是禁用的。如果你需要启用它，可以通过编程方式实现：

```php
WPSEO_Options::set( 'enable_schema_aggregation_endpoint', true );
```
或者，你也可以通过 WordPress 管理后台的 Yoast SEO 设置来启用它。

### JSON-L 格式的聚合模式示例

获取 `post` 文章类型的模式：

```bash
curl https://example.com/wp-json/yoast/v1/schema-aggregator/get-schema/post
```

响应：

```json lines
{
    "@context": "https://schema.org",
    "@type": "Article",
    "@id": "https://example.com/hello-world/#article",
    "headline": "Hello World",
    "datePublished": "2026-01-15T10:30:00+00:00",
    "author": {
        "@id": "https://example.com/#/schema/person/1"
    }
}
{
  "@context": "https://schema.org",

  "@type": "Article",
  "@id": "https://example.com/your-second-post/#article",
  "headline": "The second post",
  "datePublished": "2026-01-15T10:30:00+00:00",
  "author": {
    "@id": "https://example.com/#/schema/person/1"
  }
}
```

### 清除缓存

清除所有缓存的架构：

```bash
wp yoast clear_schema_aggregator_cache
```

清除特定文章类型的缓存：

```bash
wp yoast clear_schema_aggregator_cache post
```

## 了解更多

- [Schema.org 文档](https://schema.org/)
- [JSON-LD 规范](https://json-ld.org/)
- [Google 结构化数据指南](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data)