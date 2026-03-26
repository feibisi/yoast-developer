---
title: "Technology Approach"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Schema
    - Features
    - Repos
---


import YoastSchemaExample from '@site/src/components/YoastSchemaExample';

## JSON-LD 作为首选格式
结构化标记可以通过多种方式以及多种不同的标准来实现。鉴于谷歌严格遵守其规范，我们对 [schema.org](https://schema.org/) 定义的标准特别感兴趣。

[Schema.org](http://schema.org/) 标记可以通过多种方式添加到网页中。在所有可用的方法中，**我们认为在页面的 HTML 源代码中包含 JSON-LD 是（当前）最佳方法**。

JSON-LD 提供了我们实现一致且可扩展基础所需的灵活性、可扩展性和标准化。虽然它缺少“内联”（结构化标记直接作为表示这些实体的 HTML 代码的一部分实现）的一些潜在好处，但其优势、灵活性和可扩展性远远超过了这些限制。

## ID、关系和嵌套
JSON-LD 允许属性通过 *ID* 引用其他片段。例如，*产品页面* 无需包含（或重复）销售该产品的 *组织* 的所有标记，只需引用代表该 *组织* 的片段 ID 即可。
理论上，这使我们能够避免重复共享属性，并减少表示页面内容所需的代码/处理/开销。

遗憾的是，这项技术的 *跨页面* 支持水平有限——Google 的文档表述模糊，且 ID 与 URI 之间的关系存在歧义（据传闻，他们“无法从其他页面提取结构化数据”[*原文如此*]）。鉴于这一限制，我们要求每个页面输出所有相关片段，并通过 `hasPart`、`isPartOf`、`mainEntityOfPage` 及类似的查找机制进行交叉引用。

例如，以下（简化后的）JSON 片段定义了一个 `Organization`，并将该 `Organization` 作为 `WebSite` 的 `Publisher` 进行引用：

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/1",
              "url": "https://www.example.com/",
              "name": "Example organization name"
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com/",
              "name": "Example website",
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              }
          }
      ]
  }`}
</YoastSchemaExample>

在此示例中，我们知道网站的 `publisher` 与 `Organization` 相同。由于图中已存在代表该 `Organization` 的顶级片段，我们只需在 `WebSite` 片段中通过其 `@id` 引用它。这提供了极大的灵活性，并避免了重复。

基于此能力，我们倾向于尽可能避免属性的深度嵌套，并更愿意拆分出独立的片段，如下文示例所示。这使代码保持可读性、模块化和可扩展性。

### 构建 ID 参数
ID 参数通常应采用以下结构，下文列出的情况除外：

`{{website}}/#/schema/{{type}}/{{ID}}`

例如：

* `https://www.example.com/#/schema/organization/abc123`
* `https://www.example.com/#/schema/person/abc123`
* `https://www.example.com/#/schema/location/abc123`
* `https://www.example.com/#/schema/address/abc123`
* `https://www.example.com/#/schema/website/abc123`
* `https://www.example.com/#/schema/product/abc123`
* `https://www.example.com/#/schema/offer/abc123`
* `https://www.example.com/#/schema/article/abc123`
* `https://www.example.com/#/schema/breadcrumb/abc123`
* `https://www.example.com/#/schema/howto/abc123`
* `https://www.example.com/#/schema/itemlist/abc123`

请查阅相关[片段文档](/features/schema/pieces)以了解正确的结构（如果已定义）。

#### 异常情况
* _images_ (`ImageObject`) 的 ID 应为图像文件的完整绝对 URL（例如：`https://www.example.com/images/cat.jpg?size=large`）。

**注意：**
* `website` 是网站的 `协议` + `主机名`（例如：`https://www.example.com`）。
* `type` 应始终强制转换为小写，并移除所有空格/分隔符。

### `{{identifier}}` 组合规则
`{{identifier}}` 参数应始终按以下层级顺序构建：

* 全局唯一且稳定的系统 ID（例如对象的数据库 ID）
* 根据上下文合成的全局唯一稳定 ID（例如 `abc123-3` 表示与 ID 为 `abc123` 的 `product` 关联的第三个 `offer`）
* 实体所在 `WebPage` 内的局部顺序整数（例如页面中第四个 `itemlist` 使用 `4`）

### Exceptions
These are also documented in their various [pieces documentation](/features/schema/pieces), but bear repeating here.

* The ID of a `WebPage` should always be the unmodified canonical URL of the page (i.e. the *permalink*).
* The `{{identifier}}` fragment of the `Organization` which represents the site should always be `1` (e.g., https://www.example.com/#/schema/organization/1).
* The `{{identifier}}` fragment of the `WebSite` which represents "this site" should always be `1` (e.g., https://www.example.com/#/schema/website/1).
* The `{{identifier}}` fragment of a `person` should always be obfuscated.

### 混合类型与复合 ID
当节点为混合类型（即 `@type` 为值数组，例如 `['Organization', 'Person']`）时，`{{type}}` 值应按字母顺序连接 `@type` 值，并用连字符分隔（例如 `organization-person`）。

## 主要实体
我们的模型假设每个 URL 都应代表一个*主要实体*——无论是组织、产品、博客文章（或博客文章集合）、个人还是其他*事物*。

我们始终致力于让该“主要实体”成为每个页面网络图谱的中心。这种心智模型与我们希望搜索引擎理解我们网络的方式高度契合；它使我们能够以如下方式阐述内容：“此*URL*代表一个`食谱`，它是`文章`的一部分，由`个人`撰写，位于`网页`上，而该网页属于`网站`的一部分，该网站由`组织`运营”。

本文档中的所有代码示例均体现了这一方法；我们通过使用`hasPart`、`isPartOf`、`mainEntityOfPage`及类似连接来构建实体间的定向关系。

## 代码碎片化与放置位置
有时无法将代码放置在 `<head>` 中，例如受架构限制，在构建 `<head>` 时可能无法访问必要的上下文。考虑到 `<body>` 内容有时可能包含需要反映在结构化数据中的内容或逻辑，我们也允许在页面末尾、闭合标签 `</body>` 之前输出代码。

由于我们使用 `@id` 属性来连接片段，从技术上讲，可以通过多个 `<script>` 标签和图结构，将代码拆分并分布在整个页面中，并按照本文档概述的方法通过 ID 简单交叉引用实体。还可以通过在页面其他位置创建具有相同 ID 的新引用来*扩展*现有片段。

我们通常建议系统开发者尽量避免这种碎片化（因为它会给本已复杂的系统引入脆弱性和混淆），但也认识到有时这是必要的。

事实上，当无法在 `<head>` 中计算和输出所有内容时，我们自己的某些解决方案也采用这种方法。例如，我们的 [Yoast WooCommerce SEO 插件](https://yoast.com/wordpress/plugins/yoast-woocommerce-seo/) 依赖于解析初始化期间不可用的产品信息，因此在页面页脚输出第二个 `<script>` 块，其中包含对页面图的补充（特别是产品和评论信息）。这个附加图会无缝拼接，形成一个连贯的整体。

## 混合类型
有时，一个对象可能兼具两种不同事物的特性。例如，一本书既可以是 `book` *又*可以是 `product`，并同时拥有两者的属性。它可能既有 `author` 属性，也有 `price` 属性。

添加多种*类型*能够提供更大的灵活性，并更精确地描述对象。

然而，在我们的方法中，我们谨慎使用混合类型，因为它们可能会模糊特定页面的“焦点”。如果页面的主要实体（或 URL）是一个复杂的复合类型，我们就有可能偏离“一个 URL，一个事物”的模型。