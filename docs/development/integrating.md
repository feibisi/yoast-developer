---
title: "Integrating"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Development
    - Repos
    - Data
---


本文档是描述集成 [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) 所需技术规范和要求的中心。

## 自动优化
当软件安装并激活后，必须自动且无需用户干预地满足以下标准。

### 系统行为
* 网站必须输出并使用“美观固定链接”（人类可读、结构化的 URL）。
* 所有面向公众的 URL、元标签和内容必须在服务器端渲染，在服务器的初始响应中完成。不支持客户端渲染方案。
* 网站必须输出完整且有效的 [XML 站点地图](docs/development/features/xml-sitemaps/functional-specification)。
* 网站必须生成（或修改）[robots.txt 文件](docs/development/features/robots-txt/functional-specification)。

### 集成
* 创建、更新或发布内容时应触发 [IndexNow](docs/development/features/integrations/indexnow)（并向 Google 提交 [XML 站点地图](docs/development/features/xml-sitemaps/functional-specification)）。

## 元数据与输出

### 元标签
所有页面和内容类型都应按照以下标准输出元数据：

* [标题标签](docs/development/features/seo-tags/titles/functional-specification)
* [描述标签](docs/development/features/seo-tags/descriptions/functional-specification)
* [规范 URL 标签](docs/development/features/seo-tags/canonical-urls/functional-specification)
* [Meta robots 标签](docs/development/features/seo-tags/meta-robots/functional-specification)
* [Open Graph 标签](docs/development/features/opengraph/functional-specification)
* [X 标签](docs/development/features/twitter/functional-specification)
* [替代内容格式](/features/alternate-formats)

### 结构化数据
所有页面都应按照以下标准输出结构化数据：

* [Schema 规范](docs/development/features/schema/functional-specification)（通用/基础行为）。
* [Yoast SEO 的 Schema 规范](docs/development/features/schema/plugins/yoast-seo)（Yoast 特定行为）。

## 控制与分析
对于所有文章、页面和内容类型，用户必须能够：

* [分析](docs/development/features/analysis/overview)其内容。
* 获得[控制](docs/development/features/controls/overview)选项。
* 批量编辑页面标题和描述（无需访问/打开每个单独页面）。

## 与我们的 Yoast SEO 内容分析集成

### 当前设置

目前，我们的许多分析深度集成于 WordPress 或 Shopify 集成中。
在此，我们说明 Yoast SEO 插件如何与（默认）区块编辑器、经典编辑器以及 Elementor 集成。
出于可理解的原因，我们专注于免费开源 WordPress 插件 [Yoast SEO](https://github.com/Yoast/wordpress-seo) 的相关部分。

#### PHP 集成

在 PHP 端，我们使用 `Metabox` 的概念来处理内容分析的不同部分。这个命名略有不当，因为它们既用于我们的 **Yoast SEO metabox**（在经典编辑器和区块编辑器中），也用于 **Yoast SEO 侧边栏**（在区块编辑器、Elementor 编辑器和 Shopify 应用中）。更多详情请参考[文章相关代码](https://github.com/Yoast/wordpress-seo/tree/trunk/admin/metabox/class-metabox.php)和[分类法相关代码](https://github.com/Yoast/wordpress-seo/tree/trunk/admin/taxonomy/class-taxonomy-metabox.php)。我们目前正在[解耦 metabox 和侧边栏](https://github.com/Yoast/wordpress-seo/issues/20866)，请持续关注相关更新。

[Elementor 集成](https://github.com/Yoast/wordpress-seo/tree/trunk/src/integrations/third-party/elementor.php)展示了如何将这些 `Metabox` 单元集成到其他页面（本例中是 Elementor 编辑文章页面，参见 `Elementor_Edit_Conditional`），并实现了 `Integration_Interface`。它还展示了如何加载额外的脚本和样式（参见 `register_hooks`）。

#### JavaScript 集成

在 JavaScript 端，起点是 `packages/js/src/post-edit.js`（[GitHub 链接](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/post-edit.js)），它初始化了元框及其行为（`initTabs`、`initAdminMedia`、`initAdmin`、`initializeInsights`、`initializeAiGenerator`）、Redux 存储的初始化（`initEditorStore`），以及与编辑器各部分的集成：
* 与编辑器整体的集成（`initEditorIntegration`）。这里有针对[默认块编辑器](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/initializers/block-editor-integration.js)和[经典编辑器](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/initializers/classic-editor-integration.js)的实现。这些集成确保侧边栏显示并激活高亮功能。
* 一个更新编辑器数据并链接 Yoast SEO 内容分析的抓取器（`initPostScraper`）
* 与主要分类元素的集成（`initPrimaryCategory`）
* 与特色图片元素的集成（`initFeaturedImageIntegration`）

在 `term-edit.js`（[GitHub 链接](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/term-edit.js)）中，我们为分类术语提供了非常相似的集成，但省略了不适用于术语的元素的集成。

对于与其他编辑器的集成，例如 `packages/js/src/elementor.js` 中的 Elementor，我们采用了非常相似的方法（参见[此代码](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/elementor.js)）。不过存在一些细微差别：
- 我们使用监视器（`initElementorWatcher`）而非抓取器来更新编辑器存储。
- YoastSEO 分析应用的设置（参见[文章抓取器](https://github.com/Yoast/wordpress-seo/tree/trunk/packages/js/src/initializers/post-scraper.js)中的 `initializePostAnalysis`）在集成中完成，而非在监视器中。
- 有一些额外代码用于向新用户介绍 Yoast SEO 集成（`initializeIntroduction`）。
- 侧边栏的渲染通过 `initElementorEditorIntegration` 处理。

### 未来规划

在（希望不久的）将来，我们期望逐步将分析工具与其各自的后端解耦，并创建一个“独立”编辑器。
我们的目标是使其易于在任何内容编辑环境中实现，只需将其“指向”相关字段（并根据用户设置和控件“连接”相应的后端行为）。

#### 资源

* 一个简单的示例 [独立应用](https://github.com/Yoast/wordpress-seo/tree/feature/agnostic-analysis/apps/seo-integration)，包含可读性和 SEO 分析（支持多个关键词和同义词）以及一个可运行的片段预览（带有替换变量）
  * 请注意，目前这排除了我们的*包容性语言*、*洞察力*和*社交预览*检查/功能。
* 关于如何配置和集成独立应用的 [文档](https://github.com/Yoast/wordpress-seo/tree/feature/agnostic-analysis/packages/seo-integration)。

请注意，这些资源应被视为正在进行中的工作，在生产环境中可能不稳定。
如果您遇到问题或错误，我们鼓励您在相应的代码仓库中创建问题，和/或贡献修复和补丁。