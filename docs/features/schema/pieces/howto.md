---
title: "Howto"
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

描述一个包含一系列 `steps` 的 `HowTo` 指南。

## 触发条件
当页面内容包含 *HowTo 区块* 时，应将其作为顶级节点添加到图谱中。

## 必需属性
有效的 `HowTo` 必须包含以下属性。

* `@type`: `HowTo`。
* `@id`: 网站的首页 URL 后追加 `#/schema/HowTo/{{ID}}`，其中 `{{ID}}` 是当前“操作指南”的唯一标识符。
* `mainEntityOfPage`: 通过 ID 引用 `WebPage`。
* `name`: 描述指南的字符串。
* `step`: `HowToStep` 对象的数组，包含以下属性：
 * `@id`: 网站的首页 URL 后追加 `#/schema/HowToStep/{{ID}}`，其中 `{{ID}}` 是当前“操作步骤”的唯一标识符。
 * `url`: 指向单个 `HowToStep` 片段标识符（“ID 锚点”）的链接（例如，`https://www.example.com/example-page/#howtostep-5`）。
 * 必须包含 `name` 或 `text` 属性之一（参见*条件场景*）。

## 故障场景
如果 `HowTo` 或 `step` 的任何必填字段缺失或无效，则不应输出该节点。

如果未输出某个节点，则任何原本会声明与其存在关系的实体都应移除这些引用。

## 可选属性
以下属性应在可用且有效时添加：

* `totalTime`：完成指南所需的总时长，采用 ISO 8601 持续时间格式。
* `description`：与 HowTo 指南相关的介绍或描述内容。
* `inLanguage`：指南的语言代码；例如 `en-GB`。

## 条件属性

### 当页面包含有效的 `Article` 时
* 将 `mainEntityOfPage` 属性修改为通过 ID 引用 `Article`（而非 `WebPage`）。

### 当 HowToStep 包含一个或多个图像时
* `image`: 一个 `imageObject`（或一个指向 `ImageObject` 节点的 ID 引用数组）。

### 当 `HowToStep` 具有名称和描述时
* `name`: 步骤的摘要或标题。
* `itemListElement`: 一个 `HowToDirection` 对象数组，每个对象具有 `text` 属性，对应描述中的每个段落。

### 当 `HowToStep` 有 *name* 但 *description* 中没有文本时
* `text`：步骤的摘要或标题。

### 当 `HowToStep` 有 *description* 但没有 *name* 时
* `itemListElement`：一个 `HowToDirection` 对象数组，每个对象具有 `text` 属性，对应描述中的每个段落。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "HowTo",
              "@id": "https://www.example.com/#/schema/HowTo/abc123",
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/example-page/"
              },
              "name": "HowTo example",
              "step": [
                  {
                      "@type": "HowToStep",
                      "@id": "https://www.example.com/#/schema/HowToStep/abc123",
                      "name": "Example step 1",
                      "url": "https://www.example.com/example-page/#how-to-step-1",
                      "itemListElement": [
                          {
                              "@type": "HowToDirection",
                              "text": "The step description."
                          }
                      ]
                  },
                  {
                      "@type": "HowToStep",
                      "text": "Example step 2 (no description)",
                      "url": "https://www.example.com/example-page/#how-to-step-2"
                  }
              ]
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
              "@type": "HowTo",
              "@id": "https://www.example.com/#/schema/HowTo/abc123",
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/#/schema/Article/abc123"
              },
              "name": "HowTo example",
              "description": "HowTo description",
              "inLanguage": "en-US",
              "totalTime": "P2DT0H0M",
              "step": [
                  {
                      "@type": "HowToStep",
                      "@id": "https://www.example.com/#/schema/HowToStep/abc123",
                      "name": "Example step 1",
                      "url": "https://www.example.com/example-page/#how-to-step-1",
                      "image": {
                          "@id": "https://www.example.com/uploads/example-image.jpg"
                      },
                      "itemListElement": [
                          {
                              "@type": "HowToDirection",
                              "text": "The step description."
                          }
                      ]
                  },
                  {
                      "@type": "HowToStep",
                      "@id": "https://www.example.com/#/schema/HowToStep/def456",
                      "text": "Example step 2 (no description)",
                      "url": "https://www.example.com/example-page/#how-to-step-2",
                      "image": {
                          "@id": "https://www.example.com/uploads/example-image-2.jpg"
                      }
                  },
                  {
                      "@type": "HowToStep",
                      "@id": "https://www.example.com/#/schema/HowToStep/ghi789",
                      "name": "Example step 3",
                      "url": "https://www.example.com/example-page/#how-to-step-3",
                      "itemListElement": [
                          {
                              "@type": "HowToDirection",
                              "text": "The first paragrah in the step description."
                          },
                          {
                              "@type": "HowToDirection",
                              "text": "The second paragrah in the step description."
                          }
                      ]
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>