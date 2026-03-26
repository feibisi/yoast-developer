---
title: "News Seo"
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


[News SEO for WordPress](https://yoast.com/wordpress/plugins/news-seo/) 的 schema 输出建立在 [Yoast SEO for WordPress 的基础 schema 输出](docs/features/schema/plugins/yoast-seo)之上，以提供额外的*新闻*上下文和细节。

## 核心逻辑
当某个文章类型被选定为*新闻文章*格式时，则：
* 将 `Article` 的 `@type` 属性更改为 `NewsArticle`。
* 添加 `copyrightYear` 属性，值为发布年份。
* 添加 `copyrightHolder` 属性，通过 ID 引用发布的 `Organization`。

## 其他场景

### NewsArticle 子类型
当用户将*文章类型*或*单篇文章*指定为 `NewsArticle` 的特定子类型（例如 `OpinionNewsArticle`）时，将 `@type` 属性设置为该值。

### 在不包含 `Article` 的“新闻页面”上
在少数情况下，非文章格式（例如页面、自定义文章类型、首页、产品页面等）可能被选为新闻类型。通常，这些页面格式不包含 `Article`。

在这些情况下，应将一个 `Article`（及其依赖部分）添加到图谱中，并将其多类型化为 `Article` 和 `NewsArticle`，就像该页面是一个常规的*新闻文章*一样。

在某些场景下，这可能导致多个实体声明与 `WebPage` 存在 `mainEntityOfPage` 关系（例如，当页面已包含 `Product` 时）。发生这种情况时，应更改冲突节点（例如 `Product`）上的 `mainEntityOfPage` 属性，以引用 `NewsArticle` 的 ID。

## 更多信息
* [Article Schema 组件](docs/features/schema/pieces/article)
* [Organization Schema 组件](docs/features/schema/pieces/organization)
* [Product Schema 组件](docs/features/schema/pieces/product)
* [WebPage Schema 组件](docs/features/schema/pieces/webpage)