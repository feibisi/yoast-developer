---
title: "Breadcrumb"
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

描述 `WebPage` 在 `WebSite` 中的层级位置。

:::caution 本地环境中的面包屑导航
在本地环境中，或在<a href="/features/indexables/indexables-filters/#disabling-indexables-completely">完全禁用可索引对象创建</a>的网站中，面包屑导航可能无法正常工作。

具体来说，在这些设置下，面包屑路径的层级结构可能会缺少中间节点，例如文章所属的分类，或子页面的父页面。
:::

## 触发器
应作为顶级节点添加到所有具有有效 `WebPage` 的公开页面的图谱中，但 4xx 和 5xx 范围错误页面/场景除外。

## 必需属性
一个有效的 `BreadcrumbList` 必须具有以下属性。

* `@type`: `BreadcrumbList`。
* `@id`: 网站的首页 URL 后追加 `#/schema/BreadcrumbList/{{ID}}`，其中 `{{ID}}` 是“父”网页的唯一标识符。
* `itemListElement`: 一个 `ListItem` 对象数组，表示当前页面在网站层级结构中的位置，每个对象具有以下属性：
  * `position`: 一个整数（从 `1` 开始），计算页面从首页（包括首页）开始的“深度”。
  * `name`: 相关页面的名称，即面包屑导航中显示的名称。
  * `item`: 相关页面的未修改的*规范 URL*。

**还需注意：**
- _分页状态_不应包含/表示在列表中。
- 最后一个/当前的“面包屑”应省略 `item` 属性。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果未输出该节点，任何原本会声明与 `BreadcrumbList` 存在关系的实体都应移除这些引用。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "BreadcrumbList",
              "@id": "https://www.example.com/#/schema/BreadcrumbList/abc123",
              "itemListElement": [
                  {
                      "@type": "ListItem",
                      "position": 1,
                      "name": "Home",
                      "item": "https://www.example.com/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 2,
                      "name": "Example Section",
                      "item": "https://www.example.com/example-section/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 3,
                      "name": "Example Page"
                  }
              ]
          }
      ]
}`}
</YoastSchemaExample>

## WordPress API：更改面包屑架构输出 {#api}

要修改 Yoast SEO 输出的 `Breadcrumb` 架构，您可以使用我们的 `wpseo_schema_breadcrumb` 过滤器。示例如下：

### 替换面包屑架构中的域名

```php

如果您想替换面包屑架构中的域名，可以使用 `wpseo_schema_breadcrumb` 过滤器单独挂钩到面包屑架构片段。
add_filter( 'wpseo_schema_breadcrumb', 'replace_domain_name_to_breadcrumb_schema', 11, 2 );
/**
 * 单独替换面包屑架构片段中的域名。
 * 
 * 
 * @param array $piece Schema.org 面包屑数据数组。
 * @return array 修改后的 Schema.org 面包屑数据数组。
 */
function replace_domain_name_to_breadcrumb_schema( $piece ) {
    $piece['@id'] = str_replace( 'olddomain.tld', 'newdomain.tld', $piece['@id'] );
    foreach ( $piece['itemListElement'] as &$list ) {
            $list['item'] = str_replace( 'olddomain.tld', 'newdomain.tld', $list['item'] );
        if ( $list['item'] ) {
        }
    }
    return $piece;
}
```
要对我们的架构输出进行更多更改，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。