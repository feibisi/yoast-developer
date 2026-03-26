---
title: "Filters"
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


Schema Aggregator 提供 WordPress 过滤器，允许您自定义其行为，从缓存配置到内容增强。本文档为每个过滤器提供完整参考和实用示例。

## 快速参考

| Filter | Category            | Purpose                                                          |
|--------|---------------------|------------------------------------------------------------------|
| [`wpseo_schema_aggregator_post_types`](#post-type-configuration) | Post Types          | Control which post types to aggregate                            |
| [`wpseo_schema_aggregator_per_page`](#pagination-configuration) | Pagination          | Set default items per page (1000)                                |
| [`wpseo_schema_aggregator_per_page_big`](#pagination-configuration) | Pagination          | Set items per page for big schema post types (100)               |
| [`wpseo_schema_aggregator_big_schema_post_types`](#pagination-configuration) | Pagination          | Define which post types have large schema                        |
| [`wpseo_schema_aggregator_cache_enabled`](#cache-configuration) | Cache               | Enable/disable caching                                           |
| [`wpseo_schema_aggregator_cache_ttl`](#cache-configuration) | Cache               | Set cache duration (dynamic by default)                          |
| [`wpseo_schema_aggregator_schemamap_changefreq`](#schemamap-configuration) | schemamap           | Set XML schemamap update frequency                               |
| [`wpseo_schema_aggregator_schemamap_priority`](#schemamap-configuration) | schemamap          | Set XML schemamap priority                                         |
| [`wpseo_schema_aggregator_filtering_strategy`](#filtering-strategy) | Filtering           | Implement custom filtering logic                                 |
| [`wpseo_schema_aggregator_elements_context_map`](#elements-context-map) | Context Map         | Override complete context map                                    |
| [`wpseo_schema_aggregator_elements_context_map_{context}`](#wpseo_schema_aggregator_elements_context_map_context) | Context Map         | Customize elements for specific context                          |
| [`wpseo_article_enhance_config_{key}`](#article-enhancement-configuration) | Article Enhancement | Configure article enhancement settings                           |
| [`wpseo_article_enhance_{enhancement}`](#article-enhancement-toggles) | Article Enhancement | Toggle article enhancements (article_body, use_excerpt, keywords) |
| [`wpseo_article_enhance_body_when_excerpt_exists`](#article-body-with-excerpt) | Article Enhancement | Include body when excerpt exists                                 |
| [`wpseo_article_enhance_article_body_fallback`](#article-body-fallback) | Article Enhancement | Include body when no excerpt                                     |
| [`wpseo_person_enhance_config_{key}`](#person-enhancement-configuration) | Person Enhancement  | Configure person enhancement settings                            |
| [`wpseo_person_enhance_{enhancement}`](#person-enhancement-toggles) | Person Enhancement  | Toggle person enhancements (person_job_title)                    |
| [`wpseo_disable_robots_schemamap`](#robotstxt-integration) | Robots.txt          | Disable schemamap in robots.txt                                  |

## Post Type Configuration

### wpseo_schema_aggregator_post_types

Control which post types are included in the schema aggregation.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$post_types` | array | Array of post type names to aggregate |

**Default Value:** All indexable post types

**Example: Include Only Specific Post Types**

```php
add_filter( 'wpseo_schema_aggregator_post_types', 'limit_aggregated_post_types' );

/**
 * Limit schema aggregation to posts and pages only.
 *
 * This is useful when you have custom post types that don't need
 * structured data or when you want to improve performance.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_post_types
 *
 * @param array $post_types Array of post type names.
 *
 * @return array Modified array of post type names.
 */
function limit_aggregated_post_types( $post_types ) {
	// Only aggregate posts and pages.
	return [ 'post', 'page' ];
}
```

**Example: Exclude Specific Post Types**

```php
add_filter( 'wpseo_schema_aggregator_post_types', 'exclude_custom_post_types' );

/**
 * Exclude internal post types from schema aggregation.
 *
 * Remove post types that are used for internal purposes and
 * should not appear in structured data output.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_post_types
 *
 * @param array $post_types Array of post type names.
 *
 * @return array Modified array of post type names.
 */
function exclude_custom_post_types( $post_types ) {
	// Remove internal post types.
	$excluded = [ 'acf-field', 'acf-field-group', 'custom_css' ];

	return array_diff( $post_types, $excluded );
}
```

## 分页配置

Schema Aggregator 根据架构片段的大小使用不同的分页限制。某些文章类型（如 WooCommerce 产品）会生成大型架构对象，需要较小的页面尺寸。

### wpseo_schema_aggregator_per_page

Set the default number of items per page for schema aggregation.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$per_page` | int | Number of items per page |

**Default Value:** 1000

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_per_page', 'custom_default_per_page' );

/**
 * Reduce default items per page for better performance.
 *
 * Lower pagination can help with memory limits on shared hosting
 * or when dealing with post types that have moderate schema sizes.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_per_page
 *
 * @param int $per_page Number of items per page.
 *
 * @return int Modified number of items per page.
 */
function custom_default_per_page( $per_page ) {
	return 500; // Reduce from 1000 to 500.
}
```

### wpseo_schema_aggregator_per_page_big

Set the number of items per page for post types with large schema pieces.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$per_page_big` | int | Number of items per page for big schema post types |

**Default Value:** 100

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_per_page_big', 'custom_big_per_page' );

/**
 * Adjust pagination for post types with large schema.
 *
 * Increase or decrease based on your server's memory limits
 * and the actual size of your schema pieces.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_per_page_big
 *
 * @param int $per_page_big Number of items per page for big schema.
 *
 * @return int Modified number of items per page.
 */
function custom_big_per_page( $per_page_big ) {
	return 50; // Reduce from 100 to 50 for very large schemas.
}
```

### wpseo_schema_aggregator_big_schema_post_types

Define which post types should use the "big schema" pagination limit.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$big_schema_post_types` | array | Array of post type names with large schema pieces |

**Default Value:** `[ 'product' ]`

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_big_schema_post_types', 'add_big_schema_post_types' );

/**
 * Mark custom post types as having large schema pieces.
 *
 * Post types with extensive custom fields, galleries, or complex
 * relationships should use the lower pagination limit.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_big_schema_post_types
 *
 * @param array $big_schema_post_types Array of post type names.
 *
 * @return array Modified array of post type names.
 */
function add_big_schema_post_types( $big_schema_post_types ) {
	// Add custom post types with large schemas.
	$big_schema_post_types[] = 'property'; // Real estate listings.
	$big_schema_post_types[] = 'event'; // Events with extensive details.

	return $big_schema_post_types;
}
```

## Cache Configuration

The Schema Aggregator implements intelligent caching to balance performance and freshness. Cache automatically clears when posts are updated.

### wpseo_schema_aggregator_cache_enabled

Enable or disable the cache system entirely.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$enabled` | bool | Whether caching is enabled |

**Default Value:** `true`

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_cache_enabled', 'disable_schema_cache_in_dev' );

/**
 * Disable caching in development environment.
 *
 * This ensures you always see fresh data during development
 * without needing to manually clear the cache.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_cache_enabled
 *
 * @param bool $enabled Whether caching is enabled.
 *
 * @return bool Modified cache enabled status.
 */
function disable_schema_cache_in_dev( $enabled ) {
	// Disable cache in local/staging environments.
	if ( defined( 'WP_ENV' ) && in_array( WP_ENV, [ 'development', 'staging' ], true ) ) {
		return false;
	}

	return $enabled;
}
```

### wpseo_schema_aggregator_cache_ttl

Set the cache time-to-live (TTL) in seconds.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$ttl` | int | Cache duration in seconds |

**Default Value:** Dynamic based on serialized payload size:
- Small payloads (< 100 KB): 1800 seconds (30 minutes)
- Normal payloads: 3600 seconds (1 hour)
- Large payloads (> 1 MB): 21600 seconds (6 hours)

**Example: Fixed TTL**

```php
add_filter( 'wpseo_schema_aggregator_cache_ttl', 'custom_cache_ttl' );

/**
 * Set a fixed cache duration.
 *
 * Use this when you want consistent cache behavior regardless
 * of payload size.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_cache_ttl
 *
 * @param int $ttl Cache duration in seconds.
 *
 * @return int Modified cache duration.
 */
function custom_cache_ttl( $ttl ) {
	// Cache for 2 hours regardless of payload size.
	return 7200;
}
```

## Schemamap Configuration

The schemamap is an XML sitemap that lists all available schema endpoints. It's automatically referenced in your site's `robots.txt`.

### wpseo_schema_aggregator_schemamap_changefreq

Set the change frequency for schemamap entries.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$changefreq` | string | Change frequency (always, hourly, daily, weekly, monthly, yearly, never) |

**Default Value:** `'daily'`

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_schemamap_changefreq', 'custom_schemamap_changefreq' );

/**
 * Set schemamap change frequency to weekly.
 *
 * Use this to indicate how often search engines should check
 * for updated schema data.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_schemamap_changefreq
 *
 * @param string $changefreq Change frequency value.
 *
 * @return string Modified change frequency.
 */
function custom_schemamap_changefreq( $changefreq ) {
	return 'weekly'; // Change from daily to weekly.
}
```

### wpseo_schema_aggregator_schemamap_priority

Set the priority value for schemamap entries.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$priority` | float | Priority value between 0.0 and 1.0 |

**Default Value:** `0.8`

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_schemamap_priority', 'custom_schemamap_priority' );

/**
 * Increase schemamap priority to indicate importance.
 *
 * Higher priority suggests to search engines that this content
 * is important relative to other URLs on your site.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_schemamap_priority
 *
 * @param float $priority Priority value (0.0 to 1.0).
 *
 * @return float Modified priority value.
 */
function custom_schemamap_priority( $priority ) {
	return 1.0; // Maximum priority.
}
```

## 筛选策略

筛选策略决定哪些模式片段被包含或排除在聚合之外。默认情况下，聚合器会过滤掉 Actions、Enumerations、Meta 类型和 Website 元素。

### wpseo_schema_aggregator_filtering_strategy

Implement a custom filtering strategy by providing your own strategy class.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$strategy` | Filtering_Strategy_Interface | The filtering strategy instance |

**Default Value:** `Default_Filter` instance (filters Actions, Enumerations, Meta, and Website categories)

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_filtering_strategy', 'custom_filtering_strategy' );

/**
 * Implement custom schema filtering logic.
 *
 * Use this to create advanced filtering rules beyond the default
 * context-based filtering. Your strategy class should implement
 * the filtering logic in its filter() method.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_filtering_strategy
 *
 * @param object|null $strategy Custom filtering strategy instance.
 *
 * @return object Custom filtering strategy.
 */
function custom_filtering_strategy( $strategy ) {
	// Return custom strategy that only includes Article types.
	return new class {
		/**
		 * Filter schema pieces to only include Articles.
		 *
		 * @param array $schema_piece Schema piece to filter.
		 *
		 * @return bool Whether to include this schema piece.
		 */
		public function filter( $schema_piece ) {
			// Only include Article types and their subtypes.
			$article_types = [ 'Article', 'BlogPosting', 'NewsArticle', 'ScholarlyArticle' ];

			if ( isset( $schema_piece['@type'] ) ) {
				$type = $schema_piece['@type'];

				// Handle array of types.
				if ( is_array( $type ) ) {
					return ! empty( array_intersect( $type, $article_types ) );
				}

				return in_array( $type, $article_types, true );
			}

			return false;
		}
	};
}
```

## Elements Context Map

The context map categorizes schema.org types into contexts like Content, Commerce, Entity, etc. This is used by the default filtering strategy.

### wpseo_schema_aggregator_elements_context_map

Override the complete elements context map.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$context_map` | array | Complete mapping of schema types to contexts |

**Default Value:** Comprehensive map of 1000+ schema.org types

**Example:**

```php
add_filter( 'wpseo_schema_aggregator_elements_context_map', 'custom_context_map' );

/**
 * Provide a completely custom context map.
 *
 * This replaces the entire default mapping. Use this when you need
 * complete control over which types belong to which contexts.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_elements_context_map
 *
 * @param array $context_map Complete context map.
 *
 * @return array Custom context map.
 */
function custom_context_map( $context_map ) {
	// Define a simplified context map.
	return [
		'content' => [
			'Article',
			'BlogPosting',
			'NewsArticle',
		],
		'commerce' => [
			'Product',
			'Offer',
		],
		'entity' => [
			'Person',
			'Organization',
		],
	];
}
```

### wpseo_schema_aggregator_elements_context_map_\{context\}

Modify elements for a specific context. Available contexts: `content`, `commerce`, `entity`, `event`, `data`, `medical`, `action`, `enumeration`, `meta`, `website`.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$elements` | array | Array of schema types in this context |

**Example: Add Custom Types to Content Context**

```php
add_filter( 'wpseo_schema_aggregator_elements_context_map_content', 'add_custom_content_types' );

/**
 * Add custom schema types to the content context.
 *
 * This is useful when you have custom schema types that should
 * be treated as content entities.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_elements_context_map_context
 *
 * @param array $elements Schema types in the content context.
 *
 * @return array Modified array of schema types.
 */
function add_custom_content_types( $elements ) {
	// Add custom types to content context.
	$elements[] = 'Recipe';
	$elements[] = 'HowTo';
	$elements[] = 'FAQPage';

	return $elements;
}
```

**Example: Remove Types from Action Context**

```php
add_filter( 'wpseo_schema_aggregator_elements_context_map_action', 'remove_action_types' );

/**
 * Remove specific action types from the action context.
 *
 * This can help further refine which actions are filtered out.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#wpseo_schema_aggregator_elements_context_map_context
 *
 * @param array $elements Schema types in the action context.
 *
 * @return array Modified array of schema types.
 */
function remove_action_types( $elements ) {
	// Remove SearchAction from action context.
	// This means it won't be filtered out anymore.
	return array_diff( $elements, [ 'SearchAction' ] );
}
```

## Article Enhancement

Articles and their subtypes (BlogPosting, NewsArticle, etc.) can be enhanced with additional properties like `articleBody`, `description`, and `keywords`.

### Article Enhancement Configuration

Use the `wpseo_article_enhance_config_{key}` filter pattern to configure enhancement behavior. Available keys:

- `article_body_max_length` - Maximum length for articleBody in characters (default: 500)
- `excerpt_max_length` - Maximum length for excerpt in characters (default: 0, no limit)
- `categories_as_keywords` - Include categories as keywords (default: false)
- `excerpt_prefer_manual` - Only use manually written excerpts; skip auto-generated ones (default: false)
- `strip_shortcodes_from_body` - Strip shortcodes from article body (default: true)
- `strip_html_from_body` - Strip HTML tags from article body (default: true)

**Example: Adjust Article Body Length**

```php
add_filter( 'wpseo_article_enhance_config_article_body_max_length', 'custom_article_body_length' );

/**
 * Increase maximum article body length.
 *
 * Useful for long-form content where you want to include
 * more of the article text in the schema.
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#article-enhancement-configuration
 *
 * @param int $max_length Maximum length in characters.
 *
 * @return int Modified maximum length.
 */
function custom_article_body_length( $max_length ) {
	return 2000; // Increase from 500 to 2000 characters.
}
```

**Example: Include Categories as Keywords**

```php
add_filter( 'wpseo_article_enhance_config_categories_as_keywords', '__return_true' );
```

**Example: Only Use Manual Excerpts**

```php
add_filter( 'wpseo_article_enhance_config_excerpt_prefer_manual', '__return_true' );
```

### 文章增强功能开关

使用 `wpseo_article_enhance_{enhancement}` 过滤器模式来动态启用或禁用增强功能。可用的增强功能包括：`article_body`、`use_excerpt`、`keywords`（默认值均为 `true`）。

**示例：禁用文章正文增强**

```php
add_filter( 'wpseo_article_enhance_article_body', '__return_false' );
```

**示例：禁用摘要增强**

```php
add_filter( 'wpseo_article_enhance_use_excerpt', '__return_false' );
```

### 文章正文与摘要共存设置

控制当文章已存在摘要时，是否仍包含正文内容。

**过滤器：** `wpseo_article_enhance_body_when_excerpt_exists`

**参数说明：**

| 参数名 | 类型 | 描述 |
|-----------|------|-------------|
| `$include_body` | 布尔值 | 当摘要存在时是否包含正文 |

**默认值：** `false`

**使用示例：**

```php
add_filter( 'wpseo_article_enhance_body_when_excerpt_exists', '__return_true');
```

### 文章正文回退

控制当摘要不存在时是否包含文章正文。

**过滤器：** `wpseo_article_enhance_article_body_fallback`

**参数：**

| 参数 | 类型 | 描述 |
|-----------|------|-------------|
| `$use_fallback` | bool | 是否使用正文作为回退 |

**默认值：** `true`

**示例：**

```php
add_filter( 'wpseo_article_enhance_article_body_fallback', '__return_false' );
```

## 人员信息增强

人员架构片段可根据 WordPress 用户数据进行属性增强。

### 人员增强配置

使用 `wpseo_person_enhance_config_{key}` 过滤器模式来配置人员增强设置。这与文章增强配置遵循相同的通用模式——过滤器接收默认值并返回自定义值。

**示例：**

```php
add_filter( 'wpseo_person_enhance_config_my_custom_key', 'custom_person_config' );

/**
 * 自定义人员增强配置值。
 *
 * @link https://developer.yoast.com/features/schema/schema-aggregator/filters/#person-enhancement-configuration
 *
 * @param string|int|bool $value 默认配置值。
 *
 * @return string|int|bool 修改后的配置值。
 */
function custom_person_config( $value ) {
	return 'custom_value';
}
```

### 人员信息增强开关

使用 `wpseo_person_enhance_{enhancement}` 过滤器模式来启用或禁用特定的人员信息增强功能。可用的增强项：`person_job_title`（默认值：`true`）。

**示例：禁用职位头衔增强功能**

```php
add_filter( 'wpseo_person_enhance_person_job_title', '__return_false' );
```

## Robots.txt 集成

默认情况下，schemamap 会自动在您网站的 `robots.txt` 文件中被引用。如果需要，您可以禁用此集成。

### wpseo_disable_robots_schemamap

禁用 robots.txt 中的 schemamap 引用。

**参数:**

| 参数 | 类型 | 描述 |
|-----------|------|-------------|
| `$disable` | bool | 是否禁用 robots.txt 集成 |

**默认值:** `false`

**示例:**

```php
add_filter( 'wpseo_disable_robots_schemamap', '__return_true' );
```

### 开发环境配置

禁用缓存并调整设置以便于开发：

```php
/**
 * 开发环境下的 Schema 聚合器配置。
 */

// 在开发环境中禁用缓存。
add_filter( 'wpseo_schema_aggregator_cache_enabled', function( $enabled ) {
	return defined( 'WP_DEBUG' ) && WP_DEBUG ? false : $enabled;
} );

// 使用较小的分页以便更快地测试。
add_filter( 'wpseo_schema_aggregator_per_page', function() {
	return defined( 'WP_DEBUG' ) && WP_DEBUG ? 10 : 1000;
} );

// 在开发环境中不向 robots.txt 添加 schemamap。
add_filter( 'wpseo_disable_robots_schemamap', function() {
	return defined( 'WP_DEBUG' ) && WP_DEBUG;
} );
```

## Related Documentation

- [Schema Aggregator Overview](docs/features/schema/schema-aggregator/overview) - Introduction and key features
- [API Reference](docs/features/schema/schema-aggregator/api-reference) - REST API endpoints and CLI commands
- [Schema.org Documentation](https://schema.org/) - Schema types reference
- [JSON-LD Specification](https://json-ld.org/) - Format specification
- [Yoast Schema API](docs/features/schema/api) - Main Schema output API