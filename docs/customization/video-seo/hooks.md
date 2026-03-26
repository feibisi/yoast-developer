---
title: "Hooks"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Video Seo
    - Customization
    - Repos
---


Yoast Video SEO 插件提供了一些可用于修改其功能的钩子，具体如下：

## 输出
### `wpseo_video_{$type}_details`
参数：`$vid` (数组), `$post` (WP_Post)

此过滤器允许更改视频嵌入识别功能的输出。
`$vid` 数组至少包含 `player_loc` 或 `content_loc`、`thumbnail_loc` 和 `type`。
您需要知道类型才能过滤输出，但您可以使用此过滤器来添加例如时长信息。

### `wpseo_video_family_friendly`
参数：`$family-friendly` (布尔值), `$post_id` (整数)

此过滤器允许更改视频的家庭友好状态。

### `wpseo_video_rating`
参数：`$rating` (浮点数), `$post_id` (整数)

此过滤器允许更改视频的评分。评分是介于 0 到 5 之间的值。

### `wpseo_video_item`
参数：`$str` (字符串), `$post_id` (整数)

此过滤器允许向视频 XML 站点地图添加额外内容。
添加到 `$str` 中的输出默认为空字符串。

### `wpseo_video_yandex_support`
参数：`$yandex_on` (布尔值)

此过滤器允许禁用视频 XML 站点地图中特定于 Yandex 的元标签。

## 输入
### `wpseo_video_index_content`
参数：`$content` (字符串), `$vid` (数组), `$post` (WP_Post)

允许在内容被索引之前更改文章内容。
如果输入内容需要修改、添加内容等，此过滤器非常有用。