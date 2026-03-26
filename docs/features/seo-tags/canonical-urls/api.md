---
title: "Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Canonical Urls
    - Seo Tags
    - Features
---


要以编程方式更改 Yoast SEO 为 URL 生成的规范链接，你可以使用 `wpseo_canonical` 过滤器。

## 更改规范链接
例如，以下代码将更改 ID 为 `12345` 的页面的规范链接：

```php
/**
 * 过滤规范链接。
 *
 * @param string $canonical 当前页面生成的规范链接。
 *
 * @return string 过滤后的规范链接。
 */
function prefix_filter_canonical_example( $canonical ) {
  if ( is_page( 12345 ) ) {
    $canonical = 'https://example.com/canonical-url/';
  }

  return $canonical;
}

add_filter( 'wpseo_canonical', 'prefix_filter_canonical_example' );
```

## 移除规范链接
如果你想完全移除某个页面的规范链接，只需为该页面返回 false，如下所示：

```php
/**
 * 过滤规范链接。
 *
 * @param string $canonical 当前页面生成的规范链接。
 *
 * @return bool|string 过滤后的规范链接，或 false 表示要移除它。
 */
function prefix_filter_canonical_example( $canonical ) {
  if ( is_page( 12345 ) ) {
    return false;
  }

  return $canonical;
}

add_filter( 'wpseo_canonical', 'prefix_filter_canonical_example' );
```

或者要从所有页面移除，只需这样做：

```php
add_filter( 'wpseo_canonical', '__return_false' );
```