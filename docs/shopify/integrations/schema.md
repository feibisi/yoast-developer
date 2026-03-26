---
title: "Schema"
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


import YoastSchemaExample from '@site/src/components/YoastSchemaExample';

Yoast SEO for Shopify 会输出 [schema.org 标记](/features/schema/plugins/yoast-seo-shopify)。这是基于 Shopify 中可用的数据以及通过应用界面填写的额外数据来完成的。其他添加结构化数据功能（例如，评论、食谱等）的应用可以将这些数据整合到我们的 schema.org 数据中。关于我们的方法、技术和规范的更多信息可以在[这里](/features/schema)找到。

## 修改模式图

图中的所有片段都包含一个 `@id` 属性。此 ID 可用于属性以 _引用_ 现有片段，或通过附加数据 _扩展_ 现有片段。网页上可以放置多个 `<script>` 标签，所有 `<script>` 标签中所有片段的组合应形成一个连接的模式数据图。请使用我们[首选的验证工具](/features/schema/functional-specification#5-validation-tools)验证所有更改。

有关特定 `@id` 值及其创建方式的更多信息，请参见[此处](#constructing-id-parameters)。

### 添加片段

通常，我们的图谱会包含一个 `Organization`（组织），并将该 `Organization` 作为 `WebSite`（网站）的发布者进行引用。
我们还会添加当前 `WebPage`（网页）是 `WebSite` 的一部分。如果你想向此图谱中添加引用特定片段的其他数据，可以通过引用 `@id` 属性来实现。例如，你可以添加一个 `AggregateRating`（综合评分），其中被评级的项目是 `Organization`：

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "AggregateRating",
              "@id": "https://www.example.com/#/schema/AggregateRating/1",
              "ratingValue": "4",
              "ratingCount": "250"
              "itemReviewed": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              }
          }
      ]
  }`}
</YoastSchemaExample>

### 编辑片段

架构图中已有的片段也可以进行编辑。这只能通过向片段添加额外数据来实现。
例如，当前的 `WebPage` 可以通过添加 `FAQPage` 类型进行增强，并添加 `Question` 片段。此示例
依赖于现有 `WebPage` 的 `@id` 属性，这将导致数据合并为一个大的 `WebPage / FAQPage` 片段。

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@type": "FAQPage",
      "@id": "https://www.example.com/pages/frequently-asked-questions",
      "mainEntity": [{
          "@type": "Question",
          "name": "Is there a local shop we can visit?",
          "acceptedAnswer": {
              "@type": "Answer",
              "text": "No. The shop is only online."
          }
      }]
  }`}
</YoastSchemaExample>

### 各模块的可用性

并非所有模块都适用于网店的每个页面。我们描述了[核心逻辑](/features/schema/plugins/yoast-seo-shopify)，说明这些模块何时会被添加到页面中。在修改图谱时，也应考虑相同的逻辑。

#### 检查组件是否已启用

通过 Yoast SEO for Shopify 应用，可以禁用多个组件。在这些情况下，组件将永远不会被输出。
我们将此数据存储在 `shop` 对象上的元字段 `yoast_seo.settings` 中。这是一个 JSON 类型的元字段，其中这些组件的布尔启用状态存储在 `schema.outputControls` 中。

| Schema 组件      | 元字段                                                              |
|------------------|----------------------------------------------------------------------|
| 整个图谱         | `shop.metafields.yoast_seo.value.schema.outputControls.schema`       |
| `Organization`   | `shop.metafields.yoast_seo.value.schema.outputControls.organization` |
| `WebSite`        | `shop.metafields.yoast_seo.value.schema.outputControls.website`      |
| `WebPage`        | `shop.metafields.yoast_seo.value.schema.outputControls.webpage`      |
| `Article`        | `shop.metafields.yoast_seo.value.schema.outputControls.article`      |
| `Product`        | `shop.metafields.yoast_seo.value.schema.outputControls.product`      |
| `BreadcrumbList` | `shop.metafields.yoast_seo.value.schema.outputControls.breadcrumb`   |

#### 检查已定义的组织名称

仅当在 Yoast SEO for Shopify 应用的设置中定义了名称时，才会添加 `Organization` 片段。
在 `shop` 对象的同一 `yoast_seo.settings` 元字段中，也存在用于表示组织的数据。
组织名称不应为空。该名称存储在
`shop.metafields.yoast_seo.value.schema.siteRepresentation.organizationName` 中。

#### 检查有效文章

虽然 `WebPage` 始终可以放置在页面上，但 `Article` 依赖于 `Organization`，因此也依赖于已定义的组织名称。

`Article` 还需要 `description` 属性的数据。默认情况下，我们为此重用元描述。如果没有元描述，文章将不会输出。

`Article` 的第三个要求是拥有要显示的图像。此图像可以是 `page_image`，也可以是存储在 `shop.metafields.yoast_seo.value.siteSettings.siteDefaults.siteImage.url` 中并存在于全局 Liquid 变量 `images` 中的已定义站点图像。图像也可以取自 `article.image` 对象，或使用存储在 `article.metafields.yoast_seo.indexable.value.open_graph_image_url` 中的 OpenGraph 设置。

## 构建 ID 参数

我们架构输出中的所有片段都可以通过 `@id` 属性引用。要复制 `@id` 值，我们应用以下逻辑。

* `Organization`: `shop.url | append: '/#/schema/organization/1'`
* `WebSite`: `shop.url | append: '/#/schema/website/1'`
* `WebPage`: `canonical_url`
* `Article`: `canonical_url | append: '#/schema/article/' | append: article.id`
* `Person`: `article.author | handleize | prepend: '/#/schema/person/' | prepend: shop.url`
* `Product`: `canonical_url | append: "/#/schema/Product"`
* `Offer`: `shop.url | append: '/#/schema/Offer/' | append: variant.id`
* `BreadcrumbList`: `canonical_url | append: '/#/schema/breadcrumb'`
* `ImageObject`: `shop.url | append: '/#/schema/ImageObject/' | append: image.id`