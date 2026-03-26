---
title: "Functional Specification"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Xml Sitemaps
    - Features
    - Repos
---


用户希望被搜索引擎发现和索引的任何页面都应列在 XML 站点地图中。

应为每种*内容类型*构建单独的 XML 站点地图。大型站点地图可以拆分为独立的、分页的文件。

每个 XML 站点地图都应列在*XML 站点地图索引*中。

## 内容类型
应为以下每种内容类型创建 XML 站点地图，前提是该内容类型为*公开*且包含有效成员。

* 文章、页面和自定义文章类型
* 作者（至少拥有一篇已发布、公开、可索引文章的用户）
* 自定义文章类型
* 分类法
* 多部分文章/页面的分页状态

除了单独的 XML 站点地图外，还应创建一个*XML 站点地图索引*，其中引用所有这些站点地图。

## 排除项与空站点地图
以下情况应导致页面被排除在（相应的）XML 站点地图之外：

* 不符合搜索引擎索引条件的文章/页面（即任何导致页面通过元标签或 HTTP 头输出元机器人 noindex 指令的情况）
* 设置了手动规范 URL 且该 URL *不* 与其默认规范值相同的文章/页面
* 私有或受密码保护的页面
* 错误页面/状态/模板
* 搜索结果页面
* 任何类型归档的分页视图（例如，/blog/page/2/）
* 非公开的归档和分类法
* 当整个站点设置为“私有”时（例如，在 WordPress 中，当 *搜索引擎可见性* 选项设置为“*阻止搜索引擎索引此站点*”时）的所有 URL

其中一些情况可能导致 XML 站点地图（或 XML 站点地图索引）为空。这是预期且允许的行为。

对不存在的站点地图 URL 的请求应返回正常的 404 行为。

## 内容

### XML 站点地图索引
索引文件为每个独立的 XML 站点地图包含一个 `<sitemap>` 条目，每个条目都有 loc 和 lastmod 属性。例如：

```xml
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<sitemap>
  <loc>https://www.example.com/author-sitemap.xml</loc>
  <lastmod>2019-10-21T12:37:01+00:00</lastmod>
</sitemap>
{...}
</sitemapindex>
```

* loc 属性应引用相关 XML 站点地图的绝对 URL
* **（可选）** lastmod 属性应反映相关 XML 站点地图中最新的 lastmod 值

如果不可用，可选属性可以省略。

#### URL 与重定向
该文件应可通过 `/sitemap_index.xml` 访问。对 `/sitemap.xml` 的请求应重定向至此地址。

### XML 站点地图
每个站点地图为每个页面包含一个 `<url>` 条目，由 `loc`、`lastmod` 和 `image:image` 属性组成。例如：
```xml
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:image="http://www.google.com/schemas/sitemap-image/1.1" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd http://www.google.com/schemas/sitemap-image/1.1 http://www.google.com/schemas/sitemap-image/1.1/sitemap-image.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<url>
  <loc>https://www.example.com/blog/example-post/</loc>
  <lastmod>2018-04-21T10:00:12+00:00</lastmod>
  <image:image>		 
    <image:loc>https://www.example.com/example-image.jpg</image:loc>
  </image:image>
  <image:image>		 
    <image:loc>https://www.example.com/another-example-image.jpg</image:loc>
  </image:image>
</url>
{...}
</urlset>
```

* `loc` 属性应引用文章/页面/对象的规范 URL（永久链接）
* **（可选）** `lastmod` 属性应引用对象最后更新的时间。对于归档/类似页面，这应反映归档成员最后更新的时间（例如，对于特定分类，应反映该分类中最后发布文章的时间）
* **（可选）** 应为页面中/与页面关联的每个图像输出一个 `image:image` 属性，包含：
  * 一个 loc 属性（引用图像的绝对 URL）

如果不可用，可选属性可以省略。

#### 限制与分页
默认情况下，每个站点地图最多可包含 1,000 个项目。如果某个内容类型的项目超过 1,000 个，则会生成额外的站点地图（最多 50,000 个）来容纳溢出的项目。

#### 网址与重定向
每个站点地图的 URL 应按以下格式构建：`{{type}}-sitemap{{n}}.xml`，其中：

* `{{type}}` 为内容类型（例如：page）
* `{{n}}` 为分页状态（系列中的第一页忽略此参数）。

例如：`post-sitemap.xml`、`post-sitemap2.xml`、`custom_taxonomy_name-sitemap.xml`。

当 `{{n}}` 为 `1` 或 `0` 时，对站点地图的请求应触发 301 重定向以移除 `{{n}}`（例如：对 `post-sitemap1.xml` 和 `post-sitemap0.xml` 的请求应重定向至 `post-sitemap.xml`）。

