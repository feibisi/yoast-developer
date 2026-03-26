---
title: "Functional Specification"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Meta Robots
    - Seo Tags
    - Features
---


Yoast SEO 始终在每个页面的 `<head>` 中输出一个 _meta robots_ 标签。当用户希望通过搜索引擎主动限制页面的抓取、索引或展示时（通过我们的[控制功能](/features/controls/overview)），我们会更改该标签的内容。

该标签采用以下格式：`<meta name="robots" content="{{values}}" />`。

## 构建 `{{values}}`
鉴于一个 meta robots 标签可能包含多个（可能相互冲突的）值，并且可能适用多个重叠条件，那么：

*   应评估所有条件，并根据以下定义的规则（参见 _解决冲突_）为每个代理将结果标签值相加组合。
*   最终的值集应进行去重和协调。
请参阅[所有可能的 meta robots 值及其关系的最新列表](https://yoast.com/robots-meta-tags/)。

## 标准结构
默认情况下，Yoast SEO 在每个（公开）页面上输出以下结构的 meta robots 标签：

`<meta name="robots" content="{{values}}, max-snippet:-1, max-image-preview:large, max-video-preview:-1" />`

这为所有机器人提供了通用指令，并让用户选择加入 Google 和 Bing 对摘要/媒体限制的特定处理方式。

除非用户（或通过页面/模板/过滤逻辑）另有定义，否则 `{{values}}` 输出 `index, follow`。

## 场景

### 在非公开页面上
任何“非公开”页面——即用户确定*不应*出现在搜索结果中的页面、文章或归档类型（例如，通过我们的“搜索外观”设置）——应返回 `noindex` 和 `follow` 属性。例如：
`<meta name="robots" content="noindex, follow" />`

### 每篇文章/页面的自定义控制
用户为特定文章或页面指定的任何高级/自定义属性，应按定义在 `{{values}}` 属性中返回。

如果设置了 `noindex` 值，则该页面应被视为*非公开*页面（见上文）。

### 错误模板
在 `4xx` 和 `5xx` 模板上不应输出 robots 标签。

### 解决冲突
在指令冲突的情况下，应始终遵循限制性最强的设置，使用以下规则：

*   `noindex` 优先于 `index`。
*   `nofollow` 优先于 `follow`。
*   `none` 优先于 `nofollow` 或 `noindex`。

例如，`index`、`noindex`、`noimageindex` 和 `follow` 值的组合应导致输出 `noindex, follow`（因为 `noindex` 比 `index` 限制性更强，并且 `noimageindex` 与 `noindex` 结合时是冗余的；有关值关系的完整概述，请参阅[文档](https://yoast.com/robots-meta-tags/)）。