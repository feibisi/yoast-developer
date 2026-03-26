---
title: "Yoast Seo Premium"
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


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

我们 [Yoast SEO Premium](https://yoast.com/product/yoast-seo-premium-wordpress/) 的 schema 输出建立在 [Yoast SEO schema 输出](docs/features/schema/plugins/yoast-seo) 的基础上，旨在为组织添加更多详细信息。

## 核心功能

### 额外的可选属性
以下属性应在可用且有效时添加：

#### 网站政策：
* `publishingPrinciples`：指向描述贵组织编辑原则页面的链接。你们写什么内容，为谁而写，以及为什么？
* `ownershipFundingInfo`：指向描述贵组织所有权结构页面的链接。它应包含有关资金和资助的信息。
* `actionableFeedbackPolicy`：指向描述贵组织如何收集和回应反馈、与公众互动以及优先考虑透明度页面的链接。
* `correctionsPolicy`：指向概述贵组织处理错误（例如，发布撤回或更正）程序页面的链接。
* `ethicsPolicy`：指向描述贵组织期望的个人、组织和企业行为标准页面的链接。
* `diversityPolicy`：指向提供贵组织编辑内容多样性政策信息页面的链接。
* `diversityStaffingReport`：指向提供贵组织人员配置、招聘和就业多样性政策信息页面的链接。

附加组织信息：

通过提供更深入的信息来丰富贵组织的资料。分享的细节越多，Google 就越能理解您的网站。
* `description`：组织的简短描述。
* `email`：组织的主要公开电子邮件地址。
* `telephone`：组织的主要公开电话号码。
* `legalName`：组织的法定名称。
* `foundingDate`：公司的成立日期，采用 ISO 8601 格式（例如，`2020-01-20`）。
* `numberOfEmployees`：员工数量，采用 `QuantitativeValue` 格式。

组织标识符：
* `vatID`：组织或个人的增值税 ID。
* `taxID`：组织或个人的税务/财政 ID，例如美国的 TIN 或西班牙的 CIF/NIF。
* `iso6523Code`：以 `ISO 6523(-1)` 格式定义的组织标识符。
* `duns`：用于识别组织或商业人士的邓白氏 DUNS 号码。
* `leiCode`：唯一标识法律实体的组织标识符，定义于 `ISO 17442`。
* `naics`：特定组织或商业人士的北美行业分类系统 (`NAICS`) 代码。