---
title: "Yoast Seo Shopify"
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

本页面记录了 Yoast SEO for Shopify 应用输出的 [schema.org](https://schema.org/) 标记。

输出总体上与我们的[通用方法](https://developer.yoast.com/features/schema/overview)保持一致，但由于平台机制差异存在一些细微变化。

## 结构化数据和微数据移除

许多 Shopify 网站和主题包含模式"片段"；例如，孤立的微数据片段、断开的 JSON-LD 节点以及单薄或重复的实体描述。
为防止潜在的冲突和错误，**我们尝试移除主题模板中所有现有的结构化数据**（包括 JSON-LD 和微数据格式），当我们的应用被安装时。

大多数添加结构化数据功能（例如，评论、食谱等）的应用不应受到影响，前提是它们没有将其脚本直接放置在主题代码中。

## 核心逻辑

每个页面都应（尝试）输出以下部分：

- [组织](https://developer.yoast.com/features/schema/pieces/organization)
- [网站](https://developer.yoast.com/features/schema/pieces/website)
- [网页](https://developer.yoast.com/features/schema/pieces/webpage)

请注意，这取决于应用架构设置中的以下条件：
- 组织的名称和徽标已定义，并且；
- 相应的部分已启用。

当满足这些条件时，我们会生成[基础脚本](https://developer.yoast.com/features/schema/functional-specification)。在 Shopify 商店中，这通常看起来类似于以下内容（上述各部分的组合）：

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Organization",
            "@id": "https://www.example.com/#/schema/organization/1",
            "url": "https://www.example.com",
            "name": "Example organization name",
            "logo": {
                "@id": "https://www.example.com/#/schema/ImageObject/1234"
            },
            "image": [
                {
                    "@id": "https://www.example.com/#/schema/ImageObject/1234"
                }
            ],
            "sameAs": [
                "https://www.wikipedia.com/example-organization",
                "https://facebook.com/example-organization",
                "https://x.com/example-organization",
                "https://www.instagram.com/example-organization"
            ]
        },
        {
            "@type": "ImageObject",
            "@id": "https://www.example.com/#/schema/ImageObject/1234",
            "url": "https://www.example.com/content/images/image.jpg",
            "contentUrl": "https://www.example.com/content/images/image.jpg",
            "width": 520,
            "height": 240
        },
        {
            "@type": "WebSite",
            "@id": "https://www.example.com/#/schema/website/1",
            "url": "https://www.example.com",
            "name": "Example store",
            "potentialAction": {
                "@type": "SearchAction",
                "target": "https://www.example.com/search?q={search_term_string}",
                "query-input": "required name=search_term_string"
            },
            "inLanguage": "en",
            "publisher": {
                "@id": "https://www.example.com/#/schema/organization/1"
            }
        },
        {
            "@type": [
                "WebPage",
                "CollectionPage"
            ],
            "@id": "https://www.example.com/",
            "url": "https://www.example.com/",
            "name": "Example store",
            "about": {
                "@id": "https://www.example.com/#/schema/organization/1"
            },
            "breadcrumb": {
                "@id": "https://www.example.com/#/schema/breadcrumb"
            },
            "isPartOf": {
                "@id": "https://www.example.com/#/schema/website/1"
            }
        },
        {
            "@type": "BreadcrumbList",
            "@id": "https://www.example.com/#/schema/breadcrumb",
            "itemListElement": [
                {
                    "@type": "ListItem",
                    "name": "Example store",
                    "position": 1
                }
            ]
        }
    ]
}`}
</YoastSchemaExample>

## 在产品页面

- 将 [`WebPage`](docs/features/schema/pieces/webpage) 的 `@type` 更改为 `ItemPage`
- 添加一个 [`Product`](docs/features/schema/pieces/product) 片段（包含 [`offers`](docs/features/schema/pieces/offer) 属性）。

### 示例

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Product",
            "@id": "https://www.example.com/products/example-product/#/schema/Product",
            "mainEntityOfPage": {
                "@id": "https://www.example.com/products/example-product"
            },
            "name": "Example product",
            "image": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
            },
            "description": "Product description",
            "sku": "abc123",
            "url": "https://www.example.com/products/example-product",
            "offers": {
                "@type": "AggregateOffer",
                "@id": "https://www.example.com/products/example-product/#/schema/AggregateOffer",
                "availability": "https://schema.org/InStock",
                "offerCount": 1,
                "lowPrice": 10.0,
                "highPrice": 10.0,
                "priceCurrency": "EUR",
                "url": "https://www.example.com/products/example-product",
                "priceSpecification": {
                    "@type": "PriceSpecification",
                    "valueAddedTaxIncluded": true
                }
            }
        }
    ]
}`}
</YoastSchemaExample>

## 关于博客文章
- 添加 [`Article`](https://developer.yoast.com/features/schema/pieces/article) 片段