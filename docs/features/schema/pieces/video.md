---
title: "Video"
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

描述单个视频（通常在嵌入式媒体对象的上下文中）。

## 触发条件
应作为图谱中的顶级节点添加，当其他节点需要时。例如，当 `WebPage` 包含嵌入式视频时。

## 必需属性
一个有效的 `VideoObject` 必须包含以下属性。

* `@type`: `VideoObject`。
* `@id`: 网站的首页 URL 后追加 `#/schema/VideoObject/{{ID}}`，其中 `{{ID}}` 是一个唯一标识符。
* `name`: 视频的标题。
* `description`: 视频的描述（回退到 `caption`，再回退到 'No description'）。
* `thumbnail`: 一个通过 ID 引用、代表视频内容的 `ImageObject`。
* `uploadDate`: 视频发布的日期，采用 ISO 8601 格式（例如 `2020-01-20`）。

## 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果未输出节点，则任何原本会声明与 `VideoObject` 存在关系的实体（例如，作为 `Article` 的一部分）都应移除这些引用。

## 可选属性
以下属性应在可用且有效时添加：

* `caption`：视频内容摘要说明。
* `contentUrl`：直接指向视频文件的 URL。
* `embedUrl`：指向视频播放器的 URL。
* `duration`：视频时长，采用 ISO 8601 格式。
* `width`：视频宽度，单位为像素。
* `height`：视频高度，单位为像素。
* `isFamilyFriendly`：视频是否应被视为“适合家庭观看”，默认为 `true`，可设为 `false`。
* `inLanguage`：视频内容的语言代码；例如 `en-GB`。

## 条件属性
仅在满足必要条件时才应输出的可选属性。

#### 在 AMP 模板上
* `publisher`: 对视频发布者的 ID 引用。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "VideoObject",
              "@id": "https://www.example.com/#/schema/VideoObject/abc123",
              "name": "Example video",
              "description": "Example video description",
              "thumbnailUrl": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "uploadDate": "2016-03-31T08:00:00+08:00"
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
              "@id": "https://www.example.com/#/schema/VideoObject/abc123",
              "@type": "VideoObject",
              "contentUrl": "https://www.example.com/videos/video123.mp4",
              "description": "Example video description",
              "duration": "PT1M54S",
              "embedUrl": "https://www.example.com/videos/embed/video123/",
              "height": 640,
              "inLanguage": "en-US",
              "isFamilyFriendly": true,
              "name": "Example video",
              "thumbnail": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "uploadDate": "2016-03-31",
              "width": 480
          }
      ]
  }`}
</YoastSchemaExample>