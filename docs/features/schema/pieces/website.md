---
title: "Website"
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

描述一个 `WebSite`。是 [`WebPage`](docs/features/schema/pieces/webpage) 的父级。

## 触发条件
应在所有公开页面上输出。

## 必需属性
一个有效的 `WebSite` 必须包含以下属性。

* `@type`: `WebSite`。
* `@id`: 网站的首页 URL，后接 `#/schema/WebSite/{{ID}}`，其中 `{{ID}}` 是一个全局唯一且稳定的标识符。
  * 如果该 `WebSite` 是正在被浏览/消费的网站（即“*本网站*”），那么 `{{ID}}` 的值应为 `1`。
* `url`: 网站的首页 URL（不包含末尾斜杠）。
* `name`: 网站的名称。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

## 可选属性
以下属性应在可用且有效时添加：

* `publisher`：指向发布 `WebSite` 实体的节点的 ID 引用。
* `potentialAction`：描述网站内部搜索的 [`SearchAction`](docs/features/schema/pieces/searchaction) 对象。
* `inLanguage`：网站的语言代码；例如 `en-GB`。
 * 如果网站支持多种语言，则输出 `inLanguage` 值的数组。
* `description`：网站的描述（例如网站的标语）。
* `alternateName`：网站的备用（通常为*更短的*）名称。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com",
              "name": "Example website"
          }
      ]
  }`}
</YoastSchemaExample>

### Extended criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com",
              "name": "Example website",
              "alternateName": "Example.com",
              "inLanguage": "en-US",
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              }
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API：修改网站 Schema 输出 {#api}

要修改 Yoast SEO 输出的 `Website` schema，你可以使用我们的 `wpseo_schema_website` 过滤器。示例如下：

```php
add_filter( 'wpseo_schema_website', 'example_change_website' );

/**
 * 修改网站 Schema 数据输出，覆盖名称和备用名称。
 *
 * @param array $data Schema.org 网站数据数组。
 *
 * @return array Schema.org 网站数据数组。
 */
function example_change_website( $data ) {
    $data['name']          = 'Yoast';
    $data['alternateName'] = 'Yoast.com';

    return $data;
}
```

要修改 [`SearchAction`](docs/features/schema/pieces/searchaction) 输出，请参阅[其 API 文档](docs/features/schema/pieces/searchaction#api)。

要对我们的 Schema 输出进行更多修改，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。