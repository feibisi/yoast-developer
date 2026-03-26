---
title: "Hooks"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - News Seo
    - Customization
    - Repos
---


Yoast News SEO 插件提供了一些可用于修改其功能的钩子，具体如下：

## 头部标签
### `Yoast\WP\News\head`

此操作允许在输出头部标签前运行额外的代码。

### `Yoast\WP\News\head_display_noindex`
参数：`$display_noindex` (布尔值), `$post` (WP_Post)

此过滤器允许阻止为特定文章输出 noindex 元标签。

## 站点地图
### `wpseo_news_sitemap_content`
参数：`$str` (字符串)

此过滤器允许向新闻 XML 站点地图添加额外内容。
`$str` 中添加的输出默认为空字符串。

### `Yoast\WP\News\publication_language`
参数：`$publication_lang` (字符串), `$item` (Indexable)

此过滤器允许更改新闻 XML 站点地图中特定文章的发布语言。

### `Yoast\WP\News\sitemap_name`
参数：`$sitemap_name` (字符串)

此过滤器允许更改新闻 XML 站点地图的名称。

### `Yoast\WP\News\skip_build_item`
参数：`$skip_build_item` (布尔值), `$item_id` (整数)

此过滤器允许跳过在新闻 XML 站点地图中构建特定项目。