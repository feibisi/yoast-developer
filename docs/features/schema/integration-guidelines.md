---
title: "Integration Guidelines"
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


## 简介
与 Yoast 结构化数据框架集成既简单，又对用户有益。我们鼓励所有插件/主题/软件作者考虑采用并扩展我们的方法。

您应该首先阅读[我们的规范](docs/features/schema/functional-specification)，并确保理解其核心原则。简而言之，这些原则是：

*   我们在网站的每个页面上输出一个结构化数据块，使用 [schema.org](http://schema.org) 词汇表描述 `Organization`、`WebSite`、`WebPage` 及其*主要实体*。
*   在可能的情况下，我们会用从用户或网站设置中获取的信息（例如 `Organization` 的 `logo` 或 `Article` 的 `author`）来丰富这些“片段”。
*   输出方式会构建一个单一的、连贯的 `JSON-LD` 格式的 `@graph` 对象。这可以被搜索引擎（和其他软件）读取，并为最终用户/网站所有者带来营销/集成/可发现性方面的好处。
*   [我们的 Schema API](docs/features/schema/api) 允许开发者修改或扩展图谱的输出。

## 一个使用案例
Yoast SEO 软件已经创建了一个大型结构化图谱，但存在一些我们（目前）不支持的内容类型。您可能希望为特定内容类型添加支持。

例如，您可能提供一个允许网站撰写*书评*的插件。在这种情况下，您可能希望在自定义文章类型 `book_review` 上描述一本 `Book`。为此，您应该有条件地输出自定义的 `Book` 片段，并将其连接到主图谱中。

实现此功能的示例代码可能如下所示。

注意：这仅在用户运行版本 14.0 或更高的 [Yoast SEO 插件](https://yoast.com/product/yoast-seo-wordpress/) 时才有效。插件/主题/第三方开发者应确定 Yoast SEO 插件是否正在运行，并相应地管理其输出。

### Create the piece.
First we create the `Book` object.

```php
/**
 * Class Book
 */
class Book  {

	/**
	 * A value object with context variables.
	 *
	 * @var WPSEO_Schema_Context
	 */
	public $context;

	/**
	 * Book constructor.
	 *
	 * @param WPSEO_Schema_Context $context Value object with context variables.
	 */
	public function __construct( WPSEO_Schema_Context $context ) {
		$this->context = $context;
	}

	/**
	 * Determines whether or not a piece should be added to the graph.
	 *
	 * @return bool Whether or not a piece should be added.
	 */
	public function is_needed() {

		// We only ever want to output this on 'book review' post types.
		if ( is_singular( 'book_review' ) ) {
			return true;
		}

		return false;
	}

	/**
	 * Adds our Book piece of the graph.
	 *
	 * @return array Book Schema markup.
	 */
	public function generate() {

		$canonical = YoastSEO()->meta->for_current_page()->canonical;
		$post_id = YoastSEO()->meta->for_current_page()->post_id;

		// Set the type.
		$data['@type'] = 'Book';

		// Give it a unique ID, based on the URL and the Post ID.
		$data['@id'] = $canonical . '#/book/' . $post_id;

		// Give it a name.
		$data['name'] = the_title_attribute( array( 'echo' => false ) );

		// Make it the main entity of the webpage we're on.
		$data['mainEntityOfPage'] = [ '@id' => $canonical ];

		// Add the book's author, if we know it.
		$author = get_post_meta( $post_id, 'author', true );
		if ( ! empty( $author ) ) {
			// In reality, we'd probably want to add more information than just the name here.
			$data['author'] = $author;
		}	

		return $data;
	}
}
```

### 将片段添加到图谱中。
接着，我们将 `Book` 片段添加到图谱中：

```php
add_filter( 'wpseo_schema_graph_pieces', 'add_book_piece', 11, 2 );

/**
 * 将 Schema 片段添加到我们的输出中。
 *
 * @param array                 $pieces  要输出的图谱片段。
 * @param \WPSEO_Schema_Context $context 包含上下文变量的对象。
 *
 * @return array 要输出的图谱片段。
 */
function add_book_piece( $pieces, $context ) {
	$pieces[] = new Book( $context );
	return $pieces;
}
```

### 更新图谱中的其他相关部分
我们知道，在 `book_review` 类型的文章中，页面内容与该页面讨论的 `Book` 相关。我们可以通过引用 `Book` 的 ID 来在知识图谱中体现这一点。

```php
add_filter( 'wpseo_schema_webpage', 'make_webpage_about_book' );

/**
 * 修改网页节点，使其“关于”我们的书籍
 *
 * @param array $data 图谱数据。
 *
 * @return array 修改后的图谱
 */
public static function make_webpage_about_book( array $data ) : array {

	$canonical = YoastSEO()->meta->for_current_page()->canonical;
	$post_id = YoastSEO()->meta->for_current_page()->post_id;

	$data['about'] = array( '@id' => $canonical . '#/book/' . $post_id );
	return $data;
}
```

## 常见问题

### 我的插件/主题已输出 Schema 标记；如何集成？

如果您的插件已输出 Schema.org 元数据，您应该能够相对轻松地将其连接到 Yoast SEO 的图谱。

如果您尚未将独立节点添加到 `@graph`（并为这些节点分配 `@id`），可能需要做一些修改以实现有效集成。如果您已采用基于 ID 的方法，可以通过使用[我们的 Schema API](docs/features/schema/api) 并遵循[我们的规范](docs/features/schema/functional-specification)，将您的片段“缝合”到我们的图谱中。

如果您采用不同的方法（例如输出结构化 `JSON-LD` 树或使用微数据），可能存在信息冲突的风险，导致页面表示不准确。

在这种情况下，我们建议您调整代码以遵循上述示例中的方法，并禁用插件/主题的输出（因为我们的逻辑将管理完整图谱的构建和输出）。

注意：如果检测到用户*未*运行 [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/)，且希望最大化与*其他*插件的互操作性，您可以按照[我们的规范](docs/features/schema/functional-specification)构建自己的图谱。

### 1. 如何对 Schema 进行去重？
如果你输出的 Schema 与 Yoast SEO 的输出相同，你可能需要对其进行去重。假设你输出了 `Organization` 数据。如果你的数据没有 Yoast 输出的那么详尽，你可以简单地用 [引用](https://developer.yoast.com/features/schema/api/#referencing-other-graph-pieces) Yoast 的 `Organization` 数据来替换你的 `Organization` 数据。如果你的数据更详尽，我们建议 [过滤 Yoast SEO 对该特定部分的输出](https://developer.yoast.com/features/schema/api/#change-a-graph-pieces-data) 以包含你的数据。

或者，如果两个部分中没有相互冲突的值，你可以直接使用相同的 `@id` 属性。消费者会合并这两个部分，并整合信息。

### 2. How do I connect my Schema to Yoast's?

To connect your Schema output to Yoast SEO's Schema output, you need to determine where in the graph it needs to connect. You have to do this whether you [add a Graph piece](https://developer.yoast.com/features/schema/api/#adding-graph-pieces) through Yoast's API, or you just output your Schema via your own methods. 

In most cases, you will want to content to the `WebPage`. The `WebPage` is unique in our Schema graph, as its `@id` is always the URL of that page. So, if you have for instance a `Recipe`, you can simply add this to your `Recipe` output:

```json
  "isPartOf": { "@id": "<page url>" }
```

By adding the above, (and of course replacing `<page url>` with the actual URL of that page), Schema consumers will know to "stitch" your Schema output into the `@graph` that Yoast SEO creates.

Getting the corect URL to reference can be done easily using the `YoastSEO()` surface:

```php
YoastSEO()->meta->for_current_page()->canonical
```

### 3. 如何知道在哪里连接我的片段？

在上面的例子中，我们使用了 `isPartOf`。如果你的 Schema "片段" 不是页面上最重要的内容，这没问题。但如果你的页面是一个食谱页面，它不仅仅是"一部分"。它是主要内容。在这种情况下，你不应该使用 `isPartOf`，而应该使用 `mainEntityOfPage`，即：

```json
  "mainEntityOfPage": { "@id": "<page url>" }
```