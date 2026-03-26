---
title: "Yoast Seo"
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


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

本页面记录了 Yoast SEO 插件输出的 [schema.org](https://schema.org/) 标记。更多关于我们的 API、集成机制和方法论的信息，请参阅[概述](docs/features)。

## 核心逻辑
每个页面都应（尝试）输出以下[片段](/features/schema/pieces)：
* [Organization](/features/schema/pieces/organization)
* [WebSite](/features/schema/pieces/website)
* [WebPage](/features/schema/pieces/webpage)

这将生成一个“基础脚本”，通常类似于以下内容（上述*片段*的组合）：

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/organization/1",
              "url": "https://www.example.com/#/schema/organization/",
              "name": "Example organization name",
              "sameAs": [
                  "https://www.wikipedia.com/example-organization",
                  "https://www.linkedin.com/company/1234"
              ],
              "logo": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ]
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/website/1",
              "url": "https://www.example.com/",
              "name": "Example website",
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-page/test/",
              "url": "https://www.example.com/example-page/test/",
              "name": "Example page name",
              "description": "Example page description",
              "keywords": "cats,dogs,cake",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/website/1"
              },
              "inLanguage": "en-US",
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "breadcrumb": {
                  "@id": "https://www.example.com/example-page/#/schema/breadcrumb/abc123"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ]
          },
          {
              "@type": "BreadcrumbList",
              "@id": "https://www.example.com/example-page/#/schema/breadcrumb/abc123",
              "itemListElement": [
                  {
                      "@type": "ListItem",
                      "position": 1,
                      "item": {
                          "@type": "WebPage",
                          "@id": "https://www.example.com/",
                          "url": "https://www.example.com/",
                          "name": "Home"
                      }
                  },
                  {
                      "@type": "ListItem",
                      "position": 2,
                      "item": {
                          "@id": "https://www.example.com/example-page/test/"
                      }
                  }
              ]
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/abc123",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/def456",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          }
      ]
  }`}
</YoastSchemaExample>

## 其他场景
在以下场景中，上述基础脚本应进行修改或扩展。

### 当网站代表个人时
* 向 `Organization` 添加一个额外的 `@type` 值 `Person`。
* 将 `Person` 的属性合并到 `Organization` 的属性中。

### 针对单篇文章
* 添加一个[文章](docs/features/schema/pieces/article)片段，并将其连接到 `WebPage`（通过 `mainEntityOfPage` 属性）。

### 当页面包含 HowTo 块时
* 添加一个[HowTo](docs/features/schema/pieces/howto)片段，并将其连接到 `WebPage`——或者，如果存在 `Article`，则连接到 `Article`（通过 `mainEntityOfPage` 属性）。

### 当页面包含常见问题解答区块时
* 向 `WebPage` 额外添加一个 `@type` 值为 `FAQPage`
* 向 `WebPage` 添加 `mainEntity` 属性，引用每个 `question` 节点的 ID（以数组形式）

## 更多信息
* [Article Schema 片段](docs/features/schema/pieces/article)
* [BreadcrumbList Schema 片段](docs/features/schema/pieces/breadcrumb)
* [HowTo Schema 片段](docs/features/schema/pieces/howto)
* [ImageObject Schema 片段](docs/features/schema/pieces/image)
* [Organization Schema 片段](docs/features/schema/pieces/organization)
* [Person Schema 片段](docs/features/schema/pieces/person)
* [Question Schema 片段](docs/features/schema/pieces/question)
* [WebSite Schema 片段](docs/features/schema/pieces/website)
* [WebPage Schema 片段](docs/features/schema/pieces/webpage)