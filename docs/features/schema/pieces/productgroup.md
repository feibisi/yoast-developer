---
title: "Productgroup"
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

描述一个在属性（颜色、尺寸、价格）上存在变体的 `Product`。
更多信息请参阅 [Product Schema 片段](docs/features/schema/pieces/product)。

## 触发器

* 应在代表产品的页面（例如产品页面）上输出。具体来说，当产品具有变体时（例如 WooCommerce 中的 WC_Product_Variable 类实例）。

## 必需属性

有效的 `ProductGroup` 必须包含 `offers`。`Product` 的每个 `offer` 将被转换为 `ProductGroup` 中 `hasVariant` 下的一个 `Product` 条目。

### 失败场景

如果缺少 `offers`，我们将不会把 `Product` 更改为 `ProductGroup`。

## 可选属性

以下属性应在可用且有效时添加：

* `productGroupID`：默认情况下，使用 `sku` 作为此属性的值。

除此之外，`ProductGroup` 可以拥有 `Product` 的所有可选属性。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "ProductGroup",
            "@id": "https://www.example.com/#/schema/Product/abc123",
            "name": "Example Product",
            "image": {
                "@id": "https://www.example.com/uploads/example-image.jpg"
            },
            "hasVariant": [
                  {
                      "@id": "https://www.example.com/#/schema/Product/def456"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/Product/hij789"
                  }
            ]
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
            "@type": "ProductGroup",
            "@id": "https://www.example.com/#/schema/Product/abc123",
            "name": "Example Product",
            "description": "Example product description",
            "image": {
                "@id": "https://www.example.com/uploads/example-image.jpg"
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
            "productGroupID": "abc123",
            "aggregateRating": {
                "@type": "AggregateRating",
                "ratingValue": "4",
                "reviewCount": "3077"
            },
            "hasVariant": [
                {
                    "@type": "Product",
                    "@id": "https://www.example.com/#/schema/Product/def456",
                    "name": "Example Product - Red",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-red.jpg"
                    },
                    "color": "red"
                },
                {
                    "@type": "Product",
                    "@id": "https://www.example.com/#/schema/Product/hij789",
                    "name": "Example Product - Blue",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-blue.jpg"
                    },
                    "color": "Blue"
                }
            ]
        }
    ]
}`}
</YoastSchemaExample>