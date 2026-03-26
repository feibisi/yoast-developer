---
title: "Question"
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

描述一个 `Question`。最常用于 `FAQPage` 或 `QAPage` 内容中。

## 触发条件
当页面内容包含 *FAQ 块* 时，应将其作为顶级节点添加到图谱中。

## 必需属性
一个有效的 `Question` 必须包含以下属性。

* `@type`: `Question`。
* `@id`: 网站的首页 URL 后追加 `#/schema/Question/{{ID}}`，其中 `{{ID}}` 是唯一标识符。
* `name`: 问题的文本内容。
* `acceptedAnswer`: 一个 `Answer` 对象，其 `text` 属性包含问题的答案。

## 失败场景
如果 `Question` 的任何必需字段缺失或无效，则不应输出该节点。

如果某个节点未被输出，则任何原本会声明与其关系的实体都应移除这些引用。

## 可选属性
以下属性在可用且有效时应添加：

* `inLanguage`: 问题的语言代码；例如，`en-GB`。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Question",
              "@id": "https://www.example.com/#/schema/Question/abc123",
              "name": "What is the return policy?",
              "acceptedAnswer": {
                  "@type": "Answer",
                  "text": "Most unopened items in new condition and returned within 90 days will receive a refund or exchange. Some items have a modified return policy noted on the receipt or packing slip. Items that are opened or damaged or do not have a receipt may be denied a refund or exchange. Items purchased online or in-store may be returned to any store. Online purchases may be returned via a major parcel carrier."
              }
          }
      ]
  }`}
</YoastSchemaExample>