---
title: "Person"
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

描述一个独立的个人。最常用于标识内容（例如 `Article` 或 `Comment`）的 `author`。

在某些情况下，也可用于标识 `WebSite`（或其他内容）的 `publisher`。

## 触发器
应在其他节点需要时作为图的顶级节点添加。例如，当 `Article` 有 `author` 时。

## 必需属性
一个有效的 `Person` 必须具有以下属性。

* `@type`: `Person`。
* `@id`: 网站的首页 URL 后接 `#/schema/Person/{{ID}}`，其中 `{{ID}}` 是唯一标识符。
  * 注意，`{{ID}}` 部分不应泄露个人身份或敏感信息（例如，避免使用用户名或电子邮件地址，或对这些值进行哈希加盐处理）。
* `name`: `Person` 的全名。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果未输出该节点，则任何原本会声明与该 `Person` 存在关系的实体（例如，作为 `WebSite` 的 `publisher`，或作为 `Article` 的 `author`）都应移除这些引用。

#### 'Admin' 用户名
如果人员的 `name` 是 '*admin*' 或类似名称（或其本地化等效名称），则该 `Person` 应被标记为无效；我们绝不应将内容显示为由 'admin' 创作。

## 可选属性
以下属性应在可用且有效时添加：

* `description`：用户个人简介，截断至 250 个字符。
* `sameAs`：代表个人公开社交/权威资料页面的 URL 数组（例如维基百科页面或 Facebook 个人主页）。
* `image`：指向代表该人物的 `ImageObject` 节点的 ID 引用数组。
* `url`：用户个人资料页的 URL（若用户与当前网站有关联），或其个人主页/网站的 URL。
* `pronouns`：用户个人资料页中显示的人称代词。

## 条件属性
仅在满足所需条件时才应输出的可选属性。

## 示例

### Minimum criteria

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Person",
              "@id": "https://www.example.com/#/schema/Person/abc123",
              "name": "Example person name"
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
              "@type": "Person",
              "@id": "https://www.example.com/#/schema/Person/abc123",
              "name": "Example person name",
              "image": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "sameAs": [
                  "https://www.wikipedia.com/example-person",
                  "https://www.facebook.com/example-person"
              ]
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API: Change Organization Schema output {#api}

To make changes to the `Person` schema that Yoast SEO outputs, you can use our `wpseo_schema_person` filter. Here is an example:

### Change person output
```php
add_filter( 'wpseo_schema_person', 'schema_change_person', 11, 2 );

/**
 * Changes the Yoast SEO Person schema.
 *
 * @param array             $data    The Schema Person data.
 * @param Meta_Tags_Context $context Context value object.
 *
 * @return array $data The Schema Person data.
 */
function schema_change_person( $data, $context ) {
	if ( isset( $data['worksFor'] ) && $data['worksFor'] === 'Yoast' ) {
		// Make references to "Yoast" actually reference the organization's graph piece.
		$data['worksFor'] = [ '@id' => $context->site_url . Schema_IDs::ORGANIZATION_HASH ];
	}

	return $data;
}
```

### Change person being output

If you want to change the person being output in the schema, you can filter it like this:

```php
add_filter( 'wpseo_schema_person_user_id', 'change_schema_person_id' );

/**
 * Changes the Yoast SEO Person schema.
 *
 * @param int $person_id The Schema Person ID.
 *
 * @return int $person_id The (possibly altered) person ID.
 */
function change_schema_person_id( $person_id ) {
    if ( $person_id === 12 ) {
        return 3; // Make sure this is a valid user ID.
    }
    return $person_id;
}
```

### 社交资料
若需调整 `Person` 输出中 `sameAs` 数组显示的社交资料，可通过 `wpseo_schema_person_social_profiles` 过滤器实现。我们在 yoast.com 网站中运用此方法，将成员的 GitHub、WordPress 资料及个人网站添加至 sameAs 输出：

```php
add_filter( 'wpseo_schema_person_social_profiles', 'yoast_add_social_profiles' );

/**
 * 向 sameAs 数组添加社交资料。
 *
 * @param array $profiles 社交资料数组。
 *
 * @return array 更新后的社交资料数组。
 */
function yoast_add_social_profiles( $profiles ) {
    array_push( $profiles, 'github', 'personal', 'wordpress' );

    return $profiles;
}
```

如需对 Schema 输出进行更多调整，请参阅 [Yoast SEO Schema API](docs/features/schema/api)。