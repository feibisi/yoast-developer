---
title: "Searchaction"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Pieces
    - Schema
    - Features
---


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

描述 [`WebSite`](docs/features/schema/pieces/website) 上的一个 `SearchAction`。

## 触发条件
当网站拥有/支持内部搜索功能时，应作为 `WebSite` 节点的 `potentialAction` 属性添加。

## 必需属性
一个有效的 `SearchAction` 必须包含以下属性。

* `@type`: `SearchAction`。
* `target`: 一个 `EntryPoint` 类型的对象，其 `urlTemplate` 描述了内部搜索功能的 URL 模式（例如，`https://www.example.com/?s={search_term_string}`）。
* `query-input`: 一个 `PropertyValueSpecification` 类型的对象，包含以下属性：
 * `valueRequired`: 布尔值 `true`。
 * `valueName`: 在 `target` 中描述的搜索词字符串（例如，`search_term_string)`。

### 失败场景
如果任何必需字段缺失或无效，则不应输出该节点。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "SearchAction",
              "target": "https://www.example.com/?s={search_term_string}",
              "query-input": {
                  "@type": "PropertyValueSpecification",
                  "valueRequired": true,
                  "valueName": "search_term_string"
              }
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API：更改 SearchAction 模式输出 {#api}

要更改 Yoast SEO 输出的 `SearchAction` 模式，可以使用以下过滤器：

### 禁用 `SearchAction` 输出
这将完全禁用 `SearchAction` 输出：

```php
add_filter( 'disable_wpseo_json_ld_search', '__return_true' );
```

### 更改 `SearchAction` URL
要更改搜索 URL，请使用 `wpseo_json_ld_search_url` 过滤器。确保返回的 URL 包含搜索参数变量 `{search_term_string}`，否则将无法工作。

```php
add_filter( 'wpseo_json_ld_search_url', 'schema_change_search_url' );

function schema_change_search_url() {
    return 'https://example.com/search/?q={search_term_string}';
}
```

要对我们的模式输出进行更多更改，请参阅 [Yoast SEO 模式 API](docs/features/schema/api)。