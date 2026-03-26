---
title: "Video Seo"
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


我们的 [Video SEO 插件](https://yoast.com/wordpress/plugins/video-seo/) 的 Schema 输出基于 [Yoast SEO 的 Schema 输出](docs/features/schema/plugins/yoast-seo)，以添加关于嵌入式视频的额外细节。

对于您已启用 Video SEO 的所有文章类型，当它在页面上遇到其识别的视频嵌入时，会自动添加一个 `videoObject` 片段。

## 核心逻辑
* 为每个发现的视频在图中添加一个 `videoObject` 片段。
* 向 `WebPage` 添加一个 `video` 属性，包含一个指向每个 `videoObject` 片段的 ID 引用数组。

## 其他场景

### 在包含 `Article` 的页面上
* 向 `Article` 添加一个 `video` 属性，包含一个指向每个 `videoObject` 片段的 ID 引用数组。

## 更多信息
* [Video Schema 片段](docs/features/schema/pieces/video)
* [Article Schema 片段](docs/features/schema/pieces/article)
* [WebPage Schema 片段](docs/features/schema/pieces/webpage)