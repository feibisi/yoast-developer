---
title: "Rest Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Apis
    - Customization
    - Repos
---


:::caution

下文描述的 'raw' <code>json</code> 数据格式仅在 Yoast SEO 16.7 及以上版本中可用。

:::

Yoast SEO 包含一个 REST API，可返回站点上任意 URL 的所有 SEO 元数据（元标签、schema.org 数据等）。这使得无头 WordPress 站点能够非常便捷地利用 Yoast SEO 来管理其所有 SEO 事宜。

## Yoast SEO REST API 语法
您可以通过两种方式获取页面或 URL 的 SEO 元数据：使用 [WordPress 原生 WP REST API](https://developer.wordpress.org/rest-api/reference/) 查询对象，或查询特定 URL。

### 1. 将 SEO 元数据作为 WordPress 原生 WP REST API 响应的一部分
许多无头 WordPress 实现利用 WordPress 原生的 WP REST API 来获取特定文章或对象的数据。

通常，这涉及向类似 `https://yoast.com/wp-json/wp/v2/posts/607`（通过 `ID` 获取特定文章）、`https://yoast.com/wp-json/wp/v2/posts?slug=wordpress-seo`（通过 `slug` 获取特定文章）或 `https://yoast.com/wp-json/wp/v2/types/yoast_events`（特定自定义文章类型归档）* 的 URL 发出请求。

*请注意，请求*自定义文章归档*应使用文章类型*名称*，这可能与归档的*slug*不同（例如，`yoast_events` 对比 `/events/`）。*

当 Yoast SEO 激活时，我们会在这些响应中附加*两个*额外字段：`yoast_head` 和 `yoast_head_json`。

### 2. 查询特定 URL
在某些情况下，查询特定 URL（而非对象标识符）可能更简便或更可取。您可以通过向 `/wp-json/yoast/v1/get_head?url=` 发送 `GET` 请求来实现，适用于任何 URL。例如：

```
https://yoast.com/wp-json/yoast/v1/get_head?url=https://yoast.com/wordpress-seo/
```

此操作将查询 URL `https://yoast.com/wordpress-seo/`，并返回以下 JSON 属性：`html`、`json` 和 `status`。

## 理解和使用我们的数据
在上述两种方法中，我们以两种格式返回 SEO 数据。

1. `yoast_head`/`html` 属性包含一个经过转义的、*预制的*数据块，其中包含该页面所需的所有元标签（以及 schema.org 输出）。这与我们通常在文章或页面的 `<head>` 中输出的内容相同。

2. `yoast_head_json`/`json` 属性提供*原始数据*，作为每个标签的一系列键/值对（以及更复杂的对象）。

`status` 属性返回一个 HTTP 状态码（通常是 `200` 或 `404`）。

*原始*数据中返回的许多属性被分解为特定的字段和值。您可以使用这些来查询特定属性，和/或构建您自己的响应。
例如，`og_image` 属性包含 `width`、`height`、`size`、`path`、`alt`、`pixels` 和各种其他属性，而不仅仅是图像的 URL。

我们建议尽可能使用预制方法，因为它利用我们的内部逻辑来保证标签和值的最佳组合。
在使用*原始*数据时，我们建议使用*预制*数据作为参考，以确保您的实现是正确的。

## 修改输出
我们所有的元数据都可以通过[元数据 API](https://developer.yoast.com/customization/apis/metadata-api)进行自定义。
通过筛选每个标签对应的*呈现器*，可以修改*预制*和*原始*两种响应。

## 常见问题

### 我能使用此 API *更新* 数据吗？
Yoast REST API 目前是只读的，尚不支持通过 `POST` 或 `PUT` 调用来更新数据。

### 为什么 API 对存在的页面返回 404 状态？
在某些情况下，您可能会发现 API 对存在的文章、资源或 URL 返回 `404` 状态。

这通常发生在资源数据未在我们的 *可索引* 系统中可用时。通常可以通过以下任一方式解决：

- 在 WordPress 管理界面中保存/更新该对象，或；
- 使用 Yoast SEO *工具* 菜单中的“优化 SEO 数据”功能。

请注意，Yoast SEO 也会为非 200 状态场景（包括合法的 `404` 错误）返回完整响应，以便您能有效“优化”这些场景。

### Can I disable the API?
You can easily disable this API by going to the *Site features* section of your Yoast SEO *Settings* menu, and disabling the feature toggle.

## Example output
The following code is the result of a query to https://yoast.com/wp-json/yoast/v1/get_head?url=https://yoast.com/site-structure-the-ultimate-guide/. It contains the raw `html`, the structured `json`, and a `status` (of `200`).

```json
{
    "html": "<!-- This site is optimized with the Yoast SEO Premium plugin v23.5-RC1 (Yoast SEO v23.5-RC1) - https://yoast.com/product/yoast-seo-premium-wordpress/ -->\n<title>Site structure: the ultimate guide &#8226; Yoast</title>\n<meta name=\"description\" content=\"Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"robots\" content=\"index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1\" />\n<link rel=\"canonical\" href=\"https://yoast.com/site-structure-the-ultimate-guide/\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:locale\" content=\"en_US\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:type\" content=\"article\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:title\" content=\"Site structure: the ultimate guide\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:description\" content=\"Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:url\" content=\"https://yoast.com/site-structure-the-ultimate-guide/\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:site_name\" content=\"Yoast\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"article:publisher\" content=\"https://www.facebook.com/yoast\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"article:author\" content=\"https://www.facebook.com/mariekerakt\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"article:published_time\" content=\"2023-05-03T11:35:45+00:00\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"article:modified_time\" content=\"2024-08-21T08:37:24+00:00\" class=\"yoast-seo-meta-tag\" />\n<meta property=\"og:image\" content=\"https://yoast.com/app/uploads/2020/08/tuesday.png\" class=\"yoast-seo-meta-tag\" />\n\t<meta property=\"og:image:width\" content=\"1200\" class=\"yoast-seo-meta-tag\" />\n\t<meta property=\"og:image:height\" content=\"675\" class=\"yoast-seo-meta-tag\" />\n\t<meta property=\"og:image:type\" content=\"image/png\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"author\" content=\"Marieke van de Rakt\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"twitter:card\" content=\"summary_large_image\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"twitter:creator\" content=\"@mariekerakt\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"twitter:site\" content=\"@yoast\" class=\"yoast-seo-meta-tag\" />\n<meta name=\"twitter:label1\" content=\"Written by\" class=\"yoast-seo-meta-tag\" />\n\t<meta name=\"twitter:data1\" content=\"Marieke van de Rakt\" class=\"yoast-seo-meta-tag\" />\n\t<meta name=\"twitter:label2\" content=\"Est. reading time\" class=\"yoast-seo-meta-tag\" />\n\t<meta name=\"twitter:data2\" content=\"19 minutes\" class=\"yoast-seo-meta-tag\" />\n<script type=\"application/ld+json\" class=\"yoast-schema-graph\">{\"@context\":\"https://schema.org\",\"@graph\":[{\"@type\":\"Article\",\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#article\",\"isPartOf\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/\"},\"author\":{\"name\":\"Marieke van de Rakt\",\"@id\":\"https://yoast.com/#/schema/person/1159f46828cf5ce5fa8e3b89a1af9327\"},\"headline\":\"Site structure: the ultimate guide\",\"datePublished\":\"2023-05-03T11:35:45+00:00\",\"dateModified\":\"2024-08-21T08:37:24+00:00\",\"mainEntityOfPage\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/\"},\"wordCount\":3920,\"commentCount\":2,\"publisher\":{\"@id\":\"https://yoast.com/#organization\"},\"image\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#primaryimage\"},\"thumbnailUrl\":\"https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg\",\"keywords\":[\"Internal linking\",\"Site structure\"],\"inLanguage\":\"en-US\",\"potentialAction\":[{\"@type\":\"CommentAction\",\"name\":\"Comment\",\"target\":[\"https://yoast.com/site-structure-the-ultimate-guide/#respond\"]}]},{\"@type\":\"WebPage\",\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/\",\"url\":\"https://yoast.com/site-structure-the-ultimate-guide/\",\"name\":\"Site structure: the ultimate guide &#8226; Yoast\",\"isPartOf\":{\"@id\":\"https://yoast.com/#website\"},\"primaryImageOfPage\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#primaryimage\"},\"image\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#primaryimage\"},\"thumbnailUrl\":\"https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg\",\"datePublished\":\"2023-05-03T11:35:45+00:00\",\"dateModified\":\"2024-08-21T08:37:24+00:00\",\"description\":\"Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.\",\"breadcrumb\":{\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#breadcrumb\"},\"inLanguage\":\"en-US\",\"potentialAction\":[{\"@type\":\"ReadAction\",\"target\":[\"https://yoast.com/site-structure-the-ultimate-guide/\"]}]},{\"@type\":\"ImageObject\",\"inLanguage\":\"en-US\",\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#primaryimage\",\"url\":\"https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg\",\"contentUrl\":\"https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg\",\"width\":2400,\"height\":1350},{\"@type\":\"BreadcrumbList\",\"@id\":\"https://yoast.com/site-structure-the-ultimate-guide/#breadcrumb\",\"itemListElement\":[{\"@type\":\"ListItem\",\"position\":1,\"name\":\"Home\",\"item\":\"https://yoast.com/\"},{\"@type\":\"ListItem\",\"position\":2,\"name\":\"Site structure\",\"item\":\"https://yoast.com/tag/site-structure/\"},{\"@type\":\"ListItem\",\"position\":3,\"name\":\"Site structure: the ultimate guide\"}]},{\"@type\":\"WebSite\",\"@id\":\"https://yoast.com/#website\",\"url\":\"https://yoast.com/\",\"name\":\"Yoast\",\"description\":\"SEO for everyone\",\"publisher\":{\"@id\":\"https://yoast.com/#organization\"},\"potentialAction\":[{\"@type\":\"SearchAction\",\"target\":{\"@type\":\"EntryPoint\",\"urlTemplate\":\"https://yoast.com/?s={search_term_string}\"},\"query-input\":{\"@type\":\"PropertyValueSpecification\",\"valueRequired\":true,\"valueName\":\"search_term_string\"}}],\"inLanguage\":\"en-US\",\"copyrightHolder\":{\"@id\":\"https://yoast.com/#organization\"}},{\"@type\":[\"Organization\",\"Brand\"],\"@id\":\"https://yoast.com/#organization\",\"name\":\"Yoast\",\"url\":\"https://yoast.com/\",\"logo\":{\"@type\":\"ImageObject\",\"inLanguage\":\"en-US\",\"@id\":\"https://yoast.com/#/schema/logo/image/\",\"url\":\"https://yoast.com/app/uploads/2020/09/Yoast_Icon_SocialMedia_500x500.png\",\"contentUrl\":\"https://yoast.com/app/uploads/2020/09/Yoast_Icon_SocialMedia_500x500.png\",\"width\":500,\"height\":500,\"caption\":\"Yoast\"},\"image\":{\"@id\":\"https://yoast.com/#/schema/logo/image/\"},\"sameAs\":[\"https://www.facebook.com/yoast\",\"https://x.com/yoast\",\"https://www.instagram.com/yoast/\",\"https://www.linkedin.com/company/1414157/\",\"https://www.pinterest.com/yoast/\",\"https://www.youtube.com/yoast\",\"https://en.wikipedia.org/wiki/Yoast\",\"https://g.co/kgs/4H5sG2\",\"https://g.co/kgs/a9XfUu\"],\"numberOfEmployees\":116,\"founder\":{\"@type\":\"Person\",\"name\":\"Joost de Valk\",\"sameAs\":\"https://en.wikipedia.org/wiki/Joost_de_Valk\"},\"foundingDate\":\"2010-05-01\",\"slogan\":\"SEO for Everyone\",\"description\":\"Yoast helps you with your website optimization, whether it be through our widely used SEO software or our online SEO courses: we&#039;re here to help.\",\"legalName\":\"Yoast BV\",\"parentOrganization\":{\"@type\":\"Organization\",\"name\":\"Newfold Digital\",\"description\":\"Newfold Digital is a leading web presence solutions provider serving millions of small-to-medium businesses globally.\",\"url\":\"https://newfold.com/\",\"sameAs\":[\"https://newfold.com/\"],\"logo\":\"https://yoast.com/app/uploads/2022/01/newfold-logo.png\"},\"memberOf\":{\"@type\":\"Organization\",\"name\":\"World Wide Web Consortium (W3C)\",\"description\":\"The World Wide Web Consortium (W3C) is an international community that develops open standards to ensure the long-term growth of the Web.\",\"url\":\"https://w3.org/\",\"sameAs\":[\"https://w3.org/\"],\"logo\":\"https://www.w3.org/Icons/w3c_main.png\"}},{\"@type\":\"Person\",\"@id\":\"https://yoast.com/#/schema/person/1159f46828cf5ce5fa8e3b89a1af9327\",\"name\":\"Marieke van de Rakt\",\"image\":{\"@type\":\"ImageObject\",\"inLanguage\":\"en-US\",\"@id\":\"https://yoast.com/#/schema/person/image/\",\"url\":\"https://secure.gravatar.com/avatar/1d83533e299c379140f9fcc2cb0015cb?s=96&d=mm&r=g\",\"contentUrl\":\"https://secure.gravatar.com/avatar/1d83533e299c379140f9fcc2cb0015cb?s=96&d=mm&r=g\",\"caption\":\"Marieke van de Rakt\"},\"description\":\"Marieke was Head of Strategy and former CEO at Yoast. During her time at our company she wrote a lot. Topics that she loves writing about include SEO copywriting, site structure and readability.\",\"sameAs\":[\"https://yoast.com/\",\"https://www.facebook.com/mariekerakt\",\"https://www.instagram.com/mgarakt/\",\"https://www.linkedin.com/in/mariekerakt\",\"https://x.com/mariekerakt\",\"https://marieke.blog/\",\"https://profiles.wordpress.org/mariekerakt/\",\"https://yoast.com/about-us/team/marieke-van-de-rakt/\"],\"honorificSuffix\":\"PhD\",\"gender\":\"female\",\"jobTitle\":\"Head of Strategy\",\"worksFor\":{\"@id\":\"https://yoast.com/#organization\"}}]}</script>\n<!-- / Yoast SEO Premium plugin. -->",
    "json": {
        "title": "Site structure: the ultimate guide &#8226; Yoast",
        "description": "Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.",
        "robots": {
            "index": "index",
            "follow": "follow",
            "max-snippet": "max-snippet:-1",
            "max-image-preview": "max-image-preview:large",
            "max-video-preview": "max-video-preview:-1"
        },
        "canonical": "https://yoast.com/site-structure-the-ultimate-guide/",
        "og_locale": "en_US",
        "og_type": "article",
        "og_title": "Site structure: the ultimate guide",
        "og_description": "Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.",
        "og_url": "https://yoast.com/site-structure-the-ultimate-guide/",
        "og_site_name": "Yoast",
        "article_publisher": "https://www.facebook.com/yoast",
        "article_author": "https://www.facebook.com/mariekerakt",
        "article_published_time": "2023-05-03T11:35:45+00:00",
        "article_modified_time": "2024-08-21T08:37:24+00:00",
        "og_image": [
            {
                "width": 1200,
                "height": 675,
                "url": "https://yoast.com/app/uploads/2020/08/tuesday.png",
                "type": "image/png"
            }
        ],
        "author": "Marieke van de Rakt",
        "twitter_card": "summary_large_image",
        "twitter_creator": "@mariekerakt",
        "twitter_site": "@yoast",
        "twitter_misc": {
            "Written by": "Marieke van de Rakt",
            "Est. reading time": "19 minutes"
        },
        "schema": {
            "@context": "https://schema.org",
            "@graph": [
                {
                    "@type": "Article",
                    "@id": "https://yoast.com/site-structure-the-ultimate-guide/#article",
                    "isPartOf": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/"
                    },
                    "author": {
                        "name": "Marieke van de Rakt",
                        "@id": "https://yoast.com/#/schema/person/1159f46828cf5ce5fa8e3b89a1af9327"
                    },
                    "headline": "Site structure: the ultimate guide",
                    "datePublished": "2023-05-03T11:35:45+00:00",
                    "dateModified": "2024-08-21T08:37:24+00:00",
                    "mainEntityOfPage": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/"
                    },
                    "wordCount": 3920,
                    "commentCount": 2,
                    "publisher": {
                        "@id": "https://yoast.com/#organization"
                    },
                    "image": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/#primaryimage"
                    },
                    "thumbnailUrl": "https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg",
                    "keywords": [
                        "Internal linking",
                        "Site structure"
                    ],
                    "inLanguage": "en-US",
                    "potentialAction": [
                        {
                            "@type": "CommentAction",
                            "name": "Comment",
                            "target": [
                                "https://yoast.com/site-structure-the-ultimate-guide/#respond"
                            ]
                        }
                    ]
                },
                {
                    "@type": "WebPage",
                    "@id": "https://yoast.com/site-structure-the-ultimate-guide/",
                    "url": "https://yoast.com/site-structure-the-ultimate-guide/",
                    "name": "Site structure: the ultimate guide &#8226; Yoast",
                    "isPartOf": {
                        "@id": "https://yoast.com/#website"
                    },
                    "primaryImageOfPage": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/#primaryimage"
                    },
                    "image": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/#primaryimage"
                    },
                    "thumbnailUrl": "https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg",
                    "datePublished": "2023-05-03T11:35:45+00:00",
                    "dateModified": "2024-08-21T08:37:24+00:00",
                    "description": "Your site structure is vital for users and SEO. Our complete guide will guide you through all the steps to create a sound site structure.",
                    "breadcrumb": {
                        "@id": "https://yoast.com/site-structure-the-ultimate-guide/#breadcrumb"
                    },
                    "inLanguage": "en-US",
                    "potentialAction": [
                        {
                            "@type": "ReadAction",
                            "target": [
                                "https://yoast.com/site-structure-the-ultimate-guide/"
                            ]
                        }
                    ]
                },
                {
                    "@type": "ImageObject",
                    "inLanguage": "en-US",
                    "@id": "https://yoast.com/site-structure-the-ultimate-guide/#primaryimage",
                    "url": "https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg",
                    "contentUrl": "https://yoast.com/app/uploads/2020/08/site_structure_ultimate_guide_2400x1350.jpg",
                    "width": 2400,
                    "height": 1350
                },
                {
                    "@type": "BreadcrumbList",
                    "@id": "https://yoast.com/site-structure-the-ultimate-guide/#breadcrumb",
                    "itemListElement": [
                        {
                            "@type": "ListItem",
                            "position": 1,
                            "name": "Home",
                            "item": "https://yoast.com/"
                        },
                        {
                            "@type": "ListItem",
                            "position": 2,
                            "name": "Site structure",
                            "item": "https://yoast.com/tag/site-structure/"
                        },
                        {
                            "@type": "ListItem",
                            "position": 3,
                            "name": "Site structure: the ultimate guide"
                        }
                    ]
                },
                {
                    "@type": "WebSite",
                    "@id": "https://yoast.com/#website",
                    "url": "https://yoast.com/",
                    "name": "Yoast",
                    "description": "SEO for everyone",
                    "publisher": {
                        "@id": "https://yoast.com/#organization"
                    },
                    "potentialAction": [
                        {
                            "@type": "SearchAction",
                            "target": {
                                "@type": "EntryPoint",
                                "urlTemplate": "https://yoast.com/?s={search_term_string}"
                            },
                            "query-input": {
                                "@type": "PropertyValueSpecification",
                                "valueRequired": true,
                                "valueName": "search_term_string"
                            }
                        }
                    ],
                    "inLanguage": "en-US",
                    "copyrightHolder": {
                        "@id": "https://yoast.com/#organization"
                    }
                },
                {
                    "@type": [
                        "Organization",
                        "Brand"
                    ],
                    "@id": "https://yoast.com/#organization",
                    "name": "Yoast",
                    "url": "https://yoast.com/",
                    "logo": {
                        "@type": "ImageObject",
                        "inLanguage": "en-US",
                        "@id": "https://yoast.com/#/schema/logo/image/",
                        "url": "https://yoast.com/app/uploads/2020/09/Yoast_Icon_SocialMedia_500x500.png",
                        "contentUrl": "https://yoast.com/app/uploads/2020/09/Yoast_Icon_SocialMedia_500x500.png",
                        "width": 500,
                        "height": 500,
                        "caption": "Yoast"
                    },
                    "image": {
                        "@id": "https://yoast.com/#/schema/logo/image/"
                    },
                    "sameAs": [
                        "https://www.facebook.com/yoast",
                        "https://x.com/yoast",
                        "https://www.instagram.com/yoast/",
                        "https://www.linkedin.com/company/1414157/",
                        "https://www.pinterest.com/yoast/",
                        "https://www.youtube.com/yoast",
                        "https://en.wikipedia.org/wiki/Yoast",
                        "https://g.co/kgs/4H5sG2",
                        "https://g.co/kgs/a9XfUu"
                    ],
                    "numberOfEmployees": 116,
                    "founder": {
                        "@type": "Person",
                        "name": "Joost de Valk",
                        "sameAs": "https://en.wikipedia.org/wiki/Joost_de_Valk"
                    },
                    "foundingDate": "2010-05-01",
                    "slogan": "SEO for Everyone",
                    "description": "Yoast helps you with your website optimization, whether it be through our widely used SEO software or our online SEO courses: we&#039;re here to help.",
                    "legalName": "Yoast BV",
                    "parentOrganization": {
                        "@type": "Organization",
                        "name": "Newfold Digital",
                        "description": "Newfold Digital is a leading web presence solutions provider serving millions of small-to-medium businesses globally.",
                        "url": "https://newfold.com/",
                        "sameAs": [
                            "https://newfold.com/"
                        ],
                        "logo": "https://yoast.com/app/uploads/2022/01/newfold-logo.png"
                    },
                    "memberOf": {
                        "@type": "Organization",
                        "name": "World Wide Web Consortium (W3C)",
                        "description": "The World Wide Web Consortium (W3C) is an international community that develops open standards to ensure the long-term growth of the Web.",
                        "url": "https://w3.org/",
                        "sameAs": [
                            "https://w3.org/"
                        ],
                        "logo": "https://www.w3.org/Icons/w3c_main.png"
                    }
                },
                {
                    "@type": "Person",
                    "@id": "https://yoast.com/#/schema/person/1159f46828cf5ce5fa8e3b89a1af9327",
                    "name": "Marieke van de Rakt",
                    "image": {
                        "@type": "ImageObject",
                        "inLanguage": "en-US",
                        "@id": "https://yoast.com/#/schema/person/image/",
                        "url": "https://secure.gravatar.com/avatar/1d83533e299c379140f9fcc2cb0015cb?s=96&d=mm&r=g",
                        "contentUrl": "https://secure.gravatar.com/avatar/1d83533e299c379140f9fcc2cb0015cb?s=96&d=mm&r=g",
                        "caption": "Marieke van de Rakt"
                    },
                    "description": "Marieke was Head of Strategy and former CEO at Yoast. During her time at our company she wrote a lot. Topics that she loves writing about include SEO copywriting, site structure and readability.",
                    "sameAs": [
                        "https://yoast.com/",
                        "https://www.facebook.com/mariekerakt",
                        "https://www.instagram.com/mgarakt/",
                        "https://www.linkedin.com/in/mariekerakt",
                        "https://x.com/mariekerakt",
                        "https://marieke.blog/",
                        "https://profiles.wordpress.org/mariekerakt/",
                        "https://yoast.com/about-us/team/marieke-van-de-rakt/"
                    ],
                    "honorificSuffix": "PhD",
                    "gender": "female",
                    "jobTitle": "Head of Strategy",
                    "worksFor": {
                        "@id": "https://yoast.com/#organization"
                    }
                }
            ]
        }
    },
    "status": 200
}
```