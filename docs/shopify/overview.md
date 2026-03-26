---
title: "Overview"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Shopify
    - Repos
    - Data
---


[Yoast SEO for Shopify](https://apps.shopify.com/yoast-seo) 应用会对商店的主题文件进行修改和添加。这样做是为了优化技术 SEO 的各个方面，并将我们的内容编辑工具与网站输出连接起来。
本文档概述了我们所做的更改类型，并提供了如何还原这些更改的详细信息。

## 主题变更

Yoast SEO 会尝试用我们自己的输出来替换主题中所有与 SEO 相关的元数据。这包括：

 - `<title>` 元素的内容
 - 各种与 SEO 相关的 `<meta>` 标签（例如 `<meta name="description" ...>`）
 - 各种与社交媒体相关的 `<meta>` 标签（例如 `<meta property="og:url" ...>`）
 - 规范 URL 标签（即 `<link rel="canonical" ...>`）
 - 结构化数据 / schema.org 输出（即 `<script type="application/ld+json">...</script>`）

其实现方式是通过“注释掉”主题代码中的相关行，并渲染我们自己的输出。这一过程发生在安装时。

### 示例

此示例展示了商店 `theme.liquid` 文件的一部分在 Yoast SEO 修改前后的可能样子。

#### 修改前

```html
<!doctype html>
<html class="no-js" lang="{{ request.locale.iso_code }}">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <meta name="theme-color" content="">
    <link rel="canonical" href="{{ canonical_url }}">
```

#### After

```html
<!doctype html>
<html class="no-js" lang="{{ request.locale.iso_code }}">
  <head>{% render 'yoast-seo' %}
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <meta name="theme-color" content="">
    {%- comment -%}Auto-disabled. Yoast plugin is taking care of Meta Tags output.{%- endcomment -%}
    {%- if disabled_by_yoast_seo -%}
      <link rel="canonical" href="{{ canonical_url }}">
      <!-- disabled_by_yoast_seo -->
    {%- endif -%}
```

### 方法

在上面的示例中，您可以看到我们添加了 `{% render 'yoast-seo' %}` 和一个 `{%- if disabled_by_yoast_seo -%}` 条件。

这种方法使我们能够应用更复杂的逻辑，并集成我们应用的内容设置。例如，我们不是简单地在模板中输出一个*规范 URL*值，而是使用以下*代码片段*来控制整个标签的行为：

```html
{%- capture yoast_seo_page_type -%}{%- render 'yoast-seo-page-type' -%}{%- endcapture -%}
{%- if yoast_seo_page_type != "404" %}
	{%- capture yoast_canonical_url -%}{%- render "yoast-seo-variable", variable: "canonical_url" -%}{%- endcapture -%}
	<link rel="canonical" href="{{- yoast_canonical_url | replace: '"', '&#34;' -}}"/>
{%- endif -%}
```

我们创建并使用了许多这样的代码片段来控制 SEO 设置的优化和渲染方式。

### 其他变更

除了优化所有模板 `<head>` 部分的输出外，我们还对 _模板_、_布局_ 和 _区块_ 文件进行了大量细微调整。

这些变更旨在移除所有内联的 _结构化数据_（采用 `JSON-LD` 和 `微数据` 格式）。我们移除此代码是为了实施 [我们自己的 schema.org 解决方案](https://developer.yoast.com/features/schema/plugins/yoast-seo-shopify)，同时降低冲突和重复的风险。

请注意，我们的应用提供了控制选项，允许您针对特定 _类型_ 的 schema.org 属性切换此行为。当您希望保留自定义的 schema 输出时（例如，当主题进行了定制化修改，或其他应用添加了补充性的 schema.org 内容时），这会很有帮助。

## 恢复或移除主题更改

恢复主题更改最简单的方法是使用应用内的专用控件。该控件可在应用的设置菜单中找到。
更多相关信息请查阅[我们的应用卸载帮助页面](https://yoa.st/shopify72)。

此控件将移除所有主题自定义设置，将您的主题恢复到未修改状态，同时保留安装后进行的任何自定义配置。

### 移除应用后的更改清理

遗憾的是，Shopify 未提供在应用移除时自动清理主题更改的机制。这意味着部分用户可能无法访问我们的清理工具，必须手动移除相关更改。

若您已卸载应用且无法重新安装，建议访问[应用卸载帮助页面](https://yoa.st/shopify72)。该页面提供了最新的主题修改清理指南。