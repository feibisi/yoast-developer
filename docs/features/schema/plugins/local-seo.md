---
title: "Local Seo"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Plugins
    - Schema
    - Features
---


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

[Local SEO for WordPress](https://yoast.com/wordpress/plugins/local-seo/) 的架构输出基于 [Yoast SEO for WordPress 的基础架构输出](docs/features/schema/plugins/yoast-seo)，增加了额外的*本地商家*详细信息（例如地址和营业时间）。

## 核心逻辑

### 当定义主要地点时
* 将企业*主要地点*的属性合并到主 `Organization`（即*基础脚本*中的那个）中，尝试将其扩展为 `LocalBusiness`。

### 当未定义主要地点时
* 将任何共享/默认地点属性（如 `email`、`VAT`）添加到主 `Organization` 中，但 `geo`、`areasServed` 和 `priceRange` 属性除外。

## 其他应用场景

### 在单个地点页面

#### 当页面*不*代表主要地点时
* 为相关地点添加 `LocalBusiness` 片段。

##### 当网站代表拥有多个地点的单一企业时
* 在 `LocalBusiness` 中添加 `branchOf` 属性，该属性引用主 `Organization` 片段的 ID。

#### 所有情况下
* 在 `LocalBusiness` 片段中添加 `mainEntityOfPage` 属性，该属性引用 `WebPage` 的 ID。
  * 注意：对于主要位置，此属性应附加到基础脚本中的主 `Organization` / `LocalBusiness` 片段上。

## 示例

### 单地点商业网站上的任意文章
此示例假设：
* 文章包含 / 是一个有效的 `Article`。

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
              "@id": "https://www.example.com/#/schema/organization/1",
              "url": "https://www.example.com/",
              "email": "example@example.com",
              "logo": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "priceRange": "$$",
              "areaServed": "Bournemouth, Poole, Dorset",
              "telephone": ["01234 56789", "02345 67890"],
              "currenciesAccepted": "EUR, GBP",
              "paymentAccepted": "Cash, cheque, most major credit cards",
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
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "address": {
                  "@id": "https://www.example.com/#/schema/address/abc123"
              }
          },
          {
              "@type": "PostalAddress",
              "@id": "https://www.example.com/#/schema/address/abc123",
              "streetAddress": "123 Example Street",
              "addressLocality": "Example Town",
              "addressRegion": "Example Region",
              "postalCode": "ABC 123",
              "postOfficeBoxNumber": "123",
              "addressCountry": "US"
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/website/1",
              "url": "https://www.example.com/",
              "name": "Example website",
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-post/",
              "url": "https://www.example.com/example-post/",
              "name": "Example post name",
              "description": "Example post description",
              "keywords": "cats,dogs,cake",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/website/1"
              },
              "inLanguage": "en-US",
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ]
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/abc123",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/def456",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "Article",
              "@id": "https://www.example.com/example-post/#/schema/article/abc123",
              "headline": "Example article headline",
              "description": "Example article description",
              "isPartOf": {
                  "@id": "https://www.example.com/example-post/"
              },
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/example-post/"
              },
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "commentCount": 6,
              "articleSection": "Cats,Dogs,Recipes",
              "author": {
                  "@id": "https://www.example.com/#/schema/person/abc123"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              },
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ],
              "video": [
                  {
                      "@id": "https://www.example.com/#/schema/video/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/video/def456"
                  }
              ]
          },
          {
              "@type": "Person",
              "@id": "https://www.example.com/#/schema/person/abc123",
              "name": "Example person name",
              "image": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "sameAs": [
                  "https://www.wikipedia.com/example-person",
                  "https://www.facebook.com/example-person"
              ]
          }
      ]
  }`}
</YoastSchemaExample>

### 多地点企业的位置存档
此示例假设：

* 已设置部分共享/默认值，因此主 `Organization` 拥有 `@type` 数组、`email`、`vatID` 及其他与地址无关的位置属性。
* 未设置主要地点，因此无法为主 `Organization` 推断 `address`。

<YoastSchemaExample>
{`{
      "@context": "https://schema.org",
      "@graph": [
          {
              "@type": "Organization",
              "@id": "https://www.example.com/#/schema/organization/1",
              "url": "https://www.example.com/",
              "email": "example@example.com",
              "logo": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "telephone": "01234 56789",
              "faxNumber": "02345 67890",
              "vatID": "abc123",
              "taxID": "def456",
              "currenciesAccepted": "USD",
              "paymentAccepted": "Credit card only",
              "name": "Example business name",
              "image": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              }
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/website/1",
              "url": "https://www.example.com/",
              "name": "Example website",
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/example-local-archive/",
              "url": "https://www.example.com/example-local-archive/",
              "name": "Example local archive name",
              "description": "Example local archive description",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/website/1"
              },
              "inLanguage": "en-US",
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ]
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/abc123",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/def456",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "ItemList",
              "@id": "http://www.example.com/page/#postlist",
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/example-local-archive/"
              },
              "numberOfItems": 81,
              "itemListElement": [
                  {
                      "@type": "ListItem",
                      "position": 1,
                      "name": "Location One",
                      "url": "https://www.example.com/location-one/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 2,
                      "name": "Location Two",
                      "url": "https://www.example.com/location-two/"
                  },
                  {
                      "@type": "ListItem",
                      "position": 3,
                      "name": "Location Three",
                      "url": "https://www.example.com/location-three/"
                  }
              ]
          }
      ]
  }`}
</YoastSchemaExample>

请注意，itemList 仅出于演示目的被截断为三个项目。

### 单个地点页面
此示例假设：

* 网站代表一个拥有多个地点的单一企业，因此添加了 `branchOf` 属性。
* 已设置主要地点，因此主 `Organization` 拥有一个 `address` 和多个 `@type` 值。

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
              "@id": "https://www.example.com/#/schema/organization/1",
              "url": "https://www.example.com/",
              "email": "example@example.com",
              "logo": {
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "telephone": ["01234 56789", "02345 67890"],
              "currenciesAccepted": "EUR, GBP, USD",
              "paymentAccepted": "Cash only",
              "priceRange": "$$",
              "areaServed": [
                  "Bournemouth",
                  "Poole",
                  "Dorset"
              ],
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
                  "@id": "https://www.example.com/#/schema/image/abc123"
              },
              "address": {
                  "@id": "https://www.example.com/#/schema/address/abc123"
              }
          },
          {
              "@type": "PostalAddress",
              "@id": "https://www.example.com/#/schema/address/abc123",
              "streetAddress": "123 Example Street",
              "addressLocality": "Example Town",
              "addressRegion": "Example Region",
              "postalCode": "ABC 123",
              "postOfficeBoxNumber": "123",
              "addressCountry": "US"
          },
          {
              "@type": "PostalAddress",
              "@id": "https://www.example.com/#/schema/address/def456",
              "streetAddress": "456 Example Street",
              "addressLocality": "Example City",
              "addressRegion": "Example County",
              "postalCode": "DEF 456",
              "postOfficeBoxNumber": "456",
              "addressCountry": "UK"
          },
          {
              "@type": "WebSite",
              "@id": "https://www.example.com/#/schema/website/abc123",
              "url": "https://www.example.com/",
              "name": "Example website",
              "potentialAction": {
                  "@type": "SearchAction",
                  "target": "https://www.example.com/?s={search_term_string}",
                  "query-input": "required name=search_term_string"
              },
              "publisher": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              }
          },
          {
              "@type": "WebPage",
              "@id": "https://www.example.com/single-location-page/",
              "url": "https://www.example.com/single-location-page/",
              "name": "Example single location page name",
              "description": "Example single location page description",
              "isPartOf": {
                  "@id": "https://www.example.com/#/schema/website/abc123"
              },
              "inLanguage": "en-US",
              "datePublished": "2019-07-10T08:08:40+00:00",
              "dateModified": "2019-07-10T08:43:03+00:00",
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ]
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/abc123",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": "ImageObject",
              "@id": "https://www.example.com/#/schema/image/def456",
              "url": "https://www.example.com/uploads/example-image.jpg",
              "caption": "Example caption",
              "width": 120,
              "height": 120
          },
          {
              "@type": [
                  "Organization",
                  "Place",
                  "Dentist"
              ],
              "@id": "https://www.example.com/#/schema/organization/def456",
              "mainEntityOfPage": {
                  "@id": "https://www.example.com/single-location-page/"
              },
              "branchOf": {
                  "@id": "https://www.example.com/#/schema/organization/1"
              },
              "url": "https://www.example.com/single-location-page/",
              "email": "example2@example.com",
              "priceRange": "$$$",
              "areaServed": [
                  "Manchester",
                  "London",
                  "Scotland"
              ],
              "telephone": "01234 56789",
              "faxNumber": "02345 67890",
              "paymentAccepted": "Cash, cheque",
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
                  "latitude": "1.2345678",
                  "longitude": "2.3456789"
              },
              "vatID": "abc123",
              "taxID": "def456",
              "image": [
                  {
                      "@id": "https://www.example.com/#/schema/image/abc123"
                  },
                  {
                      "@id": "https://www.example.com/#/schema/image/def456"
                  }
              ],
              "address": {
                  "@id": "https://www.example.com/#/schema/address/def456"
              }
          }
      ]
  }`}
</YoastSchemaExample>

## 更多信息
* [LocalBusiness Schema 片段](docs/features/schema/pieces/localbusiness)
* [Article Schema 片段](docs/features/schema/pieces/article)
* [Organization Schema 片段](docs/features/schema/pieces/organization)
* [PostalAddress Schema 片段](docs/features/schema/pieces/postaladdress)
* [WebPage Schema 片段](docs/features/schema/pieces/webpage)