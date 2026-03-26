---
title: "Aggregateoffer"
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

描述一组 `Product` 的 `offers`，通常由于属性（颜色、尺寸、价格）的差异而形成。

## 触发条件
* 当 `Product` 片段需要时，应输出此结构。

## 必需属性
一个有效的 `AggregateOffer` 必须包含以下属性。

* `@type`: `AggregateOffer`。
* `@id`: 网站的首页 URL，后接 `#/schema/AggregateOffer/{{ID}}`，其中 `{{ID}}` 是相关（聚合）报价的唯一标识符。
* `lowPrice`: 该组中的最低价格，省略任何货币符号，并使用 '.' 表示小数点。
* `highPrice`: 该组中的最高价格，省略任何货币符号，并使用 '.' 表示小数点。
* `priceCurrency`: 用于描述产品价格的货币，采用三位字母的 ISO 4217 格式。
* `offerCount`: 该组中的报价数量。
* `offers`: 一个 `Offer` 片段的数组，通过 ID 引用。

### 失败场景
如果任何必需字段缺失或无效，则不应输出此节点。
如果未输出此节点，任何原本会引用该 `AggregateOffer` 的实体应移除这些引用（例如，当产品通过 ID 引用一个 `AggregateOffer` 时）。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "AggregateOffer",
              "@id": "https://www.example.com/#/schema/AggregateOffer/abc123",
              "lowPrice": "22.00",
              "highPrice": "136.00",
              "priceCurrency": "GBP",
              "offerCount": 3,
              "offers": [
                  {
                      "@id": "https://www.example.com/#/schema/Offer/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/Offer/def456"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/Offer/ghi789"
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>