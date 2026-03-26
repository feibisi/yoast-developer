---
title: "Subscription Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Apis
    - Myyoast
    - Customization
---


Yoast 合作伙伴可使用订阅 API 在 MyYoast 中创建订阅。订阅有助于通过 Yoast 插件的许可证检查。

:::tip
若您希望成为 Yoast 合作伙伴，请联系 [partners@yoast.com](docs/customization/myyoast/apis/mailto:partners@yoast.com)。
:::

我们提供 Swagger 文档供您[探索 API](https://my.yoast.com/provisioning-api/#/)，可提供查看凭证。

## 配置器

配置器是获准在 MyYoast 中创建订阅的 Yoast 合作伙伴。

我们为每位配置器创建一个账户，配置器可使用该账户的凭证调用 API 端点。

该 API 使用基本身份验证。如果您是配置器但尚未获得调用配置 API 的适当凭证，请联系我们。

## 客户端 API

为协助配置人员，我们使用 [swagger-codegen](https://swagger.io/docs/open-source-tools/swagger-codegen/) 为 API 生成了客户端。现有：

- [PHP 客户端](https://github.com/Yoast/subscription-api-php-client)
    - [附额外文档](https://github.com/Yoast/subscription-api-php-client/blob/main/docs/Api/SubscriptionProvisioningApi.md)
- [JavaScript 客户端](https://github.com/Yoast/subscription-api-javascript-client)

这些客户端的自述文件说明了如何安装和使用它们。

## 订阅

订阅为客户的指定站点提供插件在特定时间段内的有效许可。

### 订阅状态

订阅可能存在以下五种状态：

- `active`（有效）：此订阅处于有效状态，将在结束日期过后变为 `expired`（已过期）。
- `pending-cancel`（待取消）：客户已手动取消其订阅。该订阅将不会续订，并在结束日期过后变为 `cancelled`（已取消）。
- `cancelled`（已取消）：订阅已被取消，不再提供有效许可。
- `expired`（已过期）：订阅的结束日期已过且未续订，不再提供有效许可。
- `refunded`（已退款）：订阅已退款，不再提供有效许可。

### 创建订阅

您可以使用[创建端点](https://my.yoast.com/provisioning-api/#/Subscription%20Provisioning/post_api_provisioning_subscriptions_create)来创建新订阅。

创建订阅时，您应同时发送客户希望将其订阅连接到的网站。订阅将作为其连接网站的有效许可证。

如果您知道客户的名字和姓氏，也应一并发送。我们会在沟通中使用这些姓名来称呼客户。

创建订阅时，如果客户尚未拥有账户，系统将为其创建一个 MyYoast 账户。通过此账户，客户可以访问 Yoast SEO Academy 及其高级内容。
随后我们将为客户创建订阅，并将此新订阅作为 API 调用的响应返回。
如果客户账户是新创建的，客户还将收到一封电子邮件，通知他们已为其创建账户，并且可以设置密码。

如果订阅应授予用户访问课程的权限（例如 Yoast SEO Premium 订阅），这些课程可通过 MyYoast 的[课程标签页](https://my.yoast.com/courses)访问。

### 续订订阅

您可以使用[续订端点](https://my.yoast.com/provisioning-api/#/Subscription%20Provisioning/post_api_provisioning_subscriptions__id__renew)来续订现有订阅。

续订订阅会延长订阅期限。延长的时长取决于产品类型。
大多数产品提供一年的访问权限。部分产品提供一个月访问权限。这已为您的供应账户配置或即将配置。

续订订阅会将订阅的结束日期设置为未来的一个新日期。
如果订阅的结束日期已过，则额外时长将从请求日期开始计算。

**已取消或已退款的订阅无法再续订。**

### 使订阅许可证失效

有三种主动方式可使订阅失效：
- 取消并让订阅运行至期限结束。状态：`pending-cancel`
- 立即取消订阅。状态：`cancelled`
- 将订阅标记为已退款。状态：`refunded`

#### 取消订阅

若已知客户不会续订订阅，或客户不应再持有订阅，则应使用[取消端点](https://my.yoast.com/provisioning-api/#/Subscription%20Provisioning/post_api_provisioning_subscriptions__id__cancel)。

该订阅的状态将被设为 `pending-cancel`。在截止日期前它仍是有效许可，之后将变为 `cancelled`。

若需客户立即失去访问权限，可在请求体中设置 `immediately` 为 `true`。此时订阅将立即设为 `cancelled` 状态，无论剩余订阅时长多少。处于 `cancelled` 状态的订阅不再被视为有效许可。

**已取消的订阅无法再次续订。**

#### 订阅退款
如需标记客户购买已退款，应使用[退款端点](https://my.yoast.com/provisioning-api/#/Subscription%20Provisioning/post_api_provisioning_subscriptions__id__refund)。
对订阅执行退款操作意味着相关授权将立即失效。

**已退款的订阅无法续期。**

### 订阅过期与取消

若订阅的结束日期已过，该订阅即将过期。

若订阅状态为 `active`，其状态将被设为 `expired`，且不再视为有效许可。此操作每 30 分钟自动执行一次。

对于状态为 `pending-cancel` 且结束日期已过的订阅，同样适用。此类订阅将被设为 `cancelled`。

## 许可证检查

客户网站上的 Yoast 插件在访问插件页面或高级设置页面时，会发送请求以检查该站点是否拥有该插件的有效许可证。此请求会被缓存。

当站点在 MyYoast 中拥有任何授予该产品访问权限的 `active`（激活）或 `pending-cancel`（待取消）订阅时，即视为对该产品拥有有效许可证。

许可证检查的响应将始终包含用户有权访问的插件最新版本的下载 URL。

### 将网站关联至订阅。

您可以在创建订阅时关联网站，或使用 API 的 [set-site 端点](https://my.yoast.com/provisioning-api/#/Subscription%20Provisioning/post_api_provisioning_subscriptions__id__set_site)。

## 获取插件

客户获得订阅后，即可访问关联产品。

客户有权访问的任何插件均可下载。订阅 API 的任何响应中始终包含该订阅的下载链接。

若订阅包含课程访问权限（例如 Yoast SEO Premium 插件订阅），用户可通过 MyYoast 的[课程页面](https://my.yoast.com/courses)访问这些课程。

当客户获得课程访问权限时，我们也会发送邮件通知。若客户同时获得多门课程访问权限，将发送单封邮件列出所有可访问课程。