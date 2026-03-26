---
title: "Article"
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

描述 [`WebPage`](docs/features/schema/pieces/webpage) 上的一个 `Article`。
如果满足所需条件，可转换为更具体的类型（例如 `NewsArticle`）。

## 触发器
应在所有支持 _作者身份_ 且拥有作者的文章类型上输出。

## 必需属性
有效的 `Article` 必须包含以下属性。

* `@type`: `Article`。
* `@id`: 网站首页 URL 后附加 `#/schema/Article/{{ID}}`，其中 `{{ID}}` 是所讨论文章的唯一标识符。
* `headline`: 文章的标题（回退到 `WebPage` 的 *标题*）。
* `description`: 文章的摘要（回退到页面的元描述内容）。
* `isPartOf`: 对 `WebPage` 节点的 ID 引用。
* `mainEntityOfPage`: 对 `WebPage` 节点的 ID 引用。
* `datePublished`: 文章最初发布时间，采用 ISO 8601 格式；例如 `2015-10-31T16:10:29+00:00`。
* `dateModified`: 文章最后修改时间，采用 ISO 8601 格式；例如 `2015-10-31T16:10:29+00:00`。
* `author`: 对文章作者的 ID 引用，并包含其 `name` 属性（以更好地支持 Pinterest）。
* `publisher`: 对文章发布者的 ID 引用。

## 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

## 可选属性
以下属性应在可用且有效时添加：

* `image`：一个图像对象（或文章内容中所有图像的数组），通过 ID 引用。
   * 宽度必须至少为 `696` 像素。
   * 必须是以下格式/文件扩展名：`.jpg`、`.png`、`.gif` 或 `.webp`。
* `video`：文章内容中所有视频的数组，通过 ID 引用。
* `comment`：通过 ID 引用 `comment` 片段的数组。
* `articleSection`：文章所属类别名称的数组（例如，`["cats","dogs","cake"]`）。
* `inLanguage`：文章的语言代码；例如，`en-GB`。
* `speakable`：一个 `SpeakableSpecification` 对象，用于标识适合语音播报结果的内容元素。

## 条件属性
仅在满足特定条件时才应输出的可选属性。

### 若存在版权信息
* `copyrightYear`: 文章获得版权状态的起始年份。
* `copyrightHolder`: 持有版权的 [`Organization`](docs/features/schema/pieces/organization) 或 [`Person`](docs/features/schema/pieces/person) 的 ID 引用。

### 如果存在评论
* `commentCount`：一个整数值，表示与文章关联的评论数量。
* `comment`：一个数组，包含与文章关联的 `comment` 片段的引用（通过 ID）。

### 如果允许/启用评论功能
* `potentialAction`: 一个包含以下值的 `CommentAction` 对象：
 * `name`: "Comment"。
 * `target`: 父级 `WebPage` 的 `url` 属性，并附加 `#comment`。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
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
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
              },
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
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
              "articleSection": ["cats","dogs","cake"],
              "inLanguage": "en-US",
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/Organization/1"
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
                      "@type": "CommentAction",
                      "name": "Comment"
                      "target": [ "https://www.example.com/blog/example-article/#comment" ]
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API：修改文章 Schema 输出 {#api}

如需修改 Yoast SEO 输出的 `Article` schema，可使用 `wpseo_schema_article` 过滤器。示例如下：

```php
add_filter( 'wpseo_schema_article', 'change_article_to_social_posting' );

/**
 * 修改文章 Schema 数据的 @type 属性。
 *
 * @param array $data Schema.org 文章数据数组。
 *
 * @return array Schema.org 文章数据数组。
 */
function change_article_to_social_posting( $data ) {
    $data['@type'] = 'SocialMediaPosting';

    return $data;
}
```

### 在自定义文章类型上输出文章架构
默认情况下，Yoast SEO 不会在自定义文章类型上输出文章架构。以下代码可改变此行为：

```php
/**
 * 将 'book' 添加到文章类型列表中，使书籍获得文章架构。
 *
 * @param  mixed $post_types 当前文章类型列表。
 *
 * @return array Yoast SEO 为其渲染文章架构的文章类型数组。
 */
function article_schema_for_books( $post_types ) {
	$post_types[] = 'book';
	return $post_types;
}

add_filter( 'wpseo_schema_article_post_types', 'article_schema_for_books' );
```

请注意，要使文章类型能够输出 `Article` 架构，该文章类型需要支持作者功能。
对于给定的文章类型 `book`，您可以通过添加以下代码轻松实现：

```php
add_post_type_support( 'book', 'author' );
```

目前这不适用于 `page` 文章类型。我们正在修复此问题。

### Add images to your Article Schema
If you want to add an image to an object programmatically, you can do it as follows. Note that we use the `YoastSEO`
surface to get both the `canonical` from the `meta` surface as the `helpers` surface to access the `schema->image`
functionality.

```php
add_filter( 'wpseo_schema_article', 'example_change_article' );

/**
 * Adds images to Article Schema data.
 *
 * @param array $data Schema.org Article data array.
 *
 * @return array Schema.org Article data array.
 */
function example_change_article( $data ) {
    // This is the attachment ID for our image.
    $attachment_id = 12345;

    // We're going to create a graph piece for our image. Every graph piece always needs a Schema ID, so it can
    // be referenced by other graph pieces, best practice is to base that on the canonical adding an ID that's
    // always going to be unique.
    $schema_id     = YoastSEO()->meta->for_current_page()->canonical . '#/schema/image/' . $attachment_id;                 
    $data['image'] = new WPSEO_Schema_Image( $schema_id );

    return $data;
}
```

Instead of `YoastSEO()->helpers->schema->image->generate_from_attachment_id()` you can also use
`YoastSEO()->helpers->schema->image->generate_from_url()` which takes, as you've guessed, a URL as input.

To make more changes to our Schema output, see the [Yoast SEO Schema API](docs/features/schema/api).