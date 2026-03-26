---
title: "Image"
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

描述单个图像（通常在嵌入式媒体对象的上下文中）。

**注意**：虽然 `image` 属性（给定节点的）可以接受表示图像文件的简单 URL，但我们始终强制使用 `ImageObject` 以支持通过 ID 引用。

## 触发器
应在其他节点需要时，作为图的顶级节点添加。例如，当 `WebPage` 具有*特色图像*时。

## 必需属性
一个有效的 `ImageObject` 必须具有以下属性。

* `@type`: `ImageObject`。
* `@id`: 图像文件的完全限定绝对 URL（例如，`https://www.example.com/images/cat.jpg?size=large`）。
* `url`: 图像文件的完全限定绝对 URL（例如，`https://www.example.com/images/cat.jpg?size=large`）。
* `contentUrl`: 图像文件的完全限定绝对 URL（例如，`https://www.example.com/images/cat.jpg?size=large`）。

## 故障场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果未输出该节点，任何原本会声明与 `Image` 存在关系（例如，作为 `Organization` 的 `logo`）的实体都应移除这些引用。

## 可选属性
以下属性应在可用且有效时添加：

* `caption`: 描述图像的文本字符串。
  * 如果没有特定的 `caption` 字段存在或定义，则回退到图像的 `alt` 属性。

## 条件属性
仅在满足所需条件时才应输出的可选属性。

### 当高度和宽度尺寸均已知时
* `height`: 图像的高度（以像素为单位）
* `width`: 图像的宽度（以像素为单位）

### 当图像包含标题（或任何其他文本属性）时
* `inLanguage`: 文本内容的语言代码；例如 `en-GB`。

## 备注

### `contentUrl` 和 `url` 属性被有意重复设置。
`contentUrl` 属性比 `url` 属性更准确地描述/引用实际的图像资源（`url` 属性无法区分图像资源“所在”的 URL 与资源本身的位置）。因此，我们倾向于***不设置/使用*** `url` 属性——或者，将其保留用于指代图像位置的场景（例如，指向网页某部分的锚点/片段链接）。

然而，当 `url` 属性缺失时，Google 的测试工具在某些情况下会报错（针对 SDTT 中的 `logo` 和 `primaryImageOfPage`，以及 RRT 中的 `primaryImageOfPage`）。因此，我们仅添加一个 `contentUrl` 属性，其值为图像 URL。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/uploads/example-image.jpg",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "contentUrl": "https://www.example.com/uploads/example-image.jpg"
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
              "@type": "ImageObject",
              "@id": "https://www.example.com/uploads/example-image.jpg",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "contentUrl": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "inLanguage": "en-US",
              "width": 120,
              "height": 120
          }
      ]
  }`}
</YoastSchemaExample>