### XSL 样式表
站点地图索引和单个站点地图引用 XSL 文件，该文件为页面提供布局和样式配置。

## 索引控制
XML 站点地图无需被搜索引擎索引即可被读取或解析。因此，所有 XML 站点地图响应都应返回一个值为 `noindex, follow` 的 `x-robots-tag` HTTP 标头。

## 其他场景

### 视频站点地图
我们的 [WordPress 视频 SEO 插件](https://yoast.com/wordpress/plugins/video-seo/) 会添加一个额外的 *视频站点地图*（位于 `video-sitemap.xml`，并包含在站点地图索引中），其中包含网站内容中托管/引用的每个视频的信息。

对于每个包含视频的页面，站点地图包含一个 `<url>` 包装器，其中包含 `<loc>` 值（页面的规范 URL / 永久链接），以及页面中每个视频的 `<video:video>` 容器。例如：
```xml
<url>
  <loc>https://www.example.com/example-page/</loc>
  <video:video>
    <video:title><![CDATA[Example Video]]></video:title>
    <video:publication_date>2015-12-15T15:36:03+00:00</video:publication_date>
    <video:description><![CDATA[Example video description]]></video:description>
    <video:player_loc>https://www.youtube-nocookie.com/v/abc123</video:player_loc>			 
    <video:thumbnail_loc>https://www.example.com/video-thumbnail.jpg</video:thumbnail_loc>
    <video:duration>98</video:duration>
    <video:view_count>367</video:view_count>
    <video:width>640</video:width>
    <video:height>390</video:height>
    <video:tag>Example tag</video:tag>
    <video:tag>Another example tag</video:tag>
    <video:family_friendly>yes</video:family_friendly>
    <video:uploader info="https://www.example.com/example-profile-page">Example Person</video:uploader>
  </video:video>
</url>
{…}
</urlset>
```

* `title` 是视频的名称
* `description` 是视频的说明文字，回退到视频的任何可用描述，再回退到视频的 URL（例如，视频的公开 YouTube URL）
* `player_loc` 是指向视频播放器的 URL
* `thumbnail_loc` 是视频缩略图图像的 URL
* **（可选）** `publication date` 是视频“发布”（或*上传*）的时间戳
* **（可选）** `duration` 是以秒为单位的持续时间
* **（可选）** `view_count` 是观看次数
* **（可选）** `width` 和 `height` 是嵌入视频的尺寸（以像素为单位）
* **（可选）** `tag` 是与嵌入视频的页面/文章相关联的一系列标签
* **（可选）** `family_friendly` 是一个布尔值，在视频嵌入 UI 中配置
* **（可选）** `uploader` 是上传视频的人员姓名，带有可选的 info 属性，用于引用个人资料/主页

视频 XML 站点地图使用专用的 XSL 文件。

### 新闻站点地图
我们的 [WordPress 新闻 SEO 插件](https://yoast.com/wordpress/plugins/news-seo/) 会添加一个额外的 *新闻站点地图*（位于 `news-sitemap.xml`，并包含在站点地图索引中），其中包含网站上发布的新闻文章信息。

新闻站点地图应仅引用过去 48 小时内发布或修改的文章。

该站点地图包含一个 `<url>` 包装器，其中包含 `<loc>` 值（页面的规范 URL / 永久链接），以及每个新闻文章的 `<news:news>` 容器。例如：
```xml
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:news="http://www.google.com/schemas/sitemap-news/0.9" xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
<url>
  <loc>http://www.example.com/example-page/</loc>
  <news:news>
    <news:publication>
      <news:name>Example site name</news:name>
      <news:language>en</news:language>
    </news:publication>
    <news:publication_date>2019-11-15T09:59:38+00:00</news:publication_date>
    <news:title><![CDATA[Example post name]]></news:title>
  </news:news>
</url>
{…}
</urlset>
```

* `publication.name` 是 *网站标题*（例如，"Yoast"）
* `publication.language` 是文章的语言代码（采用 [ISO 639](http://www.loc.gov/standards/iso639-2/php/code_list.php) 格式）
* `publication_date` 是 [W3C 格式](https://www.w3.org/TR/NOTE-datetime) 的日期
* `title` 是文章的帖子名称
* **（可选）** 应为页面中/与页面关联的每个图像输出一个 `image:image` 属性，包含：
  * 一个 `loc` 属性（引用图像的绝对 URL）

新闻 XML 站点地图使用专用的 XSL 文件。

:::info

一些延伸阅读：
* 关于 [什么是 XML 站点地图（以及为什么你应该拥有一个）](https://yoast.com/what-is-an-xml-sitemap-and-why-should-you-have-one/) 的指南
* 关于 [如何将你的 XML 站点地图提交给搜索引擎](https://kb.yoast.com/kb/submit-sitemap-search-engines/) 的指南

:::