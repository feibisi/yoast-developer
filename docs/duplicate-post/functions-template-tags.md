---
title: "Functions Template Tags"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Duplicate Post
    - Repos
    - Data
---


本页面列出了可在插件和模板文件中使用的函数，用于与 Duplicate Posts 进行交互。

## `duplicate_post_clone_post_link`

显示一个克隆当前文章的链接，前提是用户已登录且被允许复制该文章。可在 [The Loop](https://codex.wordpress.org/The_Loop) 内部或外部使用。如果在循环外部使用，则需要传递文章 ID。
此函数仿照 [`edit_post_link()`](https://developer.wordpress.org/reference/functions/edit_post_link/) 设计。

### 用法

```php
duplicate_post_clone_post_link( $link, $before, $after, $id );
```

### 参数

 * `$link` (字符串)(可选) - 链接文本。默认为 `__('Clone','duplicate-post')`。
 * `$before` (字符串)(可选) - 放在链接文本之前的文本。默认为空字符串。
 * `$after` (字符串)(可选) - 放在链接文本之后的文本。默认为空字符串。
 * `$id` (整数)(可选) - 文章 ID。默认为当前文章 ID。

## `duplicate_post_get_original`

返回原始文章，可以是文章对象、关联数组或数字数组（取决于 `$output` 参数）。如果找不到原始文章，则返回 `null`。

它依赖于自定义字段 `_dp_original`，该字段在插件每次创建副本时都会被设置为原始 ID。
此函数仿照 [`get_post()`](https://developer.wordpress.org/reference/functions/get_post/) 设计。

### 用法

```php
$original_post = duplicate_post_get_original( $post, $output );
```

### 参数

 * `$post` (整数|WP_Post|null)(可选) - 文章 ID 或文章对象。默认为全局变量 `$post`。
 * `$output` (字符串)(可选) - 所需的返回类型。可选值为 `OBJECT`、`ARRAY_A` 或 `ARRAY_N`，分别对应 `WP_Post` 对象、关联数组或数字数组。默认为 `OBJECT`。

## `duplicate_post_create_duplicate`

根据当前设置，复制作为第一个参数传递的文章对象。这适用于除状态之外的所有内容：除非明确指定了新状态，否则默认复制原始状态。

### 用法

```php
$new_post_id = duplicate_post_create_duplicate( $post, $status, $parent_id );
```

### 参数

 * `$post` (WP_Post) - 要复制的文章对象。
 * `$status` (字符串)(可选) - 应用于复制后文章对象的状态。默认为原始文章的状态。
 * `$parent_id` (整数)(可选) - 设置为复制后文章父级的文章 ID。默认为空字符串。