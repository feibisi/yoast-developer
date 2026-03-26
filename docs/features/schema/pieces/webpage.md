---
title: "Webpage"
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

描述 [`WebSite`](docs/features/schema/pieces/website) 上的单个页面。作为子页面元素（如 [`Article`](docs/features/schema/pieces/article)）的容器。

充当页面内容到父级 `WebSite`（进而到 [`Organization`](docs/features/schema/pieces/organization)）的连接器。

如果满足所需条件，可转换为更具体的类型（如 `FAQPage`）。

## 触发器
应在所有公开页面上输出，`4xx` 和 `5xx` 系列错误页面/场景除外。

## 必需属性
有效的 `WebPage` 必须包含以下属性。

* `@type`: `WebPage`。
* `@id`: 页面的原始*规范 URL*。
* `URL`: 页面的原始*规范 URL*。
* `name`: 页面的*标题*。
* `isPartOf`: 指向 `WebSite` 节点的 ID 引用。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

## 可选属性
以下属性应在可用且有效时添加：

* `description`：页面的元描述内容。
* `inLanguage`：页面的语言代码；例如，`en-GB`。
* `datePublished`：页面最初发布时间，采用 ISO 8601 格式；例如，`2015-10-31T16:10:29+00:00`。
* `dateModified`：页面最后修改时间，采用 ISO 8601 格式；例如，`2015-10-31T16:10:29+00:00`。
* `primaryImageOfPage`：通过 ID 引用代表页面特色图片的节点。
* `breadcrumb`：通过 ID 引用代表[页面面包屑结构](https://developer.yoast.com/features/schema/pieces/breadcrumb/)的节点。
* `image`：页面内容中所有图片的数组，通过 ID 引用（包括由 `primaryImageOfPage` 引用的图片）。
* `video`：页面内容中所有视频的数组，通过 ID 引用。
* `keywords`：附加到页面的标签名称数组（例如，`["cats","dogs","cake"]`）。
* `speakable`：一个 `SpeakableSpecification` 对象，用于标识适合语音播报结果的内容元素。

## 条件属性
仅在满足特定条件时才应输出的可选属性。

### 当页面为常规*页面*时（而非文章归档、用户资料等）
* `potentialAction`: 包含以下值的 `ReadAction` 对象：
  * `target`: 页面未经修改的*规范 URL*。

### 当页面被*创作*时
* `author`: 页面作者的 ID 引用。仅当页面被明确创作时才应输出（例如，在 WordPress 中包含原生*文章*的页面上）。

### 当页面有评论，且页面*不*包含有效文章时
* `comment`：一个按 ID 引用的数组，包含与页面关联的评论。
* `commentCount`：一个整数值，表示与页面关联的评论总数。

### 当页面为主页时
* 添加一个额外的 `about` 属性，该属性引用网站的主要实体（通常是一个 `Organization` 或 `Person`，具体取决于用户配置）。
* 如果页面没有明确定义的特色图片，则将 `primaryImageOfPage` 设置为（代表）网站徽标的 `imageObject`。

## 转换
`WebPage` 类型可能在以下场景中发生转换。

### 分类索引
当查询返回文章循环时（例如分类归档、博客首页或其他分类索引），则应将 *type* 属性更改为 `CollectionPage`。

### 个人资料页
当页面涉及特定用户时（例如：成员简介/作者归档）：

* 应将 `type` 属性更改为 `ProfilePage`。
  * 若页面展示该 `Person` 创作的文章/页面，则 `type` 属性应改为数组 `['CollectionPage','ProfilePage']`。
* 为页面相关的 `Person` 添加 `mainEntityOfPage` 属性，通过 ID 引用 `WebPage`。

### 常见问题页面
当页面包含常见问题时，应将 `type` 属性转换为包含 `FAQPage` 和 `WebPage` 的数组（或按上文所述采用最具体的转换方式）。

### 搜索结果页面
在搜索结果页面上，应将 *type* 属性修改为 `[CollectionPage, SearchResultsPage]` 数组。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-page/",
              "url": "https://www.example.com/example-page/",
              "name": "Example page name",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              }
          }
      ]
  }`}
</YoastSchemaExample>

### Extended criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-page/",
              "url": "https://www.example.com/example-page/",
              "name": "Example page name",
              "description": "Example page description",
              "keywords": ["cats","dogs","cake"],
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/WebSite/1"
              },
              "inLanguage": "en-US",
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "breadcrumb": {
                  "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123"
              },
              "primaryImageOfPage": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/uploads/example-image.jpg"
                  },
                  {
                      "@id": "https://www.example.com/uploads/example-image-2.jpg"
                  }
              ],
              "video": [
                  {
                      "@id": "https://www.example.com/#/schema/VideoObject/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/VideoObject/def456"
                  }
              ],
              "potentialAction": [
                  {
                      "@type": "ReadAction",
                      "target": [ "https://www.example.com/example-page/" ]
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API：修改网页 Schema 输出 {#api}

要修改 Yoast SEO 输出的 `Webpage` Schema，你可以使用我们的 `wpseo_schema_webpage` 过滤器。以下是一个示例：

```php
add_filter( 'wpseo_schema_webpage', 'example_change_webpage' );

/**
 * 修改 Webpage Schema 数据的 @type。
 *
 * @param array $data Schema.org Webpage 数据数组。
 *
 * @return array Schema.org Webpage 数据数组。
 */
function example_change_webpage( $data ) {
    if ( ! is_page( 'about' ) ) {
        return $data;
    }

    $data['@type'] = 'AboutPage';

    return $data;
}
```

为了方便起见，我们还有一个更具体的过滤器：`wpseo_schema_webpage_type` - 用于更改页面类型，因此可以用来使上述示例更加简化。

要对我们的 Schema 输出进行更多更改，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。