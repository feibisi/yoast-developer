---
title: "Functional Specification"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Titles
    - Seo Tags
    - Features
---


网站上的每个页面都应在文档的 `<head>` 部分包含一个独特且描述性的标题标签。

有效的标题标签采用以下格式：`<title>{{VALUE}}</title>`。

以下部分将说明如何构建标签中 `{{VALUE}}` 组件的值。

## 各请求类型的默认逻辑
下表描述了我们认为的所有请求类型以及每种情况下标题标签的结构。请注意，这并未涵盖所有可想象的场景或边缘情况，因为我们只期望此规范通常在内容管理系统或网站的约束范围内应用。

另请注意，全文使用的“页面”一词指为请求的 URL 提供的任何“非系统”响应。这包括归档、分类法和错误状态。它不包括“非页面”结果（通常不支持 `<title>` 标签），如 RSS 订阅和 XML 站点地图。

| 请求类型 | 备注 | 标题结构 | 示例 |
| :--- | :--- | :--- | :--- |
| 首页 | 假设首页位于网站根目录。否则，请参见单页。 | `%%sitename%% %%page%% %%sep%% %%tagline%%` | Acme Inc - The best widgets in the world |
| 单页 | 在 WordPress 中，也考虑单篇文章。例如，“关于我们”页面或博客文章。 | `%%postname%% %%page%% %%sep%% %sitename%` | Top 10 widgets - Acme Inc |
| 分类索引页 | 任何基于分类法或类似方式列出选定文章或页面的模板。例如，分类页面或标签页面，或列出特定作者所有文章的页面。 | `%%term_title%% archive %%page%% %%sep%% %sitename%` | Blue widgets archive - Acme Inc |
| 日期索引页 | 任何按日期筛选列出选定文章或页面的模板。例如，在特定年份发布的所有文章，或在特定日期/某天添加的所有页面。 | `%%date%% %%page%% %%sep%% %sitename%` | 2018 - Acme Inc |
| 搜索页面 | 任何按用户查询筛选列出选定文章或页面的模板。例如，与“widget”一词相关的所有文章。 | `You searched for %%search_phrase%% %%page%% %%sep%% %sitename%` | You searched for red widgets - Acme Inc |
| 404 页面 | 当用户请求返回 404 错误的 URL 时提供的任何模板。 | `Page not found %%sep%% %sitename%` | Page not found - Acme Inc |

这些变量大多应具有相当的自描述性，不需要大量文档。然而，为避免歧义：

- `%%sitename%%`：网站名称。例如，“Acme Inc”。
- `%%sep%%`：分隔符。例如，`-`、`|` 或类似符号。
- `%%tagline%%`：网站的简短描述或类似内容。例如，“The best widgets in the world”。
- `%%postname%%`：页面/文章的名称。例如，“Widget FAQs”。
- `%%page%%`：分页状态的表示（在系列的第一页上省略）。例如，“Page 2 of 8”。
- `%%term_title%%`：分类法/归档的名称。例如，“Blue widgets”。
- `%%date%%`：归档的日期。例如，“July 2016”。
- `%%search_phrase%%`：搜索字符串。例如，“How much do red widgets cost”。

### 清理处理
任何用户输入值（即 URL 的任何组成部分，特别是任意字符串和细化条件，如搜索词）都应始终进行适当的清理处理。

## 用户指定模式
每当用户为标题定义了替代模式/模板时（例如，使用 Yoast SEO 的替换变量），应使用该模式替代默认设置。

## 用户指定值
当用户为页面定义了特定值时，应使用该值替代默认（或特定类型）模式。