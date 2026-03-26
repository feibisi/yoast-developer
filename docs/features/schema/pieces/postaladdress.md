---
title: "Postaladdress"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Pieces
    - Schema
    - Features
---


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

描述一个地点的邮政地址；通常在 `LocalBusiness` 的上下文中使用。

## 触发条件
应在图中作为顶级节点添加，根据其他节点的需要。例如，当 `LocalBusiness` 拥有一个地址时。

## 必需属性
一个有效的 `postalAddress` 必须包含以下属性。

* `@type`: `PostalAddress`。
* `@id`: 网站的首页 URL，后接 `#/schema/PostalAddress/{{ID}}`，其中 `{{ID}}` 是唯一标识符。
* `streetAddress`: 门牌号和街道（例如，*123 fake road*）。
* `postalCode`: 邮政编码。
* `addressCountry`: 代表国家的两位国家代码（例如，*US*）。

### 失败场景
如果任何必需字段缺失或无效，则不应输出该节点。

如果节点未输出，任何原本会声明与 `Address` 关系的实体（例如，当被 `localBusiness` 引用时）应移除这些引用。

## 可选属性
以下属性应在可用且有效时添加：

* `addressLocality`: 城镇、城市或等同区域。
* `addressRegion`: 地区或行政区。
* `postOfficeBoxNumber`: 邮政信箱号码。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "PostalAddress",
              "@id": "https://www.example.com/#/schema/PostalAddress/abc123",
              "streetAddress": "123 Fake Street",
              "postalCode": "ABC 123",
              "addressCountry": "US"
          }
      ]
  }`}
</YoastSchemaExample>

### 扩展标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "PostalAddress",
              "@id": "https://www.example.com/#/schema/PostalAddress/abc123",
              "streetAddress": "123 Example Street",
              "addressLocality": "Example Town",
              "addressRegion": "Example Region",
              "postalCode": "ABC 123",
              "postOfficeBoxNumber": "123",
              "addressCountry": "US"
          }
      ]
  }`}
</YoastSchemaExample>