---
title: "Localbusiness"
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

描述一个允许公众访问的企业。通常用于表示网站“背后”的企业，或关于特定企业的页面。

如果满足所需条件，可以转换为更具体的类型（例如 `library`），或扩展现有的 `Organization`。

## 触发器
* 应在代表相关企业的页面上输出。
* 当网站代表相关企业或是该企业的 `branchOf` 时，应在主页上输出。
* 当企业与代表整个站点的实体相同（例如，网站代表一个具有本地属性的 `Organization`）时，如果满足以下要求，该 `Organization` 应被修改/扩展为此 `LocalBusiness`。

## 必需属性
一个有效的 `LocalBusiness` 必须包含以下属性。

* `@type`: 一个包含 `Organization`、`Place` 以及所选最具体子类型（例如 `['Organization', 'Place', 'Dentist']`）的数组。
* `@id`: 网站的首页 URL，后接 `#/schema/Organization/{{ID}}`，其中 `{{ID}}` 是相关组织的唯一标识符。
* `name`: 企业名称。
* `image`: 指向代表该企业的 `imageObject` 的引用（通过 ID）。
* `address`: 指向 `PostalAddress` 片段的引用（通过 ID）。

### 故障场景
若任何必填字段缺失或无效，节点应回退为常规 `Organization`（移除所有'本地' `@type` 值），同时保留下方所有可选属性。

## 可选属性
以下属性应在可用且有效时添加：

* `telephone`：企业的主要公开电话号码。
* `email`：企业的主要公开电子邮件地址。
* `faxNumber`：企业的主要公开传真号码。
* `priceRange`：企业的价格范围，由美元符号字符串表示（例如 `$`、`$$` 或 `$$$`）。
* `areaServed`：由 `GeoShape`、`Place` 或字符串定义组成的数组。
* `openingHoursSpecification`：一个 `OpeningHoursSpecification` 对象。
* `geo`：一个 `GeoCoordinates` 对象。
* `url`：代表该企业的页面的规范 URL。
* `vatID`：企业的增值税号。
* `taxID`：企业的税号。
* `globalLocationNumber`：企业的全球位置码。
* `contactPoint`：一个 `ContactPoint` 对象。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": [
                  "Organization",
                  "Place",
                  "Dentist"
              ],
              "@id": "https://www.example.com/#/schema/Organization/abc123",
              "name": "Example business name",
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "address": {
                  "@id": "https://www.example.com/#/schema/Address/abc123"
              }
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
              "@type": [
                  "Organization",
                  "Place",
                  "Dentist"
              ],
              "@id": "https://www.example.com/#/schema/Organization/abc123",
              "url": "https://www.example.com/example-business-page/",
              "email": "example@example.com",
              "priceRange": "$$",
              "areaServed": [
                  "Bournemouth",
                  "Poole",
                  "Dorset"
              ],
              "telephone": "01234 56789",
              "faxNumber": "02345 67890",
              "name": "Example business name",
              "openingHoursSpecification": {
                  "@type": "OpeningHoursSpecification",
                  "dayOfWeek": [
                      "Monday",
                      "Tuesday",
                      "Wednesday",
                      "Thursday",
                      "Friday",
                      "Saturday",
                      "Sunday"
                  ],
                  "opens": "09:00",
                  "closes": "17:00"
              },
              "geo": {
                  "@type": "GeoCoordinates",
                  "latitude": "51.8066986",
                  "longitude": "5.7284488"
              },
              "vatID": "abc123",
              "taxID": "def456",
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "address": {
                  "@id": "https://www.example.com/#/schema/Address/abc123"
              }
          }
      ]
  }`}
</YoastSchemaExample>