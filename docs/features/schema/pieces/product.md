---
title: "Product"
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

描述企业销售的产品。

## 触发条件
* 应在代表产品的页面（例如产品页面）上输出。

## 必需属性
一个有效的 `Product` 必须包含以下属性。

* `@type`: `Product`。
* `@id`: 网站的首页 URL 后追加 `#/schema/Product/{{ID}}`，其中 `{{ID}}` 是一个唯一标识符。
* `name`: 产品名称。
* `image`: 指向一个或多个代表该产品的 `ImageObject` 的 ID 引用。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果节点未输出，任何原本声明自己属于 `Product` 的实体都应移除这些引用（例如，与 `Product` 相关的 `Offer`）。

## 可选属性
以下属性应在可用且有效时添加：

* `offers`：一个数组，包含对一个或多个 `Offer` 或 `AggregateOffer` 片段的 ID 引用。
* `brand`：对一个 `Organization` 片段的引用，表示与 `Product` 关联的品牌。
* `seller`：对一个 `Organization` 片段的引用，表示 `WebSite`。
* `description`：产品的文本描述。
* `review`：一个数组，包含对一个或多个 `Review` 片段的 ID 引用。
* `sku`：`Product` 的商家特定标识符。
* `aggregateRating`：一个 `AggregateRating` 对象。
* `manufacturer`：对一个 `Organization` 片段的引用，表示生产 `Product` 的品牌。
* `gtin8`：`Product` 的 GTIN-8（全球贸易项目代码）。
* `gtin12` / `UPC`：`Product` 的 GTIN-12（全球贸易项目代码）。
* `gtin13` / `EAN`：`Product` 的 GTIN-13（全球贸易项目代码）。
* `gtin14` / `ITF-14`：`Product` 的 GTIN-14（全球贸易项目代码）。
* `isbn`：`Book` 的 ISBN（国际标准书号）。提供此属性时，`Product` 的 `@type` 将变为 `Book`。
* `mpn`：`Product` 的 MPN（制造商部件编号）。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Product",
              "@id": "https://www.example.com/#/schema/Product/abc123",
              "name": "Example Product",
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              }
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
              "@type": "Product",
              "@id": "https://www.example.com/#/schema/Product/abc123",
              "name": "Example Product",
              "description": "Example product description",
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "offers": {
                  "@id": "https://www.example.com/#/schema/Offer/abc123"
              },
              "brand": {
                  "@id": "https://www.example.com/#/schema/Organization/abc123"
              },
              "manufacturer": {
                  "@id": "https://www.example.com/#/schema/Organization/def456"
              },
              "review": [
                  {
                      "@id": "https://www.example.com/#/schema/Review/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/Review/def456"
                  }
              ],
              "sku": "abc123",
              "aggregateRating": {
                  "@type": "AggregateRating",
                  "ratingValue": "4",
                  "reviewCount": "3077"
              }
          }
      ]
  }`}
</YoastSchemaExample>

## Yoast WooCommerce SEO API：修改产品 Schema 输出 {#api}

要修改 Yoast SEO 输出的 `Product` schema，你可以使用我们的 `wpseo_schema_product` 过滤器。示例如下：

```php
add_filter( 'wpseo_schema_product', 'example_change_product' );

/**
 * 向产品 schema 添加颜色属性。
 *
 * @param array $data Schema 产品数据。
 *
 * @return array Schema.org 产品数据数组。
 */
function example_change_product( $data ) {
	$data['color'] = 'red';

	return $data;
}
```

我们还为产品 schema 的 offer 部分提供了一个更具体的过滤器：`wpseo_schema_offer`，更多信息请参阅 [Offer Schema 片段](docs/features/schema/pieces/offer#api)。

要对我们的 Schema 输出进行更多修改，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。