---
title: "Event"
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

描述一个 `Event`，它在特定时间和地点发生，并可包含额外的可选信息。

## 触发条件
当页面内容描述一个事件时，应将其作为图谱中的顶级节点添加。

## 必需属性
一个有效的 `Event` 必须包含以下属性。

* `@type`: `Event`。
* `@id`: 网站的首页 URL 后追加 `#/schema/Event/{{ID}}`，其中 `{{ID}}` 是一个唯一标识符。
* `name`: 命名该事件的字符串。
* `location`: 一个 `Place` 或 `VirtualLocation` 节点，描述事件发生的地点。
* `startDate`: 事件的开始日期和时间，采用 [ISO-8601 格式](https://en.wikipedia.org/wiki/ISO_8601)（包含 UTC 时区偏移量）。

## 可选属性
以下属性在可用且有效时应添加：

* `description`：描述或总结事件的文本字符串。
* `endDate`：事件的结束日期和时间，采用 [ISO-8601 格式](https://en.wikipedia.org/wiki/ISO_8601)（包含 UTC 时区偏移量）。
* `eventAttendanceMode`：一个 `EventAttendanceModeEnumeration`，反映事件的出席模式（`OfflineEventAttendanceMode`、`OnlineEventAttendanceMode` 或 `MixedEventAttendanceMode`）。
* `image`：一个代表事件的 `imageObject`（或 `imageObject` 数组），通过 ID 引用。
* `organizer`：负责组织事件的 `Organization` 或 `Person`。
* `performer`：在事件中表演的 `Person`（或 `Person` 数组）。

## 条件属性

### 当活动在实体地点（或“线下”）举行时
* `location.name`: 活动地点/场馆的名称。
* `location.address`: 活动地点/场馆的 `PostalAddress`。

### 当活动为“虚拟”（或“在线”）时
* `location.url`：活动将举行的 URL。

### 当活动已取消时
* `eventStatus`：一个 `EventStatusType` 枚举，设置为 `EventCancelled`。

### 当活动已重新安排时
* `eventStatus`：一个 `EventStatusType` 枚举，设置为 `EventRescheduled`。
* `previousStartDate`：活动之前的开始日期（或之前开始日期的数组），格式为 [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601)（包含 UTC 时区偏移）。

### 当事件包含一个或多个“优惠”时（例如，售票活动）
* `offers`: 一个 `Offer`（或一个 `Offer` 数组）。

## 失败场景
如果 `Event` 的任何必填字段缺失或无效，则不应输出该节点。

如果未输出某个节点，任何原本会声明与其存在关系的实体都应移除这些引用。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Event",
            "@id": "https://www.example.com/#/schema/Event/abc123",
            "name": "My awesome party",
            "location": {
                "@type": "Place",
                "name": "Example Venue",
                "address": {
                    "@type": "PostalAddress",
                    "@id": "https://www.example.com/#/schema/PostalAddress/abc123",
                    "streetAddress": "123 Example Street",
                    "addressLocality": "Example Town",
                    "addressRegion": "Example Region",
                    "postalCode": "ABC 123",
                    "postOfficeBoxNumber": "123",
                    "addressCountry": "US"
                }
            },
            "startDate": "2025-07-21T19:00-05:00"
        }
    ]
}`}
</YoastSchemaExample>

### Extended criteria

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Event",
            "@id": "https://www.example.com/#/schema/Event/abc123",
            "name": "My birthday party",
            "description": "We're all getting together in at Example Venue in Fake Town to celebrate my special day!",
            "location": {
                "@type": "Place",
                "name": "Example Venue",
                "address": {
                    "@type": "PostalAddress",
                    "streetAddress": "123 Fake Street",
                    "addressLocality": "Fake Town",
                    "postalCode": "12345",
                    "addressRegion": "XY",
                    "addressCountry": "US"
                }
            },
            "startDate": "2025-07-21T19:00-05:00",
            "endDate": "2025-07-22T19:00-05:00",
            "eventAttendanceMode": "https://schema.org/OfflineEventAttendanceMode",
            "eventStatus": "https://schema.org/EventRescheduled",
            "previousStartDate": "2024-07-21T19:00-05:00",
            "offers": {
                "@type": "Offer",
                "@id": "https://www.example.com/#/schema/Offer/abc123",
                "priceSpecification": {
                    "@type": "PriceSpecification",
                    "price": "22.00",
                    "valueAddedTaxIncluded": "false",
                    "priceCurrency": "GBP"
                },
                "availability": "http://schema.org/InStock",
                "url": "https://www.example.com/example-product-page/",
                "seller": {
                    "@id": "https://www.example.com/#/schema/Organization/1"
                }
            },
            "image": {
                "@id": "https://www.example.com/uploads/example-image.jpg"
            },
            "organizer": {
                "@id": "https://www.example.com/#/schema/Organization/1"
            },
            "performer": {
                "@id": "https://www.example.com/#/schema/Person/abc123"
            }
        }
    ]
}`}
</YoastSchemaExample>