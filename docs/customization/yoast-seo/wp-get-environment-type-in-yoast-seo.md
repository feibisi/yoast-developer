---
title: "Wp Get Environment Type In Yoast Seo"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo
    - Customization
    - Repos
---


自 WordPress 5.5 起，可以使用新的 [`wp_get_environment_type()`](https://developer.wordpress.org/reference/functions/wp_get_environment_type/) 函数来获取网站的当前环境。这尤其使得主题和插件的作者能够轻松地了解网站的环境，并可能为每种环境类型提供特定的功能。

Yoast SEO 利用此功能来启用或禁用插件的某些选项，这些选项对于每种环境类型可能相关或不相关。

## 使用 `wp_get_environment_type()` 获取网站环境

`wp_get_environment_type()` 的使用相当简单直接。该函数返回的值由全局变量 `WP_ENVIRONMENT_TYPE` 定义。

此变量的可能值为 `local`、`development`、`staging` 和 `production`。

因此，会对函数的返回值进行条件检查，并根据结果执行给定的指令。

以下是实际应用中的示例：

```php
<?php
switch ( wp_get_environment_type() ) {
    case 'local':
    case 'development':
        do_nothing();
        break;
    
    case 'staging':
        do_staging_thing();
        break;
    
    case 'production':
    default:
        do_production_thing();
        break;
}
```

一些快速细节。如果 `wp_get_environment_type()` 返回 `development`，[`WP_DEBUG` 常量的值将被设置为 true](https://github.com/WordPress/wordpress-develop/blob/029dd1bbb929eec4df5f57789ef42c076aab83f0/src/wp-includes/default-constants.php#L77)，前提是未在 wp-config.php 文件中预先定义。

WordPress 的另一项功能，应用程序密码，也与 `wp_get_environment_type()` 返回的值相关联。[当此值为 `local` 时，默认可用](https://github.com/WordPress/wordpress-develop/blob/73b353ef95d61177fd246f6c316382c3a94e44a7/src/wp-includes/user.php#L4306)。

## Yoast SEO 如何使用 `wp_get_environment_type()`？

### 在非生产环境中

在非实时网站上 - `if ( wp_get_environment_type() !== 'production' )` - ，有几项功能是不必要的。因此，在这些情况下，插件不会获取 Ryte 数据，不会执行 Ryte 站点健康检查，会禁用数据跟踪，并禁用关于 XML 站点地图的搜索引擎 ping 通知。