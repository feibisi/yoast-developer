---
title: "Disabling Automatic Redirects Notifications"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo Premium
    - Customization
    - Repos
---


Yoast SEO Premium 的[重定向管理器](https://yoast.com/wordpress/plugins/seo/redirects-manager/)包含自动创建重定向和重定向请求通知功能。在极少数情况下，您可能希望关闭其中某些功能。

## 禁用自动创建重定向
Yoast SEO Premium 会监控 URL 变更并自动创建重定向。在大多数情况下，这是防止访问者遇到 `404` 错误页面的理想行为。但是，您可以通过将以下代码添加到主题的 `functions.php` 文件来禁用此功能：

### 文章和页面
```php
add_filter('Yoast\WP\SEO\post_redirect_slug_change', '__return_true' );
```

### 分类法（分类、标签等）
```php
add_filter('Yoast\WP\SEO\term_redirect_slug_change', '__return_true' );
```

注意：如果您看到已创建重定向的通知，但您并未更改 URL 或别名，这通常是由冲突引起的。请按照[本文](https://yoast.com/kb/how-to-check-for-plugin-conflicts)中的步骤检查是否存在冲突。

## 禁用重定向通知
当您删除网站上的内容时，我们会显示提醒通知，建议为已删除的项目添加重定向。我们强烈建议为已删除的项目添加重定向。但是，您可以通过将以下代码添加到主题的 `functions.php` 文件来禁用此功能。请注意，这只会隐藏通知消息，因此重定向仍可能在后台静默创建。

### 文章或页面：移至回收站
```php
add_filter('Yoast\WP\SEO\enable_notification_post_trash', '__return_false' );
```

### 文章或页面：更改 URL
```php
add_filter('Yoast\WP\SEO\enable_notification_post_slug_change', '__return_false' );
```

### 分类法：移至回收站
```php
add_filter('Yoast\WP\SEO\enable_notification_term_delete', '__return_false' );
```

### 分类法：更改 URL
```php
add_filter('Yoast\WP\SEO\enable_notification_term_slug_change', '__return_false' );
```