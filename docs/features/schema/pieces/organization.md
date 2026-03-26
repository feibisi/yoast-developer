---
title: "Organization"
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

描述一个组织（公司、企业或机构）。最常用于标识 [`WebSite`](docs/features/schema/pieces/website) 的发布者。

如果满足所需条件，可以转换为更具体的类型（例如 `Corporation` 或 `LocalBusiness`）。

## 触发器
应在所有公开页面上输出。

## 必需属性
一个有效的 `Organization` 必须包含以下属性。

* `@type`: `Organization`。
* `@id`: 网站主页 URL 后追加 `#/schema/Organization/{{ID}}`，其中 `{{ID}}` 是唯一标识符。
  * 如果该 `organization` 是 `WebSite` 的发布者（即“*主要组织*”或“*本组织*”），则 `{{ID}}` 的值应为 `1`。
* `name`: `Organization` 的名称。
* `URL:` 网站的主页 URL。
* `logo`: 对组织徽标 `ImageObject` 的 ID 引用。

### 失败场景
如果任何必填字段缺失或无效，则不应输出该节点。

如果未输出该节点，任何原本会声明与 `Organization` 存在关系的实体（例如，作为 `WebSite` 的 `publisher`，或 `Product` 的 `manufacturer`）应移除这些引用。

## 可选属性
以下属性应在可用且有效时添加：

* `sameAs`：一个 URL 数组，代表组织声明的社交/权威资料（例如，维基百科页面或 Facebook 个人资料）。
* `image`：一个通过 ID 引用 `ImageObject` 节点的数组，这些节点代表组织（包括 `logo`）。
* `alternateName`：组织的替代（通常为*更短的*）名称。
* `hasMerchantReturnPolicy`：一个 `MerchantReturnPolicy` 类型的数组，代表组织的退货政策。

## 转换
`Organization` 类型可能在以下场景中进行转换。

### 当业务为“本地”时
* 当位置信息可用时，`Organization` 可能有资格扩展为 [LocalBusiness](docs/features/schema/pieces/localbusiness) 类型。

## 示例

### 最低标准

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/abc123",
              "url": "https://www.example.com/",
              "logo": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "name": "示例组织名称"
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
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/Organization/abc123",
              "url": "https://www.example.com/",
              "name": "Example organization name",
              "alternateName": "Short org name",
              "sameAs": [
                  "https://www.wikipedia.com/example-organization",
                  "https://www.linkedin.com/company/1234"
              ],
              "logo": {
                  "@id": "https://www.example.com/uploads/example-image.jpg"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/uploads/example-image.jpg"
                  },
                  {
                      "@id": "https://www.example.com/uploads/example-image-2.jpg"
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>

## WordPress API：修改组织架构输出 {#api}

要修改 Yoast SEO 输出的 `Organization` 架构，您可以使用我们的 `wpseo_schema_organization` 过滤器。示例如下：

```php
add_filter( 'wpseo_schema_organization', 'change_organization_schema', 11, 2 );

/**
 * 向 Yoast SEO 组织架构添加额外属性
 *
 * @param array             $data    架构组织数据。
 * @param Meta_Tags_Context $context 上下文值对象。
 *
 * @return array $data 架构组织数据。
 */
function change_organization_schema( $data, $context ) {
	$data['@type'] = [
		'Organization',
		'Brand',
	];
	$data['founder'] = [
		'@type'  => 'Person',
		'name'   => 'Joost de Valk',
		'sameAs' => 'https://en.wikipedia.org/wiki/Joost_de_Valk',
	];
	$data['foundingDate']       = '2010-05-01';
	$data['numberOfEmployees']  = (int) wp_count_posts( 'yoast_employees' )->publish;
	$data['slogan']             = 'SEO for Everyone';
	$data['description']        = 'Yoast helps you with your website optimization, whether it be through our widely used SEO software or our online SEO courses: we&#039;re here to help.';
	$data['url']                = 'https://yoast.com/';
	$data['legalName']          = 'Yoast BV';
	$data['alternateName']      = 'Yoast';
	$data['parentOrganization'] = [
		'@type'       => 'Organization',
		'name'        => 'Newfold Digital',
		'description' => 'Newfold Digital is a leading web presence solutions provider serving millions of small-to-medium businesses globally.',
		'url'         => 'https://newfold.com/',
		'sameAs'      => [
			'https://newfold.com/',
		],
		'logo'        => 'https://yoast.com/app/uploads/2022/01/newfold-logo.png',
	];
	$data['memberOf']           = [
		'@type'       => 'Organization',
		'name'        => 'World Wide Web Consortium (W3C)',
		'description' => 'The World Wide Web Consortium (W3C) is an international community that develops open standards to ensure the long-term growth of the Web.',
		'url'         => 'https://w3.org/',
		'sameAs'      => [
			'https://w3.org/',
		],
		'logo'        => 'https://www.w3.org/Icons/w3c_main.png',
	];

	return $data;
}
```

要对我们的架构输出进行更多修改，请参阅 [Yoast SEO 架构 API](docs/features/schema/api)。