---
title: "Woocommerce Seo"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Plugins
    - Schema
    - Features
---


我们的 [WooCommerce SEO 插件](https://yoast.com/wordpress/plugins/yoast-woocommerce-seo/) 的 Schema 输出基于 [Yoast SEO 的 Schema 输出](docs/features/schema/plugins/yoast-seo)，增加了关于产品和电子商务功能的额外细节。

## 核心功能

### 在所有页面上
* 移除 WooCommerce 的面包屑 Schema。

### 在产品页面上
* 将 `WebPage` 片段的 `@type` 更改为 `ItemPage`。
* 修改 `Product` 片段。
  * 对每个现有的 WooCommerce *片段*/值应用我们的验证逻辑。
  * 向 `Product` 添加一个 `mainEntityOfPage` 属性，通过 ID 引用 `WebPage`。
  * 根据分类设置，设置 `brand` 和 `manufacturer` 属性。
  * 将 `seller` 设置为被设为 `WebSite` 的 `Publisher` 的 `Organization`（或 `Person`）。
  * 将 `offers`、`review` 和/或 `aggregateRating` 值编织到图中（当可用/适当时）。
  * 从 `ItemPage` 中取消设置 `datePublished` 和 `dateModified` 值。

#### 过滤器
* 使用 `wpseo_schema_product` 过滤器自定义 Yoast 产品 Schema。更多信息请参见 [产品 Schema 片段](docs/features/schema/pieces/product)。
* 使用 `wpseo_schema_offer` 过滤器自定义产品 Schema 的报价部分。更多信息请参见 [报价 Schema 片段](docs/features/schema/pieces/offer)。

### 在结账页面上
* 将 `WebPage` 片段的 `@type` 更改为 `CheckoutPage`。

### 在所有页面上可选。
* `hasMerchantReturnPolicy`：一个 `MerchantReturnPolicy` 类型的数组，代表组织的退货政策。

## 更多信息
* [产品 Schema 片段](docs/features/schema/pieces/product)
* [产品组 Schema 片段](docs/features/schema/pieces/productgroup)
* [聚合报价 Schema 片段](docs/features/schema/pieces/aggregateoffer)
* [组织 Schema 片段](docs/features/schema/pieces/organization)
* [报价 Schema 片段](docs/features/schema/pieces/offer)
* [个人 Schema 片段](docs/features/schema/pieces/person)
* [组织 Schema 片段](docs/features/schema/pieces/organization)
* [评论 Schema 片段](docs/features/schema/pieces/review)
* [网站 Schema 片段](docs/features/schema/pieces/website)
* [网页 Schema 片段](docs/features/schema/pieces/webpage)