---
title: "Review"
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

描述一个 `Review`（评论）。通常在 `Product`（产品）或 `Organization`（组织）的上下文中使用。

## 触发条件
应在图中作为顶级节点添加，当其他节点需要时。例如，当某个 `Product` 拥有 `review` 时。

## 必需属性
一个有效的 `Review` 必须包含以下属性。

* `@type`: `Review`。
* `@id`: 网站的首页 URL 后追加 `#/schema/Review/{{ID}}`，其中 `{{ID}}` 是唯一标识符。
* `author`: 一个通过 ID 引用撰写该评论的 `Person`（人物）。
* `reviewRating`: 一个 `Rating`（评分）对象，包含 `ratingValue` 属性。如果评分范围不是 1 到 5，还需添加 `bestRating` 和 `worstRating` 属性。

### 失败场景
如果任何必需字段缺失或无效，则不应输出该节点。

如果未输出该节点，任何原本会声明与该 `Review` 有关系的实体（例如，被 `Product` 引用的 `reviews`）应移除这些引用。

## 可选属性
以下属性在可用且有效时应添加：

* `reviewBody`: 评论的文本内容。
* `name`: 评论的标题。
* `datePublished`: 评论发布的日期，采用 ISO 8601 日期格式。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Review",
              "@id": "https://www.example.com/#/schema/Review/abc123",
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              },
              "reviewRating": {
                  "@type": "Rating",
                  "ratingValue": 4
              }
          }
      ]
  }`}
</YoastSchemaExample>

### 扩展标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Review",
              "@id": "https://www.example.com/#/schema/Review/abc123",
              "name": "Example review name",
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              },
              "reviewRating": {
                  "@type": "Rating",
                  "ratingValue": 8,
                  "bestRating": 10,
                  "worstRating": 1
              },
              "reviewBody": "Example review body",
              "datePublished": "2022-07-21"
          }
      ]
  }`}
</YoastSchemaExample>