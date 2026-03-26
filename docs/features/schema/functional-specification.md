---
title: "Functional Specification"
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

我们方法的核心是输出一个“基础脚本”——一个以 `JSON-LD` 格式渲染的 `@graph` 对象——它描述了 `WebPage`、`WebSite` 和 `Organization`（对于代表个人的网站，则是 `Person`）。运行 [Yoast SEO 插件](https://yoast.com/product/yoast-seo-wordpress/) 的网站的每个页面都包含此脚本。

在任何特定页面上，该图谱可能会被修改和/或扩展，以反映网页的具体*类型*及其属性。在任何给定场景中，我们的目标是识别页面的“主要实体”，并发展图谱以表示此实体（参见“[主要实体](docs/features/schema/technology-approach#primary-entities)”）。

所有标记、属性和属性均直接取自 [schema.org](https://schema.org/)，并且（除以下*基础脚本*外）提供的所有代码仅用于演示目的。

## 基础脚本
以下是我们为每个页面构建的图的简化表示。

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
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-page/test/",
              "url": "https://www.example.com/example-page/test/",
              "name": "Example page name",
              "description": "Example page description",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              }
          }
      ]
}`}
</YoastSchemaExample>

这种方法与大多数 Schema 标记方法的不同之处在于，我们不是尝试构建一个复杂的“树”（即嵌套属性的数组），而是为每个独立的实体输出一个独立的、顶层的“片段”（技术术语称为“节点”）。这些片段包含在一个或多个 `@graph` 对象中，这使我们能够通过 ID 交叉引用片段。有关此方法的更多信息，请参阅“[探索 ID、关系和嵌套](docs/features/schema/technology-approach#ids-relationships-and-nesting)”。

请注意，在 Google 结构化数据测试工具中测试时，结果是一个单一的、连贯的图，其中以主要实体（本例中为 `WebPage`）为核心焦点。传统方法会显示每个独立的*片段*，但不会将这些片段“缝合”成一个单一的图。

## 示例图谱
以下示例展示了如何扩展和修改我们的基础脚本以支持不同场景。无论何种情况，我们都力求呈现网页的"主要实体"（及其属性），以及其与发布该页面的网站和组织（或个人）的关系。

### A company homepage
<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/1",
              "name": "Example Organization",
              "url": "https://www.example.com/",
              "sameAs": [
                  "https://www.facebook.com/example/",
                  "https://www.instagram.com/example/",
                  "https://www.linkedin.com/example/",
                  "https://www.youtube.com/example/",
                  "https://www.pinterest.com/example/",
                  "https://en.wikipedia.org/wikiexample/",
                  "https://x.com/example/"
              ],
              "logo": {
                  "@type": "ImageObject",
                  "@id": "https://www.example.com/images/example.jpg",
                  "url": "https://www.example.com/images/example.jpg",
                  "contentUrl": "https://www.example.com/images/example.jpg",
                  "width": 120,
                  "height": 120,
                  "caption": "Example Image"
              },
              "image": {
                  "@id": "https://www.example.com/images/example.jp"
              }
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com/",
              "name": "Example Website",
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/",
              "url": "https://www.example.com/",
              "inLanguage": "en-US",
              "name": "Example Page",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              },
              "about": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "datePublished": "2015-09-14T08:13:22+00:00",
              "dateModified": "2019-03-28T12:46:37+00:00",
              "description": "Example description"
          }
      ]
  }`}
</YoastSchemaExample>

