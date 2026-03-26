---
title: "Changing Organization Url In Schema"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Local Seo
    - Customization
    - Repos
---


自 13.9 版本起，您或您的开发者可以使用 `yoast-local-seo-schema-organization-url` 过滤器来更改 [Schema](https://developer.yoast.com/features/schema/pieces/organization/) 中 `Organization` 部分的默认 URL。此过滤器接受一个参数：`$url`（必需）。
当您组织的 URL 与网站首页 URL 不匹配时，此过滤器尤其有用。

在您的自定义插件或主题的 `functions.php` 文件中，添加以下代码行，并将默认值更改为您所需的 URL。

```php
/** 
 * 更改 Schema 中主要 Organization 部分的 URL。
 *
 * @param string $url 我们要替换的 URL。
 *
 * @return string Schema 中 Organization 部分的 URL。
 */
function my_custom_organization_url( $url ) {
    return 'https://mycustomurl.tld';
}

add_filter( 'yoast-local-seo-schema-organization-url', 'my_custom_organization_url', 10, 1 );
```