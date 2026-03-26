---
title: "Filtering Yoast Blocks"
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


自 [Yoast SEO 8.2](https://yoast.com/yoast-seo-8-2/) 起，我们添加了前两个结构化数据块：常见问题解答块和操作指南块。

## 可用区块

目前，Yoast SEO 中提供以下区块：

| 区块             | 命名空间               |
|------------------|------------------------|
| 常见问题解答块   | `yoast/faq-block`      |
| 操作指南块       | `yoast/how-to-block`   |

## 示例
虽然 WordPress 允许您使用 [`allowed_block_types`](https://developer.wordpress.org/reference/hooks/allowed_block_types/) 过滤器来筛选区块，但此过滤器存在一些限制。
筛选区块的更好方法是使用 JavaScript 实现。请注意，这也意味着您需要通过自己的插件或主题（例如 `functions.php`）中的 `wp_enqueue_script` 方法注册一个 JavaScript 文件，如下所示：

```php
// MyCustomPlugin.php
<?php
// 您的插件代码在此处。

wp_enqueue_script( 
	'my-custom-blocks-filter', 
	plugins_url( 'js/MyCustomBlocksFilter.js', __FILE__ ), 
	[], 
	'1.0', 
	true 
);
```

假设您只想从 Yoast 结构化数据块列表中加载常见问题解答块，并希望过滤掉操作指南块：

```js
// js/MyCustomBlocksFilter.js
const hiddenBlocks = [ 'yoast/how-to-block' ];

wp.blocks.getBlockTypes().forEach( function( block ) {
	if ( block.name && hiddenBlocks.includes( block.name ) ) {
		wp.blocks.unregisterBlockType( block.name );
	}
} );
```

反转 if 语句的逻辑（即对 `hiddenBlocks.includes( block.name )` 取反），将允许您限制您*确实*希望加载的区块。