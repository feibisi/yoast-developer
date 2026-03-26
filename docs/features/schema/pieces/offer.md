---
title: "Offer"
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

描述一个 `Product` 的优惠信息（通常包括价格、库存情况等）。

## 触发条件
* 当 `Product` 片段需要时输出。

## Required properties
A valid `Offer` must have the following properties.

* `@type`: `Offer`.
* `@id`: The site's home URL appended by `#/schema/Offer/{{ID}}`, where `{{ID}}` is a unique identifier.
* `availability`: A schema.org URL representing a schema `itemAvailability` value (e.g., *https://schema.org/OutOfStock*).
* `url`: The canonical URL of the page containing the `Offer`.
* `priceSpecification`: A `PriceSpecification` object, including a `price`, `priceCurrency`, and `valueAddedTaxIncluded` property (of either `true` or `false`).

## 可选属性
以下属性应在可用且有效时添加：

* `priceValidUntil`：日期（格式为 YYYY-mm-dd），表示当前临时定价的有效截止时间（例如，有明确结束日期的促销期）。
* `seller`：通过 ID 引用发布该 `WebSite` 的 `Organization`。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果节点未输出，任何原本会引用该 `Offer` 的实体都应移除这些引用（例如，当产品通过 ID 引用 `Offer` 时）。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Offer",
              "@id": "https://www.example.com/#/schema/Offer/abc123",
              "priceSpecification": {
                  "@type": "PriceSpecification",
                  "price": "22.00",
                  "valueAddedTaxIncluded": "false",
                  "priceCurrency": "GBP"
              },
              "availability": "http://schema.org/InStock",
              "url": "https://www.example.com/example-product-page/",
              "seller": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              }
          }
      ]
  }`}
</YoastSchemaExample>

## Yoast WooCommerce SEO API：修改 Offer Schema 输出 {#api}

要修改 Yoast SEO 输出的 `Offer` schema，您可以使用我们的 `wpseo_schema_offer` 过滤器。示例如下：

```php
add_filter( 'wpseo_schema_offer', 'example_change_offer', 10, 3 );

/**
 * 将产品描述添加到 offer schema 中。
 *
 * @param array $offer schema offer 数据。
 * @param WC_Product_Variation $variation 我们正在处理的 WooCommerce 产品变体。
 * @param WC_Product $product 我们正在处理的 WooCommerce 产品。
 *
 * @return array Schema.org Offer 数据数组。
 */
function example_change_offer( $offer, $variation, $product ) {
	$offer['priceSpecification']['description'] = $product->get_description();

	return $offer;
}
```

我们还提供了一个针对完整产品 schema 的通用性稍弱的过滤器：`wpseo_schema_product`，更多信息请参阅 [产品 Schema 组件](docs/features/schema/pieces/product#api)。

要对我们的 Schema 输出进行更多修改，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。