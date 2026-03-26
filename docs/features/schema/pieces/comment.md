---
title: "Comment"
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

描述一条评论。通常在 `Article` 或 `WebPage` 的上下文中使用。

## 触发条件
当其他节点需要时（例如，当 `Article` 包含 `comments` 时），应作为顶级节点添加到图谱中。

## 必需属性
一个有效的 `Comment` 必须包含以下属性。

* `@type`: `Comment`。
* `@id`: 网站的首页 URL，后接 `#/schema/Comment/{{ID}}`，其中 `{{ID}}` 是该评论的唯一标识符。
* `about`: 通过 ID 引用父级 `Article`（或当没有 `Article` `present` 时引用 `WebPage`）。
* `text`: 评论的文本内容，需去除 HTML 标签。
* `author`: 通过 ID 引用撰写评论的 `Person`。

## 失败场景
如果任何必需字段缺失或无效，则不应输出该节点。

## 可选属性
在可用且有效时应添加以下属性：

* `url`: 父级 `WebPage` 未经修改的*规范 URL*，后接 `#comment-{{NUM}}`，其中 `{{NUM}}` 是评论的数据库 ID。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Comment",
              "@id": "https://www.example.com/#/schema/Comment/abc123",
              "about": {
                  "@id": "https://www.example.com/blog/example-article/"
              },
              "text": "Example comment content",
              "author": {
                  "@id": "https://www.example.com/#/schema/Person/abc123"
              }
          }
      ]
  }`}
</YoastSchemaExample>