---
title: "Review Apps"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Integrations
    - Shopify
    - Repos
---


import YoastSchemaExample from '../../../src/components/YoastSchemaExample';

为了与 Shopify 平台上的评论应用集成，我们依赖 Shopify 提供的标准元字段定义。
这些定义在整个平台内具有互操作性，允许不同的应用相互连接，并无缝集成到主题中。

## Yoast SEO for Shopify 架构中的变更

基于标准元字段定义，产品页面的架构中增加了一个额外的 `aggregateRating` 属性。
为了生成这个 `AggregateRating`，我们分别使用产品元字段数据 `reviews.rating_count` 和 `reviews.rating` 来填充 `reviewCount` 和 `ratingValue` 属性。

以下是新架构属性的示例：

<YoastSchemaExample>
{`{
    "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4",
        "reviewCount": "3077"
    }
}`}
</YoastSchemaExample>

## 支持的集成

除了与标准元字段定义的集成外，目前 Yoast SEO for Shopify 还与以下服务集成：

- [Judge.me](https://apps.shopify.com/judgeme)
- [Loox](https://apps.shopify.com/loox)
- [Ali Reviews](https://apps.shopify.com/ali-reviews)
- [Opinews](https://apps.shopify.com/photo-reviews)

**Judge.me** 在页面上添加了一个单独的架构属性，通过 `@id` 引用连接到 Yoast SEO for Shopify 产品架构。
这个单独的架构属性在 Yoast SEO for Shopify 生成的架构图中无法找到。