### An article, with an author, on a company website
<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/1",
              "name": "Example Organization",
              "url": "https://www.example.com/",
              "sameAs": [
                  "https://www.facebook.com/example/",
                  "https://www.instagram.com/example/",
                  "https://www.linkedin.com/example/",
                  "https://www.youtube.com/example/",
                  "https://www.pinterest.com/example/",
                  "https://en.wikipedia.org/wikiexample/",
                  "https://x.com/example/"
              ],
              "logo": {
                  "@type": "ImageObject",
                  "@id": "https://www.example.com/images/example.jpg",
                  "url": "https://www.example.com/images/example.jpg",
                  "contentUrl": "https://www.example.com/images/example.jpg",
                  "width": 120,
                  "height": 120,
                  "caption": "Example Image"
              },
              "image": {
                  "@id": "https://www.example.com/images/example.jpg"
              }
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com/",
              "name": "Example Website",
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/blog/example-article/",
              "url": "https://www.example.com/blog/example-article/",
              "inLanguage": "en-US",
              "breadcrumb": {
                  "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123"
              },
              "name": "Example Page",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              },
              "datePublished": "2015-09-14T08:13:22+00:00",
              "dateModified": "2019-03-28T12:46:37+00:00",
              "description": "Example description"
          },
          {
              "@type": "Article",
              "@id": "https://www.example.com/#/schema/Article/abc123",
              "headline": "Example article headline",
              "description": "Example article description",
              "isPartOf": {
                  "@id": "https://www.example.com/blog/example-article/"
              },
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/blog/example-article/"
              },
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "commentCount": 6,
              "articleSection": ["Cats","Dogs","Recipes"],
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/images/example-2.jpg"
                  },
                  {
                      "@id": "https://www.example.com/images/example-3.jpg"
                  }
              ],
              "video": [
                  {
                      "@id": "https://www.example.com/#/schema/VideoObject/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/VideoObject/def456"
                  }
              ]
          },
          {
              "@type": "Person",
              "@id": "https://www.example.com/#/schema/Person/abc123",
              "name": "Example person name",
              "image": {
                  "@id": "https://www.example.com/images/example-4.jpg"
              },
              "sameAs": [
                  "https://www.wikipedia.com/example-organization",
                  "https://www.linkedin.com/company/1234"
              ]
          },
          {
              "@type": "BreadcrumbList",
              "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123",
              "itemListElement": [
                  {
                      "@type": "ListItem",
                      "position": 1,
                      "name": "Home",
                      "item": "https://www.example.com/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 2,
                      "name": "Blog",
                      "item": "https://www.example.com/blog/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 3,
                      "name": "Example Article"
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>

### WooCommerce 商店中的产品
请注意，实际上这是两个独立的图形块拼接而成，但解析结果相同。

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/1",
              "name": "Example Organization",
              "url": "https://www.example.com/",
              "sameAs": [
                  "https://www.facebook.com/example/",
                  "https://www.instagram.com/example/",
                  "https://www.linkedin.com/example/",
                  "https://www.youtube.com/example/",
                  "https://www.pinterest.com/example/",
                  "https://en.wikipedia.org/wikiexample/",
                  "https://x.com/example/"
              ],
              "logo": {
                  "@type": "ImageObject",
                  "@id": "https://www.example.com/images/example.jpg",
                  "url": "https://www.example.com/images/example.jpg",
                  "contentUrl": "https://www.example.com/images/example.jpg",
                  "width": 120,
                  "height": 120,
                  "caption": "Example Image"
              },
              "image": {
                  "@id": "https://www.example.com/images/example.jpg"
              }
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/WebSite/1",
              "url": "https://www.example.com/",
              "name": "Example Website",
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              }
          },
          {
              "@type": [
                  "WebPage",
                  "ItemPage"
              ],
              "@id": "https://www.example.com/products/example-page/",
              "url": "https://www.example.com/products/example-page/",
              "inLanguage": "en-US",
              "breadcrumb": {
                  "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123"
              },
              "name": "Example Product Page",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              },
              "datePublished": "2015-09-14T08:13:22+00:00",
              "dateModified": "2019-03-28T12:46:37+00:00",
              "description": "Example description"
          },
          {
              "@type": "BreadcrumbList",
              "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123",
              "itemListElement": [
                  {
                      "@type": "ListItem",
                      "position": 1,
                      "name": "Home",
                      "item": "https://www.example.com/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 2,
                      "name": "Products",
                      "item": "https://www.example.com/products/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 3,
                      "name": "Example Page"
                  }
              ]
          },
          {
              "@type": "Product",
              "@id": "https://www.example.com/#/schema/Product/abc123",
              "name": "Vneck Tshirt",
              "image": {
                  "@id": "https://www.example.com/images/example-2.jpg"
              },
              "description": "Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.",
              "sku": 83,
              "offers": [
                  {
                      "@type": "Offer",
                      "@id": "https://www.example.com/#/schema/Offer/abc123",
                      "priceSpecification": {
                          "@type": "PriceSpecification",
                          "price": "18.00",
                          "priceCurrency": "EUR",
                          "valueAddedTaxIncluded": "false"
                      },
                      "availability": "https://schema.org/InStock",
                      "url": "https://www.example.com/products/example-page/",
                      "seller": {
                          "@id": "https://www.example.com/#/schema/Organization/1"
                      }
                  }
              ],
              "aggregateRating": {
                  "@type": "AggregateRating",
                  "ratingValue": "4.50",
                  "reviewCount": 2
              },
              "review": [
                  {
                      "@type": "Review",
                      "@id": "https://www.example.com/#/schema/Review/abc123",
                      "datePublished": "2019-04-09T09:10:12+00:00",
                      "description": "What a nice turtle shirt.",
                      "reviewRating": {
                          "@type": "Rating",
                          "ratingValue": "4"
                      },
                      "author": {
                          "@type": "Person",
                          "name": "Tim"
                      }
                  },
                  {
                      "@type": "Review",
                      "@id": "https://www.example.com/#/schema/Review/def456",
                      "datePublished": "2019-04-09T09:10:29+00:00",
                      "description": "Awesome shirt",
                      "reviewRating": {
                          "@type": "Rating",
                          "ratingValue": "5"
                      },
                      "author": {
                          "@type": "Person",
                          "name": "Patrice"
                      }
                  }
              ],
              "brand": {
                  "@type": "Organization",
                  "name": "Turtle"
              },
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/products/example-page/"
              },
              "manufacturer": {
                  "@type": "Organization",
                  "name": "Turtle"
              }
          }
      ]
  }`}
</YoastSchemaExample>

### 其他示例
我们的[技术文档](docs/features/schema/technology-approach)包含更广泛和多样的示例，以及关于[Yoast SEO 软件](https://yoast.com/product/yoast-seo-wordpress/)如何确定在各种场景下输出内容的详细信息。

## 修改或扩展我们的图表
我们所有的输出都可以通过[完整 API](docs/features/schema/api)进行修改、扩展或禁用（*按部分*或整体）。

在第三方插件、主题或系统导致图表"解构"、属性重复/合并或 ID 空间共享的场景中，我们建议采用我们的框架并[利用我们的 API](docs/features/schema/api)（或鼓励相关解决方案作者这样做）。

## 在 Yoast 软件中的使用
本规范构成了自 [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) 11.0 版本起我们 [schema.org](http://schema.org/) / JSON-LD 输出的基础。关于我们的软件如何利用和扩展此规范的具体信息，请参阅[此处](docs/features/schema/plugins/yoast-seo)。

## 支持与反馈
本规范将持续完善，我们始终乐于协助他人采纳、扩展或改进。若您对本文所述机制存有疑问，或希望将规范应用于您的主题/插件，欢迎在[我们的 GitHub 仓库](https://github.com/Yoast/wordpress-seo/)中创建议题。

请别忘了我们还提供[技术文档](docs/features/schema/technology-approach)以及[用于修改结构化数据输出的 API](docs/features/schema/api)！

## 附录

### 1: JSON-LD 与其他标准的比较
表面上看，与其他方法（例如 RDFa 或 Microdata）提供的*内联*灵活性相比，JSON-LD 要求在模板中输出单一、静态的代码片段可能显得有限。然而，随着用例复杂性的增加，基于内联的方法很快暴露出缺乏我们所需的灵活性和可扩展性。具体来说：

*   它们与页面 HTML 标记的结构紧密绑定。这意味着很难表示那些属性与页面布局不完全匹配的结构和实体，并且通常表示元属性或链接数据需要内联隐藏属性和元标签。
*   它们在处理复杂关系方面表现不佳。如果*实体 A* 与*实体 B* 共享或继承属性，或是*实体 C* 的子项，没有简单的方法来表示这一点。也没有简单的方法来反转关系声明的方向。
*   并非所有实体都能轻易地用一个整洁的、能与模板 HTML 标记对齐的层次结构来表示。例如，将一个包含 `Article`（由某个 `Organization` 在 `WebSite` 上发布，并具有 `Author` 和其他属性）的 `WebPage` 的*主要内容*正确定位为网络图的明确中心，在使用线性的、嵌套的标记时并不总是容易的。这需要一定量的交叉链接和关系引用来从页面代码结构中抽象出来。
*   即使你能克服所有这些困难，也很难将模式普遍集成到所有需要它的网站独立模板中——企业和用户会使用各种模板、主题、标记、插件、编码技术和流程——这使得依赖维护有效的标记变得极其困难。至少在 WordPress 领域，我们无法保证主题和插件能够可靠、清晰地进行通信、集成和构建内联标记。

由于我们试图最大限度地提高灵活性和互操作性，在这种情况下，JSON-LD 显然是赢家。

#### 1a: 通过 CSS/XPath 选择避免内容重复
对 JSON-LD 格式的主要反对意见之一是它可能导致（大块）内容重复。例如，如果我想表示一篇*评论*，就必须在页面的“人类”可读内容中包含完整的评论内容，同时在 JSON 标记中也包含它（至少根据谷歌的结构化数据要求）。

这通常被视为性能问题，而且确实如此。即使启用并配置了 GZIP（我们不能假设总是如此），这也可能导致服务器响应和浏览器处理产生（尽管较小）开销。

未来，我们希望通过使用 [schema.org](http://schema.org) 的 [CssSelectorType](https://pending.schema.org/CssSelectorType) 或 [XPathType](https://pending.schema.org/XPathType) 标记来绕过此问题。这允许我们定位代表相关内容的特定容器（或容器的组合/数组），而不是重复文本。目前，这仅支持 `webPageElement` 和 `Speakable` 内容区域。

在这些定位方法获得更广泛支持之前，仅当搜索引擎和外部代理明确要求时才应重复内容。本文档反映了这些场景。

### 2: 标签管理解决方案与 JavaScript 注入的使用
我们更倾向于在服务器端包含和渲染 JSON-LD 标记，而非通过标签管理解决方案或 JavaScript 文件/函数注入。此外，只要可能，所有属性值都应“硬编码”到图谱中，而非引用或调用来自其他位置的函数或变量。

爬取、处理和利用 JavaScript 注入值的计算成本远高于仅抓取 HTML，且更为复杂，因此许多平台对严重依赖 JavaScript 的方法仅提供部分（甚至不提供）支持。

依赖外部脚本和平台（如标签管理系统）的方法也可能因冲突、竞态条件、延迟和依赖管理而引入脆弱性。

### 3. 图像处理
在本文档的所有示例中，我们通常对图像做出以下几点假设：

* 尽管核心的 [schema.org](https://schema.org) 定义并不总是将图像列为内容的必需属性，但 Google *确实* 经常要求几乎所有内容类型都包含图像（即，要获得其“富媒体摘要”及类似体验的资格，页面、博客文章、产品和其他内容实体几乎总是需要至少一张图像）。**请假设，凡是我们包含了图像参数的地方，都应被视为是强制性的。**
* 所有图像属性都应注册为 `imageObject` 实体的数组，以便在可行时设置高级属性（如标题），并能够通过 ID 在内容之间继承/共享图像。这便于在相关内容之间共享图像（例如，`blogPosting` 的主图/特色图通常很可能与博客文章所在页面的 `primaryImageOfPage` 是同一实体）。
* 尺寸和格式限制因代理而异，但应遵循常识。

### 4. 使用规范 URL
`url` 属性应始终继承 JSON 片段所在页面的“真实规范”值（即，即使规范 URL 标签已手动设置为引用其他页面/URL，仍应使用原始的“真实”规范值）。

例如，如果*页面 A* 的 URL 为 [https://www.example.com/page-a/](https://www.example.com/page-a/)，但其包含的规范 URL 标签引用了*页面 B*（位于 [https://www.example.com/page-b/](https://www.example.com/page-b/)），则所有通常与*页面 A* 相关的 `url` 参数仍应使用*页面 A* 的“真实”规范 URL。

### 5. 验证工具
在测试、评估和调试 schema 时，我们推荐使用以下三种工具。

- [官方 schema.org 验证器](https://validator.schema.org/) 用于检查结构和语法错误。
- [Google 富媒体搜索结果测试](https://search.google.com/test/rich-results)（或称 'SDTT'）验证指定页面是否有资格在 Google 中显示特定类型的富媒体搜索结果。
- [Classy Schema](https://classyschema.org/Visualisation) 可视化页面的图谱，以确保其结构连贯一致。

### 6. 已知问题
SDTT 在许多场景下与 schema.org 的定义存在偏差。在某些情况下，我们调整或折衷了方法以找到适用于双方的解决方案；在其他情况下，我们根据具体情境偏向其中一方。

例如，SDTT 要求 `recipe` 必须包含 `image` 属性。根据 [schema.org 食谱规范](https://schema.org/Recipe)，这并非强制属性，但 Google 有此要求。类似情况还有很多，SDTT 揭示了 Google 特有的特殊要求和规则，这些要求要么是为满足其需求而对标准进行刻意"调整"的产物，要么是相对随意的决策结果。

以下是我们方法导致冲突和问题的具体场景，我们正在请求 Google 调整其对我们标记的解释和处理方式。

#### `Person` 不能作为 `Article` 的 `Publisher`
这是一个特别具有挑战性的问题，因为代表 `Person` 的 `WebSite`（与代表 `Organization` 的网站相对，即个人网站）自然会“发布”文章，而该 `Person` 应被视为“发布者”。这是一个极其常见的用例，但各种 Google 工具和标准却将其标记为无效。

此外，我们图方法的一个关键部分依赖于识别 `WebPage`（或 `Article`）与其所在 `WebSite` 之间的连接。`Publisher` 是这些实体之间的关键连接，因此，**我们选择在这种情况下忽略该错误**（但已向 Google 发出警报）。

为了解决这个问题，我们将 `Person` 与 `Organization` 合并，创建一个混合类型（`[Person, Organization]`），然后该类型期望/接受 `logo` 和其他“Publisher”属性。这在 SDTT 中验证通过，但这是一个公认的“变通方案”。

### 7. 其他消费者
在发布时，Bing 似乎不支持此方法；其“标记验证器”工具（[Bing Webmaster Tools](https://www.bing.com/toolbox/webmaster) 的一部分）无法检测（和/或解析）包含在 `@graph` 结构中的标记（这是我们方法的核心）。我们正寻求与 Bing 进行对话，以确定他们对支持此方法的立场。

Facebook、X、Pinterest 等社交平台对此标记的支持程度各不相同。大多数依赖 *Open Graph* 标记（'OG 标签'）及类似方案，但在 OG 标签缺失或无效时，可能会使用 schema.org 标记的组成部分。

其他搜索引擎（如百度、Yandex 等）的支持情况未知；我们推测其支持通常有限或不存在。我们希望我们方法的广泛采用将鼓励这些及其他消费者扩展其支持。