---
title: "Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Schema
    - Features
    - Repos
---


## 让 Schema 更易于调试
如果您正在处理 Schema，可能会发现其可读性较差。要改变这一点，您应将 `yoast_seo_development_mode` 过滤器切换为 `true`。启用后，Yoast SEO 输出的所有 Schema 都将以美观格式呈现。

```php
add_filter( 'yoast_seo_development_mode', '__return_true' );
```

注意：**不建议**在生产环境中保持此设置开启。

如果您正在寻找更便捷的 Schema 调试方式，可以尝试使用[我们的 Yoast 测试助手插件](https://wordpress.org/plugins/yoast-test-helper/)，它能帮助您更轻松地进行调试。

## 在前端完全禁用 Schema
如果在 `wpseo_json_ld_output` 过滤器上返回 false 或空数组，你将禁用 Yoast SEO 的所有 schema 输出。

```php
add_filter( 'wpseo_json_ld_output', '__return_false' );
```

#### 技术限制：REST API 响应

当使用此过滤器（或通过其他方法，例如第三方插件）禁用 Schema 时，REST API 响应（例如 `http://example.com/wp-json/wp/v2/posts/<POST_ID>`）中的 `yoast_head` 属性会正确更新以排除 Schema 输出。然而，`yoast_head_json` 属性仍将包含 Schema 数据。

这是一个已知的限制，因为 HTML 和 JSON 输出使用不同的代码路径。`wpseo_json_ld_output` 过滤器旨在禁用前端 HTML `<script>` 标签的输出，但填充 `yoast_head_json`（该属性返回用于程序化访问的原始数据）的代码路径并未应用此过滤器。

## 添加或移除图谱片段
如[文档](docs/features/schema/plugins/yoast-seo)所示，我们在某些页面输出了大量图谱片段。您可能需要移除部分片段，或更理想地添加自定义片段。为此应使用 `wpseo_schema_graph_pieces` 过滤器。

### 移除图谱片段

在某些情况下，您可能希望阻止某些图谱片段被输出。以下示例演示了如何排除面包屑片段在输出中显示。

```php
// functions.php
add_filter( 'wpseo_schema_graph_pieces', 'remove_breadcrumbs_from_schema', 11, 2 );
add_filter( 'wpseo_schema_webpage', 'remove_breadcrumbs_property_from_webpage', 11, 1 );

/**
 * 从图谱收集器中移除面包屑图谱片段。
 *
 * @param array  $pieces  当前的图谱片段。
 * @param string $context 当前上下文。
 *
 * @return array 剩余的图谱片段。
 */
function remove_breadcrumbs_from_schema( $pieces, $context ) {
    return \array_filter( $pieces, function( $piece ) {
        return ! $piece instanceof \Yoast\WP\SEO\Generators\Schema\Breadcrumb;
    } );
}

/**
 * 从 WebPage 片段中移除面包屑属性。
 *
 * @param array $data WebPage 的属性。
 *
 * @return array 修改后的 WebPage 属性。
 */
function remove_breadcrumbs_property_from_webpage( $data ) {
    if (array_key_exists('breadcrumb', $data)) {
        unset($data['breadcrumb']);
    }
    return $data;
}
```

#### Enable / disabling graph pieces by filter
Sometimes you might want to enable or disable a graph piece based on other variables then the graph piece itself can determine. For instance, you might always want to show a Person on your site. You can do this by hooking into the `wpseo_schema_needs_<class-name>` filter. In this particular case, the code would look like this:

```php
add_filter( 'wpseo_schema_needs_person', '__return_true' );
add_filter( 'wpseo_schema_person_user_id', function() {
    return 1; // Make sure 1 is a valid User ID.
} );
```

It really is as simple as returning true on that to always show it, but you can of course also hook a function there with more precise logic. The second filter is needed to provide a user ID that should be used for the Person to show, as otherwise it won't work as the code doesn't know which user to show.

### 添加图谱片段
我们的每个图谱片段都继承自 `Abstract_Schema_Piece` 类。我们会向每个片段传递一个包含大量上下文变量的 `Meta_Tags_Context` 对象。具体示例可参考我们的[示例用例](docs/features/schema/integration-guidelines#an-example-use-case)，更深入的示例可在[Github此处](https://github.com/Yoast/wordpress-seo/blob/trunk/src/generators/schema/author.php)查看。

结合上述要求，添加新的图谱片段相当简单：

```php
// functions.php
add_filter( 'wpseo_schema_graph_pieces', 'add_custom_schema_piece', 11, 2 );

/**
 * 向图谱收集器添加自定义图谱片段。
 *
 * @param array  $pieces  当前图谱片段数组。
 * @param string $context 当前上下文。
 *
 * @return array 图谱片段数组。
 */
function add_custom_schema_piece( $pieces, $context ) {
   $this->context = $context;

   $pieces[] = new MyCustomSchemaPiece( $context );

   return $pieces;
}
```

## 引用其他图谱片段
您始终可以使用固定ID引用Yoast SEO核心图谱片段。这些ID可通过`Schema_IDs`类查找。例如，您可以找到`Schema_IDs::ORGANIZATION_HASH`、`Schema_IDs::PERSON_LOGO_HASH`等众多标识符。若您添加的片段需要引用`Organization`片段，只需如下操作：

```php
$data['organization'] = [ '@id' => Schema_IDs::ORGANIZATION_HASH ];
```

## 修改图谱片段数据
如需更改特定片段的输出，请接入我们的 `wpseo_schema_<class>` 过滤器。例如：

* [文章](docs/features/schema/pieces/article#api)
* [面包屑导航](docs/features/schema/pieces/breadcrumb#api)
* [组织](docs/features/schema/pieces/organization#api)
* [个人](docs/features/schema/pieces/person#api)
* [搜索操作](docs/features/schema/pieces/searchaction#api)
* [网页](docs/features/schema/pieces/webpage#api)
* [网站](docs/features/schema/pieces/website#api)

## 对整个 Schema 执行复杂更改
如果您需要对 Schema 执行复杂操作，例如更改输出中不同部分的值，可以接入我们的 `wpseo_schema_graph` 过滤器。例如：

```php
add_filter( 'wpseo_schema_graph', 'change_image_urls_to_cdn', 10, 2 );

/**
 * 将所有图片中的主机名替换为 CDN 主机名。
 *
 * @param array             $data    Schema.org 图谱。
 * @param Meta_Tags_Context $context 上下文对象。
 *
 * @return array 修改后的 Schema.org 图谱。
 */
function change_image_urls_to_cdn( $data, $context ) {
    foreach ( $data as $key => $value ) {
        if ( $value['@type'] === 'ImageObject' ) {
            $data[$key]['contentUrl'] = str_replace( 'http://basic.wordpress.test/', 'https://cdn.domain.tld/', $value['contentUrl'] );
        }
    }
    return $data;
}
```

## Gutenberg 区块的 Schema
如果您正在为区块编辑器（有时称为 "Gutenberg"）编写区块，您可能也希望添加 Schema 输出。
有两个有用的钩子可以帮助您实现这一点。

### 了解页面包含哪些区块
首先，您需要知道页面包含哪些区块。您可以通过 `wpseo_pre-schema_block-type_<block-type>` 操作来判断。如果该操作针对您的区块类型触发，您就知道该区块类型将输出到页面上。

例如，我们使用此过滤器通过以下简单函数来更改常见问题区块中的 `@typeof WebPage`：

```php
add_action( 'wpseo_pre_schema_block_type_yoast/faq-block', [ $this, 'prepare_faq_schema' ], 10, 1 );

/**
 * 若此操作触发，则表明页面存在常见问题区块，因此过滤页面类型。
 *
 * @param array $blocks 当前页面上此类型的区块数组。
 */
public function prepare_schema( $blocks ) {
    $this->blocks    = $blocks;
    $this->is_needed = true;

    add_filter( 'wpseo_schema_webpage_type', [ $this, 'change_schema_page_type' ] );
}
```

如您所见，此代码取自某个类内部。`wpseo_pre-schema_block_type_<block-type>` 操作会接收该类型所有区块的数组作为参数。我们将这些区块保存在类中，后续会使用这些数据来确定其输出。

### 特定区块的输出
如果您需要为特定区块添加输出，可以使用 `wpseo_schema_block_<block-type>` 过滤器。该过滤器包含 3 个参数：

* `$graph`：您必须返回此参数，它允许您根据区块筛选图谱。
* `$block`：触发此过滤器的区块内容。
* `$context`：这是一个 `Meta_Tags_Context` 对象，包含您可以在输出中使用的环境变量，例如站点 URL、规范链接等。

例如，我们可以像这样挂载一个 `joost-block`：

```php
add_filter( 'wpseo_schema_block_yoast/faq-block', [ $this, 'render_joost_block_schema' ], 10, 3 );
```

然后，当插件遇到 FAQ 区块时，会调用此函数，该函数本身会调用一个新类：

```php
/**
 * 根据区块中的数据渲染 Joost 区块的 Schema。
 *
 * @param array                 $graph   当前页面的 Schema 数据。
 * @param WP_Block_Parser_Block $block   区块数据数组。
 * @param Meta_Tags_Context     $context 包含上下文变量的值对象。
 *
 * @return array 我们的 Schema 图谱。
 */
public function render_joost_block_schema( $graph, $block, $context ) {
	$graph['data'] = $block['data'];
	$graph['id']   = YoastSEO()->meta->for_current_page()->canonical . '#/schema/joost/' . $block['id'];

	return $graph;
}